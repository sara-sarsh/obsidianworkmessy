 Here are the tools and stuff  we need and use in this project,
 - Message Broker
 - [[MQTT|نحوه نصب mqtt]]
   Formerly we use the Redis Pub/Sub but since we are immigrating to the  decentralize architecture, we need new and better altarnatives. Here is what I found based on talking the Gemini 3.1 Pro:
>  MQTT (via Eclipse Mosquitto) - Top Alternative
	MQTT is the industry standard for IoT and edge devices. Instead of a database acting as a stream, it is a dedicated publish/subscribe message broker.
	- How it solves your problem: MQTT has Quality of Service (QoS) levels. If you publish a command with QoS 1 (At least once) or QoS 2 (Exactly once), the broker ensures the component (e.g., lpc_interface) receives it, even if the component briefly crashes and reconnects.
	- Retained Messages: This is a killer feature for EVSEs. If lpc_interface publishes its hardware state as a “retained message”, any component that boots up later (like the GUI) instantly receives the last known state upon subscribing, without having to ask for it.
	Pros: Extremely lightweight (Mosquitto uses almost no RAM), built-in routing, native to Python via paho-mqtt.
	Cons: You would have to replace Redis with Mosquitto in your deployment stack.
	
	\- Gemini 3.1 Pro
so my worries is about it's Retained Messages, I think my new artichitecture is to publish eash command (event) on it's own channel named accordingly, the same name maybe
for example start, stop, and other events it's messege publishs from source on it's own channel and on the distenation we get that and do the needed tasks,
the distanations are not one, as you see each event can have multiple distanations and I need to make sure all of them get the command exactly once.
so tell me, I need to do what? the script, the mock and the test.
 > Here is the approach we need to take:
 > - must separate Commands (Events) from State:
 > 	- Commands (evck/command/...): Examples: start_charge, stop_charge, reboot.
		Rule: __Retain = False. They happen at a specific moment.__
		Guarantee: Use QoS 2 (Exactly Once). The MQTT broker ensures that every subscribed component gets the command exactly once. If a component is offline, you can configure the broker (using “Clean Session = False”) to hold the command and deliver it the moment the component reconnects.
		State (evck/state/...): Examples: voltage, gun_status, ocpp_connection_status.
		Rule: Retain = True.
		Guarantee: Use QoS 1 (At Least Once). It doesn’t matter if it gets sent twice; it’s just the current status. When the GUI boots up, it reads the retained evck/state/gun_status and instantly knows if the car is plugged in.
# More about the mqtt:
## How an MQTT broker works internally
- Protocol
	MQTT (Message‑Queuing Telemetry Transport) is TCP‑based
	- very lightweight (perfect for IoT)
	- has built‑in QoS levels (exactly‑once, at‑least‑once, etc.)
	- uses topic hierarchy (evck/charger/12/status)
-  Lifecycle
	Connect
	
	Client.connect(host, port) opens a TCP + MQTT session.
	
	Publish (send)
	
	The client sends a PUBLISH packet with topic, payload, qos, and retain flag.
	
	Subscribe (receive)
	
	The client sends SUBSCRIBE for one or more topic filters.
	
	The broker keeps track: “Client X subscribes to topic evck/#”.
	
	Receive
	
	Whenever someone publishes a matching topic, the broker pushes the message to all subscribers.
	
	Disconnect / Reconnect
	
	Broker closes or reopens connections as needed.
	
	MQTT supports session persistence and automatic reconnection.
	
5. What is a “callback”?
	A callback is simply:
	
	a function you pass to another system so it can call you later.
	
	In MQTT:
	
	You register a function that will run when a message arrives.
	Example:

	
	broker.subscribe("evck/charger/+/status", handle_status)
	When a message arrives:
	
	_on_message → finds matching subscription → calls callback