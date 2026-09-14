# Gobolia Automation (Maestro)

## Automation flows included

This repo contains two black-box UI flows:

1. Admin sign-up and login
   - File: `admin_signup_login.yaml`
   - Covers selecting the supervisor role, entering the phone number, and completing the OTP login flow.

2. Booking (appointment)
   - File: `booking_flow.yaml`
   - Covers opening the booking flow and creating an appointment.

## Requirements

- Android phone or emulator connected via ADB
- Android app installed
- Maestro installed

```bash
curl -Ls "https://get.maestro.mobile.dev" | bash
export PATH="$PATH:$HOME/.maestro/bin"
export PATH="$PATH:$HOME/Library/Android/sdk/platform-tools"
```

## Check device connection

```bash
adb devices
```

The device should appear as `device`.

## Run the flows

From this folder:

```bash
cd /Users/maryam/Documents/GitHub/Maestro/automation

# Admin sign-up and login
maestro test -e ANDROID_SERIAL=R8YW70J7TMV admin_signup_login.yaml

# Booking flow
maestro test -e ANDROID_SERIAL=R8YW70J7TMV booking_flow.yaml
```

## Test values

- OTP: `112000`
- App package used in these flows: `com.practicalidea.gobolia.dev`

## Notes

- This is black-box testing with Maestro.
- If the app UI changes, update the visible text selectors in the YAML files.
- If the app package name is different on your build, update `appId` in the YAML files.

---

# اتومیشن تست‌های Practical Idea (Maestro)

## پیش‌نیاز
- نصب Maestro: `curl -Ls "https://get.maestro.mobile.dev" | bash`
- دستگاه اندروید یا امولاتور متصل (`adb devices`)
- اپلیکیشن نصب‌شده روی دستگاه

## اجرای تست‌ها

```bash
# فلوی ثبت‌نام و ورود (پوشش سه نقش، با کد OTP آزمایشی)
maestro test supervisor_login.yaml

# فلوی پایه‌ی قول و قرار
maestro test promise_flow.yaml

# فلوی کامل end-to-end قول‌وقرار (سرپرست -> فرزند -> تراپیست)
maestro test promise_full_flow.yaml

# اجرای همه
maestro test .
```

## توضیح فلوها و اصلاحات

### supervisor_login.yaml (اصلاح‌شده)
- پوشش انتخاب هر سه نقش (سرپرست، کودک، تراپیست)
- ورود شماره موبایل و تکمیل ورود با کد آزمایشی OTP (`112000`) که برای محیط تست در دسترس است
- بررسی می‌شود که پس از ورود کد، صفحه‌ی OTP بسته شده و کاربر وارد بخش اصلی اپ می‌شود

### promise_flow.yaml
- رفتن به بخش قول و قرار
- ساخت یک قول جدید
- تأیید ایجاد موفق

### promise_full_flow.yaml (جدید)
سناریوی کامل و end-to-end قول‌وقرار، شامل:
1. ورود سرپرست با شماره `09304216502` و کد OTP `112000`
2. افزودن فرزند (نام، تاریخ تولد، جنسیت، و تلاش برای انتخاب نسبت — نسبت مطابق **BUG-07** ممکن است ذخیره نشود)
3. ساخت قول‌وقرار بین سرپرست و کودک، و افزودن یک قول در بخش مدیریت قول‌وقرار
4. دریافت کد اختصاصی کودک (`58100774`) از پروفایل فرزند ایجادشده
5. خروج از حساب سرپرست (طبق **BUG-08**، برای نقش کودک مسیر خروج مستقیم وجود ندارد و باید ابتدا از مد سرپرست خارج شد)
6. ثبت‌نام حساب مستقل کودک با استفاده از کد فرزند
7. ثبت‌نام حساب تراپیست (شماره نظام پزشکی، نام، عکس پروفایل — نبود اعتبارسنجی عکس مطابق **BUG-09**)
8. اتصال تراپیست به کودک از طریق کد کودک و ساخت قول‌وقرار تراپیست-کودک
9. ارسال یک پیام آزمایشی در گفتگوی کودک-تراپیست

## نکات
- selectorها بر اساس متن‌های فارسی قابل مشاهده در اسکرین‌شات‌ها نوشته شده‌اند.
- در محیط واقعی پس از دریافت resource-idهای دقیق، اسکریپت‌ها را به‌روز کنید.
- appId را با package name واقعی جایگزین کنید.
- مقادیر OTP (`112000`) و کد کودک (`58100774`) صرفاً برای محیط تست/mock معتبرند.
