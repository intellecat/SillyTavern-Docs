---
route: /extensions/vrm/
---

# VRM

سيرشدك هذا الدليل خلال عملية إعداد وتخصيص إضافة VRM لتجربة SillyTavern الخاصة بك. تتيح لك هذه الإضافة استخدام نماذج VRM المتحركة لشخصيتك، مما يوفر عنصرًا ديناميكيًا وتفاعليًا لشخصيتك الافتراضية.

## المتطلبات المسبقة

قبل أن تبدأ، تأكد من استيفاء المتطلبات المسبقة التالية:

1. **اختيار الفرع**: تأكد من أنك تستخدم أحدث إصدار من فرع SillyTavern للوصول إلى أحدث الميزات والتحديثات.

2. **تثبيت الإضافة**: قم بتثبيت إضافة "VRM" من قائمة "Download Extensions & Assets" في لوحة Extensions (ممثلة بأيقونة المكعبات المكدسة).

3. **وضع ملف النموذج**: ضع ملفات نموذج VRM الخاصة بك (.vrm) في مجلد `/data/<user-handle>/assets/vrm/model` وملفات الحركة الخاصة بك في مجلد `/data/<user-handle>/assets/vrm/animation`. تنسيقات ملفات الحركة المدعومة حاليًا هي .fbx و .bvh المتوافقة مع نماذج VRM. يتضمن ذلك أي حركة يمكنك الحصول عليها من Mixamo (https://www.mixamo.com/) وأي حركة يمكنك تصديرها من أدوات مثل XR Animator (https://github.com/ButzYung/SystemAnimatorOnline).

## إعدادات الإضافة

توفر إضافة VRM إعدادات متنوعة لتخصيص سلوك نموذجك المتحرك. فيما يلي الإعدادات الرئيسية:

![UI global settings](/static/extensions/vrm-global.png)

### الإعدادات العامة

1. **Enabled**:
   - قم بتمكين خانة الاختيار هذه لتنشيط الإضافة، مما يسمح لنموذج VRM الخاص بك بالتفاعل داخل SillyTavern.
   - يمكنك تعطيل الإضافة إذا كنت تريد استخدام الصور العادية فقط.

2. **Look at camera**:
   - قم بتمكين خانة الاختيار هذه لجعل عيون نموذج VRM تنظر إلى الكاميرا.

3. **Blink**:
   - قم بتمكين خانة الاختيار هذه لجعل عيون نموذج VRM ترمش على فترات عشوائية. يجب أن تحدد تعبيرات النموذج خاصية وزن الرمش بشكل صحيح وإلا قد يرمش النموذج بعيون مغلقة على سبيل المثال، إذا حدث ذلك إما:
    - قم بتصحيح النموذج إذا كان لديك ملف .vroid
    - لا تستخدم تعبير الوجه غير الصحيح هذا
    - قم بتعطيل الرمش تمامًا بهذه خانة الاختيار

4. **TTS Lip sync**
    - قم بتمكين خانة الاختيار هذه لجعل حركة فم VRM تتبع صوت TTS الخاص بك عند تشغيله. يعمل فقط مع TTS الذي يتم تشغيل صوته بواسطة Sillytavern نفسه مثل XTTS (ليس في وضع البث). إذا تم تعطيله، سيتم تحريك الفم وفقًا لطول نص الرسالة عند استلام رسالة شخصية جديدة.

5. **Auto-send Interaction**:
   - قم بتمكين خانة الاختيار هذه لتشغيل تفاعلات الشخصية تلقائيًا عند النقر على المناطق ذات الرسائل المعينة (راجع قسم مناطق الضربات للحصول على التفاصيل).

### إعدادات الأداء

1. **Body hitboxes**
    - قم بتمكين خانة الاختيار هذه لتنشيط الكشف عن النقر على عدة أجزاء من نموذج VRM اعتمادًا على النموذج يمكن الكشف عن المنطقة التالية: head/chest/hands/groin/butt/legs/feets. يتم حساب مواقع Hitboxes في كل إطار وتتبع حركة الجسم، يمكن أن يؤدي تعطيل هذا الخيار إلى تحسين الأداء.

2. **Use model cache**
    - قم بتمكين خانة الاختيار هذه للاحتفاظ بنموذج VRM في الذاكرة عند التبديل بين النماذج، مما يسمح بالعودة إلى النموذج السابق بشكل أسرع. مفيد إذا كنت تستخدم نماذج مختلفة لنفس الشخصية لتغيير الزي أو الشكل على سبيل المثال. يمكن أن يؤثر على الأداء.

3. **Use animation cache**
    - قم بتمكين خانة الاختيار هذه للاحتفاظ بجميع الحركات التي تم تشغيلها أثناء الجلسة في الذاكرة. سيتم أيضًا تحميل جميع الحركات المعينة لنموذج في المرة الأولى التي يظهر فيها النموذج. سيزيد من الوقت الذي تستغرقه لتحميل النموذج في المرة الأولى لكنه يجعل تبديل جميع الحركات فوريًا. يمكن أن يؤثر على الأداء.

### إعدادات التصحيح

1. **Show grid**
    - قم بتمكين خانة الاختيار هذه لتصور الشبكة ثلاثية الأبعاد ومربع سحب النموذج و hitboxes الجسم.

2. **Reload button**
    - انقر فوق هذا الزر لإعادة تحميل المشهد ثلاثي الأبعاد، ومسح ذاكرة التخزين المؤقت وجميع نماذج VRM. استخدمه إذا حدثت بعض الأخطاء أو إذا بدأت ذاكرة التخزين المؤقت في التأثير على الأداء.

### إعدادات المشهد

![UI scene settings](/static/extensions/vrm-scene.png)

1. **Light Color**
    - اضبط لون الضوء في المشهد ثلاثي الأبعاد. انقر فوق زر إعادة التعيين لإعادة ضبطه إلى اللون الأبيض الافتراضي. اعتمادًا على المتصفح الخاص بك، يمكنك استخدام منتقي الألوان، على سبيل المثال يمكنك اختيار لون صورة الخلفية الخاصة بك لإضافة المزيد من الانغماس.

2. **Light intensity**
    - اضبط شدة الضوء بالنسبة المئوية باستخدام شريط التمرير. انقر فوق زر إعادة التعيين لإعادة ضبطه إلى القيمة الافتراضية وهي 100٪. يمكن أن يتفاعل نموذج VRM بشكل مختلف مع الضوء اعتمادًا على shaders المخبوزة في النموذج، العب بالقيمة وشاهد كيف يسير الأمر.

![UI model settings](/static/extensions/vrm-model.png)

## اختيار الشخصية

تتيح لك هذه الإعدادات إدارة الشخصيات وتعيين نماذج VRM لها.

1. **Refresh Button**:
   - انقر فوق زر التحديث لتحديث قائمة الشخصيات في الدردشة الحالية.

2. **Select Character**:
   - استخدم القائمة المنسدلة لاختيار شخصية لتعيين نموذج VRM لها.

3. **Remove Button**:
   - انقر فوق هذا الزر لحذف النموذج المعين لشخصية.

## اختيار النموذج

1. **Refresh Button**:
   - انقر فوق زر التحديث إذا لم يظهر نموذج VRM الخاص بك في القائمة.

2. **Select Model**:
   - اختر نموذجًا من القائمة لتعيينه للشخصية المحددة.
   - يجب أن يكون النموذج موجودًا في مجلد `/data/<user-handle>/assets/vrm/model`.

3. **Reset button**
    - انقر فوق هذا الزر لإعادة تعيين إعدادات النموذج إلى الإعدادات الافتراضية. إذا كانت لديك ملفات حركة تتوافق مع القيمة الافتراضية، فسيتم تعيينها تلقائيًا. راجع تعيين التسمية في نهاية README هذا.

## إعدادات النموذج

1. **Model Scale**:
   - استخدم شريط التمرير لضبط حجم النموذج، مما يجعله أكبر أو أصغر.

2. **Model Center X/Y Offset**:
   - استخدم أشرطة التمرير هذه لتغيير الموضع الأفقي/الرأسي للنموذج بالنسبة لمركز النافذة.

3. **Model X/Y Rotation**
    - استخدم أشرطة التمرير هذه لتغيير الدوران الأفقي/الرأسي للنموذج بالنسبة لوركي النموذج.

### ملاحظات
    - يتم حفظ الإعدادات لكل نموذج وليس لكل شخصية وتنتقل عبر دردشات مختلفة.
    - إذا كنت تريد استخدام نفس النموذج لشخصيتين مختلفتين بإعدادات مختلفة، فقم بعمل نسخة من ملف .vrm.
    - يمكنك أيضًا سحب النموذج بالماوس، وسيتم تحديث هذه الإعدادات وحفظها. انقر بزر الماوس الأيسر واضغط لسحب نموذج حول الشاشة. انقر بزر الماوس الأوسط واضغط لتدوير النموذج أو استخدم shift+النقر الأيسر. استخدم عجلة الماوس مع المؤشر على النموذج لتكبيره أو تصغيره أو استخدم ctrl+النقر الأيسر.
    - استخدم إعدادات UI هذه لإعادة نموذجك إلى الشاشة إذا قمت بطريقة ما بإخراجه من العرض. تحقق أيضًا من خانة الاختيار "Show frame" لرؤية بوضوح حيث يمكنك النقر لسحب النموذج.

![UI hitboxes settings](/static/extensions/vrm-hitboxes.png)

## تعيين Hitboxes

    - اعتمادًا على تعريف عظام النموذج، يمكن إنشاء بعض مناطق hitboxes، وسيتم إدراجها في هذا الجزء من UI، ويمكنك تعيين تعبير/حركة/رسالة لكل منها سيتم تشغيلها عند النقر على المنطقة.

![UI classify settings](/static/extensions/vrm-classify.png)

## تعيين التعبيرات المصنفة

1. **المتطلبات**
    - يتطلب استخدام إضافة classify expression؛ وإلا فسيعود إلى الحركة الافتراضية.

2. **التعيين**
    - لكل عاطفة يتم اكتشافها بواسطة إضافة classify، يمكنك تعيين تعبير/حركة/رسالة. يمكن أن تحتوي الرسالة على أوامر.

## الأوامر

1. **/vrmlightcolor**
    - تعيين لون الضوء
    - المعاملات: color
    - مثال: "/vrmlightcolor white" أو "/vrmlightcolor purple".
2. **/vrmlightintensity**
    - تعيين شدة الضوء بالنسبة المئوية
    - المعاملات: intensity
    - مثال: "/vrmlightintensity 0" أو "/vrmlightintensity 100
3. **/vrmmodel**
    - تعيين نموذج vrm للشخصية
    - المعاملات: character, model
    - مثال: "/vrmmodel Seraphina.vrm" في دردشة فردية أو "/vrmmodel character=Seraphina model=Seraphina.vrm" في دردشة جماعية
4. **/vrmexpression**
    - تغيير تعبير النموذج
    - المعاملات: character, expression
    - مثال: "/vrmexpression happy" في دردشة فردية أو "/vrmexpression character=Seraphina expression=happy" في دردشة جماعية

5. **/vrmmotion**
    - تغيير حركة النموذج
    - المعاملات: character, motion, loop, random
    - "/vrmmotion idle" أو "/vrmmotion character=Seraphina motion=idle loop=true random=false"

## تعيين الحركات الافتراضي
إذا كانت ملفات الحركة الخاصة بك مسماة بالطريقة التالية، فسيتم تعيينها تلقائيًا عند إعادة تعيين إعدادات النموذج. على سبيل المثال، الملفات المسماة "assets/vrm/animation/neutral.bvh" و "assets/vrm/animation/neutral1.fbx" سيتم تعيينها تلقائيًا كمجموعة للحركة الافتراضية والحركة المصنفة المحايدة. الأمر نفسه ينطبق على hitboxes.

    // Fallback
    "default": "assets/vrm/animation/neutral",

    // Classify class
    "admiration": "assets/vrm/animation/admiration",
    "amusement": "assets/vrm/animation/amusement",
    "anger": "assets/vrm/animation/anger",
    "annoyance": "assets/vrm/animation/annoyance",
    "approval": "assets/vrm/animation/approval",
    "caring": "assets/vrm/animation/caring",
    "confusion": "assets/vrm/animation/confusion",
    "curiosity": "assets/vrm/animation/curiosity",
    "desire": "assets/vrm/animation/desire",
    "disappointment": "assets/vrm/animation/disappointment",
    "disapproval": "assets/vrm/animation/disapproval",
    "disgust": "assets/vrm/animation/disgust",
    "embarrassment": "assets/vrm/animation/embarrassment",
    "excitement": "assets/vrm/animation/excitement",
    "fear": "assets/vrm/animation/fear",
    "gratitude": "assets/vrm/animation/gratitude",
    "grief": "assets/vrm/animation/grief",
    "joy": "assets/vrm/animation/joy",
    "love": "assets/vrm/animation/love",
    "nervousness": "assets/vrm/animation/nervousness",
    "neutral": "assets/vrm/animation/neutral",
    "optimism": "assets/vrm/animation/optimism",
    "pride": "assets/vrm/animation/pride",
    "realization": "assets/vrm/animation/realization",
    "relief": "assets/vrm/animation/relief",
    "remorse": "assets/vrm/animation/remorse",
    "sadness": "assets/vrm/animation/sadness",
    "surprise": "assets/vrm/animation/surprise",

    // Hitboxes
    "head": "assets/vrm/animation/hitarea_head",
    "chest": "assets/vrm/animation/hitarea_chest",
    "groin": "assets/vrm/animation/hitarea_groin",
    "butt": "assets/vrm/animation/hitarea_butt",
    "leftHand": "assets/vrm/animation/hitarea_hands",
    "rightHand": "assets/vrm/animation/hitarea_hands",
    "leftLeg": "assets/vrm/animation/hitarea_leg",
    "rightLeg": "assets/vrm/animation/hitarea_leg",
    "rightFoot": "assets/vrm/animation/hitarea_foot",
    "leftFoot": "assets/vrm/animation/hitarea_foot"

شكرًا لك على اتباع هذا الدليل! تجربة SillyTavern الخاصة بك الآن مثرية بنماذج ثلاثية الأبعاد متحركة وتفاعلية.

## ملاحظات
    - نماذج VRM التي تم تحميلها بواسطة هذه الإضافة هي ملفات .vrm وليست ملفات .vroid.
    - يجب أن تكون ملفات الحركة متوافقة مع VRM، يمكنك استخدام أداة مثل XR animation (https://github.com/ButzYung/SystemAnimatorOnline) لتحويل ملف حركة fbx/bvh.
    - يمكنك إنشاء مجموعات حركة بوجود ملفات بنفس الاسم تنتهي بأرقام مختلفة على سبيل المثال: "idle1.bvh"، "idle2.bhv"، "idle3.bvh" ستعتبر كمجموعة واحدة "idle" وعند اختيارها في تعيين سيتم تشغيل واحدة عشوائية عند التشغيل، يمكن استخدامها لإضافة تنوع إلى الحركات.
    - يمكنك الحصول على حركات منسقة من هذا المستودع: https://github.com/test157t/VRM-Animations-Pack-For-Silly-Tavern
    - Nitral لديه بعض مقاطع الفيديو التعليمية حول كيفية استخدام الإضافة ومستودع الحركة: https://www.youtube.com/@nitralai
