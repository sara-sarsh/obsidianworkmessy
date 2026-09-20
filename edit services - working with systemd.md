You can edit the service in two common ways.

---

## Method 1: edit the full service file directly

First find where the service file is:

```bash
sudo systemctl cat evck-lpc
```

You will see something like:

```text
# /etc/systemd/system/evck-lpc.service
[Unit]
...
```

Then edit that file:

```bash
sudo nano /etc/systemd/system/evck-lpc.service
```

If your path is different, use the path shown by `systemctl cat`.

---

## Example edited service

You can add this line before `ExecStart`:

```ini
ExecStartPre=/bin/sleep 5
```

Full example:

```ini
[Unit]
Description=EVCK LPC Interface
After=network-online.target mosquitto.service
Wants=network-online.target
Requires=mosquitto.service

[Service]
Type=simple
User=root
Group=root
WorkingDirectory=/opt/evck
EnvironmentFile=/etc/evck/common/base.env
EnvironmentFile=-/etc/evck/local.env
EnvironmentFile=-/etc/evck/local/ENVS.env
EnvironmentFile=-/etc/evck/local/calibration.env
EnvironmentFile=-/mnt/spi/evck/local/ENVS.env
EnvironmentFile=-/mnt/spi/evck/local/calibration.env
ExecStartPre=/bin/sleep 5
ExecStart=/opt/evck/.venv/bin/python -m lpc_interface.lpc_interface
Restart=always
RestartSec=2
SupplementaryGroups=dialout

[Install]
WantedBy=multi-user.target
```

Save in nano:

```text
Ctrl + O
Enter
Ctrl + X
```

Then reload systemd:

```bash
sudo systemctl daemon-reload
```

Restart the service:

```bash
sudo systemctl restart evck-lpc
```

Check status:

```bash
sudo systemctl status evck-lpc
```

Watch logs:

```bash
sudo journalctl -fu evck-lpc
```

---

## Method 2: use systemctl edit full

This is usually safer:

```bash
sudo systemctl edit --full evck-lpc
```

This opens the full service file in an editor.

Make your changes, save, then run:

```bash
sudo systemctl daemon-reload
sudo systemctl restart evck-lpc
sudo journalctl -fu evck-lpc
```

---

## If you only want to add delay without editing whole file

You can use a drop-in override:

```bash
sudo systemctl edit evck-lpc
```

Add:

```ini
[Service]
ExecStartPre=/bin/sleep 5
```

Save and exit.

Then:

```bash
sudo systemctl daemon-reload
sudo systemctl restart evck-lpc
sudo journalctl -fu evck-lpc
```

---

## Useful commands

Enable service on boot:

```bash
sudo systemctl enable evck-lpc
```

Restart:

```bash
sudo systemctl restart evck-lpc
```

Stop:

```bash
sudo systemctl stop evck-lpc
```

Status:

```bash
sudo systemctl status evck-lpc
```

Logs:

```bash
sudo journalctl -u evck-lpc -n 100
```

Live logs:

```bash
sudo journalctl -fu evck-lpc
```

---

Important: keep this:

```ini
User=root
Group=root
ExecStart=/opt/evck/.venv/bin/python -m lpc_interface.lpc_interface
```

Do **not** put `sudo` inside `ExecStart`. Since `User=root` is already set, the script already runs with root permission.



sudo systemctl show evck-lpc -p User -p Group -p ExecStart -p ExecStartPre
sudo systemctl show evck-lpc -p MainPID
sudo lsof /dev/ttyS5




-----
How to check the Startup logs
2) Check whether startup logs were missed due to journalctl -f
Your journalctl -fu evck-opi_hardware only follows new logs. If startup logs happened before attaching, you won’t see them.

Use:

bash
journalctl -u evck-opi_hardware -b --no-pager