---
order: 50
templating: false
route: /usage/prompts/prompt-manager/
---

# Prompt Manager

الـ Prompt Manager هو نظام يوفر مزيداً من التحكم في استراتيجية [بناء prompt](index.md) لـ Chat Completion APIs.

!!! ينطبق على: Chat Completion APIs
للإعدادات المكافئة في Text Completion APIs، استخدم [Advanced Formatting](advancedformatting.md).
!!!

!!!tip تسمية Presets
إذا شارك preset اسماً مع إحدى بطاقات الشخصيات الخاصة بك، فسيتم تحديده تلقائياً عند بدء محادثة مع تلك الشخصية. قم بتسمية presets بشيء فريد لتجنب هذا السلوك.
!!!

الوصول إلى Prompt Manager بالنقر على زر "AI Response Configuration" في شريط التنقل. يقع Prompt Manager أسفل لوحة [common settings](/Usage/Common-Settings.md).

## Quick Prompts Edit

يوفر مساحة لتحرير أقسام prompt الشائعة بسرعة، مثل **Main Prompt** و **Auxiliary Prompt** و **Post-History Instructions**. يمكن العثور على مزيد من المعلومات حول هذه prompts في صفحة [بناء prompt](index.md).

## Utility Prompts

يتم إرسال هذه prompts إلى نموذج Chat Completion لمساعدته على فهم المعلومات التي يتم إرسالها إليه، أو لتوجيهه للتصرف بطرق محددة أثناء أنواع معينة من التفاعلات.

### Format Templates

!!!tip
إذا لم يتم تعيين format template، فسيتم إرسال المعلومات كما هي، بدون أي تغليف.
!!!

هذه هي templates نصية تُستخدم لتغليف المعلومات المسحوبة من [World Info](/Usage/worldinfo.md) و [Character Cards](/Usage/Characters/characterdesign.md).

يُستخدم علامة خاصة للإشارة إلى مكان إدراج المعلومات:

- `{0}` لـ World Info format template.
- `{{scenario}}` لـ Scenario format template.
- `{{personality}}` لـ Personality format template.

### Group Nudge Prompt Template

يُستخدم فقط في group chats. يُوضع في نهاية prompt لفرض رد من شخصية محددة.

اترك هذا فارغاً لتعطيل وظيفة Group Nudge.

### New Chat, New Group Chat, New Example Chat

يتم إرسالها قبل سجل المحادثة وقبل كل كتلة [Example Dialogue](/Usage/Characters/characterdesign.md#examples-of-dialogue) لإعلام النموذج بمكان انتهاء معلومات الخلفية وبداية سجل المحادثة.

- **New Chat:** يُستخدم للمحادثات الفردية.
- **New Group Chat:** يُستخدم لـ group chats.
- **New Example Chat:** يُستخدم لكتل حوار الأمثلة.

اترك هذه فارغة لتعطيل هذه الوظيفة.

### Continue Nudge

يُرسل في نهاية prompt لتوجيه النموذج حول ما يجب فعله عند تشغيل Continue، مثل عند الضغط على زر Continue أو عند تشغيله بواسطة STScript.

!!! Chat Completion 'Continues'
ضع في اعتبارك أن نماذج Chat Completion تتعامل مع Continues بشكل مختلف عن نماذج **Text Completion**، وقد لا تقدم دائماً نتائج سلسة بغض النظر عن Continue Nudge الخاص بك.
!!!

### Replace Empty Message

يرسل محتويات هذا الحقل بدلاً من رسالة فارغة عندما يكون مربع النص فارغاً ويتم الضغط على **Send a message**.

## Character Names Behavior

يوفر استراتيجيات مختلفة لتوجيه النموذج حول كيفية ربط الرسائل بالشخصيات. إذا كان نموذج Chat Completion يواجه مشكلة في تحديد الرسائل التي تنتمي إلى أي شخصية، فقد يحتاج إلى استراتيجية مختلفة محددة.

## Continue Postfix

عند تشغيل Continue، سيتم إلحاق Continue Postfix المحدد ببداية الرسالة 'المستمرة' المرتجعة من النموذج. على سبيل المثال، يمكن إضافة مسافة قبل النص المستمر.

## Additional Settings

### Wrap in Quotes

!!!warning
خيار مهمل. يُفضل استخدام [Regex scripts](/extensions/Regex.md) بدلاً من ذلك.
!!!

يغلف رسالة المستخدم بأكملها في علامات اقتباس مخفية قبل الإرسال. هذا مفيد للجلسات حيث لا تستخدم الشخصيات علامات الاقتباس للإشارة إلى الكلام. إذا كانت جلستك تستخدم علامات الاقتباس للإشارة إلى الكلام، اترك هذا غير محدد.

### Continue Prefill

!!!warning
قد لا يعمل مع جميع مصادر Chat Completion.
!!!

يرسل Continue Nudge كرسالة بدور Assistant بدلاً من رسالة System. إذا تم تمكين هذا، فلن يتم استخدام Continue Nudge prompt.

### Squash system messages

!!!warning
خيار مهمل. يُفضل استخدام [Prompt Post-Processing](/Usage/API_Connections/openai.md#prompt-post-processing) بدلاً من ذلك.
!!!

يجمع رسائل System المتتالية في رسالة واحدة مجمعة (باستثناء Example Dialogue).

### Enable web search

!!!
لا تخلط مع [Web Search extension](/extensions/WebSearch.md).
!!!

يمكّن قدرات البحث على الويب التي يوفرها backend Chat Completion. عادة ما يتم إثراء prompt بنتائج البحث من قبل مزود النموذج وقد يتحمل تكاليف إضافية.

### Enable function calling

راجع [Function Calling](/For_Contributors/Function-Calling.md)

### Send inline images, Send inline videos

!!!
لا تخلط مع [Image Captioning extension](/extensions/captioning.md).
!!!

إذا كان نموذج Chat Completion يمتلك قدرات متعددة الوسائط لمعالجة الصور ومقاطع الفيديو المقدمة، فهذا يبدل قدرته على القيام بذلك. لإلحاق وسائط بـ prompt، استخدم خيار **Attach A File** في قائمة "Magic Wand".

### Request inline images

!!!
لا تخلط مع [Image Generation extension](/extensions/Stable-Diffusion.md).
!!!

يسمح للنموذج بإرجاع مرفقات الصور.

### Use system prompt

!!!
مدعوم فقط من قبل Google Gemini و Anthropic Claude backends.

على الرغم من وجود إعدادات متشابهة جداً لهذين الاثنين، إلا أنهما تقنياً خيارات منفصلة، لذا يمكن تكوينهما بشكل منفصل.
!!!

يدمج جميع رسائل النظام حتى الرسالة الأولى بدور غير نظام (User/Assistant) ويرسلها كحقل تعليمات نظام منفصل.

## Reasoning Settings

إذا كان نموذج Chat Completion يستخدم reasoning، فإن هذه الإعدادات تؤثر على رؤيته ووظائفه.

### Request model reasoning

راجع [Adding Reasoning: By Backend](/Usage/Prompts/reasoning.md#by-backend).

### Reasoning Effort

راجع [Reasoning Effort](/Usage/Prompts/reasoning.md#reasoning-effort).

## "Prompts"

يشكل Prompt Manager العمود الفقري لـ prompt المُرسل إلى نموذج Chat Completion. فهو يتحكم في ما يتم إرساله وكذلك *الترتيب* الذي يتم إرساله به.

### القائمة المنسدلة 'Prompts'

تحتوي على قائمة منسدلة بجميع prompts (غير الافتراضية) التي يتضمنها Chat Completion preset الحالي. لإضافة أحد هذه prompts إلى الرسالة الصادرة، يجب تحديده من القائمة المنسدلة ثم إضافته إلى Prompt Manager بالضغط على زر **Insert prompt**. لإنشاء prompt جديد لإضافته إلى هذه القائمة المنسدلة، اضغط على زر **New prompt**. بمجرد كتابة prompt الجديد وحفظه، تتم إضافته إلى القائمة المنسدلة ويمكن بعد ذلك إدراجه.

### Prompts List

هذه واجهة سحب وإفلات تسرد prompts المحددة لإرسالها إلى نموذج Chat Completion. يتم إرسال Prompts الموضوعة بالقرب من **أعلى** الواجهة في وقت أبكر. **أسفل** القائمة هو **آخر شيء** يتم إرساله إلى النموذج (عادةً، سيكون هذا **Post-History Instructions** الخاص بك).

!!! 'Pinned' prompts = Default prompts
لا يمكن إزالة prompts الافتراضية من قائمة prompts المحددة. هذا يشمل Main Prompt و World Info (before/after) و Persona Description و Character Description و Character Personality و Scenario و Enhance Definitions و Auxiliary Prompt و Chat Examples و Chat History و Post-History Instructions. إذا لم تكن هذه مرغوبة، فيمكن **تبديلها إلى 'OFF'**، ولكن لا يمكن إزالتها أو حذفها بشكل كامل.
!!!

## تحرير Prompt

النقر على **زر القلم** على prompt سينقلك إلى **واجهة التحرير**. هنا، يمكنك تحرير prompt مباشرة.

!!! تأكد من حفظ تغييراتك!
لحفظ التغييرات على هذه prompts بشكل دائم في Chat Completion preset الخاص بك، يجب النقر على زر **Save** في الجزء السفلي الأيمن من **واجهة التحرير**، بالإضافة إلى حفظ preset نفسه باستخدام زر **Save** الموجود في أعلى قسم **AI Response Configuration**! وإلا، ستُفقد التغييرات المُجراة عند التبديل إلى Chat Completion preset آخر.
!!!

### Name

اسم prompt. لا يتم إرسال هذا إلى نموذج Chat Completion؛ إنه لمرجعك داخل Prompt Manager فقط.

### Role

الدور الذي يرسل prompt. يمكنك الاختيار بين System أو AI Assistant أو User.

### Triggers

أنواع التوليد التي يتم إرسال هذا prompt من أجلها. إذا لم يتم تحديد أي شيء، فسيتم إرسال prompt لجميع أنواع التوليد. إذا تم تحديد واحد أو أكثر، فسيتم إرسال prompt فقط لأنواع التوليد المحددة:

- **Normal:** طلب توليد رسالة عادية.
- **Continue:** عند الضغط على زر Continue.
- **Impersonate:** عند الضغط على زر Impersonate.
- **Swipe:** عند تشغيل التوليد بواسطة التمرير.
- **Regenerate:** عند الضغط على زر Regenerate في المحادثات الفردية.
- **Quiet:** طلبات توليد في الخلفية، عادةً ما يتم تشغيلها بواسطة [extensions](/extensions/index.md) أو أوامر [STscript](/For_Contributors/st-script.md).

!!!
محفز "Regenerate" غير متوفر في group chats حيث يستخدم منطق إعادة توليد مختلف: يتم حذف جميع الرسائل من آخر رد، ويتم وضع الرسائل في قائمة انتظار باستخدام نوع التوليد "Normal" وفقاً لـ [Group reply strategy](/Usage/Characters/groupchats.md#reply-order-strategies) المختارة.
!!!

### Position

عند ضبط Position على **Relative**، يتم إرسال هذا prompt حيث يقع في واجهة السحب والإفلات مع جميع prompts الأخرى. عند ضبطه على **In-Chat** وإعطائه **Depth**، يتم إرساله بدلاً من ذلك **داخل Chat History** بالدور المحدد، و**يتجاهل** ترتيب واجهة السحب والإفلات.

### Depth

عند ضبط Position على **In-Chat**، يحدد هذا مدى عمق إرسال prompt داخل سجل المحادثة. كلما زاد الرقم، زاد العمق. على سبيل المثال، Depth من 0 سيتم إرساله بعد آخر رسالة محادثة، Depth من 1 سيتم إرساله قبل آخر رسالة محادثة، و Depth من 2 سيتم إرساله قبل ثاني آخر رسالة محادثة، وهكذا.

### Order

!!!
سيتم تجميع Prompts التي لها نفس Role و Depth معاً وترتيبها حسب قيمة Order الخاصة بها.
الترتيب كالتالي (من الأعلى إلى الأسفل): User، AI Assistant، System.
!!!

عند ضبط Position على **In-Chat**، يحدد هذا الترتيب الذي يتم إرسال prompt به داخل سجل المحادثة. كلما انخفض الرقم، كلما تم إرساله في وقت أبكر.

## بناء Prompt الخاص بك: نصائح وحيل

قم بزيارة قسم [بناء prompt](index.md) في توثيق SillyTavern لمزيد من المعلومات حول كيفية كتابة prompts فعالة. يمكن تطبيق المعلومات إلى حد كبير على Chat Completion presets.
