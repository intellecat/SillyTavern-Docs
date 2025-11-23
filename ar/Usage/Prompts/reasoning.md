---
order: 40
tags: ['>=1.12.12']
route: /usage/prompts/reasoning/
---

# Reasoning

في نماذج اللغة، reasoning (المعروف أيضاً باسم model thinking) يشير إلى تقنية chain-of-thought (CoT) التي تعكس حل المشكلات البشرية من خلال التحليل خطوة بخطوة. يوفر SillyTavern العديد من الميزات التي تجعل استخدام نماذج reasoning أكثر كفاءة واتساقاً عبر backends المدعومة.

## المشاكل الشائعة

1. عند استخدام نماذج reasoning، تستهلك عملية reasoning الداخلية للنموذج جزءاً من مخصص رموز الاستجابة الخاصة بك، حتى لو لم يتم عرض هذا reasoning في الإخراج النهائي (مثل o3-mini أو Gemini Thinking). إذا لاحظت أن استجاباتك تعود غير مكتملة أو فارغة، فيجب عليك محاولة ضبط إعداد Max Response Length الموجود في لوحة **<i class="fa-solid fa-sliders"></i> AI Response Configuration**. بالنسبة لنماذج reasoning، من المعتاد استخدام حدود رموز أعلى بكثير - في أي مكان من 1024 إلى 4096 رمز - مقارنة بنماذج المحادثة القياسية.

## التكوين

!!!
يمكن تكوين معظم الإعدادات المتعلقة بـ reasoning في قسم "Reasoning" من لوحة **<i class="fa-solid fa-font"></i> Advanced Formatting**.
!!!

تظهر كتل Reasoning في المحادثة كأقسام رسائل قابلة للطي. يمكن إضافتها يدوياً، أو تلقائياً بواسطة backend، أو من خلال تحليل الاستجابة (انظر أدناه).

بشكل افتراضي، يتم طي كتل reasoning لتوفير المساحة. انقر على كتلة لتوسيعها وعرض محتوياتها. يمكنك تعيين الكتل للتوسع تلقائياً من خلال تمكين **Auto-Expand** في إعدادات reasoning.

عند توسيع كتلة reasoning، يمكنك نسخ أو تحرير محتوياتها باستخدام أزرار **<i class="fa-solid fa-copy"></i> Copy** و **<i class="fa-solid fa-pencil"></i> Edit**.

بعض النماذج تدعم reasoning، لكنها لن ترسل أفكارها مرة أخرى. من الممكن أن يظل عرض كتلة reasoning مع وقت reasoning لهؤلاء من خلال تبديل إعداد **Show Hidden**.

## إضافة Reasoning

### يدوياً

أضف كتلة reasoning إلى أي رسالة من خلال قائمة **<i class="fa-solid fa-pencil"></i> Message Edit**. انقر على **<i class="fa-solid fa-lightbulb"></i>** أثناء التحرير لإضافة قسم reasoning. يمكن أيضاً للإضافات التابعة لجهات خارجية إضافة reasoning عن طريق الكتابة إلى حقل `extra.reasoning` لكائن الرسالة قبل إضافته إلى المحادثة.

### باستخدام أمر

استخدم أمر STscript `/reasoning-set` لإضافة reasoning إلى رسالة. يأخذ الأمر `at` (معرف الرسالة، افتراضياً آخر رسالة) ونص reasoning كمعاملات.

```stscript
/reasoning-set at=0 This is the reasoning for the first message.
```

### بواسطة Backend

إذا كان LLM backend والنموذج المختار يدعمان إخراج reasoning، فإن تمكين "Request model reasoning" في لوحة **<i class="fa-solid fa-sliders"></i> AI Response Configuration** سيضيف كتلة reasoning تحتوي على عملية تفكير النموذج.

المصادر المدعومة:

- Claude
- DeepSeek
- Google AI Studio
- Google Vertex AI
- OpenRouter
- xAI (Grok)
- AI/ML API

"Request model reasoning" لا يحدد ما إذا كان النموذج يقوم بـ reasoning. Claude و Google (2.5 Flash) يسمحان بتبديل thinking mode؛ راجع [Reasoning Effort](#reasoning-effort).

### بواسطة التحليل

مكّن "Auto-Parse" في لوحة **<i class="fa-solid fa-font"></i> Advanced Formatting** لتحليل reasoning تلقائياً من إخراج النموذج.

يجب أن تحتوي الاستجابة على قسم reasoning ملفوف في تسلسلات Prefix و Suffix المكوّنة. التسلسلات المقدمة بشكل افتراضي تتوافق مع تنسيق reasoning DeepSeek R1.

مثال مع prefix `<think>` و suffix `</think>`:

```txt
<think>
This is the reasoning.
</think>

This is the main content.
```

## Prompting مع Reasoning

بشكل افتراضي، لا يتم إرسال محتويات كتل reasoning المعروفة مرة أخرى إلى النموذج. لتضمين reasoning في prompts، مكّن "Add to Prompts" في لوحة **<i class="fa-solid fa-font"></i> Advanced Formatting**. سيتم تغليف محتوى Reasoning في تسلسلات Prefix و Suffix المكوّنة وفصله بواسطة Separator من السياق الرئيسي. يتحكم إعداد Max Additions الرقمي في عدد كتل reasoning التي يمكن تضمينها، بالعد من نهاية prompt.

!!!
لا توصي معظم مزودي النماذج بإرسال CoT مرة أخرى إلى النموذج في محادثات متعددة الأدوار.
!!!

### الاستمرار من Reasoning

حالة خاصة عندما يمكن إرسال reasoning مرة أخرى إلى النموذج دون تمكين تبديل "Add to Prompts" هي عند استمرار التوليد (مثل بالضغط على "Continue" من قائمة **<i class="fa-solid fa-bars"></i> Options**)، لكن الرسالة التي يتم استمرارها تحتوي فقط على reasoning بدون محتوى فعلي. هذا يمنح النموذج فرصة لإنهاء reasoning غير مكتمل وبدء توليد المحتوى الرئيسي. سيتم إرسال prompt على النحو التالي:

```txt
<think>
Incomplete reasoning...
```

## Regex Scripts

يمكن تطبيق نصوص التعبيرات العادية من [Regex extension](/extensions/Regex.md) على محتويات كتل reasoning. حدد "Reasoning" في قسم "Affects" من محرر النص البرمجي لاستهداف كتل reasoning على وجه التحديد.

تؤثر خيارات الزوال المختلفة على كتل reasoning بالطرق التالية:

1. بدون زوال: يتم تغيير محتوى reasoning بشكل دائم.
2. Run on edit: سيتم إعادة تقييم regex script عند تحرير كتلة reasoning.
3. Alter chat display: يتم تطبيق regex على نص عرض كتلة reasoning، وليس المحتوى الأساسي.
4. Alter outgoing prompts: يتم تطبيق regex فقط على كتل reasoning قبل إرسالها إلى النموذج.

## Reasoning Effort

Reasoning Effort هو إعداد Chat Completion في لوحة **<i class="fa-solid fa-sliders"></i> AI Response Configuration** يؤثر على عدد الرموز التي قد تُستخدم على reasoning. يعتمد تأثير كل خيار على المصدر المتصل به. بالنسبة للمصادر أدناه، Auto ببساطة يعني أن المعامل ذي الصلة غير مدرج في الطلب.

| Option  | Claude (≤ 21333 if no streaming) | OpenAI (keyword)     | OpenRouter (keyword)             | xAI (Grok) (keyword) | Perplexity (keyword) |
| ------- | -------------------------------- | -------------------- | -------------------------------- | -------------------- | -------------------- |
| Models  | Opus 4, Sonnet 4/3.7             | o4-mini, o3\*, o1\*  | applicable models                | grok-3-mini          | sonar-deep-research  |
| Auto    | not specified, **no thinking**   | not specified        | not specified, effect depends    | not specified        | not specified        |
| Minimum | budgets 1024 tokens              | "low"                | "low", or 20% of max response    | "low"                | "low"                |
| Low     | 15% of max response, min 1024    | "low"                | "low", or 20% of max response    | "low"                | "low"                |
| Medium  | 25% of max response, min 1024    | "medium"             | "medium", or 50% of max response | "low"                | "medium"             |
| High    | 50% of max response, min 1024    | "high"               | "high", or 80% of max response   | "high"               | "high"               |
| Maximum | 95% of max response, min 1024    | "high"               | "high", or 80% of max response   | "high"               | "high"               |

- بالنسبة لـ Claude، يتم تحديد الميزانية بـ 21333 إذا تم تعطيل streaming. إذا كانت الميزانية المحسوبة ستكون أقل من 1024، فيتم تغيير max response إلى 2048.
- بالنسبة لـ OpenRouter و Perplexity و AI/ML API، يتم إرسال كلمة رئيسية بنمط OpenAI فقط.

Google AI Studio و Vertex AI كما يلي:

| Model          | Auto (dynamic thinking) | Minimum            | Low                          | Medium     | High       | Maximum               |
| -------------- | ----------------------- | ------------------ | ---------------------------- | ---------- | ---------- | --------------------- |
| 2.5 Pro        | thinkingBudget = -1     | 128                | 15% of max response, min 128 | 25% of max | 50% of max | lower of max or 32768 |
| 2.5 Flash      | thinkingBudget = -1     | 0, **no thinking** | 15% of max response          | 25% of max | 50% of max | lower of max or 24576 |
| 2.5 Flash Lite | thinkingBudget = -1     | 0, **no thinking** | 15% of max response, min 512 | 25% of max | 50% of max | lower of max or 24576 |

- بالنسبة لـ Gemini 2.5 Pro و 2.5 Flash/Lite، يتم تحديد الميزانية بـ 32768 أو 24576 رمز على التوالي، بغض النظر عن إعداد streaming.
