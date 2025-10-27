# تعليمات بناء تطبيق إحصاء منتجات السوبرماركت عبر GitHub

## الخطوة 1: إنشاء مستودع GitHub جديد

1. قم بتسجيل الدخول إلى حسابك على [GitHub](https://github.com/)
2. انقر على زر "+" في الزاوية العلوية اليمنى واختر "New repository"
3. أدخل اسم المستودع (مثلاً: `supermarket-inventory-app`)
4. اختر ما إذا كنت تريد أن يكون المستودع عاماً أو خاصاً
5. اختياري: قم بإضافة ملف README
6. انقر على "Create repository"

## الخطوة 2: رفع المشروع إلى GitHub

### باستخدام واجهة الويب:
1. انتقل إلى المستودع الذي أنشأته
2. انقر على "Add file" ثم "Upload files"
3. اسحب وأفلت ملفات المشروع أو انقر لاختيار الملفات
4. أضف رسالة الالتزام (commit message) واضغط على "Commit changes"

### باستخدام Git من سطر الأوامر:
```bash
# انتقل إلى مجلد المشروع
cd "c:\Users\nesyou\Documents\trae_projects\phone app"

# قم بتهيئة مستودع Git محلي
git init

# أضف جميع الملفات إلى منطقة التحضير
git add .

# قم بعمل commit
git commit -m "النسخة الأولية من تطبيق إحصاء منتجات السوبرماركت"

# قم بربط المستودع المحلي بمستودع GitHub
git remote add origin https://github.com/YOUR_USERNAME/supermarket-inventory-app.git

# ادفع التغييرات إلى GitHub
git push -u origin main
```

## الخطوة 3: إعداد GitHub Actions لبناء التطبيق تلقائياً

1. في مستودع GitHub، انتقل إلى تبويب "Actions"
2. انقر على "New workflow"
3. اختر "set up a workflow yourself"
4. أنشئ ملف `.github/workflows/flutter-build.yml` بالمحتوى التالي:

```yaml
name: Flutter Build

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Flutter
        uses: subosito/flutter-action@v2
        with:
          flutter-version: '3.10.0'
          channel: 'stable'
      
      - name: Get dependencies
        run: flutter pub get
        
      - name: Build APK
        run: flutter build apk --release
        
      - name: Upload APK
        uses: actions/upload-artifact@v3
        with:
          name: app-release
          path: build/app/outputs/flutter-apk/app-release.apk
```

5. انقر على "Start commit" ثم "Commit new file"

## الخطوة 4: تنزيل التطبيق المبني

1. بعد اكتمال عملية البناء، انتقل إلى تبويب "Actions" في مستودع GitHub
2. انقر على أحدث عملية بناء ناجحة
3. قم بالتمرير لأسفل إلى قسم "Artifacts"
4. انقر على "app-release" لتنزيل ملف APK

## الخطوة 5: تثبيت التطبيق على الهاتف

1. انقل ملف APK الذي تم تنزيله إلى هاتفك (عبر البريد الإلكتروني، أو USB، أو خدمة مشاركة الملفات)
2. على هاتفك، افتح تطبيق "الملفات" أو "مدير الملفات"
3. انتقل إلى موقع ملف APK
4. انقر على الملف لبدء عملية التثبيت
5. اتبع التعليمات على الشاشة لإكمال التثبيت

## ملاحظات هامة

- تأكد من تمكين "تثبيت من مصادر غير معروفة" في إعدادات هاتفك
- قد تحتاج إلى تعديل ملف `pubspec.yaml` للتأكد من أن جميع الاعتمادات متوافقة
- إذا واجهت مشاكل في البناء، تحقق من سجلات GitHub Actions للحصول على تفاصيل الخطأ

## استكشاف الأخطاء وإصلاحها

### مشكلة: فشل البناء بسبب إصدار Flutter
- الحل: قم بتعديل إصدار Flutter في ملف workflow (`.github/workflows/flutter-build.yml`)

### مشكلة: فشل البناء بسبب اعتمادات غير متوافقة
- الحل: تحقق من ملف `pubspec.yaml` وقم بتحديث الاعتمادات إلى إصدارات متوافقة

### مشكلة: التطبيق لا يعمل على الهاتف
- الحل: تأكد من أن هاتفك يلبي الحد الأدنى من متطلبات النظام (Android 5.0 أو أحدث)