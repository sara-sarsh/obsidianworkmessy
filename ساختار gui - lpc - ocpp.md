# GUI
[[How GUI is working| رابط گرافیکی کاربر چطوری کار میکند؟]]
[[How CSMS server client is working | ارتباط با سرور چطور کار می‌کند؟]]
## Component architecture
```mermaid
graph TD
    subgraph Cloud
        CSMS((Central Server<br>CSMS))
    end

    subgraph "Protocol Domain"
        OCPP[ocpp_ws_client.py<br>OCPP Logic & WebSockets]
        Bridge[ocpp_bridge.py<br>Runner & Translator]
        OCPP <--> Bridge
    end

    subgraph "Core App (main_app.py)"
        Orch{Orchestrator<br>State Machine & Timers}
        Bus[(Message Broker<br>Redis Pub/Sub)]
        
        Orch <--"Monitors & Commands"--> Bus
    end

    subgraph "Hardware & UI Domain"
        LPC[lpc_interface.py<br>Relays & Meters]
        NFC[nfc_reader.py<br>Card Scanner]
        GUI[UI / Display<br>QR Codes & Status]
    end

    subgraph Physical World
        EV_A((EV Gun A))
        EV_B((EV Gun B))
        Card((User RFID Card))
    end

    %% Network & Core Connections
    CSMS <--"OCPP Comm"--> OCPP
    Bridge <--"Pub/Sub (ocpp channel)"--> Bus
    LPC <--"Pub/Sub (lpc channel)"--> Bus
    GUI <--"Pub/Sub (gui channel)"--> Bus
    
    %% Direct Hardware Connections
    NFC --"Card Tapped"--> Orch
    LPC <--"Power / Sensors"--> EV_A
    LPC <--"Power / Sensors"--> EV_B
    Card --"Taps"--> NFC


```

## Charging Session State Diagram
```mermaid
stateDiagram-v2
    [*] --> Booting : App Starts (ocpp_bridge)
    
    Booting --> Available : BootNotification Accepted
    note right of Booting: OcppCharger sends<br>BootNotification to CSMS
    
    Available --> Preparing : EV Plugs In
    note right of Available: LPCInterface detects<br>cable connection
    
    Preparing --> Authorizing : RFID Swiped / App Trigger
    
    Authorizing --> Charging : Auth Accepted (StartTransaction)
    note right of Charging: Bridge tells LPCInterface<br>to close relays (power on).<br>OcppCharger sends MeterValues.
    
    Charging --> Finishing : EV Full / User Stops (StopTransaction)
    note left of Finishing: Bridge tells LPCInterface<br>to open relays (power off).
    
    Finishing --> Available : Cable Unplugged

```
# LPC
# OCPP
