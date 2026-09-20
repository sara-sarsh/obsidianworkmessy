Created TUTORIAL_GUI_BUILD_AND_UPDATE.md

<div dir="rtl" style="text-align: right; font-family: Vazirmatn, Tahoma, sans-serif;">

فایل مستندات و راهنمای کامل را در قالب مارک‌داون در مسیر پروژه ایجاد کردم:  
📄 [doc/TUTORIAL_GUI_BUILD_AND_UPDATE.md](file:///C:/Users/Danesh/Documents/Sara/Tesla/github-evckcli/evck160c-%20decentralized/doc/TUTORIAL_GUI_BUILD_AND_UPDATE.md)

---

### خلاصه مراحل برای تست یکپارچه شما:

1. **ارسال سورس از ویندوز به اورنج‌پای بیلد:**
   ```powershell
   cd "C:\Users\Danesh\Documents\Sara\Tesla\github-evckcli\evck160c- decentralized"
   scp -r .\opt\evck orangepi@<IP_BUILD_BOARD>:/home/orangepi/evck-build/opt/
   ```

2. **بیلد و بسته‌بندی روی برد بیلد:**
   ```bash
   ssh orangepi@<IP_BUILD_BOARD>
   cd /home/orangepi/evck-build/opt/evck
   chmod +x build_gui.sh package_gui.sh
   ./build_gui.sh
   ./package_gui.sh
   ```
   *(خروجی: فایل `dist/evck-gui-v1.0.1.tar.gz` ساخته می‌شود).*

3. **دریافت فایل آماده روی ویندوز:**
   ```powershell
   scp orangepi@<IP_BUILD_BOARD>:/home/orangepi/evck-build/opt/evck/dist/evck-gui-v*.tar.gz C:\Users\Danesh\Desktop\
   ```

4. **ارسال و آپدیت روی دستگاه مقصد (تارگت):**
   ```powershell
   scp C:\Users\Danesh\Desktop\evck-gui-v*.tar.gz orangepi@<IP_TARGET_BOARD>:/home/orangepi/
   ```
   سپس در SSH تارگت:
   ```bash
   ssh orangepi@<IP_TARGET_BOARD>
   mkdir -p /tmp/gui-update
   tar -xzf evck-gui-v*.tar.gz -C /tmp/gui-update
   cd /tmp/gui-update
   sudo ./update_gui.sh
   ```

5. **بررسی نهایی:**
   - چک کردن سرویس: `sudo systemctl status evck-gui-bin`
   - باز کردن مرورگر: `http://<IP_TARGET_BOARD>:8082`

لطفاً این چرخه را یک‌بار از ابتدا تا انتها تست کنید؛ اگر هر سؤالی یا خروجی خاصی پیش آمد مطرح بفرمایید تا نهایی کنیم و سپس همین ساختار را برای سه سرویس دیگر پیاده‌سازی کنیم.
</div>