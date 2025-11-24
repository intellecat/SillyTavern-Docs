---
order: 10
label: Windows
route: /installation/windows/
---
# تثبيت Windows

!!!warning
لا تقم بالتثبيت في أي مجلد يتحكم به Windows (Program Files، System32، إلخ).

لا تقم بتشغيل START.BAT بصلاحيات المسؤول

التثبيت على Windows 7 مستحيل لأنه لا يمكنه تشغيل NODEJS 18.16
!!!

## التثبيت عبر Git

1. قم بتثبيت [NodeJS](https://nodejs.org/en) (يوصى بأحدث إصدار LTS)
2. قم بتثبيت [Git for Windows](https://gitforwindows.org/)
3. افتح Windows Explorer (`Win+E`)
4. انتقل إلى أو أنشئ مجلداً غير خاضع للتحكم أو المراقبة من قبل Windows. (مثال: C:\MySpecialFolder\)
5. افتح موجه الأوامر داخل ذلك المجلد بالنقر على 'شريط العنوان' في الأعلى، وكتابة `cmd`، والضغط على Enter.
6. بمجرد ظهور المربع الأسود (موجه الأوامر)، اكتب أحد الأوامر التالية واضغط على Enter:

   - لفرع Release: `git clone https://github.com/SillyTavern/SillyTavern -b release`
   - لفرع Staging: `git clone https://github.com/SillyTavern/SillyTavern -b staging`

7. بمجرد اكتمال الاستنساخ، انقر نقراً مزدوجاً على `Start.bat` لجعل NodeJS يثبت متطلباته.
8. سيبدأ الخادم بعد ذلك، وسيظهر SillyTavern في متصفحك.

## التثبيت عبر SillyTavern Launcher

1.  على لوحة المفاتيح الخاصة بك: اضغط على **`WINDOWS + R`** لفتح مربع حوار Run. ثم، قم بتشغيل الأمر التالي لتثبيت git:
    ```shell
    cmd /c winget install -e --id Git.Git
    ```
2. على لوحة المفاتيح الخاصة بك: اضغط على **`WINDOWS + E`** لفتح File Explorer، ثم انتقل إلى المجلد الذي تريد تثبيت المشغل فيه. بمجرد الوصول إلى المجلد المطلوب، اكتب `cmd` في شريط العنوان واضغط على enter. ثم، قم بتشغيل الأمر التالي:
   ```shell
    git clone https://github.com/SillyTavern/SillyTavern-Launcher.git && cd SillyTavern-Launcher && start installer.bat
    ```

## التثبيت عبر GitHub Desktop
(هذا يسمح باستخدام git **فقط** في GitHub Desktop، إذا كنت تريد استخدام `git` على سطر الأوامر أيضاً، فأنت بحاجة أيضاً إلى تثبيت [Git for Windows](https://gitforwindows.org/))

1. قم بتثبيت [NodeJS](https://nodejs.org/en) (يوصى بأحدث إصدار LTS)
2. قم بتثبيت [GitHub Desktop](https://central.github.com/deployments/desktop/desktop/latest/win32)
3. بعد تثبيت GitHub Desktop، انقر على `Clone a repository from the internet....` (ملاحظة: **لا تحتاج** إلى إنشاء حساب GitHub لهذه الخطوة)

    ![image](/static/windows-1.png)

4. في القائمة، انقر على علامة التبويب URL، وأدخل عنوان URL هذا `https://github.com/SillyTavern/SillyTavern`، وانقر على Clone. يمكنك تغيير المسار المحلي لتغيير مكان تنزيل SillyTavern.

    ![image](/static/windows-2.png)

5. لفتح SillyTavern، استخدم Windows Explorer للتصفح إلى المجلد الذي استنسخت فيه المستودع. بشكل افتراضي، سيتم استنساخ المستودع هنا: `C:\Users\[Your Windows Username]\Documents\GitHub\SillyTavern`

6. انقر نقراً مزدوجاً على ملف `start.bat`. (ملاحظة: قد يكون جزء `.bat` من اسم الملف مخفياً بواسطة نظام التشغيل الخاص بك، في هذه الحالة، سيبدو كملف يسمى "`Start`". هذا هو ما تنقر عليه نقراً مزدوجاً لتشغيل SillyTavern)

    ![image](/static/windows-3.png)

7. بعد النقر المزدوج، يجب أن تفتح نافذة وحدة التحكم السوداء الكبيرة وسيبدأ SillyTavern في تثبيت ما يحتاجه للعمل.

8. بعد عملية التثبيت، إذا كان كل شيء يعمل، يجب أن تبدو نافذة وحدة التحكم هكذا ويجب أن تكون علامة تبويب SillyTavern مفتوحة في متصفحك:

    ![image](/static/windows-4.png)

9. اتصل بأي من [واجهات برمجة التطبيقات المدعومة](/Usage/API_Connections/index.md) وابدأ الدردشة!
