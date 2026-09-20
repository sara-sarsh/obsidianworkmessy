How the gui is working?

Browser loads kiosk.js
        │
        ▼
Imports loaded
        │
        ▼
DEV_MODE set
        │
        ▼
Socket created
        │
        ▼
NozzleUI instances created
        │
        ▼
Socket listeners registered
        │
        ▼
Console: "[Kiosk] Module loaded successfully"
        │
        ▼
HTML finishes loading
        │
        ▼
DOMContentLoaded event fires
        │
        ▼
handleNozzleUpdate(A, IDLE)
handleNozzleUpdate(B, IDLE)
        │
        ▼
initAdSlider()
        │
        ▼
initAdminOverlay()
        │
        ▼
UI fully running
