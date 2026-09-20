

شما به‌عنوان یک code-review/debug agent باید پروژه را با تمرکز بر **معماری فعلی موجود** بررسی کنید، نه با هدف refactor بزرگ.

## 1) هدف اصلی بررسی

این پروژه یک سیستم **event-driven و decentralized** است که ارتباط بین سرویس‌ها از طریق **MQTT** انجام می‌شود.

### نقش سرویس‌ها در وضعیت فعلی:
- `lpc_interface.py`  
  منبع داده‌های خام سخت‌افزاری است و state فیزیکی/لحظه‌ای شارژ را publish می‌کند.

- `opi_hardware.py`  
  مسئول بخشی از سخت‌افزار محلی، وضعیت سیستم و برخی تنظیمات است.

- `gui_decentralized.py`  
  aggregator سمت UI است و stateهای دریافتی از چند سرویس را جمع می‌کند، مشتق‌سازی می‌کند، و برای نمایش استفاده می‌کند.

- `evcktypes.py`  
  شامل تعریف `GunState` و تایپ‌های مرتبط است.  
  `GunState` ترکیبی از موارد زیر را در خود دارد:
  - raw state دریافتی
  - derived state مخصوص UI
  - برخی فیلدهای session
  - decoration/state برای نمایش
  - مقادیر محاسبه‌شده

## 2) محدودیت‌های مهم

در این بررسی **نباید** پیشنهاد refactor معماری بزرگ بدهید مگر در حد observation.  
محدودیت‌ها و ترجیحات فعلی پروژه:

1. **معماری فعلی باید حفظ شود.**
2. **فعلاً session manager مستقل نمی‌خواهیم.**
3. **اسم topicهای فعلی MQTT نباید تغییر کند.**
4. **مالکیت topicها در وضع فعلی برای ما واضح است و نمی‌خواهیم topic naming را دست بزنیم.**
5. تمرکز اصلی روی **بررسی باگ‌ها و ریسک‌های احتمالی در `gui_decentralized.py`** است.
6. اگر لازم شد session tracking بررسی شود، فقط در چارچوب فعلی پروژه و بدون اضافه کردن سرویس جدید بررسی شود.

---

## 3) سوال کلیدی که باید به آن پاسخ دهید

با فرض اینکه:
- session manager نداریم
- `gui_decentralized.py` همان aggregator فعلی باقی می‌ماند
- `GunState` در همان مدل فعلی استفاده می‌شود

بررسی کنید:

### A) آیا نسخه فعلی پروژه بدون session management مستقل دچار باگ یا ریسک پنهان هست؟
به‌خصوص:
- آیا session-like behavior به‌شکل ضمنی وجود دارد ولی ناقص است؟
- آیا reset نشدن برخی فیلدها بین دو شارژ ممکن است باعث state اشتباه شود؟
- آیا transitionها درست مدیریت می‌شوند یا امکان duplicate start / missed finish وجود دارد؟
- آیا restart/reconnect می‌تواند state را inconsistent کند؟

### B) آیا `gui_decentralized.py` از نظر concurrency / race condition / partial update مشکل دارد؟
به‌خصوص بررسی کنید:
- آیا چند thread / callback / timer همزمان `gun_states` یا objectهای داخل آن را mutate می‌کنند؟
- آیا هنگام emit/update ممکن است state ناقص یا نیمه‌به‌روز به UI برسد؟
- آیا روی shared mutable state lock یا snapshot مناسب وجود دارد یا خیر؟

### C) اگر session manager نداشته باشیم، حداقل چه رفتار یا guardهایی باید در GUI وجود داشته باشد تا باگ نگیریم؟
بدون تغییر معماری، بررسی کنید:
- آیا transition-based handling لازم است؟
- آیا previous state برای charge status نگهداری می‌شود؟
- آیا بعضی فیلدهای `GunState` باید start/finish/reset مشخص داشته باشند؟
- آیا final values باید بعد از پایان session برای UI نگه داشته شوند یا فوراً پاک می‌شوند؟

---

## 4) تمرکز فنی شما باید روی این موارد باشد

### 4.1 بررسی منطق transition
دقیق بررسی کنید که آیا کد صرفاً based on current value کار می‌کند یا based on transition.

مواردی که باید بررسی شوند:
- شروع شارژ: آیا از `prev -> curr` تشخیص داده می‌شود یا فقط با `curr == CHARGING`؟
- پایان شارژ: آیا transition خروج از `CHARGING` درست handle می‌شود؟
- fault / unplug / reconnect / stop command چه اثری روی state می‌گذارند؟
- آیا eventهای تکراری باعث duplicate state change می‌شوند؟

**ریسک مهم:**  
اگر previous state tracking درست نباشد، باگ‌های زیر محتمل‌اند:
- چند بار start شدن یک session
- finish نشدن session
- باقی ماندن duration/energy/cost از session قبلی
- تغییر phase اشتباه در UI

---

### 4.2 بررسی reset و lifecycle فیلدهای `GunState`
برای هر فیلد مهم `GunState` مشخص کنید:
- owner آن کیست؟
- در چه eventی set می‌شود؟
- در چه eventی update می‌شود؟
- در چه eventی reset می‌شود؟

به‌خصوص برای فیلدهای زیر یا مشابه آن‌ها (اگر وجود دارند):
- `session_id`
- `txn_id`
- `id_tag`
- `t_start` / `started_at`
- `duration_s`
- `meter_start_wh`
- `energy_kwh` / `energy_wh`
- `cost`
- `final_cost`
- `charging_complete`
- `phase`
- `message_type`
- `last_message`
- هر فیلدی که از session قبلی ممکن است نشت کند

**ریسک مهم:**  
Ghost state / stale state  
یعنی فیلدهایی از session قبلی در session بعدی باقی بمانند.

---

### 4.3 بررسی race condition و data consistency
بررسی کنید که در `gui_decentralized.py` این منابع همزمان state را تغییر می‌دهند یا نه:
- MQTT callbackها
- timer loopها
- background updaterها
- websocket/socketio emit logic
- periodic calculations

اگر shared mutable state وجود دارد، مشخص کنید:
- آیا lock هست؟
- آیا lock کافی است؟
- آیا deep copy / snapshot قبل از emit وجود دارد؟
- آیا ممکن است یک field update شود ولی field وابسته‌اش هنوز update نشده باشد و UI حالت impossible ببیند؟

**ریسک‌های مورد انتظار:**
- partial state emission
- inconsistent derived values
- duration یا cost روی session inactive
- emit شدن object در حین mutation

---

### 4.4 بررسی reconnect / retained / repeated delivery behavior
با توجه به ماهیت MQTT بررسی کنید:
- اگر connection قطع و وصل شود، callbackها idempotent هستند یا نه؟
- اگر retained message دوباره برسد، GUI دوباره state transition اجرا می‌کند یا نه؟
- آیا startup state loading با runtime event processing قاطی می‌شود؟
- آیا duplicate delivery می‌تواند باعث start/finish تکراری یا reset ناخواسته شود؟

---

### 4.5 بررسی derived UI logic
چون `gui_decentralized.py` aggregator است، بررسی کنید:
- آیا derived UI phase از چند source محاسبه می‌شود؟
- آیا precedence مشخص بین eventها وجود دارد؟
- آیا message/state یکی دیگری را overwrite می‌کند؟
- آیا state-machine ضمنی ولی ناقص وجود دارد؟

**ریسک مهم:**  
UI phase از raw state عقب بماند یا جلو بیفتد و حالت‌های غیرممکن نشان دهد.

---

## 5) درباره session management در وضعیت فعلی

در این پروژه فعلاً **session manager مستقل نمی‌خواهیم**.  
اما لازم است بررسی کنید که:

### آیا نبود session manager مستقل ذاتاً مشکل‌ساز است؟
پاسخ باید دقیق و عملی باشد، نه صرفاً معماری‌محور.

مواردی که باید ارزیابی شوند:
- آیا GUI می‌تواند بدون session manager هم state درست نگه دارد؟
- اگر بله، تحت چه شروطی؟
- اگر نه، دقیقاً چه failure modeهایی دارد؟
- آیا مشکل فقط در persistence بین restartهاست یا حتی در runtime عادی هم bug داریم؟
- آیا `GunState` فعلی برای session-like tracking کافی است یا نه؟

**مهم:**  
هدف این بخش این نیست که session manager پیشنهاد دهید؛  
هدف این است که بگویید **در معماری فعلی چه باگ‌های واقعی یا بالقوه‌ای وجود دارد**.

---

## 6) انتظار از خروجی شما

خروجی شما باید به‌صورت یک **گزارش فنی ساختارمند** باشد و شامل این بخش‌ها باشد:

### بخش 1: جمع‌بندی معماری فعلی
خیلی کوتاه بگویید درک شما از ownership و نقش فایل‌ها چیست.

### بخش 2: باگ‌های قطعی یا محتمل
موارد را اولویت‌بندی کنید:
- Critical
- High
- Medium
- Low

برای هر مورد این قالب را رعایت کنید:
1. عنوان مشکل
2. محل احتمالی در کد
3. توضیح فنی
4. سناریوی بازتولید
5. اثر روی سیستم/UI
6. پیشنهاد اصلاح حداقلی و سازگار با معماری فعلی

### بخش 3: بررسی خاص `gui_decentralized.py`
به‌صورت خاص روی:
- state transitions
- reset lifecycle
- race conditions
- emit consistency
- reconnect behavior

### بخش 4: پاسخ صریح به این سوال
**اگر session manager نداشته باشیم، آیا نسخه فعلی مشکل جدی دارد؟**  
پاسخ باید یکی از این حالت‌ها باشد:
- "فعلاً قابل قبول است، به شرط رفع این چند مورد"
- "ریسک متوسط دارد"
- "ریسک بالا دارد"
- "در runtime عادی هم مستعد inconsistency است"

و دقیقاً توضیح دهید چرا.

### بخش 5: پیشنهادهای حداقلی
فقط پیشنهادهای **small, local, non-architectural** بدهید.  
مثلاً:
- اضافه کردن previous-state tracking
- قاعده reset واضح برای چند field
- lock یا snapshot هنگام emit
- idempotency guard در callbackها
- نگه داشتن final values بعد از پایان charging

نه پیشنهادهایی مثل:
- rewrite کامل
- event store
- session microservice
- تغییر naming topicها
- redesign معماری
- تغییر در کلاس ها یا دیتاکلاس موجود در common- evcktypes
---

## 7) مواردی که مخصوصاً باید به آن‌ها حساس باشید

در بررسی خود فعالانه دنبال این failure modeها بگردید:

1. **Session ghosting**  
   فیلدهای session قبلی در session بعدی باقی بمانند.

2. **Missed finish**  
   از `CHARGING` خارج شده ولی finalize/reset درست انجام نشده.

3. **Duplicate start**  
   به‌خاطر event تکراری یا retained message چند بار initialization انجام شود.

4. **Partial state emit**  
   UI در یک لحظه state نیمه‌کامل یا impossible ببیند.

5. **Race on GunState**  
   چند منبع همزمان object مشترک را mutate کنند.

6. **Derived-state drift**  
   phase/message/cost/duration با raw charge status سازگار نباشند.

7. **Immediate clear after finish**  
   مقادیر نهایی session آن‌قدر سریع پاک شوند که UI نتواند آن‌ها را نمایش دهد.

8. **Improper reconnect handling**  
   بعد از reconnect، state دوباره‌سازی یا overwrite اشتباه شود.

---

## 8) نوع پاسخ مورد انتظار

پاسخ شما باید:
- دقیق
- عملی
- کد-محور
- مبتنی بر فایل‌های واقعی پروژه
- بدون refactor بزرگ
- سازگار با معماری فعلی

باشد.

اگر در کد مورد مشکوکی دیدید ولی قطعی نبود، آن را با برچسب:
- "Confirmed issue"
- "Likely issue"
- "Needs verification"

مشخص کنید.

اگر لازم بود، patchهای کوچک پیشنهادی هم ارائه دهید، اما فقط در حد minimal fix.

---

# نسخه کوتاه‌تر برای ارسال سریع به Agent

اگر خواستی نسخه کوتاه‌تر هم داشته باشی، این را بفرست:

---

این پروژه event-driven و decentralized است و ارتباط سرویس‌ها از طریق MQTT انجام می‌شود.  
فعلاً **نمی‌خواهیم معماری را عوض کنیم**، **session manager مستقل نمی‌خواهیم** و **اسم topicها هم نباید تغییر کند**. ownership فعلی topicها برای ما روشن و قابل قبول است.

تمرکز شما فقط روی **بررسی باگ‌ها و ریسک‌های احتمالی در `gui_decentralized.py`** و تعامل آن با `GunState` در `evcktypes.py` باشد.

نقش فایل‌ها:
- `lpc_interface.py`: منبع داده خام سخت‌افزار
- `opi_hardware.py`: سخت‌افزار محلی و وضعیت سیستم
- `gui_decentralized.py`: aggregator و مشتق‌کننده state برای UI
- `evcktypes.py`: تعریف `GunState` و تایپ‌ها

لطفاً بررسی کنید:

1. آیا نسخه فعلی بدون session manager مستقل، bug یا risk پنهان دارد؟
2. آیا `gui_decentralized.py` از نظر transition logic، reset lifecycle، race condition، partial emit، reconnect behavior مشکل دارد؟
3. آیا بعضی fieldهای `GunState` ممکن است بین دو session نشت کنند؟
4. آیا start/finish charging با previous-state tracking درست مدیریت می‌شود یا نه؟
5. آیا duplicate delivery / retained MQTT message می‌تواند باعث start/finish تکراری یا reset اشتباه شود؟
6. آیا emit به UI ممکن است وسط mutation انجام شود و state ناقص بفرستد؟

خروجی را به‌صورت گزارش structured بدهید و برای هر issue این موارد را بنویسید:
- عنوان مشکل
- severity
- محل احتمالی در کد
- توضیح فنی
- سناریوی بازتولید
- اثر روی UI/behavior
- پیشنهاد اصلاح حداقلی بدون تغییر معماری

همچنین در پایان صریح پاسخ دهید:
**اگر session manager نداشته باشیم، آیا نسخه فعلی از نظر runtime behavior قابل قبول است یا نه؟**
و اگر مشکل دارد، فقط fixهای کوچک و local پیشنهاد دهید، نه refactor معماری.
