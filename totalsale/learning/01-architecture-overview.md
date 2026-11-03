# Lesson 01 — فهم معماری چهارلایه TotalSales

**Phase:** P1 · **Created:** 2026-11-03 · **Status:** first lesson drafted; hands-on trace pending.

## هدف مدیریتی
پس از این درس باید بتوانی توضیح بدهی برای یک درخواست تغییر وضعیت سند فروش، نقش UI، Backend، Engine و Database چیست و چگونه اثرات جانبی را ارزیابی می‌کنیم.

## ساختار و منابع
طبق فایل قدیمی [project-map](../references/project-map.md)، سامانه در سه مخزن سورس UI، Backend و Engine و یک لایه منطقی Database بررسی می‌شود. محل نگهداری SQL scripts عمدتاً در سورس Backend معرفی شده است. این یک **نقشه تاریخی** است و باید با سورس فعلی تطبیق داده شود.

```text
Angular UI
   ↓ (user action / request DTO)
Backend API & services
   ↓ (engine contract)
Engine API → Command / State machine / Rules / Domain services
   ↓
Repositories, SQL scripts, runtime rule/status metadata
   ↘
   Side effects: cache, notification, events, external systems
```

نمودار بالا یک **مدل آموزشی** است؛ ترتیب دقیق اجرای Command/Rule و محدوده تراکنش برای هر سند هنوز در سورس جاری تأیید نشده است.

## Rule در برابر Command
- **Validation/Rule:** ارزیابی شرایط تصمیم؛ مانند مجوز انتقال، وضعیت مرتبط یا شرط تجاری. تعریف واقعی و اثر Rule وابسته به پیاده‌سازی است.
- **Command:** درخواست و هماهنگی عملیات؛ ممکن است state change و اثرات مرتبط را اجرا کند.
- **Side effect:** هر تغییر ثانویه خارج از وضعیت اصلی سند، مثل cache، سند دیگر، ارسال پیام یا HTTP، که باید مستقل ردیابی شود.
- **Transaction/recovery:** Rollback دیتابیس لزوماً HTTP، cache یا رویداد ارسال‌شده را برنمی‌گرداند.

## شاهد کد موجود در Skill (تاریخی، محدود به وصول)
در [engine.md](../references/engine.md) یک trace تاریخ‌دار برای Payment Insert / Assign وجود دارد:
`PaymentCommandController.Insert → PaymentInsertCommand/Action → PaymentManager/Getter → state-machine/rules/aggregators/saver`.
این صرفاً مثال **وصول** است و نباید به عنوان ترتیب قطعی اجرای **همه اسناد فروش** معرفی شود.

## تمرین کاربردی
سناریو: کاربر می‌خواهد فاکتور نهایی‌شده با سند انبار مرتبط را ابطال کند.

1. چه شرایطی ممکن است اجازه یا منع ابطال را مشخص کند؟
2. از چه API و Command شروع به trace می‌کنی؟
3. کدام side effectها را باید پیدا کنیم؟
4. اگر بعد از ذخیره وضعیت سند یک سرویس بیرونی fail شود، چه داده‌هایی باید reconcile شوند؟

### معیار پاسخ قابل‌اعتماد
پاسخ نظری باید از «واقعیت تأییدشده کد» جدا باشد. برای واقعیت کد، path + symbol + branch/commit + تاریخ بررسی و وضعیت محیط لازم است.

## اقدام عملی بعدی
- انتخاب یک نوع سند فروش و یک transition واقعی.
- استخراج entrypoint سمت UI و Backend، سپس Engine Controller/Command، Rule و SQL.
- ثبت یافته‌ها در یک جدول با ستون‌های `Claim | Source | Commit | Verification date | Confidence | Risk`.
- به‌روزرسانی [PROGRESS.md](PROGRESS.md) و [Issue #2](https://github.com/Mostafa-Babaei/TotalSalesSkill/issues/2).

## پرسش سریع مدیر
آیا اختلاف رفتار فروش تولیدی و پخشی الزاماً در کد است؟ **خیر.** ممکن است ناشی از configuration، status-flow metadata، Rule mapping یا implementation باشد؛ نسبت واقعی هرکدام باید بررسی شود.
