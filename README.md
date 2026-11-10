# VYRO — ثيم أزياء شبابي لمنصة سلة

ثيم متجر لمنصة [سلة](https://salla.sa) مبني على **Twilight React** (TanStack Start + Vite)، ويُعرض من الخادم عبر `@salla.sa/twilight-theme-engine`.

طلب المشروع الكامل (الأدوار، الهوية، معايير النجاح) موجود في [VYRO_PROMPT.md](./VYRO_PROMPT.md).

> المرجع الرسمي: https://docs.salla.dev/twilight/react/local-development

---

## 1. المتطلبات

| الأداة | الإصدار | ملاحظة |
| --- | --- | --- |
| Node.js | 22.22.2 أو أحدث (أو 24.15.0+، أو 26+) | توثيق سلة يذكر 20.19 كحد أدنى، وحزمة الثيم تطلب 22.22.2 — التزم بالأعلى. |
| pnpm | 10 أو أحدث | **إجباري.** npm وyarn ينزّلون نسخ مكررة من المكتبات المشتركة والثيم يتعطل وقت العرض. |
| git | أي إصدار | أداة سلة تبدأ سجل git للثيم. |
| Salla CLI | 3.x | لإنشاء الثيم وتشغيله ونشره. |

لا تحتاج حساب سلة أو حساب شركاء أو GitHub لإنشاء الثيم وتشغيله محلياً. تحتاج تسجيل الدخول فقط لربط متجرك الخاص، أو محرر الثيم، أو النشر.

## 2. تثبيت أداة سلة

```bash
npm install -g @salla.sa/cli
salla --version
```

> على أجهزة Mac بمعالج Apple: ثبّت Rosetta أولاً (`softwareupdate --install-rosetta`).
> على Windows PowerShell: إذا رُفض التثبيت، شغّل `Set-ExecutionPolicy RemoteSigned -Scope CurrentUser`.

## 3. فحص الجهاز

```bash
salla theme doctor
```

النتيجة المتوقعة:

```text
Everything a Salla React theme needs is installed.
```

إذا ظهرت مشكلة:

| المشكلة | الحل |
| --- | --- |
| Node قديم | `nvm install 22 && nvm use 22` |
| pnpm غير موجود | `corepack enable pnpm` |
| pnpm قديم | `corepack prepare pnpm@latest --activate` |
| git غير موجود | ثبّته من git-scm.com |

## 4. الإعداد

### الطريقة أ — العمل على هذا المستودع

```bash
git clone https://github.com/soroomai-gif/vyro-theme.git
cd vyro-theme
pnpm install
salla theme dev
```

### الطريقة ب — إنشاء ثيم جديد من نسخة سلة الرسمية

```bash
salla theme create
```

أجب على الأسئلة:
1. **النوع:** React (الافتراضي).
2. **الاسم:** VYRO (من 3 إلى 50 حرفاً).
3. **المتجر:** المتجر التجريبي (بدون تسجيل دخول) أو متجرك التجريبي (يطلب تسجيل الدخول).

لا تستخدم `--yes` إذا تبي خطوة ربط GitHub — هذا الخيار يتخطاها.
الأمر يوقف إذا كان المجلد موجود مسبقاً، والثيم يبدأ من نسخة ثيم «رائد» (`@salla.sa/theme-raed`).

## 5. التحقق محلياً

نفّذ كل خطوة وتأكد من النتيجة قبل البدء بالتطوير:

| # | الفحص | الأمر / الإجراء | النتيجة المطلوبة |
| --- | --- | --- | --- |
| 1 | الأدوات | `salla theme doctor` | بدون أخطاء |
| 2 | ملف الثيم | `pnpm check:manifest` | `twilight.json` سليم |
| 3 | الأنواع | `pnpm typecheck` | بدون أخطاء |
| 4 | الاختبارات | `pnpm test` | كلها ناجحة |
| 5 | التشغيل | `salla theme dev` | يفتح على `http://localhost:5173` |
| 6 | العربي | افتح `http://localhost:5173/ar` | المتجر يظهر من اليمين لليسار |
| 7 | الإنجليزي | افتح `http://localhost:5173/en` | المتجر يظهر من اليسار لليمين |
| 8 | البناء | `pnpm build` | ينتهي بدون أخطاء |
| 9 | GitHub | `git remote -v` | يظهر `soroomai-gif/vyro-theme` |

> الصفحة `/` وحدها فاضية — هذا طبيعي. افتح `/ar` أو `/en`.

## 6. الأوامر

| الأمر | الوظيفة |
| --- | --- |
| `salla theme dev` | تشغيل الثيم محلياً (يكتب `.env` بالمتجر المختار) |
| `salla theme dev --store` | اختيار متجر آخر |
| `salla theme preview` | تشغيل مع محرر الثيم (يحتاج تسجيل دخول) |
| `salla theme publish` | رفع الثيم وتقديمه للمراجعة (يحتاج تسجيل دخول) |
| `pnpm dev` | خادم Vite مباشرة (يظهر الخطأ الحقيقي إذا توقف التشغيل) |
| `pnpm build` | بناء نسخة الإنتاج |
| `pnpm preview` | معاينة نسخة الإنتاج |
| `pnpm typecheck` | فحص الأنواع |
| `pnpm test` | تشغيل الاختبارات (Vitest) |
| `pnpm check:manifest` | فحص `twilight.json` |

`salla theme dev` يكتب في `.env`:
- `VITE_STORE_DOMAIN` — المتجر اللي يقرأ منه الثيم بياناته (الافتراضي: المتجر التجريبي).
- `VITE_TWILIGHT_URL` — نسخة Twilight الاحتياطية، تُستخدم فقط إذا إعدادات المتجر ما حددت نسخة (`theme.twilight.version`).

## 7. حل المشاكل الشائعة

| المشكلة | السبب | الحل |
| --- | --- | --- |
| صفحة فاضية على `localhost:5173` | `/` ليست صفحة المتجر | افتح `/ar` أو `/en` |
| «Store is temporarily unavailable» | المتجر في `.env` غير صحيح أو غير عام | `salla theme dev --store` واختر متجر آخر |
| غيّرت المتجر وما تغيّر شي | Vite يقرأ `.env` مرة وحدة عند التشغيل | أعد تشغيل الخادم واحذف `?storeId=` من الرابط |
| «Port 5173 is busy» | المنفذ مستخدم | لا شي — الأداة تختار منفذ آخر وتطبعه |
| الخادم يتوقف قبل ما يجهز | خطأ في `vite.config.ts` أو استيراد مكسور | شغّل `pnpm dev` لرؤية الخطأ الحقيقي |

## 8. النشر

سلة تبني الثيم وتنشره على خوادمها — ارفع التعديلات لمستودع الثيم وشغّل `salla theme publish`. لا تُرفق مع الثيم أي إعدادات نشر أو كلمات مرور أو مفاتيح.

متطلبات القبول في متجر ثيمات سلة:
- 4 صور للثيم.
- تصنيف واحد.
- سعر بين 250 و50,000 ر.س للثيم العام.
- سجل تغييرات بالعربي والإنجليزي.
