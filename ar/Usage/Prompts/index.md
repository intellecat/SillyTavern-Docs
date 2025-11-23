---
order: 140
icon: typography
templating: false
route: /usage/prompts/
---

# Prompts

عندما ترسل رسالة إلى الذكاء الاصطناعي الخاص بك، يتم دمج النص الذي تكتبه مع نصوص أخرى لتشكيل طلب واحد يُرسل إلى الذكاء الاصطناعي. يُطلق على هذا النص المدمج اسم "prompt" أو أحياناً "request" أو "context".

يمكن أن يتضمن الـ prompt مجموعة متنوعة من أنواع النصوص المختلفة، بما في ذلك:

* [التعليمات الرئيسية](#main-prompt-system-prompt) للذكاء الاصطناعي حول كيفية توليد استجابة
* تعريفات [الأدوار التي يجب أن يتقمصها الذكاء الاصطناعي](/Usage/Characters/characterdesign.md)
* تعريفات [الدور الذي تتقمصه أنت](/Usage/personas.md)
* [معلومات حول "العالم"](/Usage/worldinfo.md) الذي يتفاعل معه الذكاء الاصطناعي
* مستندات أو معلومات ذات صلة من [Data Bank](/Usage/Characters/data-bank.md)
* [ملخصات](/extensions/Summarize.md) للمحادثة السابقة
* نتائج [عمليات البحث على الويب](/extensions/WebSearch.md) أو [مصادر بيانات خارجية](/For_Contributors/Function-Calling.md) أخرى
* الرسائل السابقة في المحادثة
* **رسالتك إلى الذكاء الاصطناعي**
* [التعليمات النهائية](#post-history-instructions) للذكاء الاصطناعي حول كيفية توليد استجابة

قد يكون هذا كثيراً لإدارته! لمساعدتك على فهم كيفية تنظيم وتعديل الطلب المُرسل إلى الذكاء الاصطناعي، يحدد SillyTavern عناصر مختلفة قد ترغب في تضمينها في prompt الخاص بك. يمكنك بعد ذلك تنظيم prompt الخاص بك لتضمين الأشياء التي تناسب الطريقة التي تريد بها التفاعل مع الذكاء الاصطناعي.

يتم شرح العديد من هذه العناصر في الأقسام التي ستقوم فيها بتغييرها. على سبيل المثال، لوصف الدور الذي ترغب في أن يتقمصه الذكاء الاصطناعي، يمكنك استخدام حقل [Description](/Usage/Characters/characterdesign.md#personality-summary) في [Character Design](/Usage/Characters/characterdesign.md).

## عرض Prompt

قراءة الـ prompt النهائي المُرسل إلى الذكاء الاصطناعي مفيدة جداً لفهم ما تم إخبار الذكاء الاصطناعي به، ولماذا ولّد الاستجابة التي قدمها. يمكنك عرض prompt بعدة طرق:

* استخدام أيقونة Prompt Itemization على رسالة الرد من الذكاء الاصطناعي
* استخدام إضافة [Prompt Inspector](https://github.com/SillyTavern/Extension-PromptInspector)
* فحص السجلات في نافذة Terminal التي تقوم بتشغيل SillyTavern فيها
* فحص Console في أدوات المطور في متصفحك

## تغيير كيفية بناء Prompt

تقديم جميع أجزاء prompt الخاص بك إلى الذكاء الاصطناعي بالطريقة الصحيحة أمر بالغ الأهمية للحصول على أفضل الاستجابات. يمكنك التحكم في كيفية بناء prompt.

+++ Text Completion APIs

استخدم لوحة [Advanced Formatting](advancedformatting.md) لتخصيص بناء prompt لـ Text Completion APIs.

+++ Chat Completion APIs

استخدم [Prompt Manager](prompt-manager.md) لتخصيص بناء prompt لـ Chat Completion APIs.

+++

## Main Prompt (System Prompt)

الـ Main Prompt (أو System Prompt) يحدد التعليمات العامة للنموذج لاتباعها. فهو يحدد النغمة والسياق للمحادثة. على سبيل المثال، يخبر النموذج بأن يتصرف كمساعد ذكاء اصطناعي، أو شريك في الكتابة، أو شخصية خيالية.

+++ Text Completion APIs

الـ [System Prompt](advancedformatting.md#system-prompt) هو جزء من [Story String](context-template.md#story-string) وعادة ما يكون الجزء الأول من prompt الذي يتلقاه النموذج.

+++ Chat Completion APIs

الـ Main Prompt هو أحد prompts الافتراضية في [Prompt Manager](prompt-manager.md). وهو عادة أول رسالة في السياق الذي يتلقاه النموذج، منسوبة إلى ("مُرسلة بواسطة") دور النظام.

+++

الـ Main Prompt الافتراضي هو:

> Write \{\{char\}\}'s next reply in a fictional chat between \{\{char\}\} and \{\{user\}\}.

يتم استبدال العناصر النائبة \{\{char\}\} و \{\{user\}\} بأسماء الشخصية والشخصية المستخدمة التي حددتها في المحادثة.

يمكنك استخدام أي من علامات [\{\{macro\}\}](/Usage/Characters/macros.md) المدعومة في Main Prompt لتضمين معلومات قد تختلف بين المحادثات أو تتغير مع تقدم المحادثة.

### تعديل Main Prompt

يساعد main prompt الافتراضي النموذج على فهم ما هو متوقع منه فعله مع معلومات الشخصية والشخصية المستخدمة التي تليها، وكيفية تفسير المحادثة السابقة، ونوع الاستجابة التي يجب توليدها. إنه prompt مرن متعدد الأغراض يعمل بشكل جيد في العديد من الحالات، لأنه يؤسس أن الذكاء الاصطناعي يكتب كشخصية في محادثة مع شخصيتك المستخدمة.

ومع ذلك، يمكنك تعديل main prompt لتلبية احتياجاتك بشكل أفضل. فيما يلي بعض الأسباب الشائعة لتعديل main prompt:

* **توفير تعليمات إضافية**: على سبيل المثال، تريد من الذكاء الاصطناعي شرح منطقه، أو اتباع قواعد محددة، أو تجنب موضوعات معينة
* **توضيح دور الذكاء الاصطناعي**: على سبيل المثال، تريد من الذكاء الاصطناعي أن يتصرف كراوٍ، أو راوي قصص، أو مرشد
* **تغيير سياق المحادثة**: على سبيل المثال، تريد من الذكاء الاصطناعي الرد كما لو كان مساعد ذكاء اصطناعي، أو لعبة مغامرات نصية، أو شريك في الكتابة

!!! جرب الأشياء وانظر ما يعمل بشكل أفضل بالنسبة لك
جميع الأمثلة في هذا الدليل نجحت بشكل جيد مع مستخدمين آخرين، لكن prompt الذي يعمل لاحتياجاتك والنموذج الذي تستخدمه قد يكون مختلفاً. جرب تعليمات وأنماط prompting مختلفة لترى ما يعمل بشكل أفضل بالنسبة لك. إذا لم تكن متأكداً مما يجب تجربته، يمكنك دائماً طلب المساعدة في [SillyTavern Discord](https://discord.gg/sillytavern).
!!!

إعطاء الذكاء الاصطناعي تعليمات إضافية في Main Prompt يمكن أن يساعده على فهم ما تريده من المحادثة.

> Write one reply only. Write at least one paragraph, up to four.

> Markdown is enabled. Use it to format your response. Enclose code snippets in triple backticks.

> Write character dialogue in quotation marks. Write \{\{char\}\}'s thoughts in parentheses.

> You are an anime roleplay generation model for users aged 13 to 17. You always generate fun, age-appropriate responses.

> Answer truthfully and write out your thinking step by step to be sure you get the right answer.

سيتبع الذكاء الاصطناعي بشكل أسهل التعليمات حول ما يجب عليه فعله بدلاً من ما لا يجب عليه فعله. على سبيل المثال، إذا كنت تريد من الذكاء الاصطناعي تجنب الكتابة بطريقة معينة، فمن الأفضل أن تخبره كيف تريد منه الكتابة بدلاً من ذلك. وبينما *"Do not decide what \{\{user\}\} says or does"* يتم تضمينه عادة في prompts لمنع الذكاء الاصطناعي من التحكم في شخصيتك المستخدمة، يجد بعض المستخدمين أن *"Write  \{\{char\}\}'s responses in a way that respects  \{\{user\}\}'s autonomy"* أكثر فعالية.

غالباً ما يكون هناك مكان أفضل من Main Prompt لتضمين معلومات حول المستخدم أو الشخصيات، أو تعديل أسلوب الكتابة والتحدث للشخصية، أو إعطاء تعليمات محددة أخرى. يُستخدم Main Prompt بشكل أفضل للتعليمات العامة حول المحادثة ككل، أو حول نوع المحادثة التي تريد إجراءها.

### تأثير سجل الرسائل

عند تعديل main prompt لتحسين استجابات الذكاء الاصطناعي، ضع في اعتبارك أن الذكاء الاصطناعي يلتقط الكثير من سجل الرسائل. السجل هو ذاكرته للأحداث الماضية، وتفاعلات الشخصيات والعلاقات، ودليل أسلوبه لاختيار الكلمات وأسلوب الكتابة.

استخدم هذا لصالحك من خلال توفير [رسائل مثال](/Usage/Characters/characterdesign.md#examples-of-dialogue) توضح كيف تريد من الذكاء الاصطناعي الاستجابة. إظهار ما تريد غالباً ما يكون أسهل من محاولة شرحه!

عندما يكون لمحادثتك بالفعل سجل، فإن تغيير main prompt له تأثير محدود على استجابات الذكاء الاصطناعي. من حيث الأحداث والعلاقات، يفترض الذكاء الاصطناعي أن main prompt حدث في الماضي البعيد، وأن سجل الرسائل يحدثه. من حيث أسلوب الكتابة واختيار الكلمات، يفترض الذكاء الاصطناعي أن جميع الرسائل في السجل تم توليدها وفقاً للقواعد في main prompt *الحالي*، وأنه يجب أن يستمر في توليد رسائل بنفس الطريقة. بعض الاقتراحات للتعامل مع هذا هي:

* إدراج التعليمات الحالية بالقرب من نهاية سجل الرسائل أو بعده، على سبيل المثال باستخدام [Author's Note](/Usage/Characters/Author's-Note.md)
* اختبار تغييراتك على main prompt من خلال بدء محادثة جديدة
* تحرير سجل الرسائل لإزالة أو تصحيح أمثلة السلوك غير المرغوب فيه
* استخدام [Post-History Instructions](#post-history-instructions) لتوفير تعليمات نهائية للذكاء الاصطناعي

!!! احصل عليه بشكل صحيح من المرة الأولى!
لا تدع الذكاء الاصطناعي "يفلت" من شيء لا تريده أن يفعله. إذا لم تعجبك استجابة الذكاء الاصطناعي، فلا تواصل المحادثة كما لو كانت صحيحة. بدلاً من ذلك، عدّل prompts، وأعد توليد الرسالة، وواصل من هناك. سيساعد هذا الذكاء الاصطناعي على تعلم ما تريد.
!!!

### إزالة سياق "Fictional Chat"

هناك حالات قد لا يكون فيها "fictional chat" هو السياق الصحيح لمحادثتك.

يمكنك إزالة سياق "fictional" من Main Prompt:

> Write \{\{char\}\}'s next reply in a conversation with \{\{user\}\}.

قد لا تريد من الذكاء الاصطناعي أن يفكر في نفسه على أنه يلعب دوراً على الإطلاق. بدلاً من إزالة فكرة الشخصية، يمكنك إزالة فكرة الذكاء الاصطناعي:

> You are \{\{char\}\}, a helpful assistant. You provide useful information and help \{\{user\}\} with their questions.

### الذكاء الاصطناعي كراوٍ أو راوي قصص

ماذا لو كنت تريد من الذكاء الاصطناعي أن يتصرف كراوٍ، يصف الأحداث من منظور كلي العلم، ويخترع شخصياته وإعداداته الخاصة؟

أحد الأساليب هو إنشاء شخصية مسماة للذكاء الاصطناعي لاستخدامها كراوٍ. يمكن أن تُسمى هذه الشخصية "Narrator" أو "AI"، مما يشير إلى أن الذكاء الاصطناعي هو راوي قصص متعدد الأغراض، أو يمكن تسميتها على اسم سيناريو أو إعداد محدد، مما يمنح الذكاء الاصطناعي مهمة سرد قصة في هذا الإعداد. يمكن بعد ذلك تحديد تفاصيل الإعداد في [Character](/Usage/Characters/characterdesign.md) أو في [World Info](/Usage/worldinfo.md).

ستحتاج إلى تعديل main prompt الافتراضي ليعكس دور الذكاء الاصطناعي. بالنسبة لراوٍ متعدد الأغراض، قد تستخدم:

> You are \{\{char\}\}, a skilled and versatile storyteller. Narrate the story.

أو لإعداد محدد:

> You are the narrator of a fantasy scenario. Play as the characters that visit \{\{char\}\}.

يساعد توضيح دور المستخدم في المحادثة. هل رسائلك جزء من القصة، أم أنها تعليمات للراوي حول ما تفعله أو تقوله شخصيتك؟ مثال يتضمن المستخدم في القصة:

> The story should progress by responding to the actions and dialogue of \{\{user\}\}. Narrate the story in third person.

مثال يبقي المستخدم خارج القصة:

> Enter Adventure Mode. Narrate the story based on \{\{user\}\}'s dialogue and actions after ">". Describe the surroundings in vivid detail. Be detailed, creative, verbose, and proactive. Move the story forward by introducing fantasy elements and interesting characters.

تحديد دور المستخدم لا يساعد الذكاء الاصطناعي فقط على فهم كيفية الرد على رسائلك، ولكن أيضاً إلى أي مدى يُسمح له بالتحكم في شخصيتك المستخدمة. هذا يتجنب المواقف التي يتخذ فيها الذكاء الاصطناعي قرارات لشخصيتك المستخدمة تفضل أن تتخذها بنفسك.

## Post-History Instructions

الـ Post-History Instructions (PHI) هي تعليمات إضافية تُرسل إلى الذكاء الاصطناعي بعد main prompt ورسالة المستخدم. يمكن استخدامها لتوفير سياق أو تعليمات إضافية للذكاء الاصطناعي بناءً على سجل الرسائل.

نظراً لأن Post-History Instructions تُرسل بعد رسالة المستخدم، فهي التعليمات النهائية التي يتلقاها الذكاء الاصطناعي قبل توليد استجابة. عادة ما يعطيها الذكاء الاصطناعي أولوية أعلى من main prompt، ويمكنها تجاوز تعليمات main prompt.

لاستخدام Post-History Instructions لكل شخصية، أضفها إلى [Post-History Instructions](/Usage/Characters/characterdesign.md) للشخصية وفعّل [Prefer Char. Instructions](/Usage/User_Settings/index.md). للحفاظ على PHI المحدد عالمياً أثناء استخدام تعليمات خاصة بالشخصية، يمكنك استخدام macro `{{original}}` في حقل Post-History Instructions للشخصية.

+++ Text Completion APIs

يتم تحديد Post-History Instructions في لوحة [Advanced Formatting](/Usage/Prompts/advancedformatting.md) ضمن فئة System Prompt. يتم إضافة Post-History Instructions كحقن دور مستخدم غير مرئي يسبق السطر الأخير من prompt (الذي يحتوي عادةً على "رأس" رسالة الاستجابة). لاحظ أن يجب تمكين زر "Enable System Prompt" لتطبيق Post-History Instructions (حتى لو كان System Prompt نفسه فارغاً).

+++ Chat Completion APIs

Post-History Instructions هو أحد prompts الافتراضية في [Prompt Manager](prompt-manager.md). وهو عادة آخر رسالة في السياق الذي يتلقاه النموذج، منسوبة إلى ("مُرسلة بواسطة") دور النظام. إذا كان Chat Completion API الخاص بك لا يدعم دور النظام، فسيتم عادةً نسبه إلى دور المستخدم بدلاً من ذلك.

+++

## إضافة إلى Prompt (World Info)

يمكنك إدراج معلومات إضافية في أي مكان في prompt باستخدام ميزة [World Info](/Usage/worldinfo.md). من خلال تحديد الشروط لوقت إدراج المعلومات، يمكنك توجيه الذكاء الاصطناعي لتضمين تفاصيل محددة، أو تغيير كيفية استجابته، أو إضافة عناصر جديدة إلى المحادثة.

بعض الاستخدامات الشائعة لـ World Info تشمل:

* "كتاب معلومات" أو "موسوعة" تحتوي على معلومات حول العالم أو الإعداد
* طريقة لإدارة system prompts مختلفة لشخصيات ومواقف مختلفة
* مكان لتخزين ذكريات يجب على الذكاء الاصطناعي "تذكرها" في المحادثة
* نظام أكثر نمطية لإنشاء وتحرير ومشاركة تفاصيل الشخصية
* مصدر لأحداث عشوائية ومفاجآت للذكاء الاصطناعي للتفاعل معها، أو لتجعلك تتفاعل!
