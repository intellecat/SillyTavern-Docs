---
order: tts-alltalk
route: /extensions/alltalk/
---
# AllTalk TTS V2

AllTalk هو نظام استنساخ صوت مبني على Coqui XTTS و F5-TTS و VITS و Piper ومحركات نماذج TTS أخرى، مصمم لإنتاج استنساخ صوتي عالي الجودة (إما استنساخ صوتي بدون عينات أو أصوات مدمجة). في AllTalk V2، تعزز التحديثات المهمة الوظائف وسهولة الاستخدام، بما في ذلك دعم محركات TTS المتعددة، والتخصيص الموسع، وتحسينات الأداء. للحصول على قائمة شاملة بالميزات، راجع [AllTalk Wiki هنا](https://github.com/erew123/alltalk_tts/wiki).

---

## 🟩 الميزات الرئيسية في AllTalk V2
- **دعم محركات متعددة**: التبديل بسهولة بين Coqui XTTS و VITS و Piper و Parler و F5 والمحركات المخصصة.
- **تحويل الصوت (RVC)**: خط أنابيب محسّن لاستنساخ الصوت القائم على الاسترجاع.
- **إعدادات قابلة للتخصيص**: ضبط الإعدادات لكل محرك وحفظ تكوينات بدء التشغيل.
- **وظيفة الراوي**: تحديد أصوات منفصلة للسرد والشخصيات.
- **استخدام مستقل ومتكامل**: تكامل سلس مع SillyTavern.
- **أوضاع DeepSpeed و Low VRAM**: تحسين الأداء للبيئات محدودة الموارد.
- **لقطات الشاشة**: راجع واجهة AllTalk V2 [هنا](https://github.com/erew123/alltalk_tts/discussions/237).

---

## 🟨 خيارات الإعداد والتثبيت

يوفر AllTalk طرق تثبيت مستقلة ومتكاملة. يتضمن الإعداد الأسرع استخدام أحد خيارات التثبيت السريع المتوفرة، مع برامج نصية تعمل على أتمتة معظم العملية.

- **التثبيت المستقل**: موصى به لمعظم المستخدمين ([دليل التثبيت المستقل](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Standalone-Installation))
- **تكامل Text-generation-webui**: للتكامل في Text-generation-webui ([دليل تثبيت TGWUI](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Text%E2%80%90generation%E2%80%90webui-Installation))

#### 🟩 التثبيت الآلي
**هذه الطريقة لمستخدمي Windows فقط.**
للمستخدمين الجدد الذين يريدون إعداداً سريعاً، يستخدم التثبيت الآلي SillyTavern-Launcher.
ملاحظة: يفترض هذا أنك قمت بالفعل بتثبيت SillyTavern-Launcher. إذا لم تفعل، قم بزيارة https://github.com/SillyTavern/SillyTavern-Launcher واتبع التعليمات في ملف readme.md لتثبيته.
بمجرد تثبيت SillyTavern-Launcher:
1. قم بتشغيل Launcher.bat
2. انتقل إلى: `Home > Toolbox > App Installer > Voice Generation`
3. حدد الخيار المسمى: **Install AllTalk V2**

#### 🟩 التثبيت اليدوي
للمستخدمين المتقدمين الذين يحتاجون إلى تحكم تفصيلي، اتبع [دليل التثبيت اليدوي](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Manual-Installation-Guide) للحصول على إعداد خطوة بخطوة على Windows أو Linux أو Mac (غير مختبر).

#### 🟩 تثبيت Google Colab
قم بتشغيل AllTalk في بيئة سحابية مع [تثبيت Google Colab](https://github.com/erew123/alltalk_tts/wiki/Google-COLAB) للمستخدمين الذين يفضلون عدم التثبيت محلياً.

---

## 🟨 استخدام AllTalk داخل SillyTavern

بمجرد تحميل AllTalk، حدده داخل SillyTavern في صفحة TTS، مع التأكد من تحديد إصدار خادم AllTalk الصحيح في الإعدادات.

- **إدارة الإعدادات**: قد يقوم AllTalk بتمكين أو تعطيل إعدادات محددة بناءً على التكوين المحدد.
- **تسلسل التحميل**: إذا تم تحميل SillyTavern قبل AllTalk، أعد تحميل صفحة إضافات TTS.
- **تحسين الأداء**: قم بتمكين أوضاع DeepSpeed و Low VRAM بشكل انتقائي لتحسين الأداء بناءً على موارد النظام.
- **وظيفة الراوي**: يمكن العثور على تفاصيل وظيفة الراوي في [AllTalk Wiki](https://github.com/erew123/alltalk_tts/wiki/Narrator-Function).

سيتم تحديث التفاصيل الكاملة لإضافة SillyTavern AllTalk في [صفحة AllTalk Wiki لـ SillyTavern](https://github.com/erew123/alltalk_tts/wiki/SillyTavern-Extension)

يحتاج مستخدمو TGWUI الذين يستخدمون إضافة AllTalk لـ TGWUI إلى تعطيل `Enable TGWUI TTS` في واجهة الدردشة TGWUI، وإلا سيكون لديك صوت TTS مكرر.

---

## 🟨 استكشاف الأخطاء وإصلاحها

إذا واجهت مشكلات تعتقد أنها خاصة بـ AllTalk داخل SillyTavern، يرجى الرجوع إلى [صفحة AllTalk Wiki لـ SillyTavern](https://github.com/erew123/alltalk_tts/wiki/SillyTavern-Extension) للحصول على أحدث المعلومات.

---

### 🟪 الدعم والمساعدة وطلبات الميزات

لمزيد من المساعدة:
- راجع [Wiki](https://github.com/erew123/alltalk_tts/wiki) والوثائق المدمجة.
- انضم إلى المناقشات على [لوحة النقاش](https://github.com/erew123/alltalk_tts/discussions/245).
- أرسل الأخطاء أو طلبات الميزات من خلال [متتبع المشكلات](https://github.com/erew123/alltalk_tts/issues).

---
