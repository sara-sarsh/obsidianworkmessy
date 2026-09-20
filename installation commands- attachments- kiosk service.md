The great news here is that your Python backend is a complete success! The logs show that your `gui_app` is happily serving video/image files, and your `lpc_interface` is successfully connected to the MQTT broker and subscribing to all the EV charger gun topics.

The issue you are seeing now is entirely isolated to the display. Your `evck-kiosk.service` is stuck in an infinite restart loop.

### Why is the Kiosk Looping?

Look at the `ExecStart` line in your service file: `ExecStart=/usr/bin/startx`.

When `startx` runs, it boots up the X11 graphical server and immediately looks for a hidden configuration file in your home directory called `.xinitrc`. This file is supposed to tell the display what application to launch (in this case, Chromium).

If `.xinitrc` is missing, empty, or running Chromium in the background, `startx` assumes there is nothing left to display, so it shuts down. Systemd sees it shut down, waits 2 seconds (because of `Restart=always`), and tries to start it again.

### The Fix: Create the `.xinitrc` File

You need to create the hidden instruction file that tells `startx` to keep Chromium open in the foreground.

**Step 1: Open the file on the Orange Pi**
Run this command to create and edit the file in the `orangepi` user's home directory:

```bash
nano /home/orangepi/.xinitrc

```

**Step 2: Paste the Kiosk Instructions**
Paste the following code into the file. This disables the screensaver, turns off screen sleeping, and forces Chromium to open your GUI in full-screen kiosk mode.
*(Note: Change `5000` to whatever port your Flask app is running on if it is different).*

```bash
#!/bin/sh

# Turn off power management and screen blanking
xset -dpms
xset s off
xset s noblank

# Hide the mouse cursor (Optional: requires 'sudo apt install unclutter')
# unclutter -idle 0.1 -root &

# Launch Chromium in infinite Kiosk mode pointed at your local GUI
exec chromium-browser \
  --no-sandbox \
  --kiosk \
  --noerrdialogs \
  --disable-infobars \
  --no-first-run \
  --incognito \
  --window-position=0,0 \
  "http://127.0.0.1:5000"

```

**Step 3: Save and Make Executable**

1. Press `Ctrl+O`, then `Enter` to save.
2. Press `Ctrl+X` to exit nano.
3. Make the file executable so `startx` can run it:

```bash
chmod +x /home/orangepi/.xinitrc

```

**Step 4: Restart the Kiosk**
Restart the service to break the old loop and trigger the new file:

```bash
sudo systemctl restart evck-kiosk
sudo journalctl -fu evck-kiosk

```

Once this runs, the loop should stop, and your Orange Pi's HDMI output should instantly light up with your Flask GUI.


sudo usermod -aG video,render,input orangepi



حق با شماست. برای داشتن یک سیستم کیوسکِ پایدار، هم اسکریپت اینستالر باید دسترسی‌ها را تضمین کند و هم تنظیمات کرومیوم باید برای سخت‌افزارِ شما (GPU) بهینه باشد تا صفحه سفید نماند.

در اینجا نسخه نهایی و کامل را برای شما آماده کرده‌ام.

### ۱. اسکریپت اینستالرِ بهینه شده (`install.sh`)

این نسخه دسترسی‌های `Xorg` و `User` را در همان ابتدای کار فیکس می‌کند.

```bash
#!/bin/bash
# EVCK Installer - Optimized for OrangePi
set -e

USER_NAME="orangepi"
EVCK_ROOT="/opt/evck"

# ۱. تنظیم دسترسی Xorg برای اجرای بدون sudo
echo ">>> Setting SUID for Xorg..."
chmod u+s /usr/lib/xorg/Xorg

# ۲. دسترسی‌های کاربر به سخت‌افزار
usermod -a -G dialout,gpio,video,audio,render,systemd-journal,i2c $USER_NAME

# ۳. تنظیم مجوز کنسول برای کاربر
echo ">>> Setting TTY permissions..."
chown $USER_NAME /dev/tty1 /dev/tty2

# ۴. نصب وابستگی‌های لازم
apt-get install -y xserver-xorg x11-xserver-utils xinit openbox chromium-browser unclutter

```

---

### ۲. اسکریپت استارترِ بهینه شده (`start-kiosk.sh`)

این همان جایی است که تگ‌های گرافیکی (Chromium Flags) را برای رفع صفحه سفید و بهینه‌سازی GPU قرار دادیم.

```bash
#!/bin/bash
export DISPLAY=:0
export XAUTHORITY=/home/orangepi/.Xauthority

# تنظیمات نمایشگر
xset s off; xset -dpms; xset s noblank
xrandr --output HDMI-1 --rotate left

# اجرای کرومیوم با تگ‌های گرافیکی برای رفع صفحه سفید
/usr/bin/chromium-browser --app="http://192.168.0.102:8082" \
  --kiosk \
  --no-sandbox \
  --disable-gpu-watchdog \
  --enable-gpu-rasterization \
  --ignore-gpu-blocklist \
  --enable-zero-copy \
  --disable-features=Translate,TranslateUI \
  --user-data-dir="/home/orangepi/.evck-chrome-profile" \
  --disk-cache-dir="/home/orangepi/.evck-chrome-cache" > /opt/evck/logs/chromium.log 2>&1 &

wait $!

```

---

### ۳. فایل سرویس سیستم‌دی (`evck-kiosk.service`)

این فایل باید در `/etc/systemd/system/evck-kiosk.service` قرار بگیرد.

```ini
[Unit]
Description=Chromium Kiosk
After=graphical.target

[Service]
User=orangepi
Environment=DISPLAY=:0
Environment=XAUTHORITY=/home/orangepi/.Xauthority
# اجرای مستقیم استارت‌ایکس
ExecStart=/usr/bin/startx /opt/evck/start-kiosk.sh
Restart=always
RestartSec=5

[Install]
WantedBy=graphical.target

```

### چرا این اسکریپت‌ها مشکل شما را حل می‌کنند؟

* **`chmod u+s /usr/lib/xorg/Xorg`**: این دستوری است که باعث می‌شود دیگر خطای `Permission denied` در اجرای `startx` نگیرید.
* **تگ‌های گرافیکی (`--enable-gpu-rasterization`, `--ignore-gpu-blocklist`)**: این تگ‌ها به کرومیوم می‌گویند: "حتی اگر فکر می‌کنی درایور گرافیکِ این برد (Mali) خیلی قدیمی است، باز هم از سخت‌افزار استفاده کن". این دقیقاً همان چیزی است که جلوی **سفید ماندن صفحه** را می‌گیرد.
* **`wait $!`**: این دستور در اسکریپت باعث می‌شود اسکریپت تا زمانی که کرومیوم باز است زنده بماند. اگر این نباشد، اسکریپت تمام می‌شود، `systemd` فکر می‌کند برنامه کرش کرده و دوباره آن را استارت می‌زند (همان حلقه‌ای که گرفتار شده بودید).

**الان این تغییرات را اعمال کن و یک ری‌بوت بزن.** اگر باز هم تصویر نیامد، فقط کافیست یک بار در SSH بزنی: `sudo chvt 2`.

آیا با این تنظیمات جدید، تصویر اپلیکیشن شما لود شد؟