---
route: /usage/api-connections/tabbyapi/
---

# TabbyAPI
تطبيق مبني على FastAPI يسمح بتوليد النصوص باستخدام LLM باستخدام واجهة Exllamav2 الخلفية، مع دعم لنماذج Exl2 و GPTQ و FP16.

* [GitHub](https://github.com/theroyallab/tabbyAPI)

### بداية سريعة
1. اتبع [تعليمات التثبيت](https://github.com/theroyallab/tabbyAPI/wiki/01.-Getting-Started) على TabbyAPI GitHub الرسمي.
2. [أنشئ config.yml الخاص بك](https://github.com/theroyallab/tabbyAPI/wiki/02.-Server-options) لتعيين مسار النموذج، والنموذج الافتراضي، وطول التسلسل، إلخ. يمكنك تجاهل معظم (إن لم يكن كل) هذه الإعدادات إذا كنت تريد.
3. قم بتشغيل TabbyAPI. إذا نجح، يجب أن ترى شيئاً مثل هذا:

    ![طرفية TabbyAPI](/static/tabby-terminal.png)

4. تحت Text Completion API في SillyTavern، حدد TabbyAPI.
5. انسخ مفتاح API الخاص بك من طرفية TabbyAPI إلى `Tabby API key` وتأكد من أن `API URL` الخاص بك صحيح (يجب أن يكون `http://127.0.0.1:5000` افتراضياً).

إذا قمت بكل شيء بشكل صحيح، يجب أن ترى شيئاً مثل هذا في SillyTavern:

![TabbyAPI SillyTavern](/static/tabby-config.png)

يمكنك الآن الدردشة باستخدام TabbyAPI!

### TabbyAPI Loader
أنشأ مطورو TabbyAPI امتداداً رسمياً لتحميل/إلغاء تحميل النماذج مباشرة من SillyTavern. التثبيت بسيط:
1. في SillyTavern، انقر على علامة تبويب Extensions وانتقل إلى Download Extensions & Assets.
2. انسخ `https://raw.githubusercontent.com/theroyallab/ST-repo/main/index.json` في Assets URL وانقر على زر القابس على اليمين.
3. يجب أن ترى شيئاً مثل هذا. انقر على زر التنزيل بجوار Tabby Loader.

    ![Tabby Loader](/static/tabby-assets.png)

4. إذا نجح التثبيت، يجب أن ترى رسالة منبثقة خضراء في أعلى شاشتك. ضمن علامة تبويب الامتدادات، انتقل إلى TabbyAPI Loader وانسخ مفتاح الإدارة الخاص بك من طرفية TabbyAPI إلى Admin Key.
5. انقر على زر التحديث بجوار Model Select. عندما تنقر على مربع النص أسفله مباشرة، يجب أن ترى جميع النماذج في دليل النموذج الخاص بك.

![امتداد Tabby Loader](/static/tabby-loader.png)

يمكنك الآن تحميل وإلغاء تحميل نماذجك مباشرة من SillyTavern!

### الدعم
لا تزال بحاجة إلى المساعدة؟ قم بزيارة [TabbyAPI GitHub](https://github.com/theroyallab/tabbyAPI) للحصول على رابط لخادم Discord الرسمي للمطور و[اقرأ wiki](https://github.com/theroyallab/tabbyAPI/wiki/1.-Getting-Started).
