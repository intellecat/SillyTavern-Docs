---
route: /extensions/websearch/
---

# البحث في الويب

يضيف نتائج البحث في الويب إلى مطالبات LLM.

!!! Note
بعض مصادر [Chat Completion](/Usage/API_Connections/openai.md) توفر وظيفة البحث في الويب مدمجة. في هذه الحالة، ستكون هذه الإضافة زائدة إلى حد كبير. تحقق من لوحة **<i class="fa-solid fa-sliders"></i> AI Response Configuration** للحصول على مفتاح "Enable web search". على سبيل المثال، هذا متاح لـ Claude و Google AI Studio / Vertex AI و xAI و OpenRouter backends.
!!!

## المصادر المتاحة

### Selenium Plugin

يتطلب تثبيت وتمكين plugin خادم رسمي.

راجع [SillyTavern-WebSearch-Selenium](https://github.com/SillyTavern/SillyTavern-WebSearch-Selenium) للحصول على مزيد من التفاصيل.

يدعم محركات Google و DuckDuckGo.

### Extras API

يتطلب وحدة `websearch` ومتصفح ويب Chrome/Firefox مثبتًا على الجهاز المضيف.

يدعم محركات Google و DuckDuckGo.

### SerpApi

يتطلب مفتاح API.

احصل على المفتاح هنا: <https://serpapi.com/dashboard>

### SearXNG

يتطلب عنوان URL لمثيل SearXNG (خاص أو عام). يستخدم تنسيق HTML لنتائج البحث.

سلسلة تفضيلات SearXNG: تم الحصول عليها من SearXNG - preferences - COOKIES - Copy preferences hash

تعلم المزيد: <https://docs.searxng.org/>

### Tavily AI

يتطلب مفتاح API.

احصل على المفتاح هنا: <https://app.tavily.com/>

### KoboldCpp

يجب توفير عنوان URL لـ KoboldCpp في إعدادات Text Completion API. يجب أن يكون إصدار KoboldCpp >= 1.81.1 ويجب تمكين وحدة WebSearch عند بدء التشغيل: قم بتمكين Network => Enable WebSearch في مشغل GUI أو أضف `--websearch` إلى سطر الأوامر.

راجع: <https://github.com/LostRuins/koboldcpp/releases/tag/v1.81.1>

### Serper

يتطلب مفتاح API.

احصل على المفتاح هنا: <https://serper.dev/>

## كيفية الاستخدام

1. تأكد من أنك تستخدم أحدث إصدار من SillyTavern.
2. قم بتثبيت الإضافة عبر قائمة "Download Extensions & Assets" في SillyTavern.
3. افتح إعدادات إضافة "Web Search"، واضبط مفتاح API الخاص بك أو اتصل بـ Extras، وقم بتمكين الإضافة.
4. سيتم إضافة نتائج البحث في الويب إلى المطالبة بشكل طبيعي أثناء الدردشة. **فقط رسائل المستخدم تطلق البحث.**
5. لتضمين نتائج البحث بشكل أكثر طبيعية، قم بوضع استعلامات البحث بين علامات اقتباس خلفية مفردة: ```Tell me about the `latest Ryan Gosling movie`.``` سينتج استعلام بحث `latest Ryan Gosling movie`.
6. اختياريًا، قم بتكوين الإعدادات حسب رغبتك.

## الإعدادات

### عام

1. Enabled - يبدل الإضافة لتشغيلها أو إيقافها.
2. Sources = يعين مصدر نتائج البحث.
3. Cache Lifetime - كم من الوقت (بالثواني) يتم تخزين نتائج البحث مؤقتًا لمطالبتك. الافتراضي = أسبوع واحد.

### إعدادات المطالبة

1. Prompt Budget - يعين الحد الأقصى لسعة النص المدرج (بأحرف النص، وليس tokens). قاعدة عامة: 1 token ~ 3-4 أحرف، اضبط وفقًا لحدود سياق نموذجك. الافتراضي = 1500 حرف.
2. Insertion Template - كيف يتم إدراج النتيجة في المطالبة. يدعم الماكرو المعتاد + ماكرو خاص: \{\{query\}\} لاستعلام البحث و \{\{text\}\} لنتائج البحث.
3. Injection Position - أين تذهب النتيجة في المطالبة. نفس الخيارات كما هو الحال مع Author's Note: كحقن داخل الدردشة أو قبل/بعد مطالبة النظام.

### تنشيط البحث

1. Use function tool - يستخدم [function calling](/For_Contributors/Function-Calling.md) لتنشيط البحث أو كشط صفحات الويب. يجب استخدام Chat Completion API مدعوم وتمكينه في إعدادات AI Response. **يعطل جميع طرق التنشيط الأخرى عند المشاركة.**
2. Use Backticks - يمكّن تنشيط البحث باستخدام الكلمات المحاطة بعلامات الاقتباس الخلفية المفردة.
3. Use Trigger Phrases - يمكّن تنشيط البحث باستخدام عبارات التشغيل.
4. Regular expressions - توفير regex بنكهة JS لمطابقة رسالة المستخدم. إذا تطابق regex، فسيتم تشغيل البحث باستعلام معين. يدعم استعلام البحث `{{macros}}` وصيغة $1 للإشارة إلى المجموعة المطابقة. مثال: `/what is happening in (.*)/i` regex لاستعلام البحث `news in $1` سيطابق رسالة تحتوي على `what is happening in New York` ويطلق البحث باستعلام `news in New York`.
5. Trigger Phrases - أضف عبارات من شأنها أن تطلق البحث، واحدة تلو الأخرى. يمكن أن تكون في أي مكان في الرسالة، ويبدأ الاستعلام من كلمة التشغيل ويمتد إلى إجمالي "Max Words". لاستبعاد رسالة معينة من المعالجة، يجب أن تبدأ بنقطة، على سبيل المثال `.What do you think?`. أولوية المشغلات: أولاً حسب الترتيب في مربع النص، ثم الأول في رسالة المستخدم.
6. Max Words - كم عدد الكلمات المضمنة في استعلام البحث (بما في ذلك عبارة التشغيل). Google لديه حد حوالي 32 كلمة لكل مطالبة. الافتراضي = 10 كلمات.

### كشط الصفحة

1. Visit Links - سيتم استخراج النص من صفحات نتائج البحث التي تمت زيارتها وحفظه في مرفق ملف.
2. Visit Count - كم عدد الروابط التي سيتم زيارتها وتحليلها للنص.
3. Visit Domain Blacklist - نطاقات المواقع التي يجب استبعادها من الزيارة. واحد لكل سطر.
4. File Header - قالب رأس الملف، يتم إدراجه في بداية ملف النص، لديه ماكرو إضافي \{\{query\}\}.
5. Block Header - قالب كتلة الرابط، يتم إدراجه مع المحتوى المحلل لكل رابط. استخدم ماكرو \{\{link\}\} لعنوان URL للصفحة و \{\{text\}\} لمحتوى الصفحة.
6. Save Target - أين يتم حفظ نتائج الكشط. الخيارات الممكنة: مرفقات رسالة التشغيل، أو مرفقات دردشة Data Bank، أو الصور فقط (إذا كان المصدر يدعمها).
7. Include Images - إرفاق الصور ذات الصلة بالدردشة. يتطلب مصدرًا يدعم الصور (انظر أدناه).

## مزيد من المعلومات

ستبقى نتائج البحث من أحدث استعلام مضمنة في المطالبة حتى يتم العثور على الاستعلام الصالح التالي.
إذا كنت تريد طرح أسئلة إضافية دون تشغيل البحث عن طريق الخطأ، ابدأ رسالتك بنقطة.

!!!info
أداة Web Search function tool تتجاوز دائمًا المشغلات الأخرى إذا تم تمكينها وإتاحتها.
!!!

أولوية المشغلات (إذا تم تمكين عدة):

1. Backticks.
2. Regular expressions.
3. Trigger phrases.

لتجاهل جميع الاستعلامات السابقة من المعالجة، ابدأ رسالة المستخدم بعلامة تعجب، على سبيل المثال، رسالة المستخدم `!Now let's talk about...` ستتجاهل هذه الرسالة وكل رسالة فوقها.

توفر هذه الإضافة أيضًا أمر `/websearch` مائل للاستخدام في STscript. مزيد من المعلومات هنا: [STscript Language Reference](/For_Contributors/st-script.md#extension-commands)

```stscript
/websearch (links=on|off snippets=on|off [query]) – ينفذ استعلام بحث ويب. استخدم المعاملات المسماة لتحديد ما يجب إرجاعه - مقتطفات الصفحة (الافتراضي: on)، الصفحات المحللة الكاملة (الافتراضي: off) أو كليهما.

Example: /websearch links=off snippets=on how to make a sandwich
```

### ما الذي يمكن تضمينه في نتيجة البحث؟

**المعجم:**

- Answer box: إجابة مباشرة على السؤال.
- Knowledge graph: معرفة موسوعية حول الموضوع.
- Page snippets: مقتطفات ذات صلة من صفحات الويب.
- Relevant questions: أسئلة وأجوبة حول مواضيع مماثلة.
- Images: صور ذات صلة.

#### SerpApi

1. Answer box.
2. Knowledge graph.
3. Page snippets (max 10).
4. Relevant questions (max 10).
5. Images (max 10).

#### Selenium Plugin و Extras API

1. Google - answer box, knowledge graph, page snippets.
2. DuckDuckGo - page snippets.

**Selenium Plugin** يمكن أن يوفر أيضًا صورًا.

#### SearXNG

1. Infobox.
2. Page snippets.
3. Images.

#### Tavily AI

1. Answer.
2. Page contents.
3. Images (up to 5).

#### KoboldCpp

1. Page titles.
2. Page snippets.

#### Serper

1. Answer box.
2. Knowledge graph.
3. Page snippets.
4. Relevant questions.
5. Images.
