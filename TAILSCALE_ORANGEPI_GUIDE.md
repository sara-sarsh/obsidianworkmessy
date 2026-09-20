

# راهنمای جامع راه‌اندازی Tailscale و مدیریت سرویس روی Orange Pi

این مستند شامل دو بخش است:
1. **روش تجربی و عملیاتی فعلی** (دستوراتی که در حال حاضر استفاده شده و بدون وابستگی به اینترنت مستقیم تست شده و کار می‌کند).
2. **روش اصولی و استاندارد (Best Practice)** برای تولید انبوه دستگاه‌ها (تک‌خطی، خودکار و بدون نیاز به کلیک و لاگین دستی در مرورگر).

---

## بخش اول: روش تجربی و سریع (عملیاتی فعلی)

این روش برای مواقعی است که می‌خواهید دستگاه را سریع و دستی با فایل `.deb` لوکال راه‌اندازی کنید یا کانفیگ‌های قدیمی را پاکسازی نمایید.

### ۱. پاکسازی کامل نصب‌های قبلی (Clean / Purge)
در صورتی که تیل‌اسکیل قبلاً ناقص نصب شده یا لاگین روی اکانت دیگری مانده باشد:

```bash
# خروج و قطع اتصال
sudo tailscale logout 2>/dev/null
sudo tailscale down 2>/dev/null
sudo systemctl disable --now tailscaled 2>/dev/null

# حذف کامل پکیج و تنظیمات
sudo apt purge -y tailscale
sudo rm -rf /var/lib/tailscale /etc/default/tailscaled
sudo systemctl daemon-reload

# بررسی حذف شدن
which tailscale tailscaled; dpkg -l | grep -i tailscale
```

---

### ۲. نصب دستی با فایل `.deb` (نسخه ARM64)
اگر فایل پکیج روی سیستم وجود دارد (مثلاً `~/tailscale_1.102.4_arm64.deb`):

```bash
# نصب پکیج
sudo dpkg -i ~/tailscale_1.102.4_arm64.deb

# رفع وابستگی‌های احتمالی (در صورت نیاز)
sudo apt -f install -y
```

> **نکته انتقال از مک:** اگر فایل روی اورنج‌پای نیست، از ترمینال مک اجرا کنید:
> ```bash
> scp tailscale_1.102.4_arm64.deb orangepi@192.168.0.102:~/
> ```

---

### ۳. فعال‌سازی سرویس در پس‌زمینه
```bash
sudo systemctl enable --now tailscaled

# بررسی فعال بودن سرویس
systemctl is-active tailscaled
tailscale version
```

---

### ۴. اتصال و احراز هویت دستی (با مرورگر)
```bash
sudo tailscale up --reset --hostname=techno-4 --ssh
```
- پس از اجرای این دستور، یک آدرس لاگین در خروجی ظاهر می‌شود (مثلاً `https://login.tailscale.com/a/xxxxxx`).
- آن را در مرورگر لپ‌تاپ باز کرده و تایید لاگین را بزنید تا خروجی ترمینال `Success.` نشان دهد.

---

### ۵. بررسی وضعیت و گرفتن آی‌پی
```bash
tailscale status
tailscale ip -4
```

---

### ۶. ریست دیتابیس و ری‌استارت سرویس شارژر (`tesla_charge.service`)
در صورتی که نیاز به شروع مجدد دیتابیس شارژر باشد:

```bash
# توقف سرویس
sudo systemctl stop tesla_charge.service

# تهیه نسخه پشتیبان از دیتابیس فعلی
sudo cp /opt/tesla-charge/tesla_charge.db /opt/tesla-charge/tesla_charge.db.backup

# حذف دیتابیس قدیمی و فایل‌های WAL
sudo rm -f /opt/tesla-charge/tesla_charge.db
sudo rm -f /opt/tesla-charge/tesla_charge.db-wal
sudo rm -f /opt/tesla-charge/tesla_charge.db-shm

# شروع مجدد سرویس
sudo systemctl start tesla_charge.service

# بررسی وضعیت اجرا
sudo systemctl status tesla_charge.service
```

---
---

## بخش دوم: روش اصولی، استاندارد و خودکار (Best Practice)

روش قبلی کار می‌کند، اما دو ایراد فنی دارد:
1. **نیاز به لاگین دستی در مرورگر:** برای هر دستگاه باید لینک را کپی کرده و دستی تایید کنید که در خط تولید یا دیپلوی از راه دور وقت‌گیر است.
2. **عدم دریافت آپدیت خودکار:** پکیج دستی با `apt upgrade` به‌روزرسانی نمی‌شود.
3. خطای `401` که دریافت کردید به این دلیل بود که متغیرهای OAuth تنظیم نشده بودند و اصلاً نیازی به ساخت توکن از طریق API تیل‌اسکیل با Python نیست!

### راه‌حل استاندارد چیست؟ استفاده از **Auth Key**

با یک کلید **Auth Key** با قابلیت استفاده مجدد (Reusable)، ثبت دستگاه و اتصال آن **در کمتر از ۵ ثانیه و کاملاً بی‌صدا** بدون نیاز به باز کردن هیچ صفحه‌ای انجام می‌شود.

---

### مرحله ۱: ساخت Auth Key در پنل Tailscale (فقط یک‌بار انجام می‌شود)
1. وارد پنل مدیریت شوید: [login.tailscale.com/admin/settings/keys](https://login.tailscale.com/admin/settings/keys)
2. روی **Generate auth key** کلیک کنید.
3. تیک‌های زیر را فعال کنید:
   - **Reusable:** بله (تا بتوانید روی همه اورنج‌پای‌ها مثل techno-1 تا techno-10 از همین یک کلید استفاده کنید).
   - **Ephemeral:** خیر (تیک نخورد تا با ریبوت دستگاه از پنل حذف نشود).
   - **Pre-authorized:** بله (دستگاه‌ها بلافاصله بدون نیاز به تایید دستی تایید شوند).
   - **Tags:** در صورت تمایل یک تگ مانند `tag:charger` به آن بدهید.
4. کلید ساخته شده را کپی کنید (فرمت آن شبیه `tskey-auth-kXXXXX...` است).

---

### مرحله ۲: اسکریپت تک‌خطی نصب و راه‌اندازی خودکار

روی اورنج‌پای فقط کافی است دستور زیر را اجرا کنید (به جای `tskey-auth-XXXX` کلید مرحله ۱ و به جای `techno-4` نام دستگاه):

```bash
sudo tailscale up --authkey="tskey-auth-XXXXXX" --hostname="techno-4" --ssh --reset
```
دستگاه **در ۲ ثانیه بدون هیچ تایید مرورگری** آنلاین شده و آی‌پی می‌گیرد!
tskey-auth-kDYvhTURqZ11CNTRL-jAXXPm2cSFYLyigpLRTbFYVoyCsx6my74
---

### مرحله ۳: افزودن مخزن رسمی جهت آپدیت منظم با APT (اختیاری ولی توصیه شده)

اگر روی اورنج‌پای دسترسی اینترنت برقرار است، برای اینکه در آینده با `apt update && apt upgrade` تیل‌اسکیل هم به‌روز شود:

```bash
# افزودن کلید GPG رسمی
curl -fsSL https://pkgs.tailscale.com/stable/ubuntu/jammy.noarmor.gpg | sudo tee /usr/share/keyrings/tailscale-archive-keyring.gpg >/dev/null

# افزودن آدرس مخزن
curl -fsSL https://pkgs.tailscale.com/stable/ubuntu/jammy.tailscale-keyring.list | sudo tee /etc/apt/sources.list.d/tailscale.list

# نصب یا آپدیت با apt
sudo apt-get update
sudo apt-get install -y tailscale
```

---

### اسکریپت آماده استقرار یکجا (All-In-One Deployment Script)

می‌توانید یک اسکریپت تمیز مثل `setup_tailscale.sh` روی دستگاه داشته باشید:

```bash
#!/usr/bin/env bash
set -e

HOSTNAME="${1:-$(hostname)}"
AUTH_KEY="tskey-auth-YOUR_KEY_HERE"

echo "==> Configuring Tailscale for $HOSTNAME..."

# نصب از deb محلی اگر نصب نیست
if ! command -v tailscale &> /dev/null; then
    if [ -f ~/tailscale_1.102.4_arm64.deb ]; then
        sudo dpkg -i ~/tailscale_1.102.4_arm64.deb
        sudo apt-f install -y
    fi
fi

# راه‌اندازی سرویس
sudo systemctl enable --now tailscaled

# اتصال بدون نیاز به تعامل با مرورگر
sudo tailscale up \
    --authkey="$AUTH_KEY" \
    --hostname="$HOSTNAME" \
    --ssh \
    --reset

echo "==> Connected successfully!"
tailscale ip -4
```

با این کار برای دستگاه‌های بعدی فقط کافیست بزنید:
```bash
bash setup_tailscale.sh techno-5
```

</div>
