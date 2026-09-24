# راهنمای عملی: گرفتن اسکرین‌شات از کیوسک Chromium در Orange Pi / Ubuntu

این راهنما برای سرویسی به نام `evck-kiosk.service` نوشته شده که محیط گرافیکی را با
`startx` روی **VT2** و با `DISPLAY=:0` اجرا می‌کند و Chromium را در حالت kiosk بالا می‌آورد.
همه دستورها قابل کپی هستند و در صورت وجود متغیر جایگزین، آن را با مقدار واقعی خود عوض کنید.

---

## ۱. نگاه کلی به سرویس کیوسک

اول مطمئن شوید سرویس در چه حالتی است و دقیقاً چه چیزی اجرا می‌کند:

```bash
systemctl status evck-kiosk.service
```

مشاهده خروجی‌های اخیر سرویس (لاگ zram/سرویس):

```bash
journalctl -u evck-kiosk.service -n 100 --no-pager
```

دنبال‌کردن زنده لاگ‌ها هنگام راه‌اندازی مجدد:

```bash
journalctl -u evck-kiosk.service -f
```

اگر سرویس فعال (active) است ولی تصویری روی مانیتور نمی‌بینید، ادامه راهنما را طی کنید.

---

## ۲. بررسی خروجی فیزیکی HDMI با `chvt`

محیط گرافیکی روی **VT2** اجرا می‌شود. برای رفتن به آن ترمینال مجازی روی خود دستگاه:

```bash
sudo chvt 2
```

> ⚠️ **هشدار مهم:** این دستور **ترمینال فعال (active VT) را عوض می‌کند**. اگر از طریق
> کنسول محلی (کیبورد/مانیتور متصل به دستگاه) کار می‌کنید یا کاربر دیگری روی VT دیگر
> مشغول است، جلسه فعال او از نمایش خارج می‌شود. بهتر است این دستور را از طریق SSH
> اجرا کنید تا فقط خروجی HDMI به VT2 سوییچ شود.

اگر بعد از `chvt 2` تصویر روی HDMI ظاهر شد، مشکل فقط مربوط به VT فعال بوده است.
اگر باز هم صفحه سفید بود، به سراغ گرفتن اسکرین‌شات نرم‌افزاری بروید تا مشخص شود
مشکل در سمت X/Chromium است یا در خروجی سخت‌افزاری HDMI.

---

## ۳. نصب ابزارهای لازم

ابزار اسکرین‌شات (از بسته ImageMagick):

```bash
sudo apt update
sudo apt install -y imagemagick
```

ابزارهای عیب‌یابی X (اختیاری ولی مفید، شامل `xdpyinfo` و `xlsclients`):

```bash
sudo apt install -y x11-utils
```

---

## ۴. یافتن مسیر فایل Xauthority به‌صورت پویا

مسیر فایل auth را **فرض نکنید** (در برخی سیستم‌ها `/var/lib/lightdm/.Xauthority` یا `/tmp/serverauth.*` یا مسیر خانه کاربر است). آن را مستقیماً از آرگومان‌های پروسه `Xorg` استخراج کنید:

```bash
AUTH=$(sudo ps -o args= -p "$(pgrep -x Xorg | head -n1)" 2>/dev/null | grep -oP '(?<=-auth )\S+')
# اگر مقدار بالا خالی بود، روش جایگزین با proc cmdline:
[ -z "$AUTH" ] && AUTH=$(sudo tr '\0' '\n' < /proc/"$(pgrep -x Xorg | head -n1)"/cmdline 2>/dev/null | grep -A1 '^-auth$' | tail -n1)

echo "AUTH=$AUTH"
```

### اگر خروجی `AUTH` خالی بود یا پروسه Xorg پیدا نشد

به‌ترتیب این بررسی‌ها را انجام دهید:

```bash
# آیا اصلاً Xorg اجرا شده است؟
pgrep -ax Xorg

# اگر اجرا نشده، سرویس کیوسک را بررسی و لاگش را ببینید
systemctl status evck-kiosk.service
journalctl -u evck-kiosk.service -n 100 --no-pager

# اگر Xorg اجراست ولی متغیر AUTH پر نشد، خط کامل پروسه را ببینید
ps -ef | grep -E '[X]org|startx'
sudo cat /proc/"$(pgrep -x Xorg | head -n1)"/cmdline | tr '\0' ' '; echo
```

بر اساس نتیجه:

- **Xorg اصلاً اجرا نیست:** سرویس/اسکریپت `startx` شکست خورده؛ لاگ `journalctl`
  و خطاهای `.xinitrc` را بررسی کنید.
- **Xorg هست ولی `-auth` ندارد:** در برخی پیکربندی‌ها ممکن است بدون احراز هویت اجرا شده باشد که در این حالت دستور `import` بدون متغیر `XAUTHORITY` کار می‌کند.
- **فایل auth وجود ندارد** (مثلاً پاک شده): سرویس را ری‌استارت کنید تا بازسازی شود (`sudo systemctl restart evck-kiosk.service`).

---

## ۵. گرفتن اسکرین‌شات از صفحه کیوسک

دستور کامل و یک‌خطی گرفتن اسکرین‌شات با استخراج خودکار و بدون وابستگی به مراحل قبلی:

```bash
# استخراج و اجرای همزمان در یک دستور:
AUTH=$(sudo tr '\0' '\n' < /proc/"$(pgrep -x Xorg | head -n1)"/cmdline 2>/dev/null | grep -A1 '^-auth$' | tail -n1)
sudo env DISPLAY=:0 XAUTHORITY="$AUTH" import -window root /tmp/kiosk.png
```

یا اگر از قبل متغیر `AUTH` را مقداردهی کرده‌اید:

```bash
sudo env DISPLAY=:0 XAUTHORITY="$AUTH" import -window root /tmp/kiosk.png
```

> **نکته:** اگر `AUTH` خالی بود یا `-auth` در پروسه X وجود نداشت، می‌توانید مستقیماً با دسترسی root یا کاربر مربوطه بدون متغیر XAUTHORITY تست کنید:
> ```bash
> sudo env DISPLAY=:0 import -window root /tmp/kiosk.png
> ```

---

## ۶. صحت‌سنجی فایل اسکرین‌شات

```bash
file /tmp/kiosk.png
ls -lh /tmp/kiosk.png
identify /tmp/kiosk.png
```

- `file` باید چیزی شبیه `PNG image data, 1920 x 1080 ...` بدهد.
- اگر فایل بسیار کوچک است یا تماماً سفید، بخش ۸ (تفکیک سفیدی) را ببینید.
- برای انتقال سریع فایل و مشاهده محتوا بدون اینترفیت با کیوسک:

```bash
scp /tmp/kiosk.png USER@LOCAL_MACHINE_IP:/path/to/destination/
```

جای‌گزین‌ها را عوض کنید:

- `USER` → نام کاربری روی **کامپیوتر مقصد** (مثلاً لپ‌تاپ خودتان)
- `LOCAL_MACHINE_IP` → آی‌پی کامپیوتری که فایل به آن منتقل می‌شود
- `/path/to/destination/` → مسیر پوشه مقصد روی همان کامپیوتر

> توجه: مقصد `scp` روی **سیستم محلی/کامپیوتر خودتان** است، نه روی خود کیوسک؛
> یعنی از روی دستگاه Orange Pi فایل را به بیرون کپی می‌کنید.

---

## ۷. خطای Authorization پس از ری‌استارت

اگر پیام خطایی شبیه این دیدید:

```
Authorization required, but no authorization protocol specified
```

یعنی فایل `XAUTHORITY` پس از ری‌استارت سرویس **تغییر کرده** (یا بازسازی شده) است.
راه‌حل ساده: **مسیر auth را دوباره به‌صورت پویا استخراج کنید** (بخش ۴) و سپس
دستور `import` را با مقدار جدید اجرا کنید. هیچ‌وقت مسیر یا محتوای auth را هاردکد نکنید.

---

## ۸. تشخیص «اسکرین‌شات سفید» یا «خروجی HDMI سفید»

این دو حالت را اشتباه نگیرید:

| وضعیت | تفسیر |
|---|---|
| اسکرین‌شات رنگی/درست ولی HDMI سفید | مشکل در **خروجی فیزیکی/سخت‌افزاری HDMI** یا VT فعال است، نه در UI |
| اسکرین‌شات هم سفید است | مشکل در خود X/Chromium است؛ لاگ را ببینید |
| هر دو سفید + صفحه سیاه/بی‌تصویر | احتمال عدم شروع موفق X |

---

## ۹. بررسی لاگ‌ها با `journalctl`

```bash
# کل لاگ سرویس از آخرین بوت
journalctl -b -u evck-kiosk.service --no-pager

# لاگ با اولویت خطا و بالاتر
journalctl -b -u evck-kiosk.service -p err --no-pager

# لاگ سرور X
journalctl -b | grep -iE 'xorg|xinit|startx' | tail -n 60
```

به دنبال خطاهای GPU/DRM، خطای اتصال Chromium، یا خطای مفسر `.xinitrc` بگردید.

---

## ۱۰. تست اختیاری: غیرفعال‌کردن شتاب GPU در Chromium

⚠️ این یک **تست آزمایشی** است، نه راه‌حل قطعی. صرفِ بهبود symptom پس از این تغییر
به‌خودی‌خود **اثبات نمی‌کند** که GPU مقصر بوده است.

۱) اسکریپت شروع X را باز کنید:

```bash
sudo nano /home/<KIOSK_USER>/.xinitrc
```

۲) دستور واقعی اجرای Chromium را در این فایل پیدا کنید و **فقط همان خط را** ویرایش
کنید (همین دستور، نه یک دستور جدید). یک فلگ غیرفعال‌سازی GPU را اضافه کنید، مثلاً:

```bash
# قبل
chromium --kiosk http://127.0.0.1:8082 ...

# بعد
chromium --kiosk --disable-gpu http://127.0.0.1:8082 ...
```

۳) سرویس را ری‌استارت کنید:

```bash
sudo systemctl restart evck-kiosk.service
```

۴) دوباره اسکرین‌شات بگیرید (بخش ۵) و با قبلی مقایسه کنید.

> **نکته مهم:** بهبود موقت symptom پس از ری‌استارت سرویس، «ریشه‌یابی» محسوب نمی‌شود.
> برای ادعای علت، باید تغییر را برگردانید و رفتار را بازتولید/تأیید کنید.

---

## ۱۱. نکته درباره HTTP و رندر UI

اینکه سرور روی پورت **8082** جواب بدهد (مثلاً `curl -I http://127.0.0.1:8082`) فقط نشان می‌دهد
وب‌سرور **در دسترس** است؛ به‌هیچ‌وجه **اثبات نمی‌کند** که UI در Chromium واقعاً رندر شده است.
برای بررسی رندر واقعی فقط به اسکرین‌شات و بررسی گرافیکی تکیه کنید.

---

## خلاصه فرایند در یک نگاه

```bash
# ۱) وضعیت سرویس
systemctl status evck-kiosk.service
journalctl -u evck-kiosk.service -n 100 --no-pager

# ۲) سوییچ به VT2 (هشدار: VT فعال تغییر می‌کند)
sudo chvt 2

# ۳) ابزارها
sudo apt install -y imagemagick x11-utils

# ۴ و ۵) استخراج پویا و گرفتن اسکرین‌شات
AUTH=$(sudo tr '\0' '\n' < /proc/"$(pgrep -x Xorg | head -n1)"/cmdline 2>/dev/null | grep -A1 '^-auth$' | tail -n1)
sudo env DISPLAY=:0 XAUTHORITY="$AUTH" import -window root /tmp/kiosk.png

# ۶) صحت‌سنجی و انتقال
file /tmp/kiosk.png
identify /tmp/kiosk.png
scp /tmp/kiosk.png USER@LOCAL_MACHINE_IP:/path/to/destination/
```
