
Here is everything you need: the **270° rotation fix**, the explanation for **`Xwrapper.config`**, a **fully automated installer script**, a **manual `README.md` guide**, and a **system performance monitoring script**.

---

## 1. Quick Answers & Clarifications

### A. Fix 270° Screen Rotation

To rotate the screen **270 degrees** in X11 using `xrandr`, change `--rotate left` (90°) to `--rotate right` (270°).

In `/boot/orangepiEnv.txt`, update `fbcon=rotate:` to `1` (or `3` depending on orientation) for early console rotation, and in `/opt/evck/start-kiosk.sh`, use:

```bash
xrandr --output "$OUTPUT" --mode 1920x1080 --rotate right

```

### B. What does `/etc/X11/Xwrapper.config` do?

```ini
allowed_users=anybody
needs_root_rights=yes

```

* **`allowed_users=anybody`**: Standard Linux security blocks non-root users from launching an X11 server (`startx`) from a non-login background service. Setting this allows `orangepi` to run `startx` via systemd.
* **`needs_root_rights=yes`**: Gives the Xorg binary permission to open raw `/dev/tty2` virtual consoles and GPU DRM memory nodes without requiring full `root` privileges for the kiosk script itself.

---

## 2. Automated Installer Script (`install-kiosk.sh`)

Save this file as `install-kiosk.sh`, make it executable with `chmod +x install-kiosk.sh`, and run `sudo ./install-kiosk.sh`. It automatically configures dependencies, groups, Xwrapper, systemd units, startup scripts, and boot environment parameters.

```bash
#!/bin/bash
# ==============================================================================
# EVCK Kiosk Automated Installer for Orange Pi Zero 2W (Ubuntu Jammy)
# ==============================================================================

set -euo pipefail

# Ensure running as root
if [ "$EUID" -ne 0 ]; then
  echo "❌ Please run this script with sudo or as root: sudo ./install-kiosk.sh"
  exit 1
fi

TARGET_USER="orangepi"
KIOSK_DIR="/opt/evck"
LOG_DIR="${KIOSK_DIR}/logs"

echo "🚀 [1/6] Installing System Dependencies..."
apt-get update
apt-get install -y --no-install-recommends \
  xorg \
  openbox \
  chromium-browser \
  unclutter \
  x11-xserver-utils \
  curl \
  procps \
  lm-sensors

echo "👥 [2/6] Configuring Permissions & Hardware Groups..."
usermod -aG tty,input,video,render "$TARGET_USER" || true

# Configure Xwrapper
cat << 'EOF' > /etc/X11/Xwrapper.config
allowed_users=anybody
needs_root_rights=yes
EOF

echo "📂 [3/6] Creating Directories..."
mkdir -p "$LOG_DIR"
chown -R "$TARGET_USER":"$TARGET_USER" "$KIOSK_DIR"

echo "📜 [4/6] Writing Kiosk Startup Script (/opt/evck/start-kiosk.sh)..."
cat << 'EOF' > "${KIOSK_DIR}/start-kiosk.sh"
#!/bin/bash
set -u

# Disable power saving and screen blanking
xset -dpms || true
xset s off || true
xset s noblank || true

# Start Openbox Window Manager if not running
if ! pgrep -x "openbox" > /dev/null; then
    openbox &
    sleep 1
fi

# Hide cursor
pkill -x unclutter || true
if command -v unclutter >/dev/null 2>&1; then
  unclutter -idle 0.5 -root &
fi

# Set 270-degree rotation (xrandr --rotate right = 270 degrees)
OUTPUT="$(xrandr | awk '/ connected/{print $1; exit}')"
OUTPUT="${OUTPUT:-HDMI-1}"
xrandr --output "$OUTPUT" --mode 1920x1080 --rotate right || true
sleep 1

# Kiosk URL and Temporary Profiles
URL="http://127.0.0.1:8082"
PROFILE_DIR="/tmp/evck-chrome-profile"
CACHE_DIR="/tmp/evck-chrome-cache"

rm -rf "$PROFILE_DIR"
mkdir -p "$PROFILE_DIR" "$CACHE_DIR" /opt/evck/logs

# Wait for local Web GUI backend
until curl -s "$URL" >/dev/null; do
  sleep 1
done

echo "Starting Chromium Kiosk on $URL..."

# Launch Chromium with explicit geometry flags
exec chromium \
  --app="$URL" \
  --start-fullscreen \
  --window-size=1080,1920 \
  --window-position=0,0 \
  --no-first-run \
  --no-default-browser-check \
  --disable-infobars \
  --noerrdialogs \
  --disable-session-crashed-bubble \
  --user-data-dir="$PROFILE_DIR" \
  --disk-cache-dir="$CACHE_DIR" \
  --disk-cache-size=104857600 \
  --disable-extensions \
  --disable-background-networking \
  --disable-sync \
  --ignore-gpu-blocklist \
  --enable-gpu-rasterization \
  --v=0 > /opt/evck/logs/chromium.log 2>&1
EOF

chmod +x "${KIOSK_DIR}/start-kiosk.sh"
chown "$TARGET_USER":"$TARGET_USER" "${KIOSK_DIR}/start-kiosk.sh"

echo "⚙️ [5/6] Configuring Systemd Kiosk Service..."
cat << EOF > /etc/systemd/system/evck-kiosk.service
[Unit]
Description=EVCK Chromium Kiosk
After=network.target evck-gui.service
Wants=evck-gui.service

[Service]
Type=simple
User=${TARGET_USER}
PAMName=login
TTYPath=/dev/tty2
StandardInput=tty
Environment=DISPLAY=:0
Environment=XDG_RUNTIME_DIR=/run/user/1000
WorkingDirectory=/home/${TARGET_USER}
ExecStart=/usr/bin/startx ${KIOSK_DIR}/start-kiosk.sh -- :0 vt2
Restart=always
RestartSec=3

[Install]
WantedBy=multi-user.target
EOF

# Enable persistent journal logging
mkdir -p /var/log/journal
systemctl restart systemd-journald

systemctl daemon-reload
systemctl enable evck-kiosk.service

echo "🖥️ [6/6] Updating Boot Environment File (/boot/orangepiEnv.txt)..."
ENV_FILE="/boot/orangepiEnv.txt"

if [ -f "$ENV_FILE" ]; then
  # Backup existing file
  cp "$ENV_FILE" "${ENV_FILE}.bak"

  # Fix extraargs for rotation and clean overlays
  sed -i 's/^extraargs=.*/extraargs=fbcon=rotate:1 console=ttyS0 console=tty3 loglevel=3 vt.global_cursor_default=0 logo.nologo splash quiet video=HDMI-A-1:1920x1080@60/' "$ENV_FILE"
  sed -i 's/video=HDMI-A-1:[^ ]*//g' "$ENV_FILE"
fi

echo "================================================================="
echo "✅ Installation Complete! Please reboot your system now:"
echo "   sudo reboot"
echo "================================================================="

```

---

## 3. Manual Setup Guide (`README.md`)

```markdown
# EVCK Kiosk Setup Guide for Orange Pi Zero 2W

This guide explains how to manually configure an Orange Pi Zero 2W running Ubuntu Jammy as an auto-booting Chromium kiosk with 270° display rotation.

---

## Step 1: Install Required Packages

```bash
sudo apt update
sudo apt install -y xorg openbox chromium-browser unclutter x11-xserver-utils curl

```

---

## Step 2: Configure Hardware Permissions & Xwrapper

1. Add the `orangepi` user to input and video groups:
```bash
sudo usermod -aG tty,input,video,render orangepi

```


2. Edit `/etc/X11/Xwrapper.config`:
```bash
sudo nano /etc/X11/Xwrapper.config

```


Add/Update:
```ini
allowed_users=anybody
needs_root_rights=yes

```



---

## Step 3: Create Startup Kiosk Script

1. Create directory structure:
```bash
sudo mkdir -p /opt/evck/logs
sudo chown -R orangepi:orangepi /opt/evck

```


2. Create `/opt/evck/start-kiosk.sh`:
```bash
nano /opt/evck/start-kiosk.sh

```


3. Paste the contents:
```bash
#!/bin/bash
set -u

xset -dpms || true
xset s off || true
xset s noblank || true

if ! pgrep -x "openbox" > /dev/null; then
    openbox &
    sleep 1
fi

pkill -x unclutter || true
unclutter -idle 0.5 -root &

OUTPUT="$(xrandr | awk '/ connected/{print $1; exit}')"
OUTPUT="${OUTPUT:-HDMI-1}"
xrandr --output "$OUTPUT" --mode 1920x1080 --rotate right || true

URL="[http://127.0.0.1:8082](http://127.0.0.1:8082)"
PROFILE_DIR="/tmp/evck-chrome-profile"
CACHE_DIR="/tmp/evck-chrome-cache"

rm -rf "$PROFILE_DIR"
mkdir -p "$PROFILE_DIR" "$CACHE_DIR" /opt/evck/logs

until curl -s "$URL" >/dev/null; do
  sleep 1
done

exec chromium \
  --app="$URL" \
  --start-fullscreen \
  --window-size=1080,1920 \
  --window-position=0,0 \
  --no-first-run \
  --no-default-browser-check \
  --disable-infobars \
  --noerrdialogs \
  --disable-session-crashed-bubble \
  --user-data-dir="$PROFILE_DIR" \
  --disk-cache-dir="$CACHE_DIR" \
  --disk-cache-size=104857600 \
  --disable-extensions \
  --disable-background-networking \
  --disable-sync \
  --ignore-gpu-blocklist \
  --enable-gpu-rasterization \
  --v=0 > /opt/evck/logs/chromium.log 2>&1

```


4. Make executable:
```bash
chmod +x /opt/evck/start-kiosk.sh

```



---

## Step 4: Create Systemd Kiosk Service

1. Create `/etc/systemd/system/evck-kiosk.service`:
```bash
sudo nano /etc/systemd/system/evck-kiosk.service

```


2. Add configuration:
```ini
[Unit]
Description=EVCK Chromium Kiosk
After=network.target evck-gui.service
Wants=evck-gui.service

[Service]
Type=simple
User=orangepi
PAMName=login
TTYPath=/dev/tty2
StandardInput=tty
Environment=DISPLAY=:0
Environment=XDG_RUNTIME_DIR=/run/user/1000
WorkingDirectory=/home/orangepi
ExecStart=/usr/bin/startx /opt/evck/start-kiosk.sh -- :0 vt2
Restart=always
RestartSec=3

[Install]
WantedBy=multi-user.target

```


3. Enable persistent logging and start service:
```bash
sudo mkdir -p /var/log/journal
sudo systemctl restart systemd-journald
sudo systemctl daemon-reload
sudo systemctl enable evck-kiosk.service

```



---

## Step 5: Configure Kernel Display Overrides (`/boot/orangepiEnv.txt`)

Ensure `/boot/orangepiEnv.txt` has clean overlays and `video=` inside `extraargs`:

```ini
verbosity=1
bootlogo=false
console=both
disp_mode=1920x1080p60
overlay_prefix=sun50i-h616
rootdev=UUID=eaa7fbfc-c2d4-469e-b037-a160948a9c70
rootfstype=ext4
extraargs=fbcon=rotate:1 console=ttyS0 console=tty3 loglevel=3 vt.global_cursor_default=0 logo.nologo splash quiet video=HDMI-A-1:1920x1080@60
overlays=gpu ph-i2c1 ph-uart5 pi-i2c1 i2c1-100khz
param_uart5_rtscts=0

```

Reboot to apply: `sudo reboot`.

```

---

## 4. Resource & Metric Efficiency Monitoring Script (`check-metrics.sh`)

Save this as `check-metrics.sh`, grant permissions (`chmod +x check-metrics.sh`), and run `./check-metrics.sh` to analyze performance and resource utilization in real time.

```bash
#!/bin/bash
# ==============================================================================
# EVCK System Resource Efficiency Tester
# ==============================================================================

echo "================================================================="
echo "📊 EVCK System Performance & Resource Audit"
echo "   Host: $(hostname) | Time: $(date)"
echo "================================================================="

# 1. CPU Temperature Check
TEMP=$(cat /sys/class/thermal/thermal_zone0/temp 2>/dev/null || echo "0")
TEMP_C=$((TEMP / 1000))
echo -n "🌡️  CPU Temperature: ${TEMP_C}°C "
if [ "$TEMP_C" -lt 65 ]; then
  echo "(✅ Optimal)"
elif [ "$TEMP_C" -lt 75 ]; then
  echo "(⚠️ Warm - Check heatsink/airflow)"
else
  echo "(❌ HOT - Thermal throttling risk)"
fi

# 2. RAM & Swap Utilization
echo -e "\n💾 Memory Usage:"
free -h | awk '
  /^Mem:/ {print "   RAM : Used: " $3 " / Total: " $2 " (" $3/$2*100 "%)"}
  /^Swap:/ {print "   Swap: Used: " $3 " / Total: " $2}
'

# 3. CPU Load Averages
echo -e "\n⚡ CPU Load Averages (1m, 5m, 15m):"
uptime | awk -F'load average:' '{ print "   " $2 }'

# 4. Storage Wear & Usage
echo -e "\n💽 Disk Utilization (/):"
df -h / | awk 'NR==2 {print "   Used: " $3 " / Total: " $2 " (" $5 " used)"}'

# 5. Service Status Check
echo -e "\n🛠️ Service Health:"
for svc in evck-gui evck-kiosk redis-server; do
  STATUS=$(systemctl is-active "$svc" 2>/dev/null || echo "inactive")
  if [ "$STATUS" = "active" ]; then
    echo "   [  OK  ] $svc is running"
  else
    echo "   [ FAIL ] $svc is $STATUS"
  fi
done

# 6. Top Resource Consumers (CPU & RAM)
echo -e "\n🔥 Top 5 CPU Processes:"
ps aux --sort=-%cpu | head -n 6 | tail -n 5 | awk '{printf "   PID: %-6s CPU: %-5s RAM: %-5s CMD: %s\n", $2, $3"%", $4"%", $11}'

echo -e "\n🧠 Top 5 Memory Processes:"
ps aux --sort=-%mem | head -n 6 | tail -n 5 | awk '{printf "   PID: %-6s RAM: %-5s CPU: %-5s CMD: %s\n", $2, $4"%", $3"%", $11}'

echo "================================================================="

```