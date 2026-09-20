### Enums
## آموزش ساختارها و دیتا تایپ‌ها:

### ۱. **Enum (Enumerations)**

```python
from enum import Enum

class ErrorWarningCode(Enum):
    ERROR_NOT_CONNECTED = 0      # عضو Enum با مقدار 0
    ERROR_CAR = 1               # عضو Enum با مقدار 1
    ERROR_VCP_LEVEL_TIMEOUT = 2  # عضو Enum با مقدار 2
    ERROR_RELAY = 3             # عضو Enum با مقدار 3
    ERROR_EMC = 4               # عضو Enum با مقدار 4
```

**ویژگی‌های Enum:**
```python
# ایجاد نمونه از Enum
error = ErrorWarningCode.ERROR_CAR

# دسترسی به نام (رشته)
print(error.name)        # خروجی: "ERROR_CAR" (از نوع str)

# دسترسی به مقدار (عدد)
print(error.value)       # خروجی: 1 (از نوع int)

# بررسی نوع
print(type(error))       # خروجی: <enum 'ErrorWarningCode'>
print(isinstance(error, ErrorWarningCode))  # خروجی: True

# مقایسه
if error == ErrorWarningCode.ERROR_CAR:
    print("This is a car error")

# تبدیل عدد به Enum
error_from_number = ErrorWarningCode(2)  # معادل ERROR_VCP_LEVEL_TIMEOUT
```

### ۲. **نحوه تبدیل value از LPC به Enum:**

```python
# LPC می‌فرستد: 7
value = 7  # از LPC دریافت شده

if value < 5:
    # وضعیت عادی
    status = LPC_STATUS_MAP.get(value)
else:
    # وضعیت خطا
    error_code = value - 5  # 7 - 5 = 2
    status = ErrorWarningCode(error_code)  # ERROR_VCP_LEVEL_TIMEOUT
```

### ۳. **دیتا تایپ‌های مختلف در Python:**

```python
# 1. int (عدد صحیح)
error_value = 2
print(type(error_value))  # <class 'int'>

# 2. str (رشته)
error_name = "ERROR_CAR"
print(type(error_name))   # <class 'str'>

# 3. Enum (نوع خاص)
error_enum = ErrorWarningCode.ERROR_CAR
print(type(error_enum))   # <enum 'ErrorWarningCode'>

# 4. dict (دیکشنری)
error_dict = {
    "code": 1,
    "name": "ERROR_CAR"
}
print(type(error_dict))   # <class 'dict'>

# 5. bool (بولین)
is_error = True
print(type(is_error))     # <class 'bool'>
```

### ۴. **تبدیل بین دیتا تایپ‌ها:**

```python
# Enum → int
error = ErrorWarningCode.ERROR_CAR
error_code = error.value  # 1

# Enum → str
error_name = error.name   # "ERROR_CAR"

# int → Enum
error_from_int = ErrorWarningCode(1)  # ERROR_CAR

# str → Enum (اگر نام را داشته باشیم)
error_from_str = ErrorWarningCode["ERROR_CAR"]  # ERROR_CAR

# int → str
str_from_int = str(1)  # "1"

# str → int
int_from_str = int("1")  # 1
```

### ۵. **بررسی نوع داده:**

```python
def handle_status(status):
    # بررسی اینکه آیا status از نوع ErrorWarningCode است
    if isinstance(status, ErrorWarningCode):
        print(f"This is an error: {status.name}")
        
    # بررسی اینکه آیا status از نوع ChargeStatus است
    elif isinstance(status, ChargeStatus):
        print(f"This is a charge status: {status}")
        
    # بررسی نوع‌های دیگر
    elif isinstance(status, int):
        print(f"This is a number: {status}")
        
    elif isinstance(status, str):
        print(f"This is a string: {status}")
```

### ۶. **مثال کامل برای کد شما:**

```python
def _handle_charge_status_transition(self, gun_key: str, raw_status: str) -> None:
    # raw_status = "7" (از LPC دریافت شده)
    
    try:
        status_code = str(raw_status).strip()
        current_status = ChargeStatus.parse_charge_status(status_code)
    except:
        current_status = ChargeStatus.UNKNOWN
    
    # چک کردن نوع داده
    if isinstance(current_status, ErrorWarningCode):
        # اینجا current_status از نوع ErrorWarningCode است
        # یعنی یک خطا دریافت شده
        
        error_code = current_status.value  # 2 (از نوع int)
        error_name = current_status.name   # "ERROR_VCP_LEVEL_TIMEOUT" (از نوع str)
        
        # ارسال به MQTT
        error_payload = {
            "code": error_code,      # int
            "name": error_name,      # str
            "raw": raw_status,       # str
            "gun": gun_key           # str
        }
        
        # تبدیل دیکشنری به JSON string برای ارسال
        import json
        self.broker.publish(f"error/{gun_key}", json.dumps(error_payload))
        
        self._last_charge_status[gun_key] = current_status
        return
    
    # اگر اینجا رسیدیم، یعنی خطا نیست و وضعیت عادی است
    if isinstance(current_status, ChargeStatus):
        # پردازش وضعیت عادی
        pass
```

## خلاصه:

| نوع داده | مثال | کاربرد |
|---------|------|--------|
| `int` | `2` | مقدار عددی خطا |
| `str` | `"ERROR_CAR"` | نام خطا برای نمایش |
| `Enum` | `ErrorWarningCode.ERROR_CAR` | نوع داده خاص برای خطاها |
| `dict` | `{"code": 1, "name": "ERROR_CAR"}` | برای ارسال داده‌های ساختاریافته |
| `bool` | `True/False` | برای شرط‌ها و پرچم‌ها |

**قانون طلایی:** 
- **ذخیره و پردازش** با Enum انجام بده (چون type-safe و قابل اعتمادتر است)
- **ارسال به MQTT** با int یا JSON انجام بده (چون راحت‌تر parse می‌شود)
- **نمایش به کاربر** با str انجام بده (چون خوانا است)