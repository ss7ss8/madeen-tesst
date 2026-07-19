# DebtTracker — تطبيق إدارة ديون الزبائن

<div dir="rtl">

تطبيق عربي متكامل لإدارة الديون في المحلات التجارية العراقية. يجمع بين البساطة في الاستخدام والقوة في الوظائف، مع دعم كامل للعمل offline ومزامنة سحابية. التصميم عصري يشبه تطبيقات البنوك الحديثة مع لمسة شرق أوسطية دافئة.

</div>

---

## ✨ الميزات الرئيسية

| الميزة | الوصف |
|--------|-------|
| 🔐 **المصادقة** | تسجيل دخول عبر Google Sign-In + قفل التطبيق (PIN/Biometric) |
| 👥 **إدارة الزبائن** | إضافة/تعديل/حذف الزبائن مع البحث والفلترة |
| 💰 **إدارة الديون** | ديون كلية/جزئية + ديون نقدية + نظام الأقساط |
| 📦 **المخزون** | إدارة المنتجات مع الكميات والأسعار والصور |
| 📊 **التقارير** | رسوم بيانية (يومي/أسبوعي/شهري) + تصدير PDF/Excel |
| 🔄 **مزامنة ذكية** | Offline-first مع قائمة انتظار + حل التعارضات |
| 📱 **إشعارات** | تذكيرات محلية للديون المستحقة |
| 💬 **واتساب** | إرسال تذكيرات للزبائن عبر WhatsApp |
| 🖨️ **طباعة** | توليد فواتير وكشوف حساب PDF |
| 🎨 **تخصيص** | وضع ليلي/نهاري + ألوان + خطوط + خلفيات |
| 🗑️ **سلة المحذوفات** | استعادة البيانات المحذوفة + سجل تدقيق |
| 📋 **اختصارات** | App shortcuts + Home Widget |

---

## 🛠️ التقنيات المستخدمة

- **Framework:** Flutter 3.8.x / Dart 3.8.1
- **State Management:** Riverpod 2.x
- **Local DB:** Hive (مع تشفير AES-256)
- **Remote DB:** Supabase (PostgreSQL)
- **Auth:** Supabase Auth (Google Sign-In)
- **Storage:** Supabase Storage
- **PDF:** `pdf` + `printing`
- **Charts:** `fl_chart`
- **Export:** `excel` + `share_plus`
- **Security:** `flutter_secure_storage` + `local_auth`
- **Connectivity:** `connectivity_plus`

---

## 📁 هيكل المشروع

```
lib/
├── main.dart                    # نقطة الدخول
├── core/
│   ├── config/app_config.dart   # إعدادات Supabase
│   ├── services/                # الخدمات الأساسية
│   │   ├── supabase_service.dart
│   │   ├── sync_service.dart
│   │   ├── notification_service.dart
│   │   ├── home_widget_service.dart
│   │   └── app_shortcuts_service.dart
│   └── utils/
│       ├── logger.dart          # نظام تسجيل منظم
│       └── formatters.dart
├── data/
│   ├── models/                  # نماذج البيانات + Hive adapters
│   └── local/hive_service.dart  # قاعدة البيانات المحلية
├── features/
│   ├── auth/                    # المصادقة + قفل التطبيق
│   ├── dashboard/               # الرئيسية + الإحصائيات
│   ├── customers/               # الزبائن + الديون + المدفوعات
│   ├── inventory/               # المخزون
│   ├── installments/            # الأقساط
│   ├── reports/                 # التقارير
│   └── settings/                # الإعدادات + سجل التدقيق + المحذوفات
├── shared/
│   ├── widgets/                 # ويدجت مشتركة
│   └── providers/               # مزودات مشتركة (theme, pdf, sync)
└── routing/home_shell.dart      # التنقل السفلي
```

---

## 🚀 التشغيل

### المتطلبات

- Flutter 3.8.x أو أحدث
- Dart 3.8.1 أو أحدث
- حساب Supabase

### الخطوات

1. **استنساخ المشروع:**
   ```bash
   git clone <repo-url>
   cd debt_tracker
   ```

2. **تثبيت الحزم:**
   ```bash
   flutter pub get
   ```

3. **إعداد Supabase:**
   - أنشئ مشروعاً جديداً على [Supabase](https://supabase.com)
   - عدّل `lib/core/config/app_config.dart` وضع URL و Key الخاصين بك
   - نفّذ مخطط قاعدة البيانات من `supabase/schema.sql` في SQL Editor

4. **إعداد Google Sign-In:**
   - فعّل Google Auth في Supabase
   - أضف SHA-1 و SHA-256 fingerprints في Google Cloud Console
   - حدّث `android/app/google-services.json`

5. **تشغيل التطبيق:**
   ```bash
   flutter run
   ```

---

## 🗄️ قاعدة البيانات

المخطط الكامل موجود في `supabase/schema.sql` (المصدر الوحيد للحقيقة).

> ⚠️ **ملاحظة:** جميع ملفات SQL الأخرى في `supabase/` وفي جذر المشروع هي ملفات تاريخية مهملة (deprecated) وتُحتفظ بها للمرجع فقط. لا تُنفّذها. راجع `supabase/README.md` للتفاصيل.

### الجداول الرئيسية

- `customers` — الزبائن
- `debts` — الديون
- `debt_items` — عناصر الدين
- `payments` — المدفوعات
- `products` — المنتجات
- `installments` — الأقساط
- `user_settings` — إعدادات المستخدم
- `audit_logs` — سجل التدقيق
- `sync_queue` — قائمة المزامنة

---

## 🔄 استراتيجية المزامنة (Offline-first)

1. جميع العمليات تُنفّذ محلياً في Hive أولاً
2. تُضاف لجدول `sync_queue`
3. عند توفر الاتصال (يُكتشف عبر `connectivity_plus`)، تُرفع البيانات لـ Supabase
4. حل التعارضات: last-write-wins مع timestamp

---

## 📜 الترخيص

هذا المشروع ملكية خاصة. جميع الحقوق محفوظة.