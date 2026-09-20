
and give me manual commands for building the gui binary manually.
برای بیلد دستی و خط به خط باینری **GUI** روی OrangePi بدون نیاز به اجرای اسکریپت کلی، مراحل زیر را در ترمینال اجرا کنید:
۱. رفتن به دایرکتوری سورس و فعال‌سازی محیط مجازی
ابتدا به دایرکتوری که سورس کدهای پروژه در آن قرار دارد (مثلاً /home/orangepi/evck-build یا /opt/evck) بروید و venv را فعال کنید:

bash


cd /مسیر/پوشه/سورس/پروژه
# فعال کردن محیط مجازی (اگر venv در /opt/evck قرار دارد)
source /opt/evck/.venv/bin/activate
۲. اطمینان از نصب PyInstaller
نسخه پایدار و تست‌شده را نصب/بررسی کنید:

bash


pip install pyinstaller==6.11.1
۳. اجرای دستور بیلد PyInstaller برای GUI
این دستور را دقیقاً با فلگ‌های زیر اجرا کنید (دستور در یک خط است):

bash


pyinstaller --clean --noconfirm \
    --name evck-gui \
    --onedir \
    --distpath ./dist/binaries \
    --workpath ./dist/build \
    --paths . \
    --collect-submodules common \
    --collect-submodules engineio \
    --collect-submodules socketio \
    --hidden-import engineio.async_drivers.threading \
    gui_app/gui_decentralized.py
توضیح کوتاه فلگ‌ها:

--onedir: برنامه را به صورت یک پوشه کامل باینری و کتابخانه‌های کنار آن کامپایل می‌کند (استارت سریع‌تر و بدون فشار به /tmp).
--distpath ./dist/binaries: محل ذخیره باینری نهایی (./dist/binaries/evck-gui/evck-gui).
--collect-submodules common: تمام پکیج‌های مشترک (config_manager, logger, messaging و...) را جمع‌آوری می‌کند.
--collect-submodules engineio / socketio و --hidden-import ...: پکیج‌ها و درایور تردهای SocketIO را کامل اضافه می‌کند تا خطای وب‌سوکت رخ ندهد.
۴. بررسی فایل باینری تولید شده
پس از اتمام بیلد، فایل اجرایی را بررسی کنید:

bash


ls -lh ./dist/binaries/evck-gui/evck-gui
۵. تست مستقیم اجرای باینری ساخته‌شده
پیش از جابجایی، سرویس قبلی را متوقف و فایل تولیدی را اجرا کنید:

bash


sudo systemctl stop evck-gui
sudo -E ./dist/binaries/evck-gui/evck-gui
با دیدن پیام اجرای سرور، با Ctrl + C آن را متوقف کنید.

10:55 AM
(.venv) orangepi@orangepizero2w:~/evck-build/opt/evck$ ck$ pip install pyinstall
er==6.11.1
ERROR: Could not find a version that satisfies the requirement puinsyinstaller==6.11.1 (from versions: none)
ERROR: No matching distribution found for puinsyinstaller==6.11.1
(.venv) orangepi@orangepizero2w:~/evck-build/opt/evck$
به متن خطا دقت کنید: نام پکیج به صورت اشتباه تایپ شده است: ```text puinsyinstaller==6.11.1 ``` (احتمالاً هنگام پیست کردن یا تایپ در ترمینال، کلمه `puinsyinstaller` ارسال شده است).
دستور صحیح را با دقت کپی و اجرا کنید:

bash


pip install pyinstaller==6.11.1
یا اگر نسخه خاصی مد نظرتان نیست یا می‌خواهید جدیدترین نسخه هماهنگ با معماری دستگاه نصب شود:

bash


pip install pyinstaller
11:51 AM
الان بیلد از این آدرس
/home/orangepi/evck-build/opt/evck
ساخته شده؟
چطوری تست کنم؟


orangepi@orangepizero2w:~/evck-build/opt/evck$ pwd
/home/orangepi/evck-build/opt/evck
orangepi@orangepizero2w:~/evck-build/opt/evck$ ls
build_all_binaries.sh  gui_app         not_needed          start-kiosk.sh
common                 install.sh      ocpp_client         tests
compile_binaries.sh    logs            opi_hardware        tools
deploy_binaries.sh     lpc_interface   __pycache__         updater
docker-compose.yml     mosquitto.conf  start-kiosk-old.sh  VERSION
orangepi@orangepizero2w:~/evck-build/opt/evck$ source /opt/evck/.venv/bin/activate
(.venv) orangepi@orangepizero2w:~/evck-build/opt/evck$ ls
build_all_binaries.sh  gui_app         not_needed          start-kiosk.sh
common                 install.sh      ocpp_client         tests
compile_binaries.sh    logs            opi_hardware        tools
deploy_binaries.sh     lpc_interface   __pycache__         updater
docker-compose.yml     mosquitto.conf  start-kiosk-old.sh  VERSION
(.venv) orangepi@orangepizero2w:~/evck-build/opt/evck$ pip install puinstaller==6.11.1
^CERROR: Operation cancelled by user
(.venv) orangepi@orangepizero2w:~/evck-build/opt/evck$ ck$ pip install pyinstall
er==6.11.1
ERROR: Could not find a version that satisfies the requirement puinsyinstaller==6.11.1 (from versions: none)
ERROR: No matching distribution found for puinsyinstaller==6.11.1
(.venv) orangepi@orangepizero2w:~/evck-build/opt/evck$ (.venv) orangepi@orangepizero2w:~/evck-build/opt/evck$ ck$ pip install pyinstall
er==6.11.1
ERROR: Could not find a version that satisfies the requirement puinsyinstaller==6.11.1 (from versions: none)
ERROR: No matching distribution found for puinsyinstaller==6.11.1
(.venv) orangepi@orangepizero2w:~/evck-build/opt/evck$^C
(.venv) orangepi@orangepizero2w:~/evck-build/opt/evck$ pip install pyinstaller==6.11.1
Collecting pyinstaller==6.11.1
  Downloading pyinstaller-6.11.1-py3-none-manylinux2014_aarch64.whl (711 kB)
     qqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqq 711.7/711.7 KB 1.0 MB/s eta 0:00:00
Requirement already satisfied: setuptools>=42.0.0 in /opt/evck/.venv/lib/python3.10/site-packages (from pyinstaller==6.11.1) (59.6.0)
Collecting pyinstaller-hooks-contrib>=2024.9
  Downloading pyinstaller_hooks_contrib-2026.7-py3-none-any.whl (459 kB)
     qqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqq 459.4/459.4 KB 1.5 MB/s eta 0:00:00
Collecting altgraph
  Downloading altgraph-0.17.5-py2.py3-none-any.whl (21 kB)
Requirement already satisfied: packaging>=22.0 in /opt/evck/.venv/lib/python3.10/site-packages (from pyinstaller==6.11.1) (26.0)
Installing collected packages: altgraph, pyinstaller-hooks-contrib, pyinstaller
Successfully installed altgraph-0.17.5 pyinstaller-6.11.1 pyinstaller-hooks-contrib-2026.7
(.venv) orangepi@orangepizero2w:~/evck-build/opt/evck$ pyinstaller --clean --noconfirm \
    --name evck-gui \
    --onedir \
    --distpath ./dist/binaries \
    --workpath ./dist/build \
    --paths . \
    --collect-submodules common \
    --collect-submodules engineio \
    --collect-submodules socketio \
    --hidden-import engineio.async_drivers.threading \
    gui_app/gui_decentralized.py
626 INFO: PyInstaller: 6.11.1, contrib hooks: 2026.7
626 INFO: Python: 3.10.12
631 INFO: Platform: Linux-6.1.31-sun50iw9-aarch64-with-glibc2.35
631 INFO: Python environment: /opt/evck/.venv
633 INFO: wrote /home/orangepi/evck-build/opt/evck/evck-gui.spec
638 INFO: Removing temporary files and cleaning cache in /home/orangepi/.cache/pyinstaller
3481 INFO: Module search paths (PYTHONPATH):
['/usr/lib/python310.zip',
 '/usr/lib/python3.10',
 '/usr/lib/python3.10/lib-dynload',
 '/opt/evck/.venv/lib/python3.10/site-packages',
 '/home/orangepi/evck-build/opt/evck',
 '/home/orangepi/evck-build/opt/evck']
4264 INFO: checking Analysis
4265 INFO: Building Analysis because Analysis-00.toc is non existent
4265 INFO: Running Analysis Analysis-00.toc
4265 INFO: Target bytecode optimization level: 0
4265 INFO: Initializing module dependency graph...
4268 INFO: Initializing module graph hook caches...
4334 INFO: Analyzing base_library.zip ...
5931 INFO: Processing standard module hook 'hook-heapq.py' from '/opt/evck/.venv/lib/python3.10/site-packages/PyInstaller/hooks'
7350 INFO: Processing standard module hook 'hook-encodings.py' from '/opt/evck/.venv/lib/python3.10/site-packages/PyInstaller/hooks'
15510 INFO: Processing standard module hook 'hook-pickle.py' from '/opt/evck/.venv/lib/python3.10/site-packages/PyInstaller/hooks'
23776 INFO: Caching module dependency graph...
24420 INFO: Looking for Python shared library...
24493 INFO: Using Python shared library: /lib/aarch64-linux-gnu/libpython3.10.so
24493 INFO: Analyzing /home/orangepi/evck-build/opt/evck/gui_app/gui_decentralized.py
26800 INFO: Processing pre-safe-import-module hook 'hook-typing_extensions.py' from '/opt/evck/.venv/lib/python3.10/site-packages/PyInstaller/hooks/pre_safe_import_module'
26802 INFO: SetuptoolsInfo: initializing cached setuptools info...
31710 INFO: Processing standard module hook 'hook-platform.py' from '/opt/evck/.venv/lib/python3.10/site-packages/PyInstaller/hooks'
31908 INFO: Processing standard module hook 'hook-multiprocessing.util.py' from '/opt/evck/.venv/lib/python3.10/site-packages/PyInstaller/hooks'
32322 INFO: Processing standard module hook 'hook-xml.py' from '/opt/evck/.venv/lib/python3.10/site-packages/PyInstaller/hooks'
34486 INFO: Processing standard module hook 'hook-sysconfig.py' from '/opt/evck/.venv/lib/python3.10/site-packages/PyInstaller/hooks'
36780 INFO: Processing standard module hook 'hook-difflib.py' from '/opt/evck/.venv/lib/python3.10/site-packages/PyInstaller/hooks'
38051 INFO: Processing standard module hook 'hook-jinja2.py' from '/opt/evck/.venv/lib/python3.10/site-packages/_pyinstaller_hooks_contrib/stdhooks'
43805 INFO: Processing pre-safe-import-module hook 'hook-importlib_metadata.py' from '/opt/evck/.venv/lib/python3.10/site-packages/PyInstaller/hooks/pre_safe_import_module'
44392 INFO: Processing standard module hook 'hook-urllib3.py' from '/opt/evck/.venv/lib/python3.10/site-packages/_pyinstaller_hooks_contrib/stdhooks'
48448 INFO: Processing standard module hook 'hook-charset_normalizer.py' from '/opt/evck/.venv/lib/python3.10/site-packages/_pyinstaller_hooks_contrib/stdhooks'
49131 INFO: Processing standard module hook 'hook-certifi.py' from '/opt/evck/.venv/lib/python3.10/site-packages/_pyinstaller_hooks_contrib/stdhooks'
50971 INFO: Processing standard module hook 'hook-gevent.py' from '/opt/evck/.venv/lib/python3.10/site-packages/PyInstaller/hooks'
53668 INFO: Processing standard module hook 'hook-zope.interface.py' from '/opt/evck/.venv/lib/python3.10/site-packages/PyInstaller/hooks'
59350 INFO: Processing pre-safe-import-module hook 'hook-distutils.py' from '/opt/evck/.venv/lib/python3.10/site-packages/PyInstaller/hooks/pre_safe_import_module'
59351 INFO: Processing pre-find-module-path hook 'hook-distutils.py' from '/opt/evck/.venv/lib/python3.10/site-packages/PyInstaller/hooks/pre_find_module_path'
59357 INFO: Processing standard module hook 'hook-distutils.py' from '/opt/evck/.venv/lib/python3.10/site-packages/PyInstaller/hooks'
59430 INFO: Processing standard module hook 'hook-distutils.util.py' from '/opt/evck/.venv/lib/python3.10/site-packages/PyInstaller/hooks'
64576 INFO: Analyzing hidden import 'engineio.async_drivers.threading'
64590 INFO: Analyzing hidden import 'engineio.async_drivers.aiohttp'
64612 INFO: Analyzing hidden import 'engineio.async_drivers.eventlet'
64623 INFO: Analyzing hidden import 'engineio.async_drivers.gevent'
64674 INFO: Analyzing hidden import 'engineio.async_drivers.gevent_uwsgi'
64700 INFO: Analyzing hidden import 'engineio.async_drivers.sanic'
64727 INFO: Processing module hooks (post-graph stage)...
67821 INFO: Processing standard module hook 'hook-xml.etree.cElementTree.py' from '/opt/evck/.venv/lib/python3.10/site-packages/PyInstaller/hooks'
67943 INFO: Processing pre-find-module-path hook 'hook-tkinter.py' from '/opt/evck/.venv/lib/python3.10/site-packages/PyInstaller/hooks/pre_find_module_path'
67945 INFO: TclTkInfo: initializing cached Tcl/Tk info...
68084 WARNING: tkinter installation is broken. It will be excluded from the application
76268 INFO: Performing binary vs. data reclassification (37 entries)
76371 INFO: Looking for ctypes DLLs
76752 INFO: Analyzing run-time hooks ...
76788 INFO: Including run-time hook 'pyi_rth_inspect.py' from '/opt/evck/.venv/lib/python3.10/site-packages/PyInstaller/hooks/rthooks'
76796 INFO: Including run-time hook 'pyi_rth_pkgutil.py' from '/opt/evck/.venv/lib/python3.10/site-packages/PyInstaller/hooks/rthooks'
76804 INFO: Including run-time hook 'pyi_rth_multiprocessing.py' from '/opt/evck/.venv/lib/python3.10/site-packages/PyInstaller/hooks/rthooks'
76938 INFO: Looking for dynamic libraries
78432 INFO: Warnings written to /home/orangepi/evck-build/opt/evck/dist/build/evck-gui/warn-evck-gui.txt
78880 INFO: Graph cross-reference written to /home/orangepi/evck-build/opt/evck/dist/build/evck-gui/xref-evck-gui.html
79066 INFO: checking PYZ
79066 INFO: Building PYZ because PYZ-00.toc is non existent
79067 INFO: Building PYZ (ZlibArchive) /home/orangepi/evck-build/opt/evck/dist/build/evck-gui/PYZ-00.pyz
83430 INFO: Building PYZ (ZlibArchive) /home/orangepi/evck-build/opt/evck/dist/build/evck-gui/PYZ-00.pyz completed successfully.
83575 INFO: checking PKG
83576 INFO: Building PKG because PKG-00.toc is non existent
83576 INFO: Building PKG (CArchive) evck-gui.pkg
83696 INFO: Building PKG (CArchive) evck-gui.pkg completed successfully.
83700 INFO: Bootloader /opt/evck/.venv/lib/python3.10/site-packages/PyInstaller/bootloader/Linux-64bit-arm/run
83700 INFO: checking EXE
83701 INFO: Building EXE because EXE-00.toc is non existent
83701 INFO: Building EXE from EXE-00.toc
83701 INFO: Copying bootloader EXE to /home/orangepi/evck-build/opt/evck/dist/build/evck-gui/evck-gui
83702 INFO: Appending PKG archive to custom ELF section in EXE
83962 INFO: Building EXE from EXE-00.toc completed successfully.
83975 INFO: checking COLLECT
83975 INFO: Building COLLECT because COLLECT-00.toc is non existent
83976 INFO: Building COLLECT COLLECT-00.toc
84926 INFO: Building COLLECT COLLECT-00.toc completed successfully.
(.venv) orangepi@orangepizero2w:~/evck-build/opt/evck$ ^C
(.venv) orangepi@orangepizero2w:~/evck-build/opt/evck$
تبریک می‌گویم! بیلد باینری کاملاً موفقیت‌آمیز انجام شد. طبق خروجی لاگ: ```text INFO: Building COLLECT COLLECT-00.toc completed successfully. ```
۱. فایل‌ها دقیقاً کجا ساخته شده‌اند؟
با توجه به اینکه دستور را در پوشه /home/orangepi/evck-build/opt/evck اجرا کردید و مسیر خروجی ./dist/binaries بود، فایل‌های باینری دقیقاً در این مسیر قرار دارند:

text


/home/orangepi/evck-build/opt/evck/dist/binaries/evck-gui/evck-gui
برای مشاهده فایل اجرایی و حجم آن، این دستور را در همان ترمینال بزنید:

bash


ls -lh /home/orangepi/evck-build/opt/evck/dist/binaries/evck-gui/evck-gui
۲. چطور تست کنیم؟ (قدم به قدم)
گام الف: متوقف کردن سرویس قبلی GUI
اگر سرویس قبلی در حال اجرا باشد، پورت وب (۸۰۸۲ یا پورت پیش‌فرض شما) را اشغال کرده است. اول آن را متوقف کنید:

bash


sudo systemctl stop evck-gui
گام ب: اجرای مستقیم فایل باینری
حالا مستقیم فایل کامپایل‌شده را با دسترسی روت اجرا کنید:

bash


sudo -E /home/orangepi/evck-build/opt/evck/dist/binaries/evck-gui/evck-gui
گام ج: چه چیزهایی را باید ببینید؟
در ترمینال باید لاگ‌های شروع به کار سرور Flask و SocketIO را بدون ارور قرمز ببینید (مثلاً پیامی مشابه Running on http://0.0.0.0:...).
مرورگر خود را باز کنید و آدرس IP دستگاه را با پورت کیوسک بزنید (مثلاً http://192.168.x.x:8082). صفحه UI باید لود شود.
گام د: اتمام تست
بعد از اینکه دیدید صفحه باز شد و لاگ‌ها درست آمدند، در همان ترمینال کلیدهای Ctrl + C را بزنید تا برنامه بسته شود.

نتیجه اجرای مستقیم را بفرمایید تا مراحل بعدی را ادامه دهیم.

11:55 AM
خب تا اینجا این خروجیه
sudo -E /home/orangepi/evck-build/opt/evck/dist/binaries/evck-gui/evck-gui

(.venv) orangepi@orangepizero2w:~/evck-build/opt/evck$ ls -lh /home/orangepi/evck-build/opt/evck/dist/binaries/evck-gui/evck-gui
-rwxr-xr-x 1 orangepi orangepi 4.6M Sep 10 08:24 /home/orangepi/evck-build/opt/evck/dist/binaries/evck-gui/evck-gui
(.venv) orangepi@orangepizero2w:~/evck-build/opt/evck$ ls
build_all_binaries.sh  evck-gui.spec   not_needed          tests
common                 gui_app         ocpp_client         tools
compile_binaries.sh    install.sh      opi_hardware        updater
deploy_binaries.sh     logs            __pycache__         VERSION
dist                   lpc_interface   start-kiosk-old.sh
docker-compose.yml     mosquitto.conf  start-kiosk.sh
(.venv) orangepi@orangepizero2w:~/evck-build/opt/evck$ cd /home/orangepi/evck-bu
ild/opt/evck/dist/binaries/evck-gui/evck-gui
-bash: cd: /home/orangepi/evck-build/opt/evck/dist/binaries/evck-gui/evck-gui: Not a directory
(.venv) orangepi@orangepizero2w:~/evck-build/opt/evck$ sudo systemctl stop evck-gui
[sudo] password for orangepi:
(.venv) orangepi@orangepizero2w:~/evck-build/opt/evck$ sudo -E /home/orangepi/evck-build/opt/evck/dist/binaries/evck-gui/evck-gui
08:28:58 INFO     | evck.config_manager:100 | ✅ [CONFIG] called the config_changed/server
08:28:58 INFO     | evck.config_manager:130 | 🔄 Global Config Reload Triggered...
08:28:58 INFO     | evck.config_manager:135 | ✅ All configuration layers are now in sync.
08:28:58 INFO     | evck.gui_decentralized:1603 | [STATIC FOLDER] → /opt/evck/share/static
08:28:58 INFO     | evck.messaging:50 | Connected to MQTT Broker at 127.0.0.1:1883
08:28:58 INFO     | evck.messaging:60 | MQTT connected successfully.
08:28:58 INFO     | evck.messaging:65 | Resubscribing to topic: keys (mid=5)
08:28:58 INFO     | evck.messaging:65 | Resubscribing to topic: config/version (mid=9)
08:28:58 INFO     | evck.messaging:65 | Resubscribing to topic: config/serial (mid=14)
08:28:58 INFO     | evck.messaging:65 | Resubscribing to topic: config/location (mid=18)
08:28:58 INFO     | evck.gui_decentralized:1662 | qqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqq
08:28:58 INFO     | evck.gui_decentralized:1663 |   GUI Decentralized — subscribed to 68 topics
08:28:58 INFO     | evck.gui_decentralized:1664 | qqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqq
 * Serving Flask app 'gui_decentralized'
 * Debug mode: off
08:28:58 INFO     | evck.messaging:102 | [SUB ACK] Successfully subscribed to topic: keys (mid=1, qos=(0,))
08:28:58 INFO     | evck.messaging:102 | [SUB ACK] Successfully subscribed to topic: config/version (mid=2, qos=(0,))
08:28:58 DEBUG    | evck.messaging:82 | Received message on config/version
08:28:58 INFO     | evck.


inject_main.js:1 inject_main.js
(index):1  Failed to load resource: the server responded with a status of 500 (INTERNAL SERVER ERROR)
(index):1 Unchecked runtime.lastError: Could not establish connection. Receiving end does not exist.
(index):1 Unchecked runtime.lastError: Could not establish connection. Receiving end does not exist.



الان تایم کاری تموم  شده
برام گزارش امروز رو بده
تا بعدا بتونم اوکیش کنم
 
daily-report
 
📋 گزارش کار روزانه — ۲۰ شهریور ۱۴۰۵ (2026-09-10)
پروژه: ایستگاه شارژ EVCK (معماری نامتمرکز — evck160c)
حوزه کاری: بیلد باینری نرم‌افزار، مهاجرت به معماری FHS، و ارتقای Systemd Services

📌 خلاصه وضعیت روز (Executive Summary)
امروز فرآیند کامپایل باینری ماژول گرافیکی (evck-gui) با موفقیت روی برد OrangePi Zero 2W از طریق PyInstaller (حالت --onedir) اجرا شد و سرویس باینری بالا آمد و به کارگزار MQTT متصل شد. در هنگام تست وب، خطای ۵۰۰ ناشی از عدم انتقال فایل‌های استاتیک/قالب‌ها به دایرکتوری جدید ثبت شد که برای رفع در روز بعد اولویت‌بندی گردید.

✅ اقدامات انجام‌شده به تفکیک حوزه (Completed Tasks)
۱. زیرساخت بیلد و کامپایل باینری (PyInstaller & Tooling)
بررسی و رفع وابستگی‌های کامپایلر: نصب پکیج pyinstaller==6.11.1 و ماژول‌های کمکی مرتبط با هوک‌ها روی محیط پایتون برد (aarch64).
کامپایل موفق ماژول رابط کاربری: اجرای دستور سفارشی‌شده برای کامپایل gui_decentralized.py با فلگ‌های صریح جمع‌آوری پکیج‌های common، engineio و socketio.
ایجاد اسکریپت‌های اتوماسیون: توسعه دو اسکریپت شل در سورس برای خودکارسازی بیلد تمام سرویس‌ها (build_all_binaries.sh) و انتقال و نصب آن‌ها به مسیر نهایی (deploy_binaries.sh).
۲. بازنگری و یکپارچه‌سازی سرویس‌های سیستم (Systemd & FHS)
جداسازی و ایجاد سرویس‌های مبتنی بر باینری: تعریف فایل‌های سرویس جدید بدون تغییر در سرویس‌های پایدار قدیمی:
evck-gui-bin.service
evck-lpc-bin.service
evck-opi_hardware-bin.service
evck-ocpp-bin.service
evck-health-bin.service
هدف‌گذاری مسیرهای استاندارد: تنظیم مسیرهای اجرایی به /opt/evck/bin/ و مسیر فایل‌های وب و قالب‌ها به /opt/evck/share/.
۳. تست و ارزیابی اولیه روی سخت‌افزار (Bench Testing)
تست اجرای مستقیم باینری: اجرای مستقیم باینری کامپایل‌شده ۴.۶ مگابایتی evck-gui با دسترسی روت.
تأیید ارتباطات پایه: تأیید همگام‌سازی لایه‌های کانفیگ، اتصال موفق به بروکرمحلی MQTT در پورت ۱۸۸۳، و ثبت‌نام در ۶۸ تاپیک سیستمی.
⛔ موانع، چالش‌ها و پیگیری‌ها (Blockers & Dependencies)
خطای ۵۰۰ سرور داخلی (HTTP 500 Internal Server Error) در تست مرورگر:
شرح مسئله: با لود آدرس UI در مرورگر، خطای ۵۰۰ برگردانده شد.
علت ریشه‌ای: باینری ساخته‌شده طبق تنظیمات FHS انتظار دارد فایل‌های وب در /opt/evck/share/static و قالب‌ها در /opt/evck/share/templates باشند، اما چون فرآیند بیلد فعلاً درون پوشه ایزوله کاربری (~/evck-build) انجام شده، این فایل‌های استاتیک هنوز به مسیر /opt/evck/share/ منتقل (Deploy) نشده‌اند.
راه‌حل فنی روز بعد: اجرای اسکریپت deploy_binaries.sh برای کپی شدن خودکار استاتیک‌ها به /opt/evck/share، یا ست کردن متغیر محیطی EVCK_STATIC_DIR برای تست درون پوشه بیلد.
📦 خروجی‌ها و مستندات ثبت‌شده (Deliverables)
⚙️ اسکریپت بیلد جامع: opt/evck/build_all_binaries.sh (آماده برای بیلد همزمان ۴ ماژول LPC, OPI, OCPP, GUI).
🚚 اسکریپت انتقال و دیپلوی: opt/evck/deploy_binaries.sh (آماده‌سازی دایرکتوری‌های FHS و سرویس‌ها).
📑 سرویس‌های یونیت جدید: پنج فایل *-bin.service در مسیر etc/systemd/system/.
📦 باینری کامپایل‌شده روی برد: فایل اجرایی dist/binaries/evck-gui/evck-gui به حجم ۴.۶ مگابایت.
🎯 برنامه‌ها و اولویت‌های روز کاری بعد (Next Day Priorities)
 اجرای اسکریپت deploy_binaries.sh روی برد جهت انتقال فایل‌های static/ و templates/ به /opt/evck/share/ و رفع خطای ۵۰۰.
 بیلد سه ماژول باقی‌مانده (evck-lpc، evck-opi_hardware و evck-ocpp) با استفاده از build_all_binaries.sh.
 تست اجرای کامل سرویس‌ها از طریق systemctl start *-bin و مانیتورینگ لاگ‌ها در /var/log/evck/.
 ورود به فاز ۴: ساختاربندی پکیج دبیان (.deb) برای نصب یکپارچه با apt.
12:04 PM
