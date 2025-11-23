---
label: Android (Termux)
route: /installation/android-(termux)/
---

# تثبيت Android (Termux)

يمكن تشغيل SillyTavern محلياً على أجهزة Android باستخدام Termux.

## تثبيت Termux

!!!tip
تجنب تثبيت Termux من Google Play Store، فذلك الإصدار لم يعد يتم صيانته.
بدلاً من ذلك، استخدم F-Droid (موصى به) أو إصدارات GitHub للحصول على أحدث إصدار.
!!!

1. قم بتنزيل Termux من [F-Droid](https://f-droid.org/en/packages/com.termux/) أو [GitHub releases](https://github.com/termux/termux-app/releases).
2. قم بتثبيت ملف APK الذي تم تنزيله.
3. افتح Termux وقم بتشغيل أول أمر لك:

   ```bash
   termux-change-repo
   ```

4. اختر "Mirror group" واختر أقرب الخوادم إليك. يمكنك لمس الشاشة أو استخدام إيماءات السحب باستخدام [Unexpected Keyboard](https://play.google.com/store/apps/details?id=juloo.keyboard2&hl=en).
5. قم بتحديث Termux:

   ```bash
   pkg update && pkg upgrade
   ```

## تثبيت التبعيات

قم بتثبيت الحزم المطلوبة:

```bash
pkg install git nodejs-lts nano
```

!!!warning
إذا كنت تستخدم Android 32-bit، راجع قسم [الأخطاء الشائعة](#common-errors) أدناه للحصول على خطوات إضافية.
!!!

## تثبيت SillyTavern

استنسخ مستودع SillyTavern ([كيفية اختيار الفرع](/Installation/index.md#branches)):

- **فرع Release:**

    ```bash
    git clone https://github.com/SillyTavern/SillyTavern -b release
    ```

- **فرع Staging:**

    ```bash
    git clone https://github.com/SillyTavern/SillyTavern -b staging
    ```

## تشغيل SillyTavern

لتشغيل SillyTavern، انتقل إلى الدليل المستنسخ وقم بتشغيل سكريبت البدء:

```bash
cd ~/SillyTavern
bash start.sh
```

لتحديث SillyTavern، انتقل إلى دليل SillyTavern وقم بتشغيل:

```bash
cd ~/SillyTavern
git pull --rebase --autostash
```

راجع قسم [الاختصارات](#optional-create-aliases) أدناه لإنشاء اختصارات لتبسيط هذه العملية.

## الأخطاء الشائعة

### Unsupported platform: android arm LEtime-web

يتطلب Android 32-bit تبعية خارجية لا يمكن تثبيتها باستخدام npm.

استخدم الأمر التالي لتثبيتها:

```bash
pkg install esbuild
```

ثم تابع خطوات التثبيت أعلاه.

### تحسينات الأداء

!!!info
للحصول على نصائح عامة حول تحسين الأداء، راجع [قسم الأسئلة الشائعة](/Usage/faq.md#performance-tips) المعني.
!!!

نظراً لقيود الأجهزة على أجهزة Android، قد ترغب في ضبط إعدادات [config.yaml](/Administration/config-yaml.md) التالية في SillyTavern للحصول على استخدام أفضل للذاكرة والتخزين ووحدة المعالجة المركزية:

```yaml
performance:
  # تجنب تحميل جميع بيانات الشخصيات حتى الحاجة إليها
  lazyLoadCharacters: true
  # تعطيل التخزين المؤقت على القرص لتقليل استخدام التخزين
  useDiskCache: false
backups:
  chat:
    # اختياري: تعطيل النسخ الاحتياطية التلقائية للدردشة لتوفير مساحة التخزين
    enabled: false
```

!!!tip
استخدم محرر النصوص `nano` المضمن مع Termux لتحرير ملف `config.yaml`: `nano ~/SillyTavern/config.yaml`
!!!

## اختياري: إنشاء اختصارات

يمكنك إنشاء اختصارات للأوامر الشائعة لتسهيل سير عملك.

1. افتح محرراً لتعديل ملف `.bashrc` الخاص بك:

   ```bash
   nano ~/.bashrc
   ```

2. أضف الأسطر التالية لإنشاء اختصارات:

   ```bash
   # تحديث حزم Termux
   alias pkgup="pkg update && pkg upgrade"
   # تشغيل SillyTavern
   alias st='cd ~/SillyTavern && bash start.sh'
   # تحديث SillyTavern
   alias stup='cd ~/SillyTavern && git pull --rebase --autostash'
   ```

3. احفظ الملف واخرج من المحرر (في nano، اضغط على `CTRL + X`، ثم `Y`، ثم `Enter`).

4. لتطبيق التغييرات، قم بتشغيل:

   ```bash
   source ~/.bashrc
   ```

الآن يمكنك استخدام الأوامر التالية:

- `st` لبدء SillyTavern
- `stup` لتحديث SillyTavern
- `pkgup` لتحديث حزم Termux

## قراءة إضافية

!!!info
الأدلة المرتبطة أدناه لا يتم صيانتها من قبل فريق SillyTavern.
!!!

- دليل SillyTavern في Termux بواسطة ArroganceComplex#2659: <https://rentry.org/STAI-Termux>
- الوصول إلى ملفات Termux باستخدام Material Files: <https://www.learntermux.tech/2020/10/Termux-File-Manager.html>
- منع السبات العميق لعملية Termux: <https://wiki.termux.com/wiki/Termux-wake-lock>
