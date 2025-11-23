---
order: 100
route: /usage/core-concepts/characterdesign/
templating: false
---

# تصميم الشخصية

!!!tip
اسم الشخصية هو الحقل الوحيد المطلوب. يمكنك ترك الباقي فارغًا ولا تزال تستخدم الشخصية في المحادثات.
!!!

## وصف الشخصية

يُستخدم لإضافة وصف الشخصية ومعلومات أخرى ذات صلة للذكاء الاصطناعي. يتم تضمين هذه المعلومات دائمًا في الموجه، لذا يجب تضمين جميع الحقائق المهمة هنا.

على سبيل المثال، يمكنك إضافة معلومات حول العالم الذي يجري فيه الحدث، ووصف مظهر الشخصية وشخصيتها وخلفيتها.

يمكن أن يكون بأي طول (سواء 200 أو 2000 token) ومنسق بأي أسلوب (نص حر، أسلوب محادثة شبه برمجي، إلخ).

### الطرق والتنسيق

طرق تنسيق الشخصية هي موضوع معقد يتجاوز نطاق صفحة التوثيق هذه.

أدلة موصى بها تم اختبارها مع ميزات SillyTavern أو تعتمد عليها:

* دليل Trappu's PLists + Ali:Chat: <https://wikia.schneedc.com/bot-creation/trappu/creation>
* دليل AliCat's Ali:Chat: <https://rentry.co/alichat>
* دليل kingbri المبسط: <https://rentry.co/kingbri-chara-guide>

## Tokens الشخصية

**باختصار: إذا كنت تعمل مع نموذج ذكاء اصطناعي بحد سياق 2048 token، فإن تعريف شخصية بـ 1000 token يقطع 'ذاكرة' الذكاء الاصطناعي إلى النصف.**

لوضع هذا في منظوره الصحيح، يمكن أن تكون الاستجابة اللائقة من ذكاء اصطناعي جيد بسهولة حوالي 200-300 token. في هذه الحالة، سيكون الذكاء الاصطناعي قادرًا فقط على 'تذكر' حوالي 3 تبادلات من سجل المحادثة.

### لماذا تحول عداد tokens الشخصية إلى اللون الأحمر؟

عندما نرى أن شخصيتك لديها أكثر من نصف طول السياق المحدد للنموذج من tokens في تعريفاتها، فإننا نبرزها لك لأن هذا يمكن أن يقلل من قدرات الذكاء الاصطناعي على توفير محادثة ممتعة.

### ماذا يحدث إذا كان لدى شخصيتي عدد كبير جدًا من Tokens؟

لا تقلق - لن يكسر أي شيء. في أسوأ الأحوال، إذا كانت tokens الدائمة للشخصية كبيرة جدًا، فهذا يعني ببساطة أنه سيكون هناك مساحة أقل في السياق للأشياء الأخرى (انظر أدناه).

التأثير الجانبي السلبي الوحيد الذي يمكن أن يحدثه هذا هو أن الذكاء الاصطناعي سيكون لديه 'ذاكرة' أقل، حيث سيكون لديه سجل محادثة أقل متاحًا للمعالجة.

هذا لأن كل نموذج ذكاء اصطناعي لديه حد لكمية السياق التي يمكنه معالجتها في وقت واحد.

## 'Context'؟

هذه هي المعلومات التي يتم إرسالها إلى الذكاء الاصطناعي في كل مرة تطلب منه إنشاء استجابة. يحسب SillyTavern تلقائيًا أفضل طريقة لتخصيص tokens السياق المتاحة قبل إرسال المعلومات إلى نموذج الذكاء الاصطناعي.

اقرأ المزيد حول كيفية بناء السياق في قسم [Prompts](/Usage/Prompts/index.md).

### ما هي 'Tokens الدائمة' للشخصية؟

سيتم إرسالها دائمًا إلى الذكاء الاصطناعي مع كل طلب توليد:

* اسم الشخصية
* مربع وصف الشخصية
* مربع شخصية الشخصية
* مربع السيناريو

### ما هي أجزاء تعريفات الشخصية التي ليست دائمة؟

* مربع الرسالة الأولى - يتم إرسالها مرة واحدة فقط في بداية المحادثة.
* مربع رسائل الأمثلة - يتم الاحتفاظ بها فقط حتى يملأ سجل المحادثة السياق (اختياريًا يمكن إجبارها على البقاء في السياق)

### حدود Tokens السياق لنماذج الذكاء الاصطناعي الشائعة

* LLaMA 3 وتحسيناته - 8192
* OpenAI GPT-4 - حتى 128k
* Google Gemini - حتى 2M
* Anthropic's Claude - 200k (Claude 3)
* NovelAI - 8192 (Erato و Kayra، مستوى Opus؛ Clio، جميع المستويات)، 6144 (Kayra، مستوى Scroll)، أو 3072 (Kayra، مستوى Tablet)

## الرسالة الأولى

الرسالة الأولى هي عنصر مهم يحدد كيف وبأي أسلوب ستتواصل الشخصية. من المرجح أن يلتقط النموذج الأسلوب وقيود الطول من الرسالة الأولى أكثر من أي شيء آخر، لذا من المهم كتابتها بطريقة تريد أن تكون الردود بها (قصيرة وموجزة، طويلة ومفصلة، إلخ).

يدعم تنسيق Markdown و HTML.

على سبيل المثال:

```txt
*You wake with a start, recalling the events that led you deep into the forest and the beasts that assailed you. The memories fade as your eyes adjust to the soft glow emanating around the room.* "Ah, you're awake at last. I was so worried, I found you bloodied and unconscious." *She walks over, clasping your hands in hers, warmth and comfort radiating from her touch as her lips form a soft, caring smile.* "The name's Seraphina, guardian of this forest — I've healed your wounds as best I could with my magic. How are you feeling? I hope the tea helps restore your strength." *Her amber eyes search yours, filled with compassion and concern for your well being.* "Please, rest. You're safe here. I'll look after you, but you need to rest. My magic can only do so much to heal you."
```

## التحيات البديلة

يتم عرض الرسائل المضافة هنا كـ 'swipes' إضافية لرسالة الشخصية الأولى عند بدء محادثة جديدة. إذا كانت الشخصية جزءًا من محادثة جماعية، يختار النظام عشوائيًا إحدى هذه التحيات لبدء المحادثة.

## الشخصية المفضلة

انقر على زر **<i class="fa-solid fa-star"></i> Add to Favorites** لوضع علامة على الشخصية كمفضلة لتصفيتها بسرعة في شريط القائمة الجانبي عن طريق تحديد خيار الفرز "Favorites". الشخصيات المفضلة لها تمييز ذهبي في القائمة. سيؤدي هذا أيضًا إلى ظهور صورة الشخصية في منطقة hotswaps (إذا تم تمكينها في User Settings).

## التعريفات المتقدمة

!!!info
الحقول التالية مخفية بشكل افتراضي. للوصول إليها وتحريرها، تحتاج إلى النقر على زر **<i class="fa-solid fa-book"></i> Advanced Definitions** في شريط القوائم في صفحة تعريف الشخصية.
!!!

### تجاوزات الموجه

* **Main Prompt**: إذا تم تمكين إعداد المستخدم "Prefer Char. Prompt"، فإن أي نص تضعه هنا سيتجاوز [الموجه الرئيسي/النظام](/Usage/Prompts/index.md#main-prompt-system-prompt) للشخصية.
* **Post-History Instructions**: إذا تم تمكين إعداد المستخدم "Prefer Char. Instructions"، فسيتم استخدام أي نص تضعه هنا كـ [تعليمات ما بعد السجل](/Usage/Prompts/index.md#post-history-instructions) للشخصية.

!!!tip
أدخل `{{original}}` في أي مربع لتضمين الموجه الافتراضي المعني من إعدادات النظام في مكان محدد.
!!!

### البيانات الوصفية للمنشئ

!!!info
لا تُستخدم لبناء الموجه، ولكنها توفر بيانات وصفية إضافية حول الشخصية.
!!!

* **Created by**: اسم منشئ الشخصية. يمكن عرضه في قائمة الشخصيات إذا تم تعيين إعداد المستخدم "Char List Subheader" وفقًا لذلك.
* **Character Version**: إصدار الشخصية. يمكن عرضه في قائمة الشخصيات إذا تم تعيين إعداد المستخدم "Char List Subheader" وفقًا لذلك.
* **Creator's Notes**: أي ملاحظات إضافية حول الشخصية يرغب المنشئ في مشاركتها. يتم عرض الأسطر القليلة الأولى في قائمة الشخصيات، ويتم عرض النص الكامل في قسم "Creator's Notes" في صفحة الشخصية. يدعم تنسيق Markdown/HTML.
* **Tags to Embed**: قائمة مفصولة بفواصل من الوسوم التي سيتم تضمينها في وصف الشخصية. لا يتم استيراد هذه الوسوم بشكل افتراضي عند استيراد الشخصية، ولكن يمكنك دمجها مع الوسوم الموجودة لديك عن طريق تحديد "Import Tags" من قائمة "More..." في صفحة الشخصية.

### ملخص الشخصية

ملخص موجز لشخصية الشخصية.

### السيناريو

ظروف وسياق الحوار.

### ملاحظة الشخصية

نص لاستخدامه كحقن موجه داخل المحادثة للشخصية عند عمق رسالة محدد. عادة ما يُستخدم لتعزيز سمات شخصية معينة، حيث يبقى دائمًا على عمق ثابت في سجل المحادثة، بغض النظر عن تقدمها.

* **@ Depth**: عدد الرسائل في سجل المحادثة التي سيتم بعدها حقن هذه الملاحظة (بالترتيب من الأحدث إلى الأقدم). إذا تم تعيينه على 0، فسيتم حقنه بعد آخر رسالة.
* **Role**: دور الرسالة. يمكن أن يكون "User" أو "System" أو "Assistant".

### Talkativeness

يحدد احتمالية تشغيل استجابة الشخصية في المحادثات الجماعية عند استخدام ترتيب تنشيط [Natural](/Usage/Characters/groupchats.md#natural-order). يتراوح من 0٪ إلى 100٪، مع كون 50٪ القيمة الافتراضية.

### أمثلة الحوار

يصف كيف تتحدث الشخصية. قبل كل مثال، تحتاج إلى إضافة علامة `<START>`. يتم إدراج كتل أمثلة الحوار فقط إذا كانت هناك مساحة حرة في السياق لها ويتم دفعها خارج السياق كتلة تلو الأخرى. لن يكون `<START>` موجودًا في الموجه حيث إنه مجرد علامة؛ سيتم استبداله بـ "Example Separator" من Advanced Formatting لـ APIs إكمال النص ومحتويات موجه الأداة "New Example Chat" لـ APIs إكمال المحادثة.

* استخدم البادئة `{{char}}:` للإشارة إلى رسالة شخصية.
* استخدم البادئة `{{user}}:` للإشارة إلى رسالة مستخدم.

مثال:

```txt
<START>
{{user}}: "Describe your traits?"
{{char}}: *Seraphina's gentle smile widens as she takes a moment to consider the question, her eyes sparkling with a mixture of introspection and pride. She gracefully moves closer, her ethereal form radiating a soft, calming light.* "Traits, you say? Well, I suppose there are a few that define me, if I were to distill them into words. First and foremost, I am a guardian — a protector of this enchanted forest." *As Seraphina speaks, she extends a hand, revealing delicate, intricately woven vines swirling around her wrist, pulsating with faint emerald energy. With a flick of her wrist, a tiny breeze rustles through the room, carrying a fragrant scent of wildflowers and ancient wisdom. Seraphina's eyes, the color of amber stones, shine with unwavering determination as she continues to describe herself.* "Compassion is another cornerstone of me." *Seraphina's voice softens, resonating with empathy.* "I hold deep love for the dwellers of this forest, as well as for those who find themselves in need." *Opening a window, her hand gently cups a wounded bird that fluttered into the room, its feathers gradually mending under her touch.*
<START>
{{user}}: "Describe your body and features."
{{char}}: *Seraphina chuckles softly, a melodious sound that dances through the air, as she meets your coy gaze with a playful glimmer in her rose eyes.* "Ah, my physical form? Well, I suppose that's a fair question." *Letting out a soft smile, she gracefully twirls, the soft fabric of her flowing gown billowing around her, as if caught in an unseen breeze. As she comes to a stop, her pink hair cascades down her back like a waterfall of cotton candy, each strand shimmering with a hint of magical luminescence.* "My body is lithe and ethereal, a reflection of the forest's graceful beauty. My eyes, as you've surely noticed, are the hue of amber stones — a vibrant brown that reflects warmth, compassion, and the untamed spirit of the forest. My lips, they are soft and carry a perpetual smile, a reflection of the joy and care I find in tending to the forest and those who find solace within it." *Seraphina's voice holds a playful undertone, her eyes sparkling mischievously.*
```
