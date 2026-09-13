# 📋 راهنمای جامع تغییرات فورک و نگاشت فایل‌ها (Fork Changes & Upstream Migration Guide)

این مستند به منظور ثبت دقیق تمامی تغییرات، قابلیت‌های افزوده شده، و نقشه‌ی جابجایی فایل‌ها بین فورک اختصاصی شما (**BPB-Worker-Panel-pro**) و ریپوزیتوری رسمی و اصلی (**BPB-Worker-Panel**) تهیه شده است. در آینده هر زمان که پنل اصلی آپدیت جدیدی منتشر کند، با استفاده از این راهنما دقیقاً می‌دانید هر فایل، تنظیم، یا تکه کد به کجا منتقل شده و چگونه بدون تداخل، تغییرات خود را حفظ یا همگام کنید.

---

## 📌 ۱. اطلاعات مرجع و وضعیت انشعاب (Fork Overview)

* **مخزن اصلی (Upstream):** `https://github.com/bia-pain-bache/BPB-Worker-Panel`
* **مخزن فورک شما:** `https://github.com/javadSharifi/BPB-Worker-Panel-pro`
* **شاخه فعال:** `main`
* **نقطه مرجع اتصال (Merge Base):** کامیت `3de7579` (پایان نسخه‌های سری `v4.2.3` و شاخه `dev`)
* **وضعیت نسخه‌های بالادستی (Upstream):** ریپوزیتوری اصلی به نسخه‌های **v5.0.0** و **v5.1.1** ارتقا یافته و ساختار معماری فایل‌های آن به صورت ماژولار (Modular Architecture) از نو بازنویسی شده است.

---

## 🛠️ ۲. خلاصه تمام تغییرات اعمال‌شده در این فورک (Custom Features & Changes)

تمامی تغییرات ایجاد شده توسط شما در طول ۸ کامیت به همراه ۱ ادغام (Merge) انجام گرفته است:

### لیست کامیت‌های اختصاصی شما:
1. `4c0c161` : **Add Telegram bot integration with webhook setup and configuration options**
2. `270d50c` : **feat: implement user management functionality in the panel** (و بازطراحی بزرگ UI به سبک Glassmorphism/Neon)
3. `439eaf8` : **feat: add Cloudflare usage monitoring functionality and UI components**
4. `571f472` : **feat: update README and RELEASE notes to include Cloudflare Usage Monitor and Telegram bot**
5. `f3b60a7` : **feat: add emoji to generated remark for improved readability**
6. `0efbd49` : **feat: add Cloudflare account settings to global configuration and improve type safety**
7. `9a61486` : **feat: enhance URL building for user subs with optional label and improve encoding for subPaths**
8. `d3620ef` : **Refactor user management: remove user-related functions and UI components** (پاکسازی ماژول موقت یوزرها و بازگشت به ساختار تمیز)
9. `e7098b5` : **Merge branch 'dev' into main** (مرج آخرین تغییرات upstream قبل از v5)

---

### جزئیات تغییرات فنی و امکانات افزوده شده:

#### ۱. بازطراحی کامل و مدرن رابط کاربری (Glassmorphism & Neon Design)
* **پنل مدیریت (`src/assets/panel/`):**
  * جایگزینی منوهای کشویی و آکاردئونی قدیمی (`<details><summary>`) با **سیستم تب‌بندی حرفه‌ای (Tab-based Navigation)** شامل:
    * `Common` (تنظیمات عمومی و DNS)
    * `VLESS-Trojan` (تنظیمات پروتکل‌ها)
    * `Fragment` (فرگمنت)
    * `Warp` و `Warp Pro`
    * `Routing` (قوانین روتینگ)
    * `External` (کانفیگ‌های خارجی)
    * `Telegram` (تب اختصاصی برای تنظیمات ربات تلگرام)
    * `Subscriptions` (کارت‌های شیشه‌ای لینک‌های ساب و کلاینت‌ها)
  * اضافه شدن **سوئیچ تم دارک/لایت (Theme Toggle 🌙/☀️)** با ذخیره در حافظه لوکال.
  * اضافه شدن **ناوبار شناور (Floating Navbar Pill)** به سبک مدرن نئونی.
  * طراحی المان‌های شیشه‌ای مدرن (`glass-card`، `neon-input`، دکمه‌های با افکت Glow).

* **صفحه لاگین (`src/assets/login/`):**
  * بازطراحی کامل با استایل Glassmorphism و پس‌زمینه گرادینت تاریک/روشن.
  * فیلد رمز عبور مدرن به همراه دکمه نمایش/مخفی‌سازی رمز (`toggle-vis`).
  * پیل شناور بالای صفحه برای تغییر تم دارک/لایت در لاگین.

#### ۲. پیاده‌سازی کامل ربات تلگرام اختصاصی (`src/telegram.ts`)
* ایجاد یک فایل کامپوننت کامل به نام `src/telegram.ts` (۴۷۹ خط کد تایپ‌اسکریپت).
* پشتیبانی از وب‌هوک اختصاصی تلگرام در مسیر `/telegram` در ورکر (`src/worker.ts`).
* کنترل دسترسی امن فقط برای ادمین بر اساس `telegramAdminId`.
* قابلیت‌ها و دستورات ربات:
  * `/start`: منوی شیشه‌ای اینلاین با دکمه‌های شیشه‌ای واکنش‌گرا.
  * `/config`: دریافت انواع لینک‌های ساب‌اسکریپشن (Normal, Fragment, Raw, Warp, Warp Pro).
  * انتخاب کلاینت و فرمت کانفیگ (v2rayNG, sing-box, Clash Meta, Streisand, Stash, FlClash و...).
  * `/qr`: ساخت QR کد برای اسکن سریع در موبایل.
  * `/info`: دریافت وضعیت پنل و تنظیمات کلی.
  * `/usage`: استعلام مستقیم آمار مصرف کلودفلر در داخل تلگرام.
* اضافه شدن اندپوینت `/panel/setup-telegram-webhook` در بک‌اند برای ثبت خودکار وب‌هوک و دستورات ربات در سرورهای تلگرام بدون نیاز به دستورات دستی curl.

#### ۳. مانیتورینگ مصرف ترافیک و منابع کلودفلر (Cloudflare Usage Monitor)
* ایجاد اندپوینت اختصاصی `/panel/cf-usage` در بک‌اند (`src/common/handlers.ts`).
* اتصال به **Cloudflare GraphQL Analytics API** (`workersInvocationsAdaptive`).
* محاسبه و نمایش پارامترهای حیاتی:
  * تعداد ریکوئست‌های امروز (`requestsUsed` در برابر سقف رایگان 100k روزانه).
  * ساب‌ریکوئست‌ها و خطاهای ورکر (`observabilityLimit` ۲۰۰ هزارتایی).
  * درصد مصرف و وضعیت هشدار (Yellow Warning در صورت عبور از ۸۰٪ و Red در ۱۰۰٪).
* ویجت بصری و پروگرس‌بار زیبا در پنل برای پایش آسان منابع ورکر.

#### ۴. تغییرات در دیتابیس KV و تایپ‌ها (Settings & KV Schema)
* فیلدهای جدید اضافه شده به تنظیمات ورکر در فایل‌های `src/common/init.ts`، `src/kv.ts` و `src/types/global.d.ts`:
  * `telegramBotToken`: توکن ربات تلگرام
  * `telegramAdminId`: شناسه عددی چت ادمین در تلگرام
  * `cfAccountId`: اکانت آیدی اکانت کلودفلر
  * `cfApiToken`: توکن API با دسترسی Workers Analytics
  * `cfWorkerName`: نام اسکریپت ورکر در کلودفلر
  * متغیر محیطی `SUB_PATH` در آبجکت `Env`

#### ۵. زیباتر کردن ریمارک کانفیگ‌ها
* اضافه شدن اموجی‌های نمایشی به تابع `generateRemark` در `src/cores/utils.ts` برای تفکیک راحت‌تر کانفیگ‌ها در اپلیکیشن‌های کلاینت.

---

## 🗺️ ۳. جدول جامع نگاشت فایل‌ها: فورک شما (v4.x) در برابر نسخه جدید پنل اصلی (v5.x)

ریپوزیتوری اصلی در نسخه **v5.0.0** دستخوش بازنویسی ساختاری شد. جدول زیر به شما نشان می‌دهد هر فایل در فورک فعلی شما، در نسخه جدید اصلی به کجا منتقل یا تفکیک شده است:

| # | مسیر فایل در فورک شما (v4) | مسیر جدید در پنل اصلی (v5) | ماهیت و شرح تغییرات |
|---|---|---|---|
| **۱** | `src/common/handlers.ts` | **پوشه `src/handlers/` (تکه‌تکه شده)** | این فایل بزرگ در نسخه ۵ به چندین هندلر مجزا شکسته شد:<br>• `src/handlers/panel.ts` (درخواست‌های پنل)<br>• `src/handlers/login.ts` (احراز هویت ورود)<br>• `src/handlers/subscription.ts` (ساب‌اسکریپشن)<br>• `src/handlers/proxy-ip.ts` (پروکسی آی‌پی‌ها)<br>• `src/handlers/websocket.ts` (پروتکل وب‌سوکت)<br>• `src/handlers/doh.ts` (دی‌ان‌اس DoH)<br>• `src/handlers/qrcode.ts` (تولید کیوآرکد)<br>• `src/handlers/error.ts` و `utils.ts` |
| **۲** | `src/common/init.ts` | `src/settings/settings.ts` و `src/settings/main.ts` | مقادیر پیش‌فرض تنظیمات به `src/settings/settings.ts` و توابع مقداردهی اولیه محیط به `src/settings/main.ts` منتقل شدند. |
| **۳** | `src/kv.ts` | `src/settings/kv.ts` و `src/settings/validators.ts` | کدهای KV به پوشه `settings` رفتند و اعتبارسنجی مقادیر در فایلی مستقل قرار گرفت. |
| **۴** | `src/auth.ts` | `src/auth/auth.ts` | ماژول احراز هویت به زیرپوشه اختصاصی `auth` منتقل شد. |
| **۵** | `src/telegram.ts` *(کد اختصاصی شما)* | `src/api/telegram.ts` و `src/handlers/telegram.ts` | **نکته مهم:** پنل اصلی در v5 خودش نیز ربات تلگرام اضافه کرد! ساختار v5 شامل یک هندلر در `handlers/telegram.ts` و توابع اصلی در `api/telegram.ts` است. |
| **۶** | اندپوینت `/panel/cf-usage` *(کد اختصاصی شما)* | `src/api/usage.ts` | پنل اصلی در v5 ماژول مصرف ورکر را در `src/api/usage.ts` قرار داده است. |
| **۷** | `src/protocols/websocket/vless.ts` | `src/protocols/vless.ts` | پوشه واسط `websocket` حذف شد و فایل مستقیماً در `protocols` قرار گرفت. |
| **۸** | `src/protocols/websocket/trojan.ts` | `src/protocols/trojan.ts` | به طور مشابه به پوشه `protocols` منتقل شد. |
| **۹** | `src/protocols/websocket/common.ts` | `src/protocols/common.ts` | به پوشه `protocols` منتقل شد. |
| **۱۰** | `src/protocols/warp.ts` | `src/api/warp.ts` | وارپ از پروتکل به یک سرویس API در پوشه `api` ارتقا یافت. |
| **۱۱** | `src/assets/secrets/*` | **کلاً حذف شد (Deleted)** | صفحات و اسکریپت‌های مربوط به تولید Secrets در نسخه جدید حذف شدند. |
| **۱۲** | `src/assets/panel/index.html` | `src/assets/panel/index.html` | مسیر فایل یکسان است، ولی محتوای upstream همچنان آکاردئونی است در حالی که فایل شما سیستم مدرن تب‌بندی نئونی دارد. |
| **۱۳** | `src/assets/panel/script.js` | `src/assets/panel/script.js` | در v5 پنل اصلی کیوآرکد محلی و کلاینت‌ها اضافه شده، و کدهای هندلینگ تم/تب شما در فایل فعلی قرار دارد. |
| **۱۴** | `src/assets/panel/style.css` | `src/assets/panel/style.css` | فایل شما حاوی استایل‌های نئونی، گلس‌مورفیسم، متغیرهای تم تاریک/روشن و تب‌ها است. |
| **۱۵** | `src/assets/login/*` | `src/assets/login/*` | مسیر یکسان است؛ نسخه شما طراحی مدرن Glassmorphism و سوئیچ تم دارد. |
| **۱۶** | `src/types/global.d.ts` | `src/types/global.d.ts` و `src/types/settings.ts` | تایپ‌های تنظیمات به فایل اختصاصی `settings.ts` در پوشه types منتقل شده‌اند. |
| **۱۷** | `src/worker.ts` | `src/worker.ts` | ورودی اصلی در v5 از روترهای ماژولار پوشه `handlers` استفاده می‌کند. |
| **۱۸** | `tsconfig.json` | `tsconfig.json` | آلیاس‌های مسیرها (`@settings`, `@handlers`, `@api/*`, `@auth`) در v5 به‌روزرسانی شده‌اند. |

---

## 🎯 ۴. بررسی تطبیقی امکانات شما با امکانات جدید پنل اصلی (Feature Comparison)

یک کشف بسیار جالب در بررسی نسخه v5 پنل اصلی: **پنل اصلی نیز پس از فورک شما، قابلیت ربات تلگرام و مانیتور مصرف ورکر را پیاده‌سازی کرده است!**

| قابلیت | در فورک شما (v4.x) | در نسخه جدید اصلی (v5.x) | پیشنهاد برای مهاجرت / آپدیت |
|---|---|---|---|
| **طراحی و تم پنل (UI/UX)** | کامپوننت‌های نئونی، Glassmorphic، تب‌بندی افقی، تم دارک/لایت حرفه‌ای | طراحی کلاسیک با فونت وزیر و آکاردئون‌های عمودی | **حتماً UI خودتان را نگه دارید؛** رابط کاربری فورک شما بسیار زیباتر، مرتب‌تر و کاربرپسندتر از ظاهر پیش‌فرض پنل اصلی است. |
| **ربات تلگرام** | پیاده‌سازی مستقل در `src/telegram.ts` با منوی شیشه‌ای، تفکیک کلاینت‌ها و وب‌هوک مستقیم | پیاده‌سازی در `src/api/telegram.ts` و `src/handlers/telegram.ts` با دستورات تلگرامی | می‌توانید ربات تلگرام خود را به معماری جدید منتقل کنید یا از پیاده‌سازی رسمی v5 که با تمام گزینه‌های جدید کانفیگ هماهنگ است استفاده نمایید. |
| **مانیتور مصرف (Usage)** | کوئری اختصاصی به Cloudflare GraphQL در `handlers.ts` با کارت ویجت در پنل | پیاده‌سازی در `src/api/usage.ts` و نمایش در پنل و بات | ساختار v5 از لحاظ پشتیبانی از Workers و Pages کامل‌تر است؛ می‌توانید فرانت‌اند شیشه‌ای خود را به اندپوینت v5 متصل کنید. |
| **ماژول یوزرز (`users.ts`)** | در کامیت `270d50c` تست شد و در `d3620ef` پاکسازی گردید | در هیچ نسخه‌ای از پنل اصلی پیاده نشده است | عدم وجود در هر دو سورس؛ نیازی به انتقال ندارد. |

---

## 🚀 ۵. راهنمای گام‌به‌گام آپدیت و همگام‌سازی با پنل اصلی در آینده (Sync & Update Guide)

اگر در آینده تصمیم گرفتید پروژه‌تان را با آخرین آپدیت‌های پنل اصلی (v5 به بعد) سینک کنید، بهترین متدولوژی برای جلوگیری از به هم ریختن کدهایتان به این ترتیب است:

### مرحله ۱: افزودن ریموت بالادستی (Upstream Remote)
در ترمینال پروژه، ریموت اصلی را ثبت و فچ کنید:
```bash
# اضافه کردن ریپوی اصلی به عنوان upstream (در صورتی که اضافه نشده باشد)
git remote add upstream https://github.com/bia-pain-bache/BPB-Worker-Panel.git

# دریافت آخرین تغییرات و شاخه‌های رسمی
git fetch upstream
```

### مرحله ۲: ایجاد شاخه آزمایشی برای ارتقا (Migration Branch)
هرگز مستقیماً روی شاخه `main` تغییرات عمیق ساختاری نزنید:
```bash
# ساخت شاخه جدید بر پایه آخرین نسخه پنل اصلی
git checkout -b upgrade-v5 upstream/main
```

### مرحله ۳: انتقال لایه‌های اختصاصی شما (Porting Your Features)

1. **انتقال رابط کاربری اختصاصی (UI):**
   * فایل‌های `src/assets/panel/index.html`، `style.css` و `script.js` خود را به شاخه جدید ببرید.
   * المان‌های جدید نسخه ۵ (مانند بخش Supported Clients یا تنظیمات اضافه شده) را در تب‌های مربوطه در `index.html` قرار دهید.

2. **انتقال ماژول تلگرام یا مانیتورینگ:**
   * در نسخه ۵، به جای دستکاری `worker.ts` قدیمی، کدهای وب‌هوک در `src/handlers/telegram.ts` و منطق آن در `src/api/telegram.ts` قرار دارد. کافی است توابع و دکمه‌های شیشه‌ای دلخواه خود را در `src/api/telegram.ts` جایگزین یا ادغام نمایید.
   * اندپوینت‌های پنل در نسخه ۵ به سادگی در `src/handlers/panel.ts` اضافه می‌شوند.

3. **تنظیمات و متغیرها:**
   * اگر متغیری به تنظیمات اضافه می‌کنید، در نسخه ۵ آن را در `src/settings/settings.ts` (پیش‌فرض‌ها) و `src/types/settings.ts` (تایپ‌ها) اضافه کنید.

### مرحله ۴: تست و بیلد پروژه
```bash
# بررسی عدم وجود ارورهای تایپ‌اسکریپت
npm run check

# بیلد نهایی پروژه
npm run build
```

---

## 📝 ۶. چک‌لیست سریع مکان فایل‌ها برای توسعه‌دهنده

هر زمان سوالی برایتان پیش آمد که فلان بخش در نسخه جدید کجاست، به این چک‌لیست رجوع کنید:

* **می‌خواهم یک روت یا اندپوینت جدید به پنل اضافه کنم:**  
  * در فورک شما: `src/common/handlers.ts` تابع `handlePanel`
  * در نسخه v5 به بعد: `src/handlers/panel.ts`

* **می‌خواهم فیلدی به تنظیمات پنل یا دیتابیس KV اضافه کنم:**  
  * در فورک شما: `src/common/init.ts` و `src/kv.ts` و `src/types/global.d.ts`
  * در نسخه v5 به بعد: `src/settings/settings.ts` و `src/settings/kv.ts` و `src/types/settings.ts`

* **می‌خواهم لاجیک ربات تلگرام را ویرایش کنم:**  
  * در فورک شما: `src/telegram.ts`
  * در نسخه v5 به بعد: `src/api/telegram.ts` و `src/handlers/telegram.ts`

* **می‌خواهم ظاهر و استایل صفحات را تغییر دهم:**  
  * در هر دو نسخه: `src/assets/panel/` (پنل اصلی) و `src/assets/login/` (صفحه ورود)

---
*تهیه شده برای مدیریت و نگهداری آسان فورک `BPB-Worker-Panel-pro`*
