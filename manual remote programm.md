# Manual Remote Programming Guide (LPC via lpc21isp)

## ۱. نصب و کامپایل ابزار lpc21isp (در صورت عدم نصب قبلی)
```bash
cd /tmp
git clone https://github.com/capiman/lpc21isp.git
cd lpc21isp
make
sudo install -m 755 lpc21isp /usr/local/bin/
lpc21isp -version
```

---

## ۲. متوقف کردن سرویس‌های درگیر
پیش از پروگرم کردن، حتماً سرویس‌هایی که با پورت سریال `/dev/ttyS5` یا پین‌های GPIO درگیرند (مانند `lpc-interface`) را متوقف کنید:
```bash
sudo systemctl stop evck-lpc.service
sudo fuser -k /dev/ttyS5 2>/dev/null || true
```

---

## ۳. آزاد کردن GPIO 257 (در صورت خطای Device or resource busy)
```bash
sudo sh -c 'echo 257 > /sys/class/gpio/unexport' 2>/dev/null || true
```

---

## ۴. ورود میکروکنترلر به حالت ISP Bootloader
```bash
# فعال کردن پین‌های Reset و Boot
sudo gpioset gpiochip0 262=1
sudo gpioset gpiochip0 261=1
sleep 0.1

# رهاسازی Reset (میکرو وارد بوت‌لودر ISP می‌شود)
sudo gpioset gpiochip0 262=0
sleep 0.1

# رهاسازی پین Boot
sudo gpioset gpiochip0 261=0

# فعال کردن بافر / تراسیور ارتباطی
sudo gpioset gpiochip0 257=0
```

---

## ۵. پروگرم کردن فایل Hex با ابزار lpc21isp
توجه: فایل `.hex` به تنهایی اجراشدنی نیست و باید به عنوان ورودی به `lpc21isp` داده شود:

```bash
cd /opt/evck/tools/RemoteProgramTest
sudo lpc21isp -wipe -verify -hex Charger_CCU_Test_1.hex /dev/ttyS5 38400 12000


sudo lpc21isp -wipe -verify -hex /opt/evck/tools/remote_program/RemoteProgramTest/Charger_CCU_Test_2.hex /dev/ttyS5 38400 12000


sudo lpc21isp -wipe -verify -hex /opt/evck/tools/Charger_CCU_V9.2.hex /dev/ttyS5 38400 12000
```
یا با آدرس کامل فایل:
```bash
sudo lpc21isp -wipe -verify -hex /opt/evck/tools/RemoteProgramTest/Charger_CCU_Test_1.hex /dev/ttyS5 38400 12000
```

---

## ۶. خروج از ISP و ریستارت میکرو جهت اجرای برنامه عادی
```bash
# اطمینان از خاموش بودن Boot
sudo gpioset gpiochip0 261=0

# اعمال پالس Reset به میکرو
sudo gpioset gpiochip0 262=1
sleep 0.2
sudo gpioset gpiochip0 262=0

# راه‌اندازی مجدد سرویس سیستم در صورت نیاز
sudo systemctl start evck-lpc.service
```