# ToyStore Pro V2 — دنیای اسباب بازی

وب‌اپ مدیریت فروشگاه + فروشگاه مشتری با معماری production-ready.

## 14 قابلیت اصلی + V2

1. **ورود امن مدیر** با Supabase Auth؛ رمز عبور داخل کد ذخیره نمی‌شود.
2. **بارکد واقعی CODE128** برای هر محصول و آمادهٔ چاپ.
3. **ذخیره عکس محصول** در معماری Supabase Storage؛ URL در محصول ذخیره می‌شود.
4. **فروشگاه مشتری + سبد خرید + سفارش** و جدول order/order_items.
5. **درگاه پرداخت** با Edge Function adapter آمادهٔ اتصال به درگاه انتخابی؛ کلیدها فقط server-side.
6. **AI برای ثبت محصول از عکس** و AI برای تبدیل متن‌هایی مثل «قطار ۳ تا فروخته شد» به عملیات انبار.
7. **ارسال گزارش با ایمیل** با Edge Function و Resend؛ می‌توان کانال دیگری را نیز به همین لایه اضافه کرد.
8. **Cron روزانه/هفتگی/ماهانه** برای ساخت و ارسال گزارش.
9. **RLS** برای کنترل دسترسی دیتابیس.
10. **Audit Log** برای ثبت تغییرات مهم.
11. **داشبورد فروشگاه** با موجودی، ارزش انبار، کمبود موجودی و سفارش‌ها.
12. **آماده برای اسکن بارکد با دوربین موبایل**؛ می‌توان BarcodeDetector یا کتابخانهٔ scanner را در همین UI متصل کرد.
13. **QR محصول** که به صفحهٔ محصول هدایت می‌شود.
14. **Responsive** برای موبایل، تبلت و دسکتاپ.

### قابلیت‌های V2 تکمیلی

- افزایش/کاهش گروهی قیمت با درصد
- ثبت فروش، ورود کالا، برگشت و اصلاح موجودی
- هشدار حداقل موجودی
- جستجو با نام، SKU و بارکد
- دسته‌بندی محصولات
- گزارش ارزش موجودی
- تاریخچهٔ گردش انبار
- سفارش مشتری
- API/Edge Function برای AI
- ساختار آماده برای اتصال درگاه
- GitHub Actions برای GitHub Pages

## معماری

Frontend: React + TypeScript + Vite

Backend: Supabase PostgreSQL + Auth + Storage + Edge Functions + Cron

GitHub Pages فقط Frontend را منتشر می‌کند. اطلاعات حساس و عملیات سروری نباید در GitHub Pages انجام شوند. GitHub Actions می‌تواند build را انجام داده و خروجی را روی Pages منتشر کند. مستندات GitHub نمونهٔ رسمی همین الگو را ارائه می‌کند.

Supabase Edge Functions برای عملیات server-side و اتصال به سرویس‌های ثالث مناسب‌اند و احراز هویت آن‌ها می‌تواند با JWT انجام شود.

Supabase Cron نیز می‌تواند SQL، Database Function یا HTTP/Edge Function را طبق زمان‌بندی اجرا کند.

## راه‌اندازی

### 1) Supabase

یک پروژه بسازید و فایل `supabase/schema.sql` را در SQL Editor اجرا کنید.

سپس در Authentication یک حساب مدیر بسازید.

### 2) Environment در GitHub

در Repository → Settings → Secrets and variables → Actions، این Variables را اضافه کنید:

- `VITE_SUPABASE_URL`
- `VITE_SUPABASE_PUBLISHABLE_KEY`

کلید secret/service role را هرگز در Vite یا GitHub Pages قرار ندهید.

### 3) Secrets در Supabase

برای AI:

`OPENAI_API_KEY`

برای گزارش ایمیلی:

`RESEND_API_KEY`
`REPORT_TO_EMAIL`
`REPORT_FROM_EMAIL`

برای عملیات server-side حساس، از secret key سمت Supabase استفاده کنید.

### 4) نصب

```bash
npm install
npm run dev
```

### 5) Deploy به GitHub Pages

کد را داخل Repository قرار دهید، Branch اصلی را `main` بگذارید و GitHub Pages را روی GitHub Actions قرار دهید.

Workflow موجود در `.github/workflows/deploy.yml` build و deploy را انجام می‌دهد.

### 6) زمان‌بندی گزارش

بعد از تنظیم Vault/secretهای پروژه، در Supabase Cron می‌توانید گزارش را مثلاً روزانه اجرا کنید. برای اتصال Cron به Edge Function، مستندات رسمی Supabase از pg_cron + pg_net پشتیبانی می‌کند.

## نکات مهم production

- قیمت، موجودی و بارکد نباید صرفاً بر اساس حدس AI وارد دیتابیس شوند؛ AI پیشنهاد می‌دهد و مدیر تأیید می‌کند.
- برای پرداخت واقعی باید اطلاعات Merchant/Secret درگاه در Edge Function تنظیم شود.
- برای SMS باید سرویس پیامک و secret آن اضافه شود.
- RLS در این Starter برای شروع تنظیم شده است؛ قبل از استفادهٔ چندفروشگاهی باید role/tenant isolation دقیق‌تر اعمال شود.
- اسکن دوربین و PWA می‌توانند در مرحلهٔ بعد بدون تغییر معماری اضافه شوند.
