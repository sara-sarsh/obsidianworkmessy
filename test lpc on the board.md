source /opt/evck/.venv/bin/activate
scp -r /Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evckcli-debug/opt/evck orangepi@10.115.112.222:/opt/evck
export PYTHONPATH="$(pwd)/opt/evck"
mosquitto -v
mosquitto_pub -t day_night/gun_1 -m 1
mosquitto_pub -t day_night/gun_2 -m 1
mosquitto_sub -h localhost -t "day_night/# -v
mosquitto_sub -t "day_night/#" -v

mosquitto_pub -t starting/gun_1 -m 1
mosquitto_pub -t stop/gun_1 -m 1
mosquitto_pub -t max_power/gun_1 -m 1
sudo minicom -D /dev/ttyS5 -b 115200

sudo systemctl daemon-reload
sudo systemctl restart lpc-interface.service


well, fix lpc service
by openning and closing a minicom
#!/bin/bash

/usr/bin/minicom -D /dev/ttyS5 -b 115200 -o -m </dev/null >/dev/null 2>&1 &

PID=$!

sleep 1

kill $PID 2>/dev/null

wait $PID 2>/dev/null

exit 0

Make executable:

sudo chmod +x /usr/local/bin/minicom_init.sh

Then update service:

ExecStartPre=/usr/local/bin/minicom_init.sh

ExecStart=/opt/evck/.venv/bin/python -m lpc_interface.lpc_interface