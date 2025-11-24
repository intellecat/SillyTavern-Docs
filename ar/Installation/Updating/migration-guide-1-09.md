---
order: 109
route: /installation/updating/migration-guide-1-09/
---

# دليل الترحيل 1.9.0

## كيفية الترحيل إلى فرع جديد إذا كنت أستخدم main/dev؟

_**يُوصى بإجراء تثبيت جديد.**_ ومع ذلك، إذا كنت ترغب في استخدام نسخة موجودة من SillyTavern، فيرجى اتباع الإرشادات أدناه.

**مهم!** قبل القيام بأي شيء، قم بعمل *نسخة احتياطية كاملة* من التثبيت الخاص بك. قد *تفقد بياناتك* في العملية، لذا لا تتجاهل هذا التحذير.

غير متأكد من الملفات التي يجب عمل نسخة احتياطية منها؟ راجع القائمة هنا: [كيفية تحديث SillyTavern](/Installation/Updating/index.md#updating-from-1120-to-1120)

### تثبيتات git

1. افتح موجه terminal (cmd، PowerShell، Termux، إلخ) في مجلد تثبيت SillyTavern الخاص بك.
2. اكتب `git fetch` ثم `git pull` لسحب التحديثات.
3. قد تفقد إعداداتك. هل قمت بعمل نسخة احتياطية؟ `git switch release` أو `git switch staging` سيغير فرعك، على التوالي
4. تخطَّ إلى العنصر التالي إذا لم يكن لديك أخطاء. قد يكون لديك شيء مثل:
   ```
   error: Your local changes to the following files would be overwritten by checkout:
        config.conf
        public/css/bg_load.css
        public/settings.json
   ```
   سترى قائمة بالملفات المتأثرة. إذا كنت لا تهتم باستبدال ملفات الإعدادات هذه، فإن `git switch -f release` أو `git switch -f staging` سيعين فرعك.
   إذا كنت تهتم بحفظ تلك التغييرات، قم بالاستعادة من النسخة الاحتياطية.

5. اكتب `npm install` ثم `npm run start` لاختبار أن كل شيء يتصرف بشكل صحيح.
6. استمتع! قم باستعادة بياناتك من نسخة احتياطية إذا لزم الأمر.

### fatal: invalid reference: release

قد يحدث هذا إذا قمت باستنساخ فرع واحد فقط من remote قديم (قبل الترحيل إلى مستودع المنظمة). لإصلاح هذا، تحتاج إلى إضافة وجلب فرع من remote جديد:

```
git remote add st https://github.com/SillyTavern/SillyTavern
git fetch st
git checkout -t st/release
```

ثم تابع من الخطوة 5.

### تثبيتات ZIP

لا شيء يتغير بالنسبة لك. فقط قم بتنزيل zip الفرع/الإصدار كالمعتاد.
