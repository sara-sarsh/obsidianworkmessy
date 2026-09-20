You’re essentially designing the **event backbone of the charger**, so it’s worth thinking about this like a distributed system rather than “just MQTT topics”. I’ll walk through:

1. How MQTT topics should be structured  
2. How each microservice should use the messaging layer  
3. When to use retained messages (and when NOT to)  
4. How to guarantee reliable command delivery  
5. Improvements to your `_on_message` implementation  

---

# 1. First principle: Topics are your system architecture

MQTT topics should represent **semantic domains**, not random channels.

A good topic hierarchy looks like:

```
evck/{device_id}/{domain}/{event}
```

Example:

```
evck/charger-1/gui/start
evck/charger-1/hardware/status
evck/charger-1/ocpp/transaction_started
```

General pattern:

```
system / device / service / event
```

For your charger:

```
evck/{charger_id}/{service}/{event}
```

Example:

```
evck/charger-1/gui/start
evck/charger-1/gui/stop
evck/charger-1/hardware/plug_detected
evck/charger-1/ocpp/authorize_request
evck/charger-1/ocpp/authorize_result
```

This makes wildcard subscriptions powerful.

Example:

```
evck/charger-1/gui/#
```

means:

```
all GUI commands
```

---

# 2. How each microservice should use MQTT

Your services:

```
GUI (Flask)
OCPP interface
LPC hardware interface
```

Think **event-driven**.

---

# GUI service

GUI publishes commands.

Example:

```
User clicks "Start Charging"
```

GUI publishes:

```
topic:
evck/charger-1/gui/start

payload:
{
  "type": "command",
  "name": "start",
  "source": "gui",
  "payload": {
    "connector": 1
  }
}
```

Subscribers:

```
hardware service
ocpp service
```

---

# Hardware service

Subscribes:

```
evck/charger-1/gui/start
evck/charger-1/gui/stop
```

Hardware receives start → sends command to LPC.

Then publishes status:

```
evck/charger-1/hardware/charging_started
```

---

# OCPP service

Subscribes:

```
evck/charger-1/hardware/#
```

When charging starts:

```
hardware → event
```

OCPP converts to:

```
StartTransaction.req
```

---

# Resulting event flow

```
GUI
 ↓
MQTT
 ↓
hardware service
 ↓
MQTT event
 ↓
OCPP service
```

No service calls another directly.

This is **true decoupling**.

---

# 3. Your idea: one topic per command

You suggested:

```
start → its own topic
stop → its own topic
```

That is actually **good MQTT design**.

Example:

```
evck/charger-1/cmd/start
evck/charger-1/cmd/stop
evck/charger-1/cmd/reset
```

Subscribers:

```
hardware → evck/charger-1/cmd/#
```

or:

```
evck/charger-1/cmd/start
```

MQTT is optimized for this.

---

# 4. Ensuring every service receives commands exactly once

MQTT gives reliability through **QoS**.

### QoS levels

```
QoS 0
at most once
(no retry)
```

```
QoS 1
at least once
(duplicates possible)
```

```
QoS 2
exactly once
(two-phase handshake)
```

For **commands**:

```
QoS 2
```

Example:

```python
broker.publish(topic, message, qos=QoS.EXACTLY_ONCE)
```

Now MQTT guarantees:

```
publisher → broker → subscriber
```

exactly once.

But important caveat:

If **multiple subscribers exist**, each receives it once.

Example:

```
GUI → start command

hardware service
ocpp service
logger service
```

All receive it exactly once.

---

# 5. Should commands be retained?

Short answer:

**No.**

Commands should **NOT** be retained.

---

### What happens if `retain=True`

Example:

```
publish start command (retain=True)
```

Broker stores it.

Now imagine:

```
hardware service crashes
```

Later it reconnects and subscribes again.

The broker immediately sends the **retained command**.

Result:

```
charger starts again unexpectedly
```

That is dangerous.

Commands must be **transient events**.

So:

```
retain = False
```

for commands.

---

# 6. When retained messages ARE useful

Use retain for **state**, not commands.

Examples:

```
evck/charger-1/status
evck/charger-1/connector/1/state
evck/charger-1/network/status
```

Payload:

```
{
 "state": "charging"
}
```

Now when a service restarts it immediately knows:

```
current charger state
```

This is extremely useful.

---

# 7. Clean architecture suggestion

Separate topics by type:

```
evck/{charger}/cmd/{command}
evck/{charger}/event/{event}
evck/{charger}/state/{state}
```

Example:

```
evck/charger-1/cmd/start
evck/charger-1/cmd/stop

evck/charger-1/event/plug_inserted
evck/charger-1/event/charging_started

evck/charger-1/state/charger
evck/charger-1/state/connector/1
```

Rules:

```
cmd → QoS2, retain=False
event → QoS1 or 2, retain=False
state → QoS1, retain=True
```

---

# 8. Now let’s analyze your `_on_message`

Your code:

```python
def _on_message(self, client, userdata, msg):
    try:
        data = json.loads(msg.payload.decode())
    except Exception:
        logger.warning("Invalid JSON message on topic %s", msg.topic)
        return
```

This is good.

But decoding should also handle **bytes errors**.

Better:

```python
payload = msg.payload.decode("utf-8", errors="ignore")
```

---

### Structured logging

Your logging:

```python
logger.info(
    "[MQTT RX] topic=%s type=%s name=%s",
    msg.topic, data.get("type"), data.get("name")
)
```

This is excellent.

I would add source:

```
source=%s
```

---

# 9. Major issue in your dispatch logic

Your code does:

```
exact topic match
then wildcard match
```

This can cause **double callback execution**.

Example:

```
subscription1 = evck/charger-1/cmd/start
subscription2 = evck/charger-1/cmd/#
```

Both will trigger.

Sometimes that's intended, sometimes not.

Better approach:

Only use **wildcard-aware matching**.

Remove exact check.

---

# 10. Clean improved version

```python
def _on_message(self, client, userdata, msg):

    payload = msg.payload.decode("utf-8", errors="ignore")

    try:
        data = json.loads(payload)
    except Exception:
        logger.warning(
            "[MQTT RX] invalid json topic=%s payload=%s",
            msg.topic,
            payload[:120],
        )
        return

    logger.info(
        "[MQTT RX] topic=%s type=%s name=%s source=%s",
        msg.topic,
        data.get("type"),
        data.get("name"),
        data.get("source"),
    )

    # dispatch callbacks
    for sub, cb in self.callbacks.items():

        if mqtt.topic_matches_sub(sub, msg.topic):

            try:
                cb(msg.topic, data)

            except Exception as e:
                logger.exception(
                    "Callback failed for topic=%s sub=%s",
                    msg.topic,
                    sub,
                )
```

Improvements:

• safe decoding  
• better logs  
• wildcard only  
• exception isolation  
• avoids double execution  

---

# 11. Critical improvement for microservices

Add **service identity**.

Example envelope:

```
{
 "type": "command",
 "name": "start",
 "source": "gui",
 "timestamp": 1712480000,
 "payload": {}
}
```

This helps with:

```
debugging
replay
tracing
```

---

# 12. One more thing professionals always add

Add **message id**.

```
{
 "id": "uuid",
 "type": "command",
 "name": "start",
 "source": "gui"
}
```

This allows **deduplication protection** if needed.

---

# 13. Final architecture for your charger

```
               MQTT Broker
                     │
      ┌──────────────┼──────────────┐
      │              │              │
    GUI           Hardware        OCPP
      │              │              │
 publish cmd      execute       talk to CSMS
      │              │              │
      └──────→ events/state ←──────┘
```

Everything flows through **events**.

No service directly calls another.

This is exactly what you want for **decentralized architecture**.

---

If you want, I can also show you **the exact production-grade topic layout used in real EV chargers and IoT fleets** (it solves several subtle scaling problems most people only discover later).