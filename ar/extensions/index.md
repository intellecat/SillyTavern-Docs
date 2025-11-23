---
label: الإضافات
icon: plug
expanded: true
order: 35
route: /extensions/
---

# الإضافات

يأتي SillyTavern مع العديد من الإضافات التي يمكن تمكينها أو تعطيلها في لوحة الإضافات. يمكن للإضافات إضافة ميزات جديدة، أو تغيير سلوك الميزات الموجودة، أو توفير محتوى إضافي لاستخدام الذكاء الاصطناعي الخاص بك. يمكن تثبيت المزيد من الإضافات من قائمة "تنزيل الإضافات والأصول" في لوحة الإضافات.

## لوحة الإضافات

لفتح أو إغلاق لوحة الإضافات، اختر **<i class="fa-solid fa-cubes fa-fw"></i> Extensions** في الشريط العلوي.

- **<i class="fa-solid fa-cubes"></i> Manage extensions**: تنشيط وإلغاء تنشيط وتحديث الإضافات
- **Download Extensions & Assets**: تثبيت [المزيد من الإضافات](#installable-extensions)، والشخصيات، والأصوات، والخلفيات من مستودع SillyTavern
- **Notify on extension updates**: حدد هذا الخيار ليتم إعلامك عند توفر تحديثات للإضافات المثبتة
- **<i class="fa-solid fa-cloud-arrow-down"></i> Install extension**: استيراد [إضافة من طرف ثالث](#third-party-extensions) من عنوان URL لمستودع Git

## الإضافات المدمجة

هذه الإضافات مدمجة في SillyTavern ولا تحتاج إلى تثبيت. يمكن تمكينها أو تعطيلها في لوحة الإضافات.

:::callout
**[Chat Translation](Translation.md)**

ترجمة رسائل الدردشة إلى لغة مختلفة
:::

:::callout
**[Image Captioning](captioning.md)**

يولد نصاً من الصور حتى يتمكن الذكاء الاصطناعي الخاص بك من "رؤية" والاستجابة للمحتوى المرئي في محادثاتك
:::

:::callout
**[Image Generation](Stable-Diffusion.md)**

استخدم واجهات برمجة التطبيقات المحلية أو السحابية لـ Stable Diffusion أو FLUX أو DALL-E لإنشاء الصور
:::

:::callout
**[Expression Images](Expression-Images.md)**

صور (المعروفة باسم 'sprites') لشخصية الذكاء الاصطناعي الخاصة بك، تُعرض بجانب نافذة الدردشة أو خلفها
:::

:::callout
**[Summarize](Summarize.md)**

ملخص تلقائي لسجل الدردشة
:::

:::callout
**[Chat Vectorization](Chat-vectorization.md)**

يعثر على الرسائل ذات الصلة من سجل الدردشة ويضيفها إلى السياق
:::

:::callout
**[Text To Speech](TTS.md)**

سرد صوتي لرسائل الدردشة الخاصة بك عبر ElevenLabs، Silero، نظام TTS الخاص بك، **[AllTalk](AllTalk.md)**، **[XTTS](XTTS.md)**، والمزيد
:::

:::callout
**[Quick Reply](/For_Contributors/st-script.md#quick-replies-script-library-and-auto-execution)**

الرد على رسائل الدردشة بنقرة واحدة، وتشغيل الأوامر وبرامج STscripts، والمزيد
:::

:::callout
**Token Counter**

يحول النص إلى رموز ويحسب عدد الرموز
:::

---

## الإضافات القابلة للتثبيت

!!!tip
يجب أن يكون لديك git مثبتاً لتنزيل الإضافات. اتبع التعليمات في [صفحة تثبيت Git](https://git-scm.com/downloads) إذا لم يكن لديك مثبتاً.
!!!

يمكنك تصفح قائمة بجميع الإضافات المتاحة مباشرة من التطبيق بالانتقال إلى قائمة **<i class="fa-solid fa-cubes"></i> Extensions** => **Download Extensions & Assets** والنقر على زر **<i class="fa-solid fa-plug-circle-exclamation"></i> Load Asset List**. لتثبيت إضافة، انقر على زر **<i class="fa-solid fa-download"></i> Download**. لقراءة المزيد عن إضافة، انقر على زر **<i class="fa-solid fa-arrow-up-right-from-square"></i> Link** بجانب اسمها لفتح صفحتها على GitHub.

!!!info الإضافات ليست Extras
تم إيقاف مشروع Extras في أبريل 2024. لا تحتاج إلى تثبيت Extras لاستخدام الإضافات.
!!!

:::callout
**[Blip](Blip.md)**

تحريك نص رسائل الشخصية بسرعة متغيرة وتشغيل الصوت مع الرسوم المتحركة.
:::

:::callout
**[Dynamic Audio](Dynamic-Audio.md)**

يضيف موسيقى خلفية غامرة وأصوات محيطة إلى دردشاتك.
:::

:::callout
**[EmulatorJS](EmulatorJS.md)**

العب ألعاب الكونسول القديمة مباشرة في دردشات SillyTavern.
:::

:::callout
**[Live2d](Live2d.md)**

يضيف دعماً لنماذج live2d. تعبيرات ورسوم متحركة وتفاعلات قابلة للتخصيص.
:::

:::callout
**[Objective](Objective.md)**

حدد هدفاً للذكاء الاصطناعي ليسعى إليه أثناء الدردشة.
:::

:::callout
**[RVC](RVC.md)**

يضيف قدرات استنساخ الصوت في الوقت الفعلي إلى وحدة تحويل النص إلى كلام.
:::

:::callout
**[Speech Recognition](Speech-Recognition.md)**

تحويل كلامك إلى نص باستخدام المتصفح أو extras.
:::

:::callout
**[VRM](VRM.md)**

يضيف دعماً لنماذج VRM. تعبيرات ورسوم متحركة وتفاعلات قابلة للتخصيص.
:::

:::callout
**[Web Search](WebSearch.md)**

يضيف نتائج البحث على الويب إلى مطالبات LLM.
:::

:::callout
**[AccuWeather](https://github.com/SillyTavern/Extension-AccuWeather)**

يوفر معلومات الطقس باستخدام واجهة برمجة التطبيقات AccuWeather كأمر slash أو أداة وظيفة.
:::

:::callout
**[Chat Top Bar](https://github.com/SillyTavern/Extension-TopInfoBar)**

يضيف شريطاً علوياً إلى نافذة الدردشة مع اختصارات للإجراءات السريعة.
:::

:::callout
**[Chess](https://github.com/SillyTavern/SillyTavern-Chess)**

العب لعبة الشطرنج مع LLM.
:::

:::callout
**[Code Runner](https://github.com/SillyTavern/Extension-CodeRunner)**

يسمح بتشغيل كود JavaScript وSTscript من كتل الكود في الدردشة.
:::

:::callout
**[D&D Dice](https://github.com/SillyTavern/Extension-Dice)**

مجموعة من 7 نرد D&D الكلاسيكية لجميع احتياجات رمي النرد الخاصة بك.
:::

:::callout
**[Duplicate Finder](https://github.com/SillyTavern/Extension-DupeFinder)**

يضيف القدرة على تجميع الشخصيات حسب مجموعات التشابه للعثور بسهولة على التكرارات.
:::

:::callout
**[Emoji Picker](https://github.com/SillyTavern/Extension-EmojiPicker)**

يضيف زراً لإدراج الرموز التعبيرية بسرعة في رسالة الدردشة.
:::

:::callout
**[Group Greetings](https://github.com/SillyTavern/Extension-GroupGreetings)**

يسمح بتعيين تحيات بديلة خاصة بدردشات المجموعة.
:::

:::callout
**[Group SendAs](https://github.com/SillyTavern/SillyTavern-GroupSendAs)**

يضيف زراً لإدراج قالب أمر /sendas بسرعة لعضو المجموعة المحدد.
:::

:::callout
**[HypeBot](https://github.com/SillyTavern/Extension-HypeBot)**

عرض اقتراحات مخصصة بناءً على دردشاتك الأخيرة باستخدام محرك HypeBot من NovelAI. يتطلب اشتراكاً نشطاً في NovelAI.
:::

:::callout
**[Idle](https://github.com/SillyTavern/Extension-Idle)**

يضيف "مطالبة الخمول" بعد أن يكون المستخدم خاملاً لبعض الوقت لمواصلة المحادثة بشكل طبيعي.
:::

:::callout
**[Image Metadata Viewer](https://github.com/SillyTavern/Extension-ImageMetadataViewer)**

عرض البيانات الوصفية للصور المكبرة المرفقة بالدردشة.
:::

:::callout
**[LaTeX](https://github.com/SillyTavern/Extension-LaTeX)**

عرض صيغ LaTeX وAsciiMath في رسائل الدردشة.
:::

:::callout
**[Mermaid](https://github.com/SillyTavern/Extension-Mermaid)**

يضيف عرض رسوم بيانية ومخططات تدفق Mermaid إلى دردشات SillyTavern.
:::

:::callout
**[Notebook](https://github.com/SillyTavern/Extension-Notebook)**

يضيف مكاناً لتخزين ملاحظاتك. يدعم تنسيق النص الغني.
:::

:::callout
**[Parameter Randomizer](https://github.com/SillyTavern/Extension-Randomizer)**

يضيف القدرة على عشوائية منزلقات إعدادات API مع كل عملية توليد.
:::

:::callout
**[Prome Visual Novel Extension](https://github.com/Bronya-Rand/Prome-VN-Extension)**

يعزز تجربة الرواية المرئية الحالية بمزيد من الميزات (وضع التركيز، وضع Letterbox، والمزيد)!
:::

:::callout
**[Prompt Inspector](https://github.com/SillyTavern/Extension-PromptInspector)**

يضيف خياراً لفحص وتحرير المطالبات الناتجة قبل إرسالها إلى الخادم.
:::

:::callout
**[Push Notifications](https://github.com/SillyTavern/SillyTavern-PushNotifications)**

يسمح بتلقي إشعارات فورية لرسائل الدردشة الواردة.
:::

:::callout
**[Quick Persona](https://github.com/SillyTavern/Extension-QuickPersona)**

يضيف قائمة منسدلة لاختيار شخصيات المستخدم من شريط الدردشة.
:::

:::callout
**[RSS](https://github.com/SillyTavern/Extension-RSS)**

يحصل على آخر الأخبار من موجزات RSS كأمر slash أو أداة وظيفة.
:::

:::callout
**[Screen Share](https://github.com/SillyTavern/Extension-ScreenShare)**

يوفر صورة الشاشة للنماذج متعددة الوسائط عند إرسال رسالة.
:::

:::callout
**[Silence Player](https://github.com/SillyTavern/Extension-Silence)**

يضيف مشغل صوت صامت إلى قائمة الإضافات. يمكن أن يساعد إذا كانت علامة تبويب المتصفح تُغلق في الخلفية.
:::

:::callout
**[Timelines](https://github.com/SillyTavern/SillyTavern-Timelines)**

يضيف تنقلاً زمنياً إلى سجل الدردشة.
:::

:::callout
**[Variable Viewer](https://github.com/LenAnderson/SillyTavern-Variable-Viewer)**

طريقة سهلة لعرض وتعديل المتغيرات.
:::

:::callout
**[WebLLM](https://github.com/SillyTavern/Extension-WebLLM)**

يوفر واجهة للإضافات لاستخدام نماذج اللغة مباشرة في المتصفح.
:::

## إضافات الطرف الثالث

!!!danger
استخدام إضافات الطرف الثالث يمكن أن يكون له آثار جانبية غير مقصودة وقد يشكل مخاطر أمنية.
تأكد دائماً من أنك تثق في المصدر قبل استيراد إضافة عبر **<i class="fa-solid fa-cloud-arrow-down"></i> Install extension**.
نحن لسنا مسؤولين عن أي ضرر ناتج عن إضافات الطرف الثالث.
!!!

لتثبيت إضافة من طرف ثالث، انتقل إلى قائمة **<i class="fa-solid fa-cubes"></i> Extensions** => **<i class="fa-solid fa-cloud-arrow-down"></i> Install Extension** والصق عنوان URL لمستودع الإضافة. اختيارياً، حدد الفرع و(في سيناريوهات [متعدد المستخدمين](../Administration/multi-user.md)) هدف التثبيت: جميع المستخدمين أو المستخدم الحالي فقط. سيتم تنزيل الإضافة وتحميلها تلقائياً.
