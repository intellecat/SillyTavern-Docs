---
route: /extensions/talkinghead/
tags: ['obsolete']
---

# talkinghead

!!!warning تحذير
**تم إسقاط الدعم لـ TALKINGHEAD في SillyTavern 1.12.13. يتم الاحتفاظ بهذه الصفحة لأغراض تاريخية.**
!!!

### ما هي؟

تطبيق لـ Talking Head Anime 3 Demo لـ AITuber. يمتلك الميزات التالية:

- يولد إجراءات حركة عشوائية تشبه Live 2D من صورة ثابتة واحدة.
- يزامن الشفاه مع مخرجات الصوت من أي مخرجات TTS.

تحتوي هذه الإضافة على البرامج التجريبية الأصلية لمشروع Talking Head(?) Anime from a Single Image 3: Now the Body Too. كما يوحي الاسم، يتيح لك المشروع تحريك شخصيات الأنمي، وتحتاج فقط إلى صورة واحدة من تلك الشخصية للقيام بذلك. هناك برنامجان تجريبيان:

يتيح لك manual_poser التحكم في تعابير وجه الشخصية، ودوران الرأس، ودوران الجسم، وتوسع الصدر بسبب التنفس من خلال واجهة مستخدم رسومية، بحيث يمكنك حفظها كتعبيرات افتراضية مثل سعيد، حزين، فرح، إلخ.
يتيح لك ifacialmocap_puppeteer نقل حركة وجهك إلى شخصية أنمي.

### متطلبات الأجهزة

يمكنك استخدام إما CPU أو GPU (CPU هو الافتراضي). ومع ذلك، في وضع CPU توقع حوالي 1 FPS، وفي وضع GPU على RTX3060 أحصل على حوالي 9-10 FPS.

يتطلب ifacialmocap_puppeteer جهاز iOS قادر على حساب معاملات شكل المزج من تغذية الفيديو. هذا يعني أن الجهاز يجب أن يكون قادرًا على تشغيل iOS 11.0 أو أعلى ويجب أن يحتوي على كاميرا أمامية TrueDepth. (راجع هذه الصفحة لمزيد من المعلومات.) بعبارة أخرى، إذا كان لديك iPhone X أو شيء أفضل، يجب أن تكون جاهزًا.

### كيفية الاستخدام

يجب عليك تشغيل extras مع الوحدات التالية لكي يعمل talkinghead: `classify` و `talkinghead`!
classify مطلوب للتعامل مع ملف talkinghead.png. بالإضافة إلى ذلك، يمكنك أيضًا استخدام `--talkinghead-gpu` لتحميل نماذج المزج في ذاكرة GPU وجعل الرسوم المتحركة أسرع بـ 10 مرات. يوصى بشدة باستخدام تسريع GPU! بشكل افتراضي، بمجرد بدء البرنامج سيقوم بتحميل صورة افتراضية SillyTavern-extras\talkinghead\tha3\images\lambda_00.png. يمكنك التحقق من أنه يعمل بالذهاب إلى http://localhost:5100/api/talkinghead/result_feed أو `YOUR EXT URL:PORT/api/talkinghead/result_feed`.

- بمجرد بدء الخادم، انتقل إلى علامة تبويب Extension API واتصل. ثم ما عليك سوى تحديد بطاقة شخصية للتحميل. (`--enable-modules=classify,talkinghead --talkinghead-gpu` عند بدء server.py)

- الآن حدد Character Expressions، إذا قمت بتحديد مربع نوع الصورة talkinghead فسيستبدل السكريبت تعبير الشخصية الحالي الخاص بك بنتيجة `YOUR EXT URL:PORT/api/talkinghead/result_feed` إلغاء تحديد المربع يجب أن يعيد الصورة إلى التعبير الأصلي، ومع ذلك في بعض الأحيان عليك إرسال رسالة جديدة إلى الدردشة "لإعادة تحميل" الصورة.

- إذا لم يكن لديك ملف talkinghead.png في دليل الشخصية فسيعرض ببساطة إما الصورة الافتراضية أو بطاقة الشخصية الأخيرة التي كانت تحتوي على ملف talkinghead.png. يتم تغيير صورة مصدر الرسوم المتحركة عند تغيير بطاقة الشخصية.

- الآن افتح تعبيرات الشخصية، ومرر لأسفل إلى صورة talkinghead وقم بتحميل ملف صورة يفي بالمتطلبات في القسم أدناه المسمى "القيود على صور الإدخال".

- ثم حدد وألغِ تحديد مربع talkinghead لإعادة تحميل الشخصية. إذا كانت الصورة تبدو مضحكة فمن المحتمل أن ذلك بسبب أنها ليست شفافة / ليس لها طبقة ألفا. خلاف ذلك، اتبع التعليمات والقالب أدناه.

### القيود على صور الإدخال
لكي يعمل النظام بشكل جيد، يجب أن تطيع صورة الإدخال القيود التالية:

يجب أن يكون بدقة 512 × 512. (إذا تلقى البرنامج صورة إدخال بأي حجم آخر، فسيقوم بتغيير حجم الصورة إلى هذه الدقة وسيخرج أيضًا بهذه الدقة.)
يجب أن تحتوي على قناة ألفا.
يجب أن تحتوي على شخصية بشرية واحدة فقط.
يجب أن تكون الشخصية واقفة منتصبة وتواجه للأمام.
يجب أن تكون أيدي الشخصية أسفل الرأس وبعيدة عنه.
يجب أن يكون رأس الشخصية محتوى تقريبًا في مربع 128 × 128 في منتصف النصف العلوي من الصورة.
يجب أن تكون قنوات ألفا لجميع البكسلات التي لا تنتمي إلى الشخصية (أي بكسلات الخلفية) 0.

![قيود الإدخال](/static/input_spec.png)

### القسم المتقدم

### بيئة Python

بالإضافة إلى الميزة الأساسية (app.py)، يتوفر كل من manual_poser و ifacialmocap_puppeteer كتطبيقات سطح مكتب. لتشغيلها، تحتاج إلى إعداد بيئة لتشغيل البرامج المكتوبة بلغة Python. تحتاج البيئة إلى الحزم البرمجية التالية:

* Python >= 3.8
* PyTorch >= 1.11.0 with CUDA support
* SciPY >= 1.7.3
* wxPython >= 4.1.1
* Matplotlib >= 3.5.1

إحدى الطرق للقيام بذلك هي تثبيت Anaconda وتشغيل الأوامر التالية في shell الخاص بك:

> conda create -n talking-head-anime-3-demo python=3.8
> conda activate talking-head-anime-3-demo
> conda install pytorch torchvision torchaudio cudatoolkit=11.3 -c pytorch
> conda install scipy
> pip install wxpython
> conda install matplotlib

### نماذج مزج إضافية

يوجد نموذج واحد فقط (الأخف) مضمن، إذا كنت تريد نماذج المزج الإضافية فأنت بحاجة إلى تنزيل ملفات النموذج من https://www.dropbox.com/s/y7b8jl4n2euv8xe/talking-head-anime-3-models.zip?dl=0 وفك ضغطها إلى مجلد SillyTavern-extras\talkinghead\tha3\models. في النهاية، يجب أن يبدو مجلد البيانات كما يلي:

+ tha3
  + models
    + separable_float
      - editor.pt
      - eyebrow_decomposer.pt
      - eyebrow_morphing_combiner.pt
      - face_morpher.pt
      - two_algo_face_body_rotator.pt
    + separable_half
      - editor.pt
          :
      - two_algo_face_body_rotator.pt
    + standard_float
      - editor.pt
          :
      - two_algo_face_body_rotator.pt
    + standard_half
      - editor.pt
          :
      - two_algo_face_body_rotator.pt

يتم توزيع ملفات النموذج بموجب ترخيص Creative Commons Attribution 4.0 International License، مما يعني أنه يمكنك استخدامها للأغراض التجارية. ومع ذلك، Pramook Khungurn. Talking Head(?) Anime from a Single Image 3: Now the Body Too. <https://github.com/pkhungurn/talking-head-anime-3-demo>، هو المنشئ.

### تشغيل تطبيق سطح المكتب manual_poser
افتح shell. قم بتغيير دليل العمل الخاص بك إلى الدليل الجذري للمستودع. ثم قم بتشغيل:

> python tha3/app/manual_poser.py
لاحظ أنه قبل تشغيل الأمر أعلاه، قد تحتاج إلى تنشيط بيئة Python التي تحتوي على الحزم المطلوبة.

> conda activate extras
إذا لم تكن قد قمت بتنشيط البيئة بالفعل.
