---
order: 80
route: /usage/core-concepts/instructmode/
---

# Instruct Mode

يتيح لك Instruct Mode ضبط prompting لنماذج اتباع التعليمات المدربة على تنسيقات prompt مختلفة، مثل Alpaca و ChatML و Llama2، إلخ.

!!! ينطبق على: Text Completion APIs
للإعدادات المكافئة في Chat Completion APIs، استخدم [Prompt Manager](prompt-manager.md).
!!!

## دعم API

### Text Completion API

مدعوم بالكامل. هذا يشمل:

* جميع المصادر تحت Text Completion
* KoboldAI Classic
* AI Horde

#### اختيار تنسيق

يجب أن يتطابق instruct template المختار مع توقعات النموذج الفعلي الذي يعمل على backend.

عادة ما ينعكس هذا في بطاقة النموذج على HuggingFace، وبعضها يوفر حتى ملفات JSON متوافقة مع SillyTavern.

مثال: [NeverSleep/Noromaid-13b-v0.1.1](https://huggingface.co/NeverSleep/Noromaid-13b-v0.1.1#prompt-template-custom-format-or-alpaca)

### Chat Completion API (OpenAI, Claude, etc)

هذا غير مدعوم **(وغير مطلوب)** لـ Chat Completion APIs. فهي تستخدم منشئ prompt مختلف تماماً.

### NovelAI

بينما *تقنياً* مدعوم لـ NovelAI، لم يتم تدريب أي من نماذجهم لفهم تنسيق instruct. يمكن لنماذج NovelAI استخدام وحدة instruct خاصة يتم تنشيطها *تلقائياً* عند مواجهة تعليمة ملفوفة في أقواس معقوفة في رسائل المحادثة، لذا فإن استخدام Instruct Mode للـ prompt بأكمله سيؤدي إلى **تدهور جودة** المخرجات.

إليك مثال ينشط تلقائياً وحدة instruct لـ NovelAI:

```txt
User: { Write a happy song about Nintendo Switch. }
```

## إعدادات Instruct Mode

### System Prompt

!!!warning تغيير حديث
الـ System Prompt الآن كيان منفصل. راجع صفحة [Advanced Formatting](advancedformatting.md#system-prompt) لمزيد من التفاصيل.
!!!

### Templates

يوفر templates جاهزة مع تسلسلات لبعض نماذج instruct المعروفة.

*تغيير template يعيد تعيين الإعدادات غير المحفوظة إلى آخر حالة محفوظة! لا تنس حفظ template الخاص بك إذا قمت بأي تغييرات لا تريد فقدها.*

### Activation Regex

إذا تم تحديده كتعبير عادي صالح، عند الاتصال بنموذج واسمه يطابق هذا regex، سيتم تحديد هذا template تلقائياً.

يجب تمكين instruct mode مسبقاً. فقط أول تطابق regex عبر templates سيتم تحديده (يتم تقييمه بترتيب أبجدي).

### Wrap Sequences with Newline

سيتم تغليف كل نص تسلسل بأحرف سطر جديد عند إدراجه في prompt. مطلوب لـ Alpaca ومشتقاته.

عطّله إذا كنت تريد التحكم الكامل في محددات الأسطر.

### Replace Macro in Sequences

إذا تم تمكينه، سيتم استبدال بدائل \{\{macro\}\} المعروفة إذا تم تحديدها في تسلسلات تغليف الرسائل.

أيضاً، يمكن استخدام macro خاص \{\{name\}\} في بادئات الرسائل للإشارة إلى الاسم الفعلي المرفق برسالة (بدلاً من \{\{char\}\} أو \{\{user\}\} النشط حالياً)، والذي يمكن أن يكون مفيداً عند استخدام group chats أو أمر /sendas. إذا تعذر تحديد الاسم، يُستخدم "System" كعنصر نائب احتياطي.

### Include Names

إذا تم تمكينه، يُضاف أسماء الشخصيات والمستخدمين إلى سجلات سجل المحادثة بعد تسلسل البادئة.

الخيارات التالية متاحة:

* **Never**: لا تضف بادئات الأسماء قبل محتويات الرسالة.
* **Groups and Past Personas**: فقط أضف بادئات الأسماء للرسائل من شخصيات المجموعة و personas الماضية.
* **Always**: دائماً أضف بادئات الأسماء قبل محتويات الرسالة.

### Sequences: Story String Wrapping

!!!warning تغيير حديث
تمت إزالة System Prompt wrapping واستبداله بـ Story String wrapping.
!!!

حدد كيفية تغليف Story String عندما يتم ضبط Position على "Default (top of context)"

#### Story String Prefix

يُدرج قبل Story String.

#### Story String Suffix

يُدرج بعد Story String.

### Sequences: Chat Messages Wrapping

تحدد هذه الإعدادات كيفية تغليف الرسائل التابعة لأدوار مختلفة عند بناء prompt.

سيتم أيضاً استخدام جميع تسلسلات البادئة تلقائياً كـ stopping strings.

#### User Message Prefix

يُدرج قبل رسالة User وكسطر prompt أخير عند الانتحال.

#### User Message Suffix

يُدرج بعد رسالة User.

#### Assistant Message Prefix

يُدرج قبل رسالة Assistant وكسطر prompt أخير عند توليد رد ذكاء اصطناعي.

#### Assistant Message Suffix

يُدرج بعد رسالة Assistant

#### System Message Prefix

يُدرج قبل رسالة System (المضافة بواسطة أوامر slash أو إضافات).

#### System Message Suffix

يُدرج بعد رسالة System.

#### System same as User

إذا تم تحديده كصحيح، فستستخدم رسائل System تسلسلات رسائل دور User.

وإلا، فستستخدم رسائل System تسلسلاتها الخاصة (إذا لم تكن فارغة) أو لن تقوم بأي تغليف على الإطلاق (إذا كانت فارغة).

### Misc. Sequences

تكوينات متقدمة متنوعة لضبط أدق لبناء prompt

#### First Assistant Prefix

يُدرج قبل أول رسالة Assistant.

!!!info
فقط الرسالة الأولى من **سجل المحادثة** تُحسب، وليس الرسالة التي تدخل فعلاً prompt أولاً!
!!!

#### Last Assistant Prefix

يُدرج قبل آخر رسالة Assistant أو كسطر prompt أخير عند توليد رد ذكاء اصطناعي.

!!!info
لا يُستخدم عند توليد نص في الخلفية (مثل prompts Stable Diffusion أو Summaries). سيُستخدم System Instruction Prefix أو Regular Assistant Prefix بدلاً من ذلك.
!!!

#### System Instruction Prefix

يُدرج كسطر prompt أخير عند توليد نص محايد/نظام في الخلفية (مثل prompts Stable Diffusion أو Summaries).

#### User Filler Message

سيتم إدراجه في بداية سجل المحادثة إذا لم يبدأ برسالة User.

**حالة الاستخدام:** عندما يتطلب تنسيق instruct *بشكل صارم* أن تكون prompts بدءاً من المستخدم أولاً وأن يكون للرسائل أدوار متناوبة فقط، أمثلة: Llama 2 Chat، Mistral Instruct.

#### Stop Sequence

نص يشير إلى نهاية الرد. يُرسل أيضاً كـ stopping string إلى backend API.

إذا تم توليد stop sequence، فسيتم إزالة كل شيء بعده من الإخراج (بما في ذلك التسلسل نفسه).
