(.venv) sara@MacBook-Pro evck160c- decentralized % "/Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evck160c- decentralized/.venv/bin/python" "/Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evck160c- decentralized/opt/evck/gui_app/gui_decentralized.py"
LOG_DIR: /opt/evck/logs
EVCK_LOG_DIR env: None
18:54:11 DEBUG | evck.common.env:239 | Version from git: v0.9.0-dev-93-g985c52a
18:54:11 DEBUG | evck.common.env:239 | Version from git: v0.9.0-dev-93-g985c52a
18:54:11 DEBUG | evck.common.env:145 | Using device.json path: /Users/sara/Documents/Personal/Work/Tesla/github-evckcli/etc/evck/common/device.json
18:54:11 WARNING | evck.common.env:148 | device.json NOT FOUND at /Users/sara/Documents/Personal/Work/Tesla/github-evckcli/etc/evck/common/device.json
18:54:11 DEBUG | evck.common.env:195 | Using DEFAULT device config
18:54:11 INFO | evck.gui_decentralized:1206 | [STATIC FOLDER] → /Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evck160c- decentralized/opt/evck/gui_app/static
18:54:11 INFO | evck.messaging:52 | MQTT connected successfully.
18:54:11 INFO | evck.messaging:42 | Connected to MQTT Broker at 127.0.0.1:1883
18:54:11 INFO | evck.gui_decentralized:1247 | ══════════════════════════════════════════════
18:54:11 INFO | evck.gui_decentralized:1248 |   GUI Decentralized — subscribed to 46 topics
18:54:11 INFO | evck.gui_decentralized:1250 | ══════════════════════════════════════════════
 * Tip: There are .env or .flaskenv files present. Do "pip install python-dotenv" to use them.
 * Serving Flask app 'gui_decentralized'
 * Debug mode: off
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: keys (mid=1, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: config/version (mid=2, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: config/serial (mid=3, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: config/location (mid=4, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: internet_status (mid=5, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: lan_status (mid=6, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: lpc_status (mid=7, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: server_status (mid=8, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: id_token/gun_1 (mid=9, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: gun_status/gun_1 (mid=10, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: auth_resp/gun_1 (mid=11, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: start/gun_1 (mid=12, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: stoped/gun_1 (mid=13, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: stopreq/gun_1 (mid=14, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: error/gun_1 (mid=15, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: power_kwh/gun_1 (mid=16, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: voltage/gun_1 (mid=17, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: current/gun_1 (mid=18, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: charge_percentage/gun_1 (mid=19, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: price_per_kwh/gun_1 (mid=20, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: battery_voltage/gun_1 (mid=21, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: demand_current/gun_1 (mid=22, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: charge_time/gun_1 (mid=23, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: used_energy/gun_1 (mid=24, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: charge_mode/gun_1 (mid=25, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: charge_limiter/gun_1 (mid=26, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: insulator_resistor/gun_1 (mid=27, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: pole_temp_positive/gun_1 (mid=28, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: pole_temp_negetive/gun_1 (mid=29, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: each_cell_voltage/gun_1 (mid=30, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: dual_power/gun_1 (mid=31, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: id_token/gun_2 (mid=32, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: gun_status/gun_2 (mid=33, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: auth_resp/gun_2 (mid=34, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: start/gun_2 (mid=35, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: stoped/gun_2 (mid=36, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: stopreq/gun_2 (mid=37, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: error/gun_2 (mid=38, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: power_kwh/gun_2 (mid=39, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: voltage/gun_2 (mid=40, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: current/gun_2 (mid=41, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: charge_percentage/gun_2 (mid=42, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: price_per_kwh/gun_2 (mid=43, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: battery_voltage/gun_2 (mid=44, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: demand_current/gun_2 (mid=45, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: charge_time/gun_2 (mid=46, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: used_energy/gun_2 (mid=47, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: charge_mode/gun_2 (mid=48, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: charge_limiter/gun_2 (mid=49, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: insulator_resistor/gun_2 (mid=50, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: pole_temp_positive/gun_2 (mid=51, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: pole_temp_negetive/gun_2 (mid=52, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: each_cell_voltage/gun_2 (mid=53, qos=(0,))
18:54:11 INFO | evck.messaging:94 | [SUB ACK] Successfully subscribed to topic: dual_power/gun_2 (mid=54, qos=(0,))