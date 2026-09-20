
brew services start mosquitto
mosquitto -v
# Manual Charging Session Scenario (Step-by-Step)

Follow these steps in order to simulate a full charging session on **Gun 1** using your terminal.

### **Step 1: Connect the Vehicle**
Simulate the hardware detecting a plug-in.
```bash
mosquitto_pub -t gun_status/gun_1 -m "CONNECTED"
```
*(The GUI should show the QR Code panel)*
mosquitto_pub -t charge_status/gun_1 -m 4
### **Step 2: Start Authentication**
Simulate pressing the physical button (PIN_SW1) to start the card scan.
```bash
mosquitto_pub -t keys -m "0"
```
*(The GUI should show "Please Scan Your Card")*

### **Step 3: Scan NFC Card & Authorize**
Simulate the card reader finding a card and the server accepting it.
```bash
mosquitto_pub -t id_token/gun_1 -m "A1B2C3D4"
mosquitto_pub -t auth_resp/gun_1 -m "Accepted"
```
*(The GUI should show "Authorized / Starting")*

### **Step 4: Transition to Charging**
Simulate the transition from preparing to active charging.
```bash
mosquitto_pub -t start/gun_1 -m "by_nfc"
mosquitto_pub -t gun_status/gun_1 -m "CHECKING"
# Wait 2 seconds...
mosquitto_pub -t gun_status/gun_1 -m "CHARGING"
```
*(The GUI should enter the Charging Panel)*

### **Step 5: Push Real-time Metrics**
Update the screen with charging data.
```bash
mosquitto_pub -t voltage/gun_1 -m "380"
mosquitto_pub -t current/gun_1 -m "32"
mosquitto_pub -t used_energy/gun_1 -m "1.5"
mosquitto_pub -t charge_percentage/gun_1 -m "45"
```

### **Step 6: Navigate Extended Metrics**
```bash
mosquitto_pub -t keys -m "0"
```

### **Step 7: Finish Charging**
Simulate the vehicle disconnecting or the session ending.
```bash
mosquitto_pub -t stop/gun_1 -m "by_ev"
mosquitto_pub -t gun_status/gun_1 -m "FINISH"
# Wait a few seconds to see the summary, then:
mosquitto_pub -t gun_status/gun_1 -m "NOT_CONNECTED"
```

### **Gun 2 (Nozzle B) Controls**
Simply change `gun_1` to `gun_2` in the topics above.

## 3. Standalone GUI Test Panel
There is a "Standalone" version of the GUI that includes a built-in web-based admin panel. This is often easier than typing terminal commands.

### How to run:
1. Run the standalone test:
   ```bash
   python3 opt/evck/gui_app/guitest_decentralized_standalone.py
   ```
2. Open your browser:
   - **GUI:** `http://localhost:8082`
   - **Test Panel:** `http://localhost:8082/test-admin`
3. Use the buttons on the Test Panel to change states and metrics instantly.

## 4. Hardware/LPC Unit Tests
To run the logic tests for the LPC interface (without hardware):
```bash
pytest opt/evck/tests/test_lpc_decentralized.py
```

## User Review Required
> [!IMPORTANT]
> - Ensure your MQTT broker (Mosquitto) is running on the device.
> - If you are testing from a different machine, replace `localhost` with the device's IP in the commands.



Here is a comprehensive list of manual test commands you can use with `mosquitto_pub` to verify the new UI status icons and configuration updates.

### 1. Hardware Status Icons
These commands toggle the colors of the four icons in the header. Use `on` for green, `off` for red, and `gray` or `na` for the inactive states.

```bash
# LAN Status Icon
mosquitto_pub -t "lan_status" -m "on"
mosquitto_pub -t "lan_status" -m "off"

# Internet Status Icon
mosquitto_pub -t "internet_status" -m "on"
mosquitto_pub -t "internet_status" -m "off"

# LPC (Hardware Controller) Status Icon
mosquitto_pub -t "lpc_status" -m "on"
mosquitto_pub -t "lpc_status" -m "off"

# Server (chargeapp.app) Status Icon
mosquitto_pub -t "server_status" -m "on"
mosquitto_pub -t "server_status" -m "off"
```

### 2. Decentralized Config Topics (Simple & Flat)
These update specific text fields in the UI without needing a full JSON blob.

```bash
# Update Serial Number (Header)
mosquitto_pub -t "config/serial" -m "EVCK-DECENT-2024"

# Update Location (Header)
mosquitto_pub -t "config/location" -m "Tehran, Azadi Squre"

# Update Version (Footer)
mosquitto_pub -t "config/version" -m "v1.6.0-stable"
```

### 3. Monolithic Config (Bulk Update)
If you want to test the full configuration update that happens at power-up:

```bash
# Full Config Payload
mosquitto_pub -t "config" -m '{
  "name": "config",
  "payload": {
    "device": {
      "company": "Tesla Power",
      "serial": "SN-999",
      "location": "Main Lab"
    },
    "pricing": {
      "per_kwh": {"A": 5500, "B": 6000}
    }
  }
}'
```

---

### Pro Tip: Monitoring the Traffic
To see what the hardware is actually publishing in real-time, run this in a second terminal:

```bash
mosquitto_sub -t "#" -v
```

This will show you every message passing through the broker so you can verify that `opi_hardware.py` and `lpc_interface.py` are sending the correct values automatically.