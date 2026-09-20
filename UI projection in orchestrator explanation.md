I'll explain both methods line by line:

## `_calculate_ui_phase` method

```python
def _calculate_ui_phase(self, gun: str, gun_live_state: GunState) -> Phase:
```
Method signature that takes a gun identifier and its current state, returns a Phase enum.

```python
charge_s = gun_live_state.charge_status
```
Extract the hardware charge status from the gun's state.

```python
if self._emc_active:
    return Phase.FAULTED
```
If emergency stop is active, immediately return FAULTED phase regardless of other conditions.

```python
hold_code = self._error_hold_code.get(gun)
hold_until = self._error_display_until.get(gun, 0.0)
```
Retrieve any error code being held for display and its expiration timestamp.

```python
if hold_code and time.time() < hold_until:
    return hold_code
```
If there's an active error hold that hasn't expired, return that error code as the phase.

```python
if hold_code and str(charge_s) != hold_code and time.time() >= hold_until:
    self._error_hold_code[gun] = None
```
If the error hold expired AND the current status differs from the held error, clear the hold.

```python
cs_str = charge_s.name if hasattr(charge_s, 'name') else str(charge_s)
```
Convert charge status to string, using `.name` if it's an enum, otherwise string conversion.

```python
if cs_str.startswith("ERROR") or cs_str.startswith("ChargeStatus.ERROR"):
```
Check if the status string indicates an error condition.

```python
if cs_str.startswith("ChargeStatus."):
    return cs_str.split(".")[1]
return cs_str
```
If error has "ChargeStatus." prefix, strip it and return just the error name. Otherwise return the full string.

```python
match charge_s:
```
Pattern matching on the hardware charge status to map to UI phases.

```python
case ChargeStatus.NOT_CONNECTED:
    return Phase.IDLE
```
No cable connected → IDLE phase.

```python
case ChargeStatus.CONNECTED:
```
Cable connected but not charging yet.

```python
if gun_live_state.phase == Phase.AUTHORIZING:
    return Phase.AUTHORIZING
```
Preserve AUTHORIZING phase if auth is in progress.

```python
if gun_live_state.phase == Phase.AUTH_REJECTED:
    return Phase.AUTH_REJECTED
if gun_live_state.phase == Phase.AUTHORIZED:
    return Phase.AUTHORIZED
```
Preserve auth-related phases while cable remains connected.

```python
if gun_live_state.phase == Phase.WAIT_CARD:
    start_wait = self._wait_card_ts.get(gun, 0)
    if (time.time() - start_wait) > self._wait_card_timeout_s:
        log.info("[TIMEOUT] Gun %s WAIT_CARD timed out. Reverting to QR.", gun)
        return Phase.PLUGGED
    return Phase.WAIT_CARD
```
If waiting for card payment, check timeout. If expired, revert to QR flow (PLUGGED), otherwise keep WAIT_CARD.

```python
return Phase.PLUGGED
```
Default for CONNECTED status is PLUGGED (ready for auth).

```python
case ChargeStatus.CHECKING:
    return Phase.WAITING
```
Hardware is checking/preparing → WAITING phase.

```python
case ChargeStatus.CHARGING:if gun_live_state.phase == Phase.SCAN_WHILE_CHARGING:
        return Phase.SCAN_WHILE_CHARGING
    return Phase.CHARGING
```
If actively charging, preserve SCAN_WHILE_CHARGING if set, otherwise return CHARGING.

```python
case ChargeStatus.FINISH:
    f_lock = self._finished_lock.get(gun)
```
Charging complete. Check if there's a "finished lock" timestamp.

```python
if f_lock is not None:
    if (time.time() - f_lock) < self._finished_lock_timeout_s:
        return Phase.FINISHED
    else:
        self._set_finished_lock(gun, None, "finished_lock_timeout")
```
If lock exists and hasn't timed out, return FINISHED. If timed out, clear the lock.

```python
return Phase.FINISHED
```
Default for FINISH status is FINISHED phase.

```python
case ChargeStatus.UNKNOWN:
    return Phase.FAULTED
```
Unknown hardware status → FAULTED.

```python
case _:
    log.warning("[PHASE] Unknown charge_status: %s", charge_s)
    return Phase.IDLE
```
Catch-all for unexpected statuses, log warning and default to IDLE.

## `_pub_nozzle` method

```python
def _pub_nozzle(self, state: str | None = None, gun: str = "A", message: str = None):
```
Method to publish UI status for a gun, with optional state override.

```python
gun = gun.upper()
gun_live_state: GunState = self.state_by_gun[gun]
```
Normalize gun ID to uppercase and retrieve its state object.

```python
final_phase = self._calculate_ui_phase(gun, gun_live_state)
```
Calculate the UI phase using the previous method.

```python
if state:
    m = { ... }
    final_phase = m.get(state, final_phase)
```
If a legacy state string was provided, map it to a Phase enum, overriding the calculated phase.

```python
phase_name = final_phase.name if isinstance(final_phase, Phase) else str(final_phase)
```
Extract phase name as string.

```python
if not isinstance(final_phase, Phase) and not phase_name.startswith("ERROR"):
    log.warning("[GUI STATE] Unknown UI Phase: %s", final_phase)
```
Warn if phase isn't a proper Phase enum and isn't an error string.

```python
gun_live_state.phase = final_phase
```
Update the gun's state with the final phase.

```python
qr_candidate = (final_phase == Phase.PLUGGED)
```
QR code should be shown only in PLUGGED phase.

```python
now = time.time()
cur = self._qr_visible[gun]
pend = self._qr_pending[gun]
```
Get current time, current QR visibility state, and any pending visibility change.

```python
if qr_candidate == cur:
    self._qr_pending[gun] = None
```
If desired QR state matches current state, clear any pending change.

```python
else:
    if pend is None:
        self._qr_pending[gun] = (qr_candidate, now)
```
If no pending change exists, create one with the new desired state and timestamp.

```python
else:
    pend_val, pend_ts = pend
    if pend_val != qr_candidate:
        self._qr_pending[gun] = (qr_candidate, now)
```
If pending change exists but differs from new desired state, replace it with new pending change.

```python
elif (now - pend_ts) >= self._qr_debounce_s:
    self._qr_visible[gun] = qr_candidate
    self._qr_last_change_ts[gun] = now
    self._qr_pending[gun] = None
```
If pending change has been stable for debounce duration, apply it and clear pending state.

```python
show_qr_code = self._qr_visible[gun]
```
Use the debounced visibility state.

```python
if final_phase in (Phase.WAITING, Phase.CHARGING, ...):
    show_qr_code = False
```
Force hide QR in certain advanced phases regardless of debounce logic.

```python
duration_s = gun_live_state.duration_s
```
Get stored duration.

```python
if gun_live_state.t_start:
    if final_phase in (Phase.CHARGING, Phase.SCAN_WHILE_CHARGING):
        duration_s = int(now - gun_live_state.t_start)
        gun_live_state.duration_s = duration_s
```
If charging is active, calculate live duration from start time and update state.

```python
elif final_phase == Phase.FINISHED and gun_live_state.duration_s:
    duration_s = int(gun_live_state.duration_s)
```
If finished, use the stored final duration.

```python
price = self.price_per_kwh_irr.get(gun, PRICE_IRR)
total_cost = int(gun_live_state.energy_kwh * price)
gun_live_state.total_cost = total_cost
```
Calculate total cost from energy consumed and price per kWh, update state.

```python
if final_phase == Phase.FINISHED:
    if gun_live_state.final_duration_s is None: gun_live_state.final_duration_s = duration_s
    if gun_live_state.final_energy_kwh is None: gun_live_state.final_energy_kwh = gun_live_state.energy_kwh
    if gun_live_state.final_cost is None: gun_live_state.final_cost = total_cost
```
When session finishes, snapshot final metrics if not already set (prevents overwriting).

```python
metrics = { ... }
```
Build metrics dictionary with all current and final session data, including extended hardware metrics.

```python
pl = { ... }
```
Build payload dictionary with gun ID, phase, status, metrics, and other UI-relevant fields.

```python
if gun_live_state.phase != gun_live_state._prev_logged_phase:
    log.info("[FLOW] ...")
    gun_live_state._prev_logged_phase = phase_name
```
Log phase transitions only (not every status update) to reduce noise, then update the last logged phase.

```python
self.bus.publish("gui_status_channel", envelope("status", "nozzle_status", "orchestrator", pl))
```
Publish the UI payload to the message bus for the kiosk interface to consume.