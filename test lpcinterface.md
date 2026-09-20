خلاصه وضعیت: unit test قبلاً نوشته شد و 14/14 پاس کرد. الان فقط دستورها را مرحله‌به‌مرحله می‌دهم.

---

## مرحله ۱ — Unit Test (همین الان آماده است)

فقط این یک دستور را در ترمینال بزن:

```bash
cd "/Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evck160c- decentralized"

.venv/bin/python -m pytest opt/evck/lpc_interface/test_lpc_interface.py -v
```

باید ببینی `14 passed` ✅

---

## مرحله ۲ — Integration Test (با پورت مجازی روی Mac)

روی Mac به جای `socat` می‌توانیم از همان `pty` که در `mock_lpc_interface.py` هست استفاده کنیم — نیازی به نصب چیز اضافه نیست.

### قدم ۱ — نصب mosquitto (فقط یک بار)

```bash
brew install mosquitto
```

### قدم ۲ — اجرای mosquitto broker

ترمینال جدید باز کن و بزن:

```bash
/opt/homebrew/sbin/mosquitto -v
```

بگذار باز بماند.

### قدم ۳ — نصب پکیج‌های python در venv (اگر نصب نشده)

```bash
cd "/Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evck160c- decentralized"

.venv/bin/python -m pip install pyserial paho-mqtt
```

### قدم ۴ — اجرای Mock (شبیه‌ساز LPC)

ترمینال جدید:

```bash
cd "/Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evck160c- decentralized"

.venv/bin/python opt/evck/lpc_interface/mock_lpc_interface.py
```

خروجی می‌دهد:
```
Mock serial slave at /dev/ttys003
```
**این آدرس `/dev/ttys003` را نگه دار** (عدد آن روی هر اجرا فرق دارد).

### قدم ۵ — subscribe روی همه تاپیک‌ها

ترمینال جدید:

```bash
mosquitto_sub -h localhost -t "#" -v
```

بگذار باز بماند.

### قدم ۶ — ارسال یک فریم تستی به پورت مجازی

ترمینال جدید — **آدرس پورت را جایگزین کن** (`/dev/ttys003` → آنچه در قدم ۴ دیدی):

```bash
printf '*1&1&2&3&4&5&6&7&8&9&10&11&12&13&14&15&16&17&18&19#' > /dev/ttys003
```

### قدم ۷ — نتیجه را در ترمینال قدم ۵ ببین

باید پیام‌هایی شبیه این ببینی:

```
gun_id/gun_1 1
charge_status/gun_1 2
voltage/gun_1 3
...
sc/gun_1 19
```

---

> **نکته برای Mac:** اگر `mosquitto_sub` در PATH نبود، از این مسیر استفاده کن:
> ```bash
> /opt/homebrew/bin/mosquitto_sub -h localhost -t "#" -v
> ```