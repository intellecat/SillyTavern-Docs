---
route: /
---

# ما هو SillyTavern؟

![SillyTavern - LLM Frontend for Power Users](/static/banner.png)

SillyTavern (أو ST اختصارًا) هو واجهة مستخدم مثبتة محليًا تتيح لك التفاعل مع نماذج اللغة الكبيرة لتوليد النصوص، ومحركات توليد الصور، ونماذج الصوت TTS. هدفنا هو تمكين المستخدمين بأكبر قدر ممكن من الفائدة والتحكم في مطالبات LLM الخاصة بهم، مع احتضان منحنى التعلم الحاد كجزء من المتعة.

SillyTavern هو مشروع شغف يقدمه لك مجتمع مخصص من عشاق LLM وسيكون دائمًا مجانيًا ومفتوح المصدر. بدأ في فبراير 2023 كفرع من TavernAI 1.2.8، وأصبح لدى SillyTavern الآن أكثر من 200 مساهم وعامين من التطوير المستقل، ويستمر في العمل كبرنامج رائد لهواة الذكاء الاصطناعي المتمرسين.

## لقطات الشاشة

|   [![API Connection](/static/screenshot1.jpg)](/static/screenshot1.jpg)    |  [![Chat UI](/static/screenshot2.jpg)](/static/screenshot2.jpg)   |
|:--------------------------------------------------------------------------:|:-----------------------------------------------------------------:|
| [![Advanced Formatting](/static/screenshot3.jpg)](/static/screenshot3.jpg) | [![World Info](/static/screenshot4.jpg)](/static/screenshot4.jpg) |

## متطلبات التثبيت

متطلبات الأجهزة ضئيلة: سيعمل على أي شيء يمكنه تشغيل NodeJS 18 أو أعلى. إذا كنت تنوي إجراء استنتاج LLM على جهازك المحلي، نوصي ببطاقة رسومات NVIDIA من سلسلة 3000 بذاكرة VRAM لا تقل عن 6GB.

اتبع دليل التثبيت لمنصتك:

* [Windows](/Installation/Windows.md)
* [Linux و Mac](/Installation/LinuxMacOS.md)
* [Android](/Installation/Android.md)
* [Docker](/Installation/Docker.md)

## الفروع

يتم تطوير SillyTavern باستخدام نظام ذو فرعين لضمان تجربة سلسة لجميع المستخدمين.

* `release` -🌟 **موصى به لمعظم المستخدمين.** هذا هو الفرع الأكثر استقرارًا والموصى به، يتم تحديثه فقط عند إصدار الإصدارات الرئيسية. وهو مناسب لغالبية المستخدمين. يتم تحديثه عادةً مرة واحدة في الشهر.
* `staging` - ⚠️ **غير موصى به للاستخدام العادي.** يحتوي هذا الفرع على أحدث الميزات، ولكن كن حذرًا لأنه قد ينكسر في أي وقت. فقط للمستخدمين المتقدمين والمتحمسين. يتم تحديثه عدة مرات يوميًا.

## ما الذي أحتاجه بخلاف SillyTavern؟

نظرًا لأن SillyTavern هو مجرد واجهة، ستحتاج إلى الوصول إلى خلفية LLM لتوفير الاستنتاج. يمكنك استخدام AI Horde للدردشة الفورية خارج الصندوق. بصرف النظر عن ذلك، ندعم العديد من خلفيات LLM المحلية والسحابية الأخرى: OpenAI-compatible API، KoboldAI، Tabby، وغيرها الكثير. يمكنك قراءة المزيد حول واجهات برمجة التطبيقات المدعومة لدينا في قسم [اتصالات API](/Usage/API_Connections/index.md).

## بطاقات الشخصيات

تم بناء SillyTavern حول مفهوم "بطاقات الشخصيات". بطاقة الشخصية هي مجموعة من المطالبات التي تحدد سلوك LLM ومطلوبة لإجراء محادثات مستمرة في SillyTavern. تعمل بشكل مشابه لـ GPTs في ChatGPT أو bots في Poe. يمكن أن يكون محتوى بطاقة الشخصية أي شيء: سيناريو تجريدي، أو مساعد مصمم لمهمة معينة، أو شخصية مشهورة، أو شخصية خيالية.

لإجراء محادثة سريعة دون تحديد بطاقة شخصية أو لاختبار اتصال LLM فقط، ما عليك سوى كتابة مطالبتك في شريط الإدخال على [شاشة الترحيب](/Usage/welcome-assistants.md) بعد فتح SillyTavern. سيؤدي هذا إلى إنشاء بطاقة شخصية "مساعد" فارغة يمكنك تخصيصها لاحقًا.

للحصول على فكرة عامة حول كيفية تحديد بطاقات الشخصيات، راجع الشخصية الافتراضية (Seraphina) أو قم بتنزيل بطاقات مختارة من صنع المجتمع من قائمة "Download Extensions & Assets".

يمكنك أيضًا إنشاء بطاقات الشخصيات الخاصة بك من الصفر. راجع دليل [تصميم الشخصيات](/Usage/Characters/characterdesign.md) لمزيد من المعلومات.

## الميزات الرئيسية

* [إعدادات توليد النصوص المتقدمة](/Usage/Prompts/advancedformatting.md) مع العديد من الإعدادات المسبقة من صنع المجتمع
* [دعم World Info](Usage/worldinfo.md): أنشئ معلومات غنية أو وفر الرموز على بطاقة الشخصية الخاصة بك
* [الدردشات الجماعية](/Usage/Characters/groupchats.md): غرف متعددة الروبوتات للشخصيات للتحدث إليك و/أو لبعضها البعض
* [خيارات تخصيص واجهة المستخدم الغنية](/Usage/User_Settings/uicustomization.md): ألوان المظهر، وصور الخلفية، وCSS المخصص، والمزيد
* [شخصيات المستخدم](/Usage/personas.md): دع الذكاء الاصطناعي يعرف القليل عنك لمزيد من الانغماس
* [دعم RAG المدمج](/Usage/Characters/data-bank.md): أضف مستندات إلى محادثاتك ليرجع إليها الذكاء الاصطناعي
* نظام فرعي واسع [لأوامر الدردشة](/Usage/Chatting/slashcommands.md) و[محرك البرمجة النصية](/For_Contributors/st-script.md) الخاص

## الإضافات

يدعم SillyTavern قابلية التوسع.

* [التعبيرات العاطفية للشخصيات (sprites)](/extensions/Expression-Images.md)
* [الملخص التلقائي لتاريخ الدردشة](/extensions/Summarize.md)
* واجهة المستخدم التلقائية و[ترجمة الدردشة](extensions/Translation.md)
* [توليد الصور بـ Stable Diffusion/FLUX/DALL-E](/extensions/Stable-Diffusion.md)
* [تحويل النص إلى كلام لرسائل استجابة الذكاء الاصطناعي (عبر ElevenLabs أو Silero أو نظام TTS للنظام)](/extensions/TTS.md)
* [قدرات البحث على الويب لإضافة سياق إضافي من العالم الحقيقي إلى مطالباتك](/extensions/WebSearch.md)
* العديد من الإضافات الأخرى متاحة للتنزيل من قائمة "Download Extensions & Assets".

## كيف يمكنني التواصل مع المطورين مباشرة؟

* Discord: cohee, rossascends, wolfsblvt
* Reddit: [/u/RossAscends](https://www.reddit.com/user/RossAscends/), [/u/sillylossy](https://www.reddit.com/user/sillylossy/), [u/Wolfsblvt](https://www.reddit.com/user/Wolfsblvt/)
* [انشر مشكلة على GitHub](https://github.com/SillyTavern/SillyTavern/issues)

## أحب مشروعكم! كيف أساهم؟

* نرحب بطلبات السحب! اتبع [إرشادات المساهمة](https://github.com/SillyTavern/SillyTavern/blob/release/CONTRIBUTING.md) للبدء.
* نرحب أيضًا بتقارير الأخطاء المفيدة والمستنيرة التي تستخدم القوالب المقدمة في GitHub الخاص بنا.
* نحن لا نقبل التبرعات المالية للمشروع نفسه.

## التبرعات الشخصية

دعمك للمساهمين الأفراد موضع تقدير، لكنه لن يؤثر على الاتجاه العام لتطوير SillyTavern.

* RossAscends لديه [Patreon](https://www.patreon.com/RossAscends) و [Kofi](https://ko-fi.com/rossascends) شخصي

## الترخيص

SillyTavern هو مشروع مجاني ومفتوح المصدر صدر بموجب [ترخيص AGPL-3.0](https://github.com/SillyTavern/SillyTavern/blob/release/LICENSE).
