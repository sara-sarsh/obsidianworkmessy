
Here’s how this GUI app works, end‑to‑end, based on the files you attached (`gui.py`, `index4.html`, `kiosk.js`, `kiosk-ui.js`, `kiosk-utils.js`).

I’ll walk through it like a system architect would, but keep it focused and practical.

---

## 1. Big Picture

This is a **kiosk GUI** (likely for an EV charger) built as:

- **Backend:**
  - `gui.py` → Flask + Flask‑SocketIO server.
  - Talks to the “real system” (charger/orchestrator) via a `MessageBroker`.
  - Keeps a **cache of the current device state**.
  - Pushes real‑time updates to browsers via WebSockets (Socket.IO).

- **Frontend (browser):**
  - `index4.html` → main HTML layout for the kiosk screen.
  - `kiosk.js` → main JS controller: socket connection + global orchestration.
  - `kiosk-ui.js` → per‑nozzle UI controller (`NozzleUI` class).
  - `kiosk-utils.js` → formatting + DOM utility functions.

Conceptually:

```text
[Hardware / Orchestrator] 
      ⇅ MessageBroker
   [gui.py backend]
      ⇅ Socket.IO (WebSocket)
  [Browser: index4.html + kiosk.js + kiosk-ui.js]
```

---

## 2. Backend (`gui.py`): What it does

### 2.1. Flask app

- Creates a Flask app, serving:
  - Static files (`static/`), which include your JS, CSS, fonts, slider images.
  - Templates (`templates/`), including `index4.html`.
- Registers routes such as:
  - `/static/fonts/<path:filename>` → `serve_fonts()`  
  - `/static/slider/<path:filename>` → `serve_slider()`
- Adds a Jinja helper `static_path()` so that in `index4.html` you can do:
  ```jinja2
  <script src="{{ static_path('js/kiosk.js') }}"></script>
  ```

There will also be a route that returns the main page:

```python
@app.route("/")
def index():
    return render_template("index4.html", ...)
```

(even if not shown explicitly in the snippet, the whole structure implies this).

### 2.2. Real‑time channel (Socket.IO)

- `socketio = SocketIO(app, async_mode="threading", ...)`
- This is what lets the backend **push** updates instead of the browser polling.
- Backend will do things like:
  ```python
  socketio.emit("update", payload)   # or some event name
  ```
  where `payload` encodes the **current status of each nozzle**.

### 2.3. Talking to the rest of the system

- `bus = MessageBroker(client_name="gui")`  
  This connects to the rest of the app (the real charger/orchestrator process).
- The orchestrator sends:
  - “Nozzle A started charging”
  - “Nozzle B error: card failed”
  - “Nozzle A finished, here are kWh/time/price”
- `gui.py` receives those, **normalizes** them, and updates:
  ```python
  state_cache = {
      "config": ...,
      "net_status": ...,
      "ad_image": ...,
      "nozzle_A": ...,
      "nozzle_B": ...,
  }
  ```
- Then it emits events over Socket.IO so the browser can re‑render.

### 2.4. Configuration & version

- Finds `device.json` in several possible locations (`CONFIG_PATHS`).
- Uses `_deep_merge(base, override)` to combine configs.
- `get_current_version()` inspects:
  1. A version marker file (e.g. `v1.0.21`),
  2. `git describe --always`,
  3. A `VERSION` file.
- This info is used both in the backend and passed into the front‑end (e.g. for display or debugging overlays).

---

## 3. Frontend HTML (`index4.html`): Structure

`index4.html` is the actual kiosk page that you see on the screen.

It includes:

- CSS for layout and theming.
- Fonts (served via `/static/fonts/...`).
- JS modules: `kiosk.js`, `kiosk-ui.js`, `kiosk-utils.js` (or a bundled version).

The DOM has a structured layout:

- **Panels per nozzle** (A & B):
  - `panel-gun-A`, `panel-charge-A`, `panel-loading-A`, `panel-finished-A`, `panel-error-A`
  - and similar `...-B` for nozzle B.
- **Battery indicator**:
  - `battery-A`, `battery-B`
- **Ad/Guide slider**:
  - `#ad-slider-track`
  - `#ad-dots`
- Various metric fields (with IDs) for:
  - Energy, time, cost, etc.
- Error / message areas.

All those IDs are what `NozzleUI` and utility functions use to selectively update the UI.

---

## 4. Frontend main controller (`kiosk.js`)

This file is the **brain on the browser side**.

### 4.1. Socket setup

It decides how to connect:

```js
window.DEV_MODE = false; // can be toggled

function createDevSocket() {
  // stubbed object that imitates socket.io
  // used for UI testing without backend
}

let sock = DEV_MODE ? createDevSocket() : io();
window.sock = sock;      // for debugging
window.socket = sock;    // often same alias
```

- In **normal mode**:
  - `io()` connects to `gui.py`’s Socket.IO server.
- In **dev mode**:
  - `createDevSocket()` gives a fake socket object:
    - `.on(event, handler)` → stores handlers.
    - `.emit(...)` → may be no‑ops.
    - Keeps `window._devUpdateHandler`, so you can manually inject events like:
      ```js
      window._devUpdateHandler({ gun: 'A', state: 'CHARGING', ... });
      ```

### 4.2. Nozzle controllers

```js
const nozzles = {
  A: new NozzleUI('A'),
  B: new NozzleUI('B'),
};
```

You now have two independent UI controllers, one for each gun.

### 4.3. Handling incoming events

There’s a main handler, conceptually:

```js
sock.on("update", (payload) => {
  // figure out which gun this relates to
  // call handleNozzleUpdate(gun, payload);
});
```

`handleNozzleUpdate(gun, payload)` is where the interesting logic lives.

#### 4.3.1. Phases & finish‑screen hold

- Phase is derived from the payload:

  ```js
  const phase = (payload.state || 'IDLE').toUpperCase();
  ```

- States come from a `STATES` constant (in a config file), e.g.:

  - `IDLE`, `WAITING`, `CHARGING`, `SCAN_WHILE_CHARGING`,
  - `FINISHED`, `ERROR`, `FAULTED`, `ERROR_CARD`, etc.

- There’s a **“finished hold” mechanism**:

  ```js
  const finishHold   = { A: false, B: false };
  const finishQueue  = { A: [], B: [] };
  ```

  Behavior:

  - If a gun is currently showing a **FINISHED** screen (summary of a completed session), you don’t want it to instantly disappear when the backend transitions back to `IDLE`.
  - So:
    - When phase is `FINISHED`:
      - Immediately update the UI (`nozzles[gun].update(payload)`).
      - Start a timer for `VISUAL_CONFIG.TIMINGS.FINISH_DISPLAY_MS`.
      - `finishHold[gun] = true`.
    - If **any further events** arrive for that gun while `finishHold[gun]` is `true`:
      - They are added to `finishQueue[gun]` and **not processed immediately**.
    - When the timer ends:
      - `finishHold[gun] = false`.
      - Process the next queued event, if any.

This is purely for UX: the user gets a stable “Charging finished” summary screen.

#### 4.3.2. Slider logic

`kiosk.js` manages an **ad/guide slider** that changes content depending on interaction state.

State:

```js
let sliderItems = [];
let sliderIndex = 0;
let sliderTimer = null;
let currentSliderManifest = null;

const currentPhases = { A: STATES.IDLE, B: STATES.IDLE };
```

Key function: `checkSliderContext(gun, phase)`:

- Updates `currentPhases[gun] = phase`.
- Checks if **any** gun is in an “interaction phase”:
  - Interaction phases are enumerated in `SLIDER_CONFIG.INTERACTION_PHASES` (e.g. “WAITING”, “SCAN”, “CHARGING_INIT”).
- If interacting:
  - It uses a **guide** slider manifest:
    ```js
    targetManifest = SLIDER_CONFIG.GUIDE_URL;
    ```
- If not interacting:
  - Uses a **promo** slider manifest:
    ```js
    targetManifest = SLIDER_CONFIG.PROMO_URL;
    ```
- When `targetManifest` changes:
  - It calls `loadAdSlider(targetManifest)`.
  - `loadAdSlider` fetches the manifest, builds `items`, and calls:

    ```js
    buildAdSlider(items);
    ```

`buildAdSlider(items)`:

- Clears `#ad-slider-track` and `#ad-dots`.
- Creates slide elements per `items`.
- Resets `sliderItems`, `sliderIndex`, and restarts the auto‑advance timer.

Result: the big ad/guide area **automatically switches** between generic promos and step‑by‑step guides depending on what users are doing at the guns.

---

## 5. Per‑nozzle UI controller (`kiosk-ui.js`)

This file defines the `NozzleUI` class.

### 5.1. Construction

```js
class NozzleUI {
  constructor(gun) {
    this.gun = gun;         // 'A' or 'B'
    this.currentPhase = STATES.IDLE;

    // Pre-generate frames for finish animation, e.g.
    this.finishedFrames = this.generateFinishedFrames(60);
  }
}
```

It assumes certain DOM IDs exist, like:

- `panel-gun-A`, `panel-charge-A`, `panel-loading-A`, `panel-finished-A`, `panel-error-A`
- `battery-A`
- metric IDs (energy, time, cost, etc.)
- message/error text areas.

### 5.2. Main method: `.update(payload)`

Simplified:

```js
update(payload) {
  const phase = (payload.state || STATES.IDLE).toUpperCase();
  this.currentPhase = phase;

  // Verbose debug logging
  console.debug('[NozzleUI]', this.gun, phase, payload.message, payload.show_qr_code);

  this.updatePanels(phase);
  this.updateGunImage(phase);
  this.updateGunLabel(phase);
  this.updateStatusText(phase, payload);
  this.updateQR(phase, payload);
  this.updateCardsPlace(phase, payload);
  this.updateMetrics(phase, payload);
  this.updateExtendedMetrics(phase, payload);
  this.updateErrorMessage(phase, payload);
  this.updateFinishedMetrics(phase, payload);
  this.updateScanOverlay(phase, payload);
}
```

So `NozzleUI` takes a purely **data‑driven approach**: the backend sends a payload; UI decides what to show.

### 5.3. Panel switching: `updatePanels(phase)`

This is the core of visual behavior:

1. Fetch DOM elements for this gun, e.g.:

   ```js
   const gunPanel      = el(`panel-gun-${this.gun}`);
   const chargePanel   = el(`panel-charge-${this.gun}`);
   const loadingPanel  = el(`panel-loading-${this.gun}`);
   const finishedPanel = el(`panel-finished-${this.gun}`);
   const errorPanel    = el(`panel-error-${this.gun}`);
   ```

2. Remove `is-active` from all.

3. Decide which one to show:

   - **Charging‑like**:
     - `STATES.CHARGING`, `STATES.SCAN_WHILE_CHARGING`, etc.
     - → `chargePanel` gets `is-active`.
     - Stop any finished animation.

   - **Error‑like**:
     - `phase.startsWith("ERROR_")` OR `phase === STATES.ERROR` OR `phase === STATES.FAULTED`
     - → `errorPanel` gets `is-active`.
     - Stop finished animation.

   - **Waiting**:
     - `phase === STATES.WAITING`
     - → `loadingPanel` gets `is-active`.
     - Stop finished animation.

   - **Finished**:
     - `phase === STATES.FINISHED`
     - → `finishedPanel` gets `is-active`.
     - `startFinishedAnimation()`.

   - **Default (idle)**:
     - → `gunPanel` gets `is-active`.
     - Stop finished animation.

4. Update the battery indicator:

   ```js
   const battery = el(`battery-${this.gun}`);
   const isChargingLike = (phase === STATES.CHARGING || phase === STATES.SCAN_WHILE_CHARGING);

   battery.classList.toggle('is-charging', isChargingLike);
   battery.setAttribute('aria-hidden', isChargingLike ? 'false' : 'true');
   ```

So this method alone drives the **main view transitions** you see on the screen.

### 5.4. Finished animation

`startFinishedAnimation()`:

- Uses `this.finishedFrames` (precomputed list of frame URLs: `finished_bg_001.svg`, `finished_bg_002.svg`, …).
- Likely cycles through them with a timer using CSS background or `<img>` tag updates.

There’s a helper in `kiosk-utils.js` (`generateFinishedFrames(count)`) that does the same logic in a reusable way.

### 5.5. Metrics & text

These methods use `kiosk-utils`:

- `updateMetrics(phase, payload)`:
  - Shows current session metrics:
    - kWh — formatted using `fmt(val, " kWh")`.
    - Time — using `secFullFormat()` or `sec()`.
    - Cost — using `fmtIRR(num)` with Persian digits.
- `updateExtendedMetrics(...)`:
  - Additional details: voltage/current, power, etc.
- `updateFinishedMetrics(...)`:
  - Summary after FINISHED (total energy, time, money).

---

## 6. Utility helpers (`kiosk-utils.js`)

This is a shared library of helpers.

### 6.1. Number and time formatting

- `fmtIRR(num)`:
  ```js
  num.toLocaleString("fa-IR") + " ﷼"
  ```
- `fmtFaNumber(num, decimals = 0)`:
  ```js
  Number(num).toLocaleString("fa-IR", {
    minimumFractionDigits: decimals,
    maximumFractionDigits: decimals
  });
  ```
- `secFullFormat(totalSeconds)`:
  - Converts `seconds → HH:MM:SS` with Persian digits.
- `sec(seconds)`:
  - Shorter `MM:SS` format.

### 6.2. DOM helpers

- `el(id)`:
  ```js
  document.getElementById(id)
  ```
- `setTxt(id, txt)`:
  ```js
  const node = el(id);
  if (node) node.textContent = txt;
  ```
- `setIcon(id, mode)`:
  - Toggles classes like `ic-on`, `ic-off`, `ic-gray`, `ic-na` to show statuses.

### 6.3. Finished frames

- `generateFinishedFrames(count)`:
  - Returns an array of URLs:
    ```js
    [
      "/static/finished_bg_001.svg",
      "/static/finished_bg_002.svg",
      ...
    ]
    ```

---

## 7. Putting it all together: runtime sequence

Let’s trace a concrete scenario:

### Scenario: User charges on nozzle A

1. **Hardware / Orchestrator**:
   - Nozzle A transitions:
     - `IDLE` → `WAITING` (user plugged in, system preparing).
     - → `CHARGING`.
     - → `FINISHED`.

2. **Backend (`gui.py`)**:
   - Receives messages via `MessageBroker`.
   - Updates `state_cache["nozzle_A"]` with each new state + metrics.
   - Emits events via Socket.IO:
     ```python
     socketio.emit("update", {"gun": "A", "state": "WAITING", ...})
     socketio.emit("update", {"gun": "A", "state": "CHARGING", ...})
     socketio.emit("update", {"gun": "A", "state": "FINISHED", ...})
     ```

3. **Browser Socket (`kiosk.js`)**:
   - `sock.on("update", payload => handleNozzleUpdate(gun, payload));`
   - For each payload:
     - `phase = payload.state.toUpperCase();`
     - If `FINISHED`: run finished logic, start hold timer.
     - Otherwise: directly call:
       ```js
       nozzles["A"].update(payload);
       checkSliderContext("A", phase);
       ```

4. **Nozzle UI (`NozzleUI.update`)**:
   - For `WAITING`:
     - `updatePanels("WAITING")` → show loading panel.
   - For `CHARGING`:
     - Show charge panel, metrics, QR, etc.
   - For `FINISHED`:
     - Show finished panel with summary and animation.

5. **Slider**:
   - While in interaction phases (e.g. `WAITING`, `CHARGING_INIT`), `checkSliderContext` switches slider to guide content.
   - When both A & B settle back to `IDLE`, slider switches back to promo content.

---

## 8. How you can explore and modify

If you want to really internalize how this app works, here’s a nice hands‑on path:

1. **Run backend and open the page.**  
   - Start `gui.py` (which starts Flask & Socket.IO).
   - Visit the URL in a browser (often `http://localhost:5000/` or similar).

2. **Enable DEV mode to simulate states from console.**
   - Set `window.DEV_MODE = true` in the code or via query param (if supported).
   - Reload the page.
   - In browser console, see if `window._devUpdateHandler` exists.
   - Try something like:
     ```js
     window._devUpdateHandler({
       gun: 'A',
       state: 'CHARGING',
       energy_kwh: 2.5,
       price_irr: 100000,
       elapsed_sec: 300,
       message: 'در حال شارژ'
     });
     ```
   - Watch `NozzleUI` transition the UI.

3. **Play with slider behavior.**
   - Send states that are in `INTERACTION_PHASES` and some that are not.
   - Observe how slider content changes.

4. **Add a new state (experiment).**
   - Add a new state in `STATES`, e.g. `MAINTENANCE`.
   - Update `NozzleUI.updatePanels` to map it to a dedicated panel or reuse `error`/`loading`.
   - Adjust backend to send `state: "MAINTENANCE"` and see the effect.

---

If you tell me what level you want to go to next, I can:

- Map out **all exact Socket.IO events** and payload shapes from `gui.py` → `kiosk.js`.
- Walk line‑by‑line through `NozzleUI.update()` with an example payload.
- Or help you modify the app (e.g., add a third nozzle, change currency, change states).