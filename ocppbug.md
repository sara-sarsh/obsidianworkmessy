## sent topics
Microsoft Windows [Version 10.0.19045.6456]
(c) Microsoft Corporation. All rights reserved.

C:\Users\Danesh># ترمینال ۳: شبیه‌سازی اتصال فیزیکی کابل به ماشین
'#' is not recognized as an internal or external command,
operable program or batch file.

C:\Users\Danesh>mosquitto_pub -h localhost -t "charge_status/gun_1" -m "CONNECTED"

C:\Users\Danesh># ترمینال ۳: راننده کارت RFID خود را جلوی کارت‌خوان می‌گیرد
'#' is not recognized as an internal or external command,
operable program or batch file.

C:\Users\Danesh>mosquitto_pub -h localhost -t "charge_status/gun_1" -m "CONNECTED"

C:\Users\Danesh>
C:\Users\Danesh># ترمینال ۳: شبیه‌سازی اتصال فیزیکی کابل به ماشین
'#' is not recognized as an internal or external command,
operable program or batch file.

C:\Users\Danesh>mosquitto_pub -h localhost -t "charge_status/gun_1" -m "CONNECTED"

C:\Users\Danesh># ترمینال ۳: راننده کارت RFID خود را جلوی کارت‌خوان می‌گیرد
'#' is not recognized as an internal or external command,
operable program or batch file.

C:\Users\Danesh>mosquitto_pub -h localhost -t "auth_req/gun_1" -m '"04A1B2C3D4E5"'

C:\Users\Danesh>mosquitto_pub -h localhost -t "auth_req/gun_1" -m '"04A1B2C3D4E5"'

C:\Users\Danesh>mosquitto_pub -h localhost -t "auth_req/gun_1" -m '"04A1B2C3D4E5"'

C:\Users\Danesh>mosquitto_pub -h localhost -t "auth_req/gun_1" -m '"0112066260752345"'

C:\Users\Danesh>mosquitto_pub -h localhost -t "auth_req/gun_1" -m "0112066260752345"

C:\Users\Danesh>"auth_req/gun_1"

C:\Users\Danesh># تغییر وضعیت به حالت شارژ
'#' is not recognized as an internal or external command,
operable program or batch file.

C:\Users\Danesh>mosquitto_pub -h localhost -t "charge_status/gun_1" -m "CHARGING"

C:\Users\Danesh>
C:\Users\Danesh># ارسال پی‌درپی متریک‌های ولتاژ، جریان و انرژی مصرفی (در واحد کیلووات‌ساعت)
'#' is not recognized as an internal or external command,
operable program or batch file.

C:\Users\Danesh>mosquitto_pub -h localhost -t "voltage/gun_1" -m "229.5"

C:\Users\Danesh>mosquitto_pub -h localhost -t "current/gun_1" -m "32.1"

C:\Users\Danesh>mosquitto_pub -h localhost -t "used_energy/gun_1" -m "2.450"

C:\Users\Danesh># شبیه‌سازی سیگنال اتمام شارژ خودرو توسط lpc_interface
'#' is not recognized as an internal or external command,
operable program or batch file.

C:\Users\Danesh>mosquitto_pub -h localhost -t "stopped/gun_1" -m "1"

C:\Users\Danesh>


## received topics:
Microsoft Windows [Version 10.0.19045.6456]
(c) Microsoft Corporation. All rights reserved.

C:\Users\Danesh>mosquitto_sub -h localhost -t '#' -v
Error: Invalid subscription topic ''#'', are all '+' and '#' wildcards correct?

Use 'mosquitto_sub --help' to see usage.

C:\Users\Danesh>mosquitto_sub -h localhost -t # -v
price/price_per_kwh 0.0
price/price_per_hr 0.0
price/price_per_conn 0.0
available_power_kw/gun_1 80
available_power_kw/gun_2 805555
out_of_service_status True
available_power_kw/gun_1 80
available_power_kw/gun_2 1.2
server_status red
error/gun_1 251
error/gun_2 251
server_status red
error/gun_1 251
error/gun_2 251
transactionStopRequested/gun_1 true
transactionStopRequested/gun_2 true
transactionStopRequested/gun_1 true
transactionStopRequested/gun_2 true
server_status red
error/gun_1 251
error/gun_2 251
server_status red
error/gun_1 251
error/gun_2 251
transactionStopRequested/gun_1 true
transactionStopRequested/gun_2 true
transactionStopRequested/gun_1 true
transactionStopRequested/gun_2 true
server_status red
error/gun_1 251
error/gun_2 251
server_status red
error/gun_1 251
error/gun_2 251
transactionStopRequested/gun_1 true
transactionStopRequested/gun_2 true
transactionStopRequested/gun_1 true
transactionStopRequested/gun_2 true
server_status red
error/gun_1 251
error/gun_2 251
server_status red
error/gun_1 251
error/gun_2 251
transactionStopRequested/gun_1 true
transactionStopRequested/gun_2 true
transactionStopRequested/gun_1 true
transactionStopRequested/gun_2 true
server_status red
error/gun_1 251
error/gun_2 251
server_status red
error/gun_1 251
error/gun_2 251
transactionStopRequested/gun_1 true
transactionStopRequested/gun_2 true
transactionStopRequested/gun_1 true
transactionStopRequested/gun_2 true
charge_status/gun_1 CONNECTED
server_status red
error/gun_1 251
error/gun_2 251
server_status red
error/gun_1 251
error/gun_2 251
transactionStopRequested/gun_1 true
transactionStopRequested/gun_2 true
transactionStopRequested/gun_1 true
transactionStopRequested/gun_2 true
out_of_service_status True
available_power_kw/gun_1 80
available_power_kw/gun_2 1.2
server_status red
error/gun_1 251
error/gun_2 251
server_status red
error/gun_1 251
error/gun_2 251
transactionStopRequested/gun_1 true
transactionStopRequested/gun_2 true
transactionStopRequested/gun_1 true
transactionStopRequested/gun_2 true
server_status red
error/gun_1 251
error/gun_2 251
server_status red
error/gun_1 251
error/gun_2 251
transactionStopRequested/gun_1 true
transactionStopRequested/gun_2 true
transactionStopRequested/gun_1 true
transactionStopRequested/gun_2 true
price/price_per_kwh 0.0
price/price_per_hr 0.0
price/price_per_conn 0.0
available_power_kw/gun_1 80
available_power_kw/gun_2 1.2
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
charge_status/gun_1 CONNECTED
out_of_service_status True
server_status green
server_status red
error/gun_1 251
error/gun_2 251
server_status red
error/gun_1 251
error/gun_2 251
server_status red
transactionStopRequested/gun_1 true
transactionStopRequested/gun_2 true
transactionStopRequested/gun_1 true
transactionStopRequested/gun_2 true
server_status green
out_of_service_status True
charge_status/gun_1 CONNECTED
out_of_service_status True
server_status green
out_of_service_status True
auth_req/gun_1 '04A1B2C3D4E5'
auth_resp/gun_1 Invalid
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
auth_req/gun_1 '04A1B2C3D4E5'
auth_resp/gun_1 Invalid
server_status green
out_of_service_status True
out_of_service_status True
auth_req/gun_1 '04A1B2C3D4E5'
auth_resp/gun_1 Invalid
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
auth_req/gun_1 '0112066260752345'
auth_resp/gun_1 Invalid
server_status green
out_of_service_status True
auth_req/gun_1 0112066260752345
start_req/gun_1 by_nfc
auth_resp/gun_1 Accepted
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
stop_req/gun_1 by_server
out_of_service_status True
total_cost/gun_1 0.0
stop_req/gun_1 Remote
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
charge_status/gun_1 CHARGING
voltage/gun_1 229.5
current/gun_1 32.1
used_energy/gun_1 2.450
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
server_status green
out_of_service_status True
out_of_service_status True
stopped/gun_1 1
server_status green
out_of_service_status True
out_of_service_status True
server_status green
^C
C:\Users\Danesh>^T

## code's log:
16:59:47 INFO     | evck_ocpp:1182 | [HEARTBEAT] Heartbeat sent successfully.
16:59:47 DEBUG    | evck.messaging:94 | [TX ACK] message id=202 delivered to topic: server_status
16:59:47 DEBUG    | evck.messaging:94 | [TX ACK] message id=203 delivered to topic: out_of_service_status
16:59:47 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
16:59:47 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
16:59:47 DEBUG    | evck_ocpp:1189 | [HEARTBEAT] Sleeping for 15s...
16:59:51 DEBUG    | evck.messaging:94 | [TX ACK] message id=204 delivered to topic: out_of_service_status
16:59:51 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
16:59:51 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
16:59:51 DEBUG    | evck_ocpp:1165 | [REG-CHECK] Device still registered.
17:00:02 INFO     | evck_ocpp:1179 | [HEARTBEAT] Sending Heartbeat...
17:00:02 DEBUG    | evck_ocpp:398 | Calling the call and wait for Heartbeat
17:00:02 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Heartbeat | UID: 6295b8e87b5843bb87c6005dd8f245c5 | Payload: {}
17:00:02 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Heartbeat | UID: 6295b8e87b5843bb87c6005dd8f245c5 | Response: {'currentTime': '2026-09-19T13:30:02.3963692+00:00'}
17:00:02 INFO     | evck_ocpp:1182 | [HEARTBEAT] Heartbeat sent successfully.
17:00:02 DEBUG    | evck.messaging:94 | [TX ACK] message id=205 delivered to topic: server_status
17:00:02 DEBUG    | evck.messaging:94 | [TX ACK] message id=206 delivered to topic: out_of_service_status
17:00:02 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:00:02 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:00:02 DEBUG    | evck_ocpp:1189 | [HEARTBEAT] Sleeping for 15s...
17:00:07 DEBUG    | evck.messaging:94 | [TX ACK] message id=207 delivered to topic: out_of_service_status
17:00:07 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:00:07 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:00:07 DEBUG    | evck_ocpp:1165 | [REG-CHECK] Device still registered.
17:00:17 INFO     | evck_ocpp:1179 | [HEARTBEAT] Sending Heartbeat...
17:00:17 DEBUG    | evck_ocpp:398 | Calling the call and wait for Heartbeat
17:00:17 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Heartbeat | UID: 015d80771a1b4dabbf18861a9a7ffe0b | Payload: {}
17:00:17 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Heartbeat | UID: 015d80771a1b4dabbf18861a9a7ffe0b | Response: {'currentTime': '2026-09-19T13:30:17.8964575+00:00'}
17:00:17 INFO     | evck_ocpp:1182 | [HEARTBEAT] Heartbeat sent successfully.
17:00:17 DEBUG    | evck.messaging:94 | [TX ACK] message id=208 delivered to topic: server_status
17:00:18 DEBUG    | evck.messaging:94 | [TX ACK] message id=209 delivered to topic: out_of_service_status
17:00:18 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:00:18 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:00:18 DEBUG    | evck_ocpp:1189 | [HEARTBEAT] Sleeping for 15s...
17:00:22 DEBUG    | evck.messaging:94 | [TX ACK] message id=210 delivered to topic: out_of_service_status
17:00:22 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:00:22 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:00:22 DEBUG    | evck_ocpp:1165 | [REG-CHECK] Device still registered.
17:00:33 INFO     | evck_ocpp:1179 | [HEARTBEAT] Sending Heartbeat...
17:00:33 DEBUG    | evck_ocpp:398 | Calling the call and wait for Heartbeat
17:00:33 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Heartbeat | UID: 58bbcf0efdc44a12a7e2ac096ef5e493 | Payload: {}
17:00:33 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Heartbeat | UID: 58bbcf0efdc44a12a7e2ac096ef5e493 | Response: {'currentTime': '2026-09-19T13:30:33.3666169+00:00'}
17:00:33 INFO     | evck_ocpp:1182 | [HEARTBEAT] Heartbeat sent successfully.
17:00:33 DEBUG    | evck.messaging:94 | [TX ACK] message id=211 delivered to topic: server_status
17:00:33 DEBUG    | evck.messaging:94 | [TX ACK] message id=212 delivered to topic: out_of_service_status
17:00:33 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:00:33 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:00:33 DEBUG    | evck_ocpp:1189 | [HEARTBEAT] Sleeping for 15s...
17:00:38 DEBUG    | evck.messaging:94 | [TX ACK] message id=213 delivered to topic: out_of_service_status
17:00:38 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:00:38 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:00:38 DEBUG    | evck_ocpp:1165 | [REG-CHECK] Device still registered.
17:00:48 INFO     | evck_ocpp:1179 | [HEARTBEAT] Sending Heartbeat...
17:00:48 DEBUG    | evck_ocpp:398 | Calling the call and wait for Heartbeat
17:00:48 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Heartbeat | UID: 8d9213e11d1143f1adb58a8bd1ecf5bf | Payload: {}
17:00:48 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Heartbeat | UID: 8d9213e11d1143f1adb58a8bd1ecf5bf | Response: {'currentTime': '2026-09-19T13:30:48.8063002+00:00'}
17:00:48 INFO     | evck_ocpp:1182 | [HEARTBEAT] Heartbeat sent successfully.
17:00:48 DEBUG    | evck.messaging:94 | [TX ACK] message id=214 delivered to topic: server_status
17:00:49 DEBUG    | evck.messaging:94 | [TX ACK] message id=215 delivered to topic: out_of_service_status
17:00:49 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:00:49 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:00:49 DEBUG    | evck_ocpp:1189 | [HEARTBEAT] Sleeping for 15s...
17:00:53 DEBUG    | evck.messaging:94 | [TX ACK] message id=216 delivered to topic: out_of_service_status
17:00:53 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:00:53 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:00:53 DEBUG    | evck_ocpp:1165 | [REG-CHECK] Device still registered.
17:01:04 INFO     | evck_ocpp:1179 | [HEARTBEAT] Sending Heartbeat...
17:01:04 DEBUG    | evck_ocpp:398 | Calling the call and wait for Heartbeat
17:01:04 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Heartbeat | UID: 66da50cd7e1a41b7a76bb23edd5413bf | Payload: {}
17:01:04 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Heartbeat | UID: 66da50cd7e1a41b7a76bb23edd5413bf | Response: {'currentTime': '2026-09-19T13:31:04.3662442+00:00'}
17:01:04 INFO     | evck_ocpp:1182 | [HEARTBEAT] Heartbeat sent successfully.
17:01:04 DEBUG    | evck.messaging:94 | [TX ACK] message id=217 delivered to topic: server_status
17:01:04 DEBUG    | evck.messaging:94 | [TX ACK] message id=218 delivered to topic: out_of_service_status
17:01:05 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:01:05 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:01:05 DEBUG    | evck_ocpp:1189 | [HEARTBEAT] Sleeping for 15s...
17:01:08 DEBUG    | evck.messaging:94 | [TX ACK] message id=219 delivered to topic: out_of_service_status
17:01:09 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:01:09 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:01:09 DEBUG    | evck_ocpp:1165 | [REG-CHECK] Device still registered.
17:01:20 INFO     | evck_ocpp:1179 | [HEARTBEAT] Sending Heartbeat...
17:01:20 DEBUG    | evck_ocpp:398 | Calling the call and wait for Heartbeat
17:01:20 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Heartbeat | UID: c4e7ae4273714676bc01a31ea6819486 | Payload: {}
17:01:20 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Heartbeat | UID: c4e7ae4273714676bc01a31ea6819486 | Response: {'currentTime': '2026-09-19T13:31:20.0615183+00:00'}
17:01:20 INFO     | evck_ocpp:1182 | [HEARTBEAT] Heartbeat sent successfully.
17:01:20 DEBUG    | evck.messaging:94 | [TX ACK] message id=220 delivered to topic: server_status
17:01:20 DEBUG    | evck.messaging:94 | [TX ACK] message id=221 delivered to topic: out_of_service_status
17:01:20 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:01:20 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:01:20 DEBUG    | evck_ocpp:1189 | [HEARTBEAT] Sleeping for 15s...
17:01:25 DEBUG    | evck.messaging:94 | [TX ACK] message id=222 delivered to topic: out_of_service_status
17:01:25 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:01:25 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:01:25 DEBUG    | evck_ocpp:1165 | [REG-CHECK] Device still registered.
17:01:35 INFO     | evck_ocpp:1179 | [HEARTBEAT] Sending Heartbeat...
17:01:35 DEBUG    | evck_ocpp:398 | Calling the call and wait for Heartbeat
17:01:35 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Heartbeat | UID: 5ed363bfd9d44924b66ffb93a6a01f42 | Payload: {}
17:01:35 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Heartbeat | UID: 5ed363bfd9d44924b66ffb93a6a01f42 | Response: {'currentTime': '2026-09-19T13:31:35.5620583+00:00'}
17:01:35 INFO     | evck_ocpp:1182 | [HEARTBEAT] Heartbeat sent successfully.
17:01:35 DEBUG    | evck.messaging:94 | [TX ACK] message id=223 delivered to topic: server_status
17:01:35 DEBUG    | evck.messaging:94 | [TX ACK] message id=224 delivered to topic: out_of_service_status
17:01:36 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:01:36 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:01:36 DEBUG    | evck_ocpp:1189 | [HEARTBEAT] Sleeping for 15s...
17:01:40 DEBUG    | evck.messaging:94 | [TX ACK] message id=225 delivered to topic: out_of_service_status
17:01:41 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:01:41 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:01:41 DEBUG    | evck_ocpp:1165 | [REG-CHECK] Device still registered.
17:01:51 INFO     | evck_ocpp:1179 | [HEARTBEAT] Sending Heartbeat...
17:01:51 DEBUG    | evck_ocpp:398 | Calling the call and wait for Heartbeat
17:01:51 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Heartbeat | UID: afd6058137164a39802953a01b63be4d | Payload: {}
17:01:51 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Heartbeat | UID: afd6058137164a39802953a01b63be4d | Response: {'currentTime': '2026-09-19T13:31:51.0969848+00:00'}
17:01:51 INFO     | evck_ocpp:1182 | [HEARTBEAT] Heartbeat sent successfully.
17:01:51 DEBUG    | evck.messaging:94 | [TX ACK] message id=226 delivered to topic: server_status
17:01:51 DEBUG    | evck.messaging:94 | [TX ACK] message id=227 delivered to topic: out_of_service_status
17:01:51 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:01:51 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:01:51 DEBUG    | evck_ocpp:1189 | [HEARTBEAT] Sleeping for 15s...
17:01:56 DEBUG    | evck.messaging:94 | [TX ACK] message id=228 delivered to topic: out_of_service_status
17:01:56 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:01:56 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:01:56 DEBUG    | evck_ocpp:1165 | [REG-CHECK] Device still registered.
17:01:57 DEBUG    | evck.messaging:82 | Received message on auth_req/gun_1
17:01:57 INFO     | evck_ocpp:266 | [NFC] Received auth_req for gun_1 with tag: '0112066260752345'
17:01:57 INFO     | evck_ocpp:526 | [ACTION] Processing: authorize | Data: {'connector_id': 1, 'id_tag': "'0112066260752345'"} | Reason: auth_start
17:01:57 DEBUG    | evck_ocpp:398 | Calling the call and wait for Authorize
17:01:57 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Authorize | UID: 453982a9be744beca9f4a3e8d0829201 | Payload: {'idTag': "'0112066260752345'"}
17:01:57 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Authorize | UID: 453982a9be744beca9f4a3e8d0829201 | Response: {'idTagInfo': {'status': 'Invalid'}}
17:01:57 WARNING  | evck_ocpp:559 | [AUTH] Rejected for tag '0112066260752345'
17:01:57 DEBUG    | evck.messaging:94 | [TX ACK] message id=229 delivered to topic: auth_resp/gun_1
17:02:06 INFO     | evck_ocpp:1179 | [HEARTBEAT] Sending Heartbeat...
17:02:06 DEBUG    | evck_ocpp:398 | Calling the call and wait for Heartbeat
17:02:06 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Heartbeat | UID: 1e5fd8a7ce084bfb92017cf57761f52a | Payload: {}
17:02:06 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Heartbeat | UID: 1e5fd8a7ce084bfb92017cf57761f52a | Response: {'currentTime': '2026-09-19T13:32:06.7765623+00:00'}
17:02:06 INFO     | evck_ocpp:1182 | [HEARTBEAT] Heartbeat sent successfully.
17:02:06 DEBUG    | evck.messaging:94 | [TX ACK] message id=230 delivered to topic: server_status
17:02:07 DEBUG    | evck.messaging:94 | [TX ACK] message id=231 delivered to topic: out_of_service_status
17:02:07 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:02:07 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:02:07 DEBUG    | evck_ocpp:1189 | [HEARTBEAT] Sleeping for 15s...
17:02:10 DEBUG    | evck.messaging:82 | Received message on auth_req/gun_1
17:02:10 INFO     | evck_ocpp:266 | [NFC] Received auth_req for gun_1 with tag: 0112066260752345
17:02:10 INFO     | evck_ocpp:526 | [ACTION] Processing: authorize | Data: {'connector_id': 1, 'id_tag': '0112066260752345'} | Reason: auth_start
17:02:10 DEBUG    | evck_ocpp:398 | Calling the call and wait for Authorize
17:02:10 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Authorize | UID: 11763151a5f64b6080d1d1d47f6118fd | Payload: {'idTag': '0112066260752345'}
17:02:10 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Authorize | UID: 11763151a5f64b6080d1d1d47f6118fd | Response: {'idTagInfo': {'status': 'Accepted'}}
17:02:10 INFO     | evck_ocpp:551 | [AUTH] Accepted for tag 0112066260752345 on gun_1
17:02:10 INFO     | evck_ocpp:574 | [START] Attempting start_transaction | Connector: 1 | ID_Tag: 0112066260752345 | Reason: by_nfc
17:02:10 INFO     | evck_ocpp:123 | energy file path : C:\Users\Danesh\Documents\Sara\Tesla\github-evckcli\evck160c- decentralized\etc\evck\total_energy.json
17:02:10 DEBUG    | evck_ocpp:595 | [START] Sending StartTransaction CALL to CSMS | Payload: {'connectorId': 1, 'idTag': '0112066260752345', 'meterStart': 0.0, 'timestamp': '2026-09-19T13:32:10.633Z', 'reservationId': 0, 'reason': 'by_nfc'}
17:02:10 DEBUG    | evck_ocpp:398 | Calling the call and wait for StartTransaction
17:02:10 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL StartTransaction | UID: 41f0ddd4e2a6413e83473c5690b92934 | Payload: {'connectorId': 1, 'idTag': '0112066260752345', 'meterStart': 0.0, 'timestamp': '2026-09-19T13:32:10.633Z', 'reservationId': 0, 'reason': 'by_nfc'}
17:02:10 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for StartTransaction | UID: 41f0ddd4e2a6413e83473c5690b92934 | Response: {'transactionId': 397703210, 'idTagInfo': {'status': 'Accepted'}}
17:02:10 INFO     | evck_ocpp:601 | [START] CSMS Response received: {'transactionId': 397703210, 'idTagInfo': {'status': 'Accepted'}}
17:02:10 INFO     | evck_ocpp:609 | [START] Notifying CSMS of status: Connected
17:02:10 INFO     | evck_ocpp:617 | [START] create charging task: Transaction 397703210 successfully initialized on gun_1
17:02:10 DEBUG    | evck.messaging:94 | [TX ACK] message id=232 delivered to topic: start_req/gun_1
17:02:10 INFO     | evck_ocpp:686 | [METERING] Started charging loop for gun_1
17:02:10 DEBUG    | evck.messaging:94 | [TX ACK] message id=233 delivered to topic: auth_resp/gun_1
17:02:10 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:02:11 DEBUG    | evck.messaging:94 | [TX ACK] message id=234 delivered to topic: out_of_service_status
17:02:12 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:02:12 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:02:12 DEBUG    | evck_ocpp:1165 | [REG-CHECK] Device still registered.
17:02:12 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:02:14 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:02:16 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:02:18 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:02:20 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:02:22 INFO     | evck_ocpp:1179 | [HEARTBEAT] Sending Heartbeat...
17:02:22 DEBUG    | evck_ocpp:398 | Calling the call and wait for Heartbeat
17:02:22 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Heartbeat | UID: 5f9711447bf64860b893402fab92faa2 | Payload: {}
17:02:22 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Heartbeat | UID: 5f9711447bf64860b893402fab92faa2 | Response: {'currentTime': '2026-09-19T13:32:22.5371010+00:00'}
17:02:22 INFO     | evck_ocpp:1182 | [HEARTBEAT] Heartbeat sent successfully.
17:02:22 DEBUG    | evck.messaging:94 | [TX ACK] message id=235 delivered to topic: server_status
17:02:22 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:02:22 DEBUG    | evck.messaging:94 | [TX ACK] message id=236 delivered to topic: out_of_service_status
17:02:23 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:02:23 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:02:23 DEBUG    | evck_ocpp:1189 | [HEARTBEAT] Sleeping for 15s...
17:02:24 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:02:26 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:02:27 DEBUG    | evck.messaging:94 | [TX ACK] message id=237 delivered to topic: out_of_service_status
17:02:27 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:02:27 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:02:27 DEBUG    | evck_ocpp:1165 | [REG-CHECK] Device still registered.
17:02:28 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:02:30 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:02:32 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:02:34 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:02:36 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:02:38 INFO     | evck_ocpp:1179 | [HEARTBEAT] Sending Heartbeat...
17:02:38 DEBUG    | evck_ocpp:398 | Calling the call and wait for Heartbeat
17:02:38 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Heartbeat | UID: be18af4cdba14d72b1db92633efb7a54 | Payload: {}
17:02:38 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Heartbeat | UID: be18af4cdba14d72b1db92633efb7a54 | Response: {'currentTime': '2026-09-19T13:32:38.0219558+00:00'}
17:02:38 INFO     | evck_ocpp:1182 | [HEARTBEAT] Heartbeat sent successfully.
17:02:38 DEBUG    | evck.messaging:94 | [TX ACK] message id=238 delivered to topic: server_status
17:02:38 DEBUG    | evck.messaging:94 | [TX ACK] message id=239 delivered to topic: out_of_service_status
17:02:38 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:02:38 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:02:38 DEBUG    | evck_ocpp:1189 | [HEARTBEAT] Sleeping for 15s...
17:02:38 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:02:40 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:02:42 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:02:42 DEBUG    | evck.messaging:94 | [TX ACK] message id=240 delivered to topic: out_of_service_status
17:02:43 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:02:43 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:02:43 DEBUG    | evck_ocpp:1165 | [REG-CHECK] Device still registered.
17:02:44 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:02:46 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:02:48 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:02:50 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:02:52 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:02:53 INFO     | evck_ocpp:1179 | [HEARTBEAT] Sending Heartbeat...
17:02:53 DEBUG    | evck_ocpp:398 | Calling the call and wait for Heartbeat
17:02:53 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Heartbeat | UID: 9d2ef9afd13f4039bb73ef7b8003b6bd | Payload: {}
17:02:53 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Heartbeat | UID: 9d2ef9afd13f4039bb73ef7b8003b6bd | Response: {'currentTime': '2026-09-19T13:32:53.5171573+00:00'}
17:02:53 INFO     | evck_ocpp:1182 | [HEARTBEAT] Heartbeat sent successfully.
17:02:53 DEBUG    | evck.messaging:94 | [TX ACK] message id=241 delivered to topic: server_status
17:02:53 DEBUG    | evck.messaging:94 | [TX ACK] message id=242 delivered to topic: out_of_service_status
17:02:53 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:02:53 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:02:53 DEBUG    | evck_ocpp:1189 | [HEARTBEAT] Sleeping for 15s...
17:02:54 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:02:56 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:02:58 DEBUG    | evck.messaging:94 | [TX ACK] message id=243 delivered to topic: out_of_service_status
17:02:58 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:02:58 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:02:58 DEBUG    | evck_ocpp:1165 | [REG-CHECK] Device still registered.
17:02:58 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:03:00 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:03:02 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:03:04 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:03:06 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:03:08 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:03:08 INFO     | evck_ocpp:1179 | [HEARTBEAT] Sending Heartbeat...
17:03:08 DEBUG    | evck_ocpp:398 | Calling the call and wait for Heartbeat
17:03:08 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Heartbeat | UID: 512874b8abe84119ae66ea964368f5c9 | Payload: {}
17:03:09 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Heartbeat | UID: 512874b8abe84119ae66ea964368f5c9 | Response: {'currentTime': '2026-09-19T13:33:08.9512087+00:00'}
17:03:09 INFO     | evck_ocpp:1182 | [HEARTBEAT] Heartbeat sent successfully.
17:03:09 DEBUG    | evck.messaging:94 | [TX ACK] message id=244 delivered to topic: server_status
17:03:09 DEBUG    | evck.messaging:94 | [TX ACK] message id=245 delivered to topic: out_of_service_status
17:03:09 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:03:09 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:03:09 DEBUG    | evck_ocpp:1189 | [HEARTBEAT] Sleeping for 15s...
17:03:10 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:03:12 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:03:13 DEBUG    | evck.messaging:94 | [TX ACK] message id=246 delivered to topic: out_of_service_status
17:03:14 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:03:14 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:03:14 DEBUG    | evck_ocpp:1165 | [REG-CHECK] Device still registered.
17:03:15 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:03:17 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:03:19 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:03:21 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:03:23 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:03:24 INFO     | evck_ocpp:1179 | [HEARTBEAT] Sending Heartbeat...
17:03:24 DEBUG    | evck_ocpp:398 | Calling the call and wait for Heartbeat
17:03:24 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Heartbeat | UID: 50240d7a9b8f485ca93ca9511c294269 | Payload: {}
17:03:24 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Heartbeat | UID: 50240d7a9b8f485ca93ca9511c294269 | Response: {'currentTime': '2026-09-19T13:33:24.4366943+00:00'}
17:03:24 INFO     | evck_ocpp:1182 | [HEARTBEAT] Heartbeat sent successfully.
17:03:24 DEBUG    | evck.messaging:94 | [TX ACK] message id=247 delivered to topic: server_status
17:03:24 DEBUG    | evck.messaging:94 | [TX ACK] message id=248 delivered to topic: out_of_service_status
17:03:24 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:03:24 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:03:24 DEBUG    | evck_ocpp:1189 | [HEARTBEAT] Sleeping for 15s...
17:03:25 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:03:27 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:03:29 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:03:29 DEBUG    | evck.messaging:94 | [TX ACK] message id=249 delivered to topic: out_of_service_status
17:03:29 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:03:29 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:03:29 DEBUG    | evck_ocpp:1165 | [REG-CHECK] Device still registered.
17:03:31 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:03:33 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:03:35 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:03:37 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:03:39 DEBUG    | evck_ocpp:701 | [METERING] Waiting for CHARGING status, current: CONNECTED
17:03:39 INFO     | evck_ocpp:1179 | [HEARTBEAT] Sending Heartbeat...
17:03:39 DEBUG    | evck_ocpp:398 | Calling the call and wait for Heartbeat
17:03:39 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Heartbeat | UID: 1eacf0e16f19424b81a074ee95d17a47 | Payload: {}
17:03:40 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Heartbeat | UID: 1eacf0e16f19424b81a074ee95d17a47 | Response: {'currentTime': '2026-09-19T13:33:39.9120418+00:00'}
17:03:40 INFO     | evck_ocpp:1182 | [HEARTBEAT] Heartbeat sent successfully.
17:03:40 DEBUG    | evck.messaging:94 | [TX ACK] message id=250 delivered to topic: server_status
17:03:40 INFO     | evck_ocpp:444 | [OCPP MSG IN] CALL RemoteStopTransaction | Payload: {'transactionId': 397703210}
17:03:40 INFO     | evck_ocpp:420 | [OCPP MSG OUT] CALLRESULT | UID: c5f9953b-a38f-486e-b704-ff547b4d066a | Result: {'status': 'Accepted'}
17:03:40 INFO     | evck_ocpp:526 | [ACTION] Processing: stop_transaction | Data: {'connector_id': 1, 'reason': 'Remote'} | Reason: by_server
17:03:40 INFO     | evck_ocpp:704 | [METERING] Charging loop for gun_1 was safely cancelled.
17:03:40 DEBUG    | evck.messaging:82 | Received message on stop_req/gun_1
17:03:40 INFO     | evck_ocpp:375 | The Stop is already handled.17:03:40 DEBUG    | evck.messaging:94 | [TX ACK] message id=251 delivered to topic: stop_req/gun_1
17:03:40 INFO     | evck_ocpp:179 | Total energy saved for gun_1: 0Wh
17:03:40 DEBUG    | evck.messaging:94 | [TX ACK] message id=252 delivered to topic: out_of_service_status
17:03:40 DEBUG    | evck_ocpp:398 | Calling the call and wait for StopTransaction
17:03:40 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL StopTransaction | UID: b38aed7864164dceab5d154a49b5c4ca | Payload: {'idTag': '0112066260752345', 'transactionId': 397703210, 'connectorId': 1, 'meterStop': 0, 'timestamp': '2026-09-19T13:33:40.225Z', 'reason': 'Remote'}
17:03:40 DEBUG    | evck.messaging:94 | [TX ACK] message id=253 delivered to topic: total_cost/gun_1
17:03:40 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:03:40 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:03:40 DEBUG    | evck_ocpp:1189 | [HEARTBEAT] Sleeping for 15s...
17:03:40 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for StopTransaction | UID: b38aed7864164dceab5d154a49b5c4ca | Response: {}
17:03:40 DEBUG    | evck.messaging:82 | Received message on stop_req/gun_1
17:03:40 INFO     | evck_ocpp:378 | [STOP REQ] Hardware requested stop for gun_1 with reason: Remote
17:03:40 DEBUG    | evck.messaging:94 | [TX ACK] message id=254 delivered to topic: stop_req/gun_1
17:03:44 DEBUG    | evck.messaging:94 | [TX ACK] message id=255 delivered to topic: out_of_service_status
17:03:44 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:03:44 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:03:44 DEBUG    | evck_ocpp:1165 | [REG-CHECK] Device still registered.
17:03:55 INFO     | evck_ocpp:1179 | [HEARTBEAT] Sending Heartbeat...
17:03:55 DEBUG    | evck_ocpp:398 | Calling the call and wait for Heartbeat
17:03:55 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Heartbeat | UID: c5466f6a3ab348498069b51ead2478db | Payload: {}
17:03:55 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Heartbeat | UID: c5466f6a3ab348498069b51ead2478db | Response: {'currentTime': '2026-09-19T13:33:55.4470140+00:00'}
17:03:55 INFO     | evck_ocpp:1182 | [HEARTBEAT] Heartbeat sent successfully.
17:03:55 DEBUG    | evck.messaging:94 | [TX ACK] message id=256 delivered to topic: server_status
17:03:56 DEBUG    | evck.messaging:94 | [TX ACK] message id=257 delivered to topic: out_of_service_status
17:03:56 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:03:56 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:03:56 DEBUG    | evck_ocpp:1189 | [HEARTBEAT] Sleeping for 15s...
17:04:00 DEBUG    | evck.messaging:94 | [TX ACK] message id=258 delivered to topic: out_of_service_status
17:04:00 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:04:00 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:04:00 DEBUG    | evck_ocpp:1165 | [REG-CHECK] Device still registered.
17:04:11 INFO     | evck_ocpp:1179 | [HEARTBEAT] Sending Heartbeat...
17:04:11 DEBUG    | evck_ocpp:398 | Calling the call and wait for Heartbeat
17:04:11 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Heartbeat | UID: d22811785bd046ca94f61b60cd52f342 | Payload: {}
17:04:11 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Heartbeat | UID: d22811785bd046ca94f61b60cd52f342 | Response: {'currentTime': '2026-09-19T13:34:11.2572139+00:00'}
17:04:11 INFO     | evck_ocpp:1182 | [HEARTBEAT] Heartbeat sent successfully.
17:04:11 DEBUG    | evck.messaging:94 | [TX ACK] message id=259 delivered to topic: server_status
17:04:11 DEBUG    | evck.messaging:94 | [TX ACK] message id=260 delivered to topic: out_of_service_status
17:04:11 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:04:11 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:04:11 DEBUG    | evck_ocpp:1189 | [HEARTBEAT] Sleeping for 15s...
17:04:16 DEBUG    | evck.messaging:94 | [TX ACK] message id=261 delivered to topic: out_of_service_status
17:04:16 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:04:16 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:04:16 DEBUG    | evck_ocpp:1165 | [REG-CHECK] Device still registered.
17:04:26 INFO     | evck_ocpp:1179 | [HEARTBEAT] Sending Heartbeat...
17:04:26 DEBUG    | evck_ocpp:398 | Calling the call and wait for Heartbeat
17:04:26 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Heartbeat | UID: 0ff322c73d7c4ad38494857ea31d3367 | Payload: {}
17:04:26 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Heartbeat | UID: 0ff322c73d7c4ad38494857ea31d3367 | Response: {'currentTime': '2026-09-19T13:34:26.7264219+00:00'}
17:04:26 INFO     | evck_ocpp:1182 | [HEARTBEAT] Heartbeat sent successfully.
17:04:26 DEBUG    | evck.messaging:94 | [TX ACK] message id=262 delivered to topic: server_status
17:04:26 DEBUG    | evck.messaging:94 | [TX ACK] message id=263 delivered to topic: out_of_service_status
17:04:27 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:04:27 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:04:27 DEBUG    | evck_ocpp:1189 | [HEARTBEAT] Sleeping for 15s...
17:04:30 DEBUG    | evck.messaging:82 | Received message on charge_status/gun_1
17:04:30 DEBUG    | evck_ocpp:287 | [HW] gun_1 status changed to CHARGING
17:04:30 DEBUG    | evck_ocpp:398 | Calling the call and wait for StatusNotification
17:04:30 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL StatusNotification | UID: 936b27074c0540ddbcd6115165906d58 | Payload: {'connectorId': 1, 'errorCode': 'NoError', 'status': 'Charging', 'timestamp': '2026-09-19T13:34:30.013Z'}
17:04:30 DEBUG    | evck.messaging:82 | Received message on voltage/gun_1
17:04:30 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for StatusNotification | UID: 936b27074c0540ddbcd6115165906d58 | Response: {}
17:04:30 DEBUG    | evck.messaging:82 | Received message on current/gun_1
17:04:31 DEBUG    | evck.messaging:82 | Received message on used_energy/gun_1
17:04:31 DEBUG    | evck.messaging:94 | [TX ACK] message id=264 delivered to topic: out_of_service_status
17:04:31 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:04:31 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:04:31 DEBUG    | evck_ocpp:1165 | [REG-CHECK] Device still registered.
17:04:42 INFO     | evck_ocpp:1179 | [HEARTBEAT] Sending Heartbeat...
17:04:42 DEBUG    | evck_ocpp:398 | Calling the call and wait for Heartbeat
17:04:42 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Heartbeat | UID: 3acc458de3404300ad172515f647e914 | Payload: {}
17:04:42 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Heartbeat | UID: 3acc458de3404300ad172515f647e914 | Response: {'currentTime': '2026-09-19T13:34:42.7970167+00:00'}
17:04:42 INFO     | evck_ocpp:1182 | [HEARTBEAT] Heartbeat sent successfully.
17:04:42 DEBUG    | evck.messaging:94 | [TX ACK] message id=265 delivered to topic: server_status
17:04:43 DEBUG    | evck.messaging:94 | [TX ACK] message id=266 delivered to topic: out_of_service_status
17:04:43 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:04:43 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:04:43 DEBUG    | evck_ocpp:1189 | [HEARTBEAT] Sleeping for 15s...
17:04:47 DEBUG    | evck.messaging:94 | [TX ACK] message id=267 delivered to topic: out_of_service_status
17:04:47 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:04:47 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:04:47 DEBUG    | evck_ocpp:1165 | [REG-CHECK] Device still registered.
17:04:58 INFO     | evck_ocpp:1179 | [HEARTBEAT] Sending Heartbeat...
17:04:58 DEBUG    | evck_ocpp:398 | Calling the call and wait for Heartbeat
17:04:58 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Heartbeat | UID: b8cc8512c7bc455ab27a1580392ea22a | Payload: {}
17:04:58 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Heartbeat | UID: b8cc8512c7bc455ab27a1580392ea22a | Response: {'currentTime': '2026-09-19T13:34:58.3312065+00:00'}
17:04:58 INFO     | evck_ocpp:1182 | [HEARTBEAT] Heartbeat sent successfully.
17:04:58 DEBUG    | evck.messaging:94 | [TX ACK] message id=268 delivered to topic: server_status
17:04:58 DEBUG    | evck.messaging:94 | [TX ACK] message id=269 delivered to topic: out_of_service_status
17:04:58 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:04:58 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:04:58 DEBUG    | evck_ocpp:1189 | [HEARTBEAT] Sleeping for 15s...
17:05:02 DEBUG    | evck.messaging:94 | [TX ACK] message id=270 delivered to topic: out_of_service_status
17:05:02 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:05:02 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:05:02 DEBUG    | evck_ocpp:1165 | [REG-CHECK] Device still registered.
17:05:13 INFO     | evck_ocpp:1179 | [HEARTBEAT] Sending Heartbeat...
17:05:13 DEBUG    | evck_ocpp:398 | Calling the call and wait for Heartbeat
17:05:13 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Heartbeat | UID: 99f6e4e4aac846efa10e98e0892e18a9 | Payload: {}
17:05:13 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Heartbeat | UID: 99f6e4e4aac846efa10e98e0892e18a9 | Response: {'currentTime': '2026-09-19T13:35:13.8062483+00:00'}
17:05:13 INFO     | evck_ocpp:1182 | [HEARTBEAT] Heartbeat sent successfully.
17:05:13 DEBUG    | evck.messaging:94 | [TX ACK] message id=271 delivered to topic: server_status
17:05:14 DEBUG    | evck.messaging:94 | [TX ACK] message id=272 delivered to topic: out_of_service_status
17:05:14 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:05:14 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:05:14 DEBUG    | evck_ocpp:1189 | [HEARTBEAT] Sleeping for 15s...
17:05:17 DEBUG    | evck.messaging:94 | [TX ACK] message id=273 delivered to topic: out_of_service_status
17:05:18 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:05:18 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:05:18 DEBUG    | evck_ocpp:1165 | [REG-CHECK] Device still registered.
17:05:29 INFO     | evck_ocpp:1179 | [HEARTBEAT] Sending Heartbeat...
17:05:29 DEBUG    | evck_ocpp:398 | Calling the call and wait for Heartbeat
17:05:29 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Heartbeat | UID: acbd457900ab40f0bba806d4991cd3eb | Payload: {}
17:05:29 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Heartbeat | UID: acbd457900ab40f0bba806d4991cd3eb | Response: {'currentTime': '2026-09-19T13:35:29.3381816+00:00'}
17:05:29 INFO     | evck_ocpp:1182 | [HEARTBEAT] Heartbeat sent successfully.
17:05:29 DEBUG    | evck.messaging:94 | [TX ACK] message id=274 delivered to topic: server_status
17:05:29 DEBUG    | evck.messaging:94 | [TX ACK] message id=275 delivered to topic: out_of_service_status
17:05:29 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:05:29 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:05:29 DEBUG    | evck_ocpp:1189 | [HEARTBEAT] Sleeping for 15s...
17:05:33 DEBUG    | evck.messaging:94 | [TX ACK] message id=276 delivered to topic: out_of_service_status
17:05:33 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:05:33 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:05:33 DEBUG    | evck_ocpp:1165 | [REG-CHECK] Device still registered.
17:05:40 DEBUG    | evck.messaging:82 | Received message on stopped/gun_1
17:05:40 INFO     | evck_ocpp:361 | [LPC] Received hardware stopped trigger for gun_1
17:05:44 INFO     | evck_ocpp:1179 | [HEARTBEAT] Sending Heartbeat...
17:05:44 DEBUG    | evck_ocpp:398 | Calling the call and wait for Heartbeat
17:05:44 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Heartbeat | UID: e2b0789debb24806bba5f6fa82d6de0a | Payload: {}
17:05:44 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Heartbeat | UID: e2b0789debb24806bba5f6fa82d6de0a | Response: {'currentTime': '2026-09-19T13:35:44.8514698+00:00'}
17:05:44 INFO     | evck_ocpp:1182 | [HEARTBEAT] Heartbeat sent successfully.
17:05:44 DEBUG    | evck.messaging:94 | [TX ACK] message id=277 delivered to topic: server_status
17:05:45 DEBUG    | evck.messaging:94 | [TX ACK] message id=278 delivered to topic: out_of_service_status
17:05:45 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:05:45 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:05:45 DEBUG    | evck_ocpp:1189 | [HEARTBEAT] Sleeping for 15s...
17:05:49 DEBUG    | evck.messaging:94 | [TX ACK] message id=279 delivered to topic: out_of_service_status
17:05:49 DEBUG    | evck_ocpp:909 | [STATION INFO] id=CHRG160M040002 raw_data={'name': 'Sara Tesla 5', 'power': 80, 'fixedCost': 0, 'hourlyCost': 0, 'pricePerkWh': 0, 'connectors': [{'id': 1, 'maxPower': 80}, {'id': 2, 'maxPower': 1.2}]}
17:05:49 INFO     | evck_ocpp:1052 | No config changes detected. Skipping update.
17:05:49 DEBUG    | evck_ocpp:1165 | [REG-CHECK] Device still registered.
17:06:00 INFO     | evck_ocpp:1179 | [HEARTBEAT] Sending Heartbeat...
17:06:00 DEBUG    | evck_ocpp:398 | Calling the call and wait for Heartbeat
17:06:00 INFO     | evck_ocpp:392 | [OCPP MSG OUT] CALL Heartbeat | UID: 69682923bc1442fd8a5edb2a7c103c86 | Payload: {}
17:06:00 DEBUG    | evck_ocpp:410 | [OCPP MSG IN] CALLRESULT for Heartbeat | UID: 69682923bc1442fd8a5edb2a7c103c86 | Response: {'currentTime': '2026-09-19T13:36:00.8569909+00:00'}
17:06:00 INFO     | evck_ocpp:1182 | [HEARTBEAT] Heartbeat sent successfully.
17:06:00 DEBUG    | evck.messaging:94 | [TX ACK] message id=280 delivered to topic: server_status
17:06:01 INFO     | evck_ocpp:1323 | Client stopped by user.

(.venv) C:\Users\Danesh\Documents\Sara\Tesla\github-evckcli\evck160c- decentralized\opt\evck>


## my analyse:
I tested the steps from step 1 to 5:
so, I feel as for the step 1 to 5, I see that this is not working. as I right?
let's fix this and then check others.