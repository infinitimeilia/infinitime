# انتشار نسخهٔ جدید InfiniTime

نشانی مخزن: https://github.com/infinitimeilia/-infinitime

## آماده‌سازی اولیهٔ مخزن، فقط یک بار

1. فایل‌های همین پوشه را در **ریشهٔ شاخهٔ main** آپلود کنید؛ نه خود فایل ZIP و نه پوشهٔ والد `repository` را.
2. مطمئن شوید فایل `.github/workflows/prepare-release.yml` هم آپلود شده باشد. اگر مرورگر پوشهٔ مخفی `.github` را منتقل نکرد، از **Add file → Create new file** همین مسیر کامل را وارد و متن فایل را کپی کنید. فایل YAML باید زیر همین مسیر باشد، نه در ریشه.
3. در Settings → Actions → General مطمئن شوید اجرای GitHub Actions فعال است. این گردش‌کار فقط مجوز `contents: write` همان مخزن را درخواست می‌کند. در صورت مسدود شدن گردش‌کار به‌علت سیاست حساب/سازمان، می‌توانید روش محلی پایین را استفاده کنید.
4. هیچ کلید امضا، رمز، توکن یا Secret برای این گردش‌کار لازم نیست؛ چیزی از این نوع آپلود یا به Secrets اضافه نکنید.

## هر بار انتشار — روش خودکار پیشنهادی

1. در پروژهٔ اصلی، `versionCode` را افزایش بدهید و `versionName` را چهار قسمتی نگه دارید. مثلاً نسخهٔ بعد از `1.0.0.4` می‌تواند نام `1.0.0.5` و کد `5` داشته باشد. این عدد صرفاً مثال است؛ شمارهٔ واقعی باید از APK خوانده شود.
2. **Release APK را با همان کلید اصلی قبلی امضا کنید.** از Debug APK یا AAB استفاده نکنید. کلید را تغییر ندهید.
3. فقط نام فایل نهایی را به `InfiniTime.apk` تغییر دهید. تغییر نام، امضا و محتوای فایل را عوض نمی‌کند.
4. در فایل `release-notes.json` متن واقعی تغییرات را در `releaseNotes.fa`، `releaseNotes.en` و `releaseNotes.ar` بنویسید. برای خط جدید از `\n` استفاده کنید. عنوان‌ها نیز قابل ویرایش‌اند. متن ساده است؛ HTML لازم نیست. جای خالی قابل قبول است و ادعای قابلیت خیالی نمایش داده نمی‌شود.
5. از صفحهٔ **Releases → Draft a new release** یک نسخه بسازید. تگ دقیقاً باید `v` به‌اضافهٔ `versionName` APK باشد؛ مثلاً `v1.0.0.5`. عنوان می‌تواند `InfiniTime 1.0.0.5` باشد. توضیحات را از `RELEASE_TEMPLATE.md` تکمیل کنید.
6. **قبل از Publish** این دو فایل را به بخش Assets همان Draft اضافه کنید:
   - `InfiniTime.apk` — APK امضاشدهٔ خودتان.
   - `release-notes.json` — توضیحات همین نسخه.
7. نسخه را **پایدار و عمومی** منتشر کنید؛ Pre-release نباشد و به‌عنوان **Latest release** انتخاب شود.
8. در تب Actions گردش‌کار **Prepare verified update metadata** را ببینید. پس از سبز شدن، فایل سوم یعنی `update.json` به Assets همان Release اضافه شده است.
9. آدرس زیر باید فایل JSON را برگرداند:

   https://github.com/infinitimeilia/-infinitime/releases/latest/download/update.json

   فایل APK نیز باید در آدرس زیر باشد:

   https://github.com/infinitimeilia/-infinitime/releases/latest/download/InfiniTime.apk

فقط آپلود APK در ریشهٔ مخزن، یا فقط آپلود ZIP، کانال آپدیت را فعال نمی‌کند. هر نسخه باید Release جدا داشته باشد. کد نسخه باید بیشتر از نسخهٔ نصب‌شده باشد تا آپدیت پیشنهاد شود.

## اگر Actions خطا داد

- نبودن یکی از دو Asset، نام متفاوت فایل‌ها یا یکسان نبودن تگ با نسخهٔ داخل APK باعث توقف می‌شود.
- APK بدون امضا، با بستهٔ Debug یا نسخهٔ نامعتبر پذیرفته نمی‌شود.
- فایل یا نام را اصلاح کنید و از Actions → Prepare verified update metadata → Run workflow تگ همان Release را وارد کنید. بهتر است پیش از آماده‌شدن metadata نسخه را Latest نکنید.
- موفق بودن امضای فایل در ابزار انتشار به‌تنهایی ثابت نمی‌کند کلید آن همان کلید قبلی شماست؛ کنترل یکسان بودن کلید در خود اپ، موقع دانلود هم انجام می‌شود.
- اگر APK را عوض کردید، گردش‌کار را دوباره اجرا کنید تا اندازه و هش برای **همان فایل جدید** ساخته شود. هش یا اندازهٔ ساختگی ننویسید.

## روش محلی، بدون GitHub Actions

به Python 3.11 یا جدیدتر و Android SDK Build Tools نصب‌شده در Android Studio نیاز دارید. در Terminal پوشهٔ مخزن اجرا کنید:

```sh
python tools/create_update_metadata.py InfiniTime.apk --notes release-notes.json --aapt "/path/to/Android/sdk/build-tools/35.0.0/aapt2" --apksigner "/path/to/Android/sdk/build-tools/35.0.0/apksigner"
```

مسیرها را با مسیر واقعی SDK خودتان جایگزین کنید. در ویندوز ابزارها معمولاً `aapt2.exe` و `apksigner.bat` هستند. فایل خروجی `update.json` را همراه APK و توضیحات به **Assets همان Release** اضافه کنید. آپلود آن فقط در ریشهٔ مخزن کافی نیست.

هیچ فایل `update.json` آماده با هش فرضی در این بسته نیست، چون هش، اندازه و شمارهٔ نسخه باید از APK نهایی و امضاشدهٔ خود شما استخراج شوند.
