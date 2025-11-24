---
order: 60
route: /usage/prompts/cfg/
---

# CFG

كُتبت الصفحة بواسطة: kingbri

المساهمون: kingbri، Guillaume "Vermeille" Sanchez، AliCat

## ما هو؟

CFG، أو classifier-free guidance هي طريقة تُستخدم للمساعدة في جعل أجزاء من prompt أقل أو أكثر بروزاً.

### Backend APIs المدعومة

حالياً، backends المدعومة هي oobabooga's textgen WebUI و NovelAI و TabbyAPI.
لدى NovelAI [توثيق خاص بها لـ CFG](https://web.archive.org/web/20240917150051/https://docs.novelai.net/text/cfg.html).

تحذير: CFG يزيد استخدام vram بسبب استيعاب أكثر من prompt واحد! إذا نفدت ذاكرة GPU أثناء توليد prompt مع تشغيل CFG، فكر في تقليل حجم context الخاص بك، أو استخدام نموذج بمعاملات أقل، أو إيقاف تشغيل CFG تماماً.

---

## التكوين

الوصول إلى إعدادات CFG هو نفس الوصول إلى Author's note:

![CFGhamburgermenupng](/static/cfg-hamburger.png)

وإليك كيف تبدو لوحة CFG:

![CFGchatpanelpng](/static/cfg-panel.png)

هناك أربع قوائم منسدلة في لوحة CFG:

- Chat CFG

  - يحدد نطاق CFG scale و prompts لهذه المحادثة فقط
- Character CFG

  - يحدد نطاق CFG scale و prompts للشخصية المحددة
- Global CFG

  - يتجاوز عالمياً CFG scale و prompts (يتجاوز أيضاً model preset!)
- CFG Advanced Settings (كانت تسمى سابقاً CFG Prompt Cascading)

  - مكان لدمج prompts من القوائم المنسدلة الثلاث السابقة وتعيين عمق الإدراج.

ملاحظة: إذا تم تعيين guidance scale إلى 1، فلن يتم إرسال أي شيء لأن هذا عندما يكون CFG في حالة "إيقاف".

#### Group Chats

في group chats، تبدو لوحة CFG scale هكذا:

![CFGpanelgcpng](/static/cfg-groups.png)

التغيير الرئيسي هو أن character CFG تمت إزالته ومربع اختيار يسمى `Use Character CFG Scales` موجود في القائمة المنسدلة chat CFG. هذا يسمح باستخدام guidance scale للشخصية الحالية بدلاً من أي شيء تم تعيين chat CFG scale إليه.

الفائدة الرئيسية من هذه الميزة هي تغيير scale بناءً على الاحتياجات الفردية لكل شخصية.

بالإضافة إلى ذلك، تحديد مربع `Character Negatives` في prompt cascading سيضيف character negative prompts المستقلة مع chat prompts (إذا تم تمكينها).

---

## المفاهيم

### أليس هذا في Stable Diffusion؟

نعم ولا. CFG مع LLMs يعمل بطريقة مختلفة عما قد يكون المرء معتاداً عليه في Stable Diffusion. CFG القائم على LLM يعمل على مبدأ "خلط prompt". معادلة CFG تأخذ positive و negative prompt، ثم تخلط *الاختلافات* بينهما. من هناك، يتم إرسال prompt مجمع وتوليد استجابة!

إليك رسم توضيحي للمساعدة في تصور هذا المفهوم. يمثل اللون الأحمر negative prompt، ويمثل الأزرق neutral prompt، ويمثل الأرجواني النتيجة المختلطة التي يتم تفسيرها. كل المساحة البيضاء هي نفسها عبر جميع prompts الثلاثة، لذلك لا تُستخدم لخلط CFG.

![stcfgdiagrampng](/static/cfg-diagram.png)

إذا كنت تريد معرفة المزيد عن CFG و LLMs، فإن ورقة Vermifuge الأصلية موجودة هنا. أقترح قراءتها/الاستماع إليها:

- Paper - [[2306.17806] Stay on topic with Classifier-Free Guidance (arxiv.org)](https://arxiv.org/abs//2306.17806)

- نسخة صوتية - [https://www.youtube.com/watch?v=MGY00YFcyco](https://www.youtube.com/watch?v=MGY00YFcyco)


### هل أحتاج إلى CFG prompts؟

لا! CFG prompts اختيارية تماماً. مجرد ضبط guidance scale فوق `1` سيساعد أيضاً في إنتاج تأثير على الاستجابات، والذي يمكن أن يبرز المحادثات وتفاعل الشخصيات.

### ما الذي يجعل CFG prompt جيداً؟

إذاً، أسسنا أن CFG prompting ليس نفس negative tags و embeddings في Stable Diffusion. كيف نصنع prompt؟

تحذير: هذا يفترض أنك أنشأت شخصية باستخدام PLists و Ali:Chat. إذا لم تكن قد فعلت ذلك، فلا تتردد في تجربة تقنيات prompting مختلفة.

لنفترض أن لدي شخصية اسمها "John". من المفترض أن يشعر John بالسعادة والإثارة طوال الوقت من حوارات الأمثلة الخاصة به. ومع ذلك، عند الدردشة مع John، يكون أحياناً حزيناً ومكتئباً.

لإزالة هذا، CFG يأتي للإنقاذ! فقط اجعل negative prompt `[John's feelings: sad, depressed]` للمساعدة في إزالة أجزاء الحزن. يمكنك اختيارياً جعل positive prompt `[John's feelings: happy, joyful]` لإبراز أجزاء John السعيدة بشكل أكبر.

### Positive Prompts

تناولت هذا في القسم السابق، لكنني أود أن أتطرق إلى هذا أكثر قليلاً. تُستخدم positive prompts لإبراز أجزاء من الشخصية بشكل أكبر. لنستخدم John مرة أخرى كمثالنا. من خلال جعله أكثر سعادة مع positive prompt من `[John's feelings: happy, joyful]`، يجب أن يبدأ John في إخراج حوار بشعور أكثر سعادة مما لو لم يتم تضمين positive prompt.

### لكن...

هذه مجرد **إرشادات فضفاضة** من التجربة مع تنسيق شخصية محدد واحد. هناك العديد من الطرق الأخرى لإنشاء prompts التي يجب أن تجربها. لا تتردد في مشاركة أفكارك مع مستخدمين آخرين!

### Guidance Scale

إليك قاعدة عامة. guidance scale من `1` يعني أن CFG معطل. في الواقع، SillyTavern لن يرسل أي شيء إلى backend الخاص بك إذا كان guidance scale هو 1. guidance scale `>1` سيعطي النتائج الموضحة في الأقسام الأخرى بدرجات متفاوتة.

ومع ذلك، guidance scale من `<1` سيعطي التأثير *المعاكس* لأن negative prompt يُستخدم كـ primary prompt هنا.

لنستخدم المثال مع John مرة أخرى. الـ negative prompt هو `[John's feelings: sad, depressed]` والـ positive prompt هو `[John's feelings: happy, joyful]` مع guidance scale من `0.8`.

هذا بدوره سيبرز *negative* prompt أكثر وسترى John يبدأ في التصرف بحزن أكثر من المعتاد بدلاً من أن يكون أكثر سعادة.

باختصار؛ استخدم guidance scale من `1.5` واعمل لأعلى ولأسفل من هناك بناءً على إخراجاتك.

### Prompt Cascading

يمكن تسلسل Negatives و positives بين أنواع CFG (الأنواع هي per-chat و per-character و global overrides). راجع رأس Configuration لمزيد من المعلومات.

### Insertion Depth

اتبع القاعدة الأساسية: كلما كان شيء ما موجوداً في أسفل prompt، كلما كان أكثر تأثيراً على الاستجابة. للدردشة، أوصي باستخدام العمق الافتراضي من `1` لأنه مرن جداً مع مكونات SillyTavern الأخرى.

ومع ذلك، إذا كنت تريد التجربة، فإن insertion depth من `0` متاح. ومع ذلك، يمكن أن تغير هذه بشكل كبير كيف سيبدو ردك ولا يُنصح باستخدام prompt cascading هنا!
