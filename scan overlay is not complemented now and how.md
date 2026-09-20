Edited gui_decentralized.py
Edited index4_offline.html
Edited index4_offline.html
Edited index4_offline.html
Edited index4_offline.html
Viewed kiosk-config.js:1-128
Viewed gui_decentralized.py:432-451
Searched for "SCAN_WHILE_CHARGING"
Ran command: `grep -n "SCAN_WHILE_CHARGING" "/Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evck160c- decentralized/opt/evck/gui_app/gui_decentralized.py"`

Based on my analysis of the code, the **Scan Overlay** (which prompts the user to scan their card to stop charging) is activated through the following mechanism:

### 1. Frontend Activation (`kiosk-ui.js`)
The visibility of the overlay is controlled by the `updateScanOverlay` method in the frontend. It is **phase-driven**:
*   When the browser receives a `nozzle_status` update where the **`phase`** is equal to **`SCAN_WHILE_CHARGING`**, the overlay becomes visible.
*   The logic also updates the text inside the overlay using the `message` field from the payload (defaulting to: *"برای توقف، کارت آغازگر را نزدیک کنید"*).

### 2. Backend Trigger (`gui_decentralized.py`)
In the original version of the code, this phase was triggered in the `handle_keys` function:
*   When a "Stop" key (SW1/SW3) was detected while the gun was in the `CHARGING` phase, the backend would set `gs.phase = Phase.SCAN_WHILE_CHARGING`.
*   This would then be emitted to the browser, triggering the overlay.

### 3. Current Status
In our recent simplification of `gui_decentralized.py`, I removed the specific state-switching logic from `handle_keys` to make it a pure "pass-through" for keys. Consequently:
*   **The automatic internal transition to `SCAN_WHILE_CHARGING` is currently missing.**
*   The hardware service (`opi_hardware.py`) currently publishes `stop/gun_x` directly when the button is pressed, which skips the "Scan to Stop" overlay and goes straight to the stopping/finished phase.

**Would you like me to re-implement the "Scan to Stop" flow?** 
We can do this in two ways:
1.  **Hardware-led**: `opi_hardware.py` could publish to a `request_stop/gun_x` topic, and the GUI reacting by showing the scan overlay.
2.  **GUI-led**: We add logic to `gui_decentralized.py` to intercept the `stop` command or the raw key press and enter the `SCAN_WHILE_CHARGING` state if the gun is currently charging.