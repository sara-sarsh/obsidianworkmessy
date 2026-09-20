python -m py_compile gui_decentralized.py
11) تست مرحله‌ای
تست 1: syntax
اول فقط این را بزن:

bash
python -m py_compile gui_decentralized.py
اگر error نداد، syntax اوکی است.

تست 2: اتصال socket
سرور را بالا بیاور.

یک client ساده داشته باش:


نمایش کد


<script src="https://cdn.socket.io/4.7.2/socket.io.min.js"></script>
<script>
const socket = io("http://localhost:5000");

socket.on("connect", () => console.log("connected"));

[
  "update",
  "nozzle_status",
  "config",
  "net_status",
  "config_version",
  "config_serial",
  "config_location",
  "lan_status",
  "internet_status",
  "lpc_status",
  "server_status",
  "log"
].forEach(evt => {
  socket.on(evt, data => console.log(evt, data));
});
</script>
تست 3: fallback connect
وقتی صفحه را refresh می‌کنی، باید event های cache شده دوباره بیایند.

تست 4: sermon namespace
اگر /sermon namespace داری، برای همان namespace هم یک client جدا تست کن.




برای اجرای **تست کلاینت HTML** لازم نیست داخل پروژه اصلی حتماً ادغامش کنی. ساده‌ترین راه این است:

## 1) فایل HTML را کجا ذخیره کنم؟
یک فایل جدید بساز با اسم مثلاً:

```text
test_client.html
```

و آن را هر جا خواستی ذخیره کن؛ مثلاً کنار پروژه:

```text
project/
├── gui_decentralized.py
├── index4_offline.html
└── test_client.html
```

یا حتی روی Desktop.  
برای تست ساده، محل ذخیره خیلی مهم نیست.

---

## 2) چطور اجراش کنم؟
دو حالت داری:

### حالت ساده
روی فایل `test_client.html` دوبار کلیک کن تا در مرورگر باز شود.

### حالت بهتر
با یک HTTP server محلی بازش کن. مثلاً در همان پوشه:

```bash
python -m http.server 5500
```

بعد در مرورگر برو به:

```text
http://localhost:5500/test_client.html
```

این روش بهتر است چون بعضی مرورگرها روی فایل مستقیم (`file://`) محدودیت دارند.

---

## 3) در بک‌اند کجا را باید عوض کنم؟
معمولاً فقط **آدرس و پورت اتصال** را باید با بک‌اند یکی کنی.

در فایل HTML این خط را پیدا کن:

```javascript
const socket = io("http://localhost:8082");
```

### اگر بک‌اندت روی همین پورت اجرا می‌شود
هیچ چیز لازم نیست عوض کنی.

### اگر بک‌اند روی پورت دیگری اجرا می‌شود
مثلاً اگر Flask/SocketIO روی `5000` است، این را بکن:

```javascript
const socket = io("http://localhost:5000");
```

---

## 4) از کجا بفهمم بک‌اند روی چه پورتی اجرا می‌شود؟
باید در `gui_decentralized.py` جایی شبیه این باشد:

```python
socketio.run(app, host="0.0.0.0", port=8082)
```

یا:

```python
app.run(port=8082)
```

اگر پورت اینجا `8082` نیست، همان عدد را در HTML بگذار.

---

## 5) آیا لازم است بک‌اند route جدیدی داشته باشد؟
نه، برای این تست معمولاً **هیچ route جدیدی لازم نیست**.  
فقط باید:

- سرور روشن باشد
- Socket.IO فعال باشد
- namespace پیش‌فرض `/` یا `/sermon` تعریف شده باشد

---

## 6) اگر از مرورگر وصل نشد، چه چیزی را در بک‌اند عوض کنم؟
مهم‌ترین مورد معمولاً **CORS** است.  
اگر خطای connection یا CORS گرفتی، موقع ساخت SocketIO این را چک کن:

```python
socketio = SocketIO(app, cors_allowed_origins="*")
```

اگر الان محدود است، موقتاً برای تست این را `*` بگذار.

---

## 7) برای namespace `/sermon` چیزی باید عوض کنم؟
فقط اگر می‌خواهی آن را هم تست کنی، در HTML یا console باید این را بزنی:

```javascript
const sermonSocket = io("http://localhost:8082/sermon");
```

و در بک‌اند هم باید namespace `/sermon` واقعاً ثبت شده باشد.

---

## 8) خلاصه خیلی عملی
### فایل را اینجا بگذار:
کنار `gui_decentralized.py` یا هر پوشه‌ای که راحتی.

### در بک‌اند فقط این‌ها را چک کن:
- پورت سرور
- `cors_allowed_origins="*"`

### در HTML فقط این خط را با بک‌اند یکی کن:
```javascript
const socket = io("http://localhost:PORT");
```

---

اگر بخواهی، در پیام بعدی می‌توانم **با نگاه به خود `gui_decentralized.py` دقیق بگویم کدام خط بک‌اند را باید تغییر بدهی** و حتی نسخه آماده‌ی `test_client.html` را با پورت درست برایت تنظیم کنم.