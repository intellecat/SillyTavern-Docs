---
order: tts
route: /extensions/tts/
---

# TTS

يحتوي SillyTavern على مجموعة واسعة من خيارات TTS (تحويل النص إلى كلام) المستخدمة لجعل صوت يقرأ أجزاء من محادثتك. تشرح هذه الصفحة الإعداد والاستخدام.

## تكوين TTS

### اختيار مزود TTS

يُستخدم لتحديد خدمة TTS التي تريد استخدامها. بعض الخيارات مجانية، وبعضها يتطلب اشتراكًا مدفوعًا، والبعض الآخر يعمل محليًا على جهاز الكمبيوتر الخاص بك.

الخيارات المتاحة (قد تتغير القائمة بمرور الوقت):

- **AllTalk** - مجاني، تثبيت محلي مفتوح المصدر، يقدم مجموعة متنوعة من محركات TTS. راجع صفحة [AllTalk](./AllTalk.md) للحصول على إرشادات الإعداد.
- **Azure TTS** - نفس الأصوات مثل Microsoft Edge. يتطلب حساب Azure واشتراكًا مدفوعًا.
- **Coqui-TTS** (مهمل) - مجاني، يتطلب Extras API للتشغيل. نماذج Text2Speech عالية الأداء (Tacotron، Tacotron2، Glow-TTS، SpeedySpeech) بالإضافة إلى Bark.
- **Edge** - مجاني، يعمل عبر Azure. عند التشغيل مع تحديد "Plugin" كمزود، تحتاج أيضًا إلى تثبيت [هذا الملحق الخادم](https://github.com/SillyTavern/SillyTavern-EdgeTTS-Plugin). يتطلب الخيار الآخر Extras API (مهمل) للتشغيل.
- **Electron Hub** - يعيد استخدام مفتاح API الخاص بـ [Electron Hub](https://electronhub.ai/) للوصول إلى الأصوات السحابية (GPT-4o Mini TTS، Microsoft neural voices، إلخ.) مع عناصر التحكم لكل نموذج.
- **ElevenLabs** - يتطلب اشتراكًا مدفوعًا. احصل على مفتاح API من [ElevenLabs](https://elevenlabs.io/).
- **Google Translate** - صوت مجاني يوفره Google، صوت واحد لكل لغة، قد تختلف الجودة بشكل كبير.
- **Google Gemini TTS** - يتطلب مفتاح API من إما [Vertex AI](/Usage/API_Connections/google.md#google-vertex-ai) أو [AI Studio](/Usage/API_Connections/google.md#google-ai-studio)، يستخدم نماذج [Gemini TTS](https://cloud.google.com/text-to-speech/docs/gemini-tts).
- **Kokoro** - مجاني، يستخدم [kokoro.js](https://www.npmjs.com/package/kokoro-js) لتشغيل النموذج محليًا في المتصفح الخاص بك. ومع ذلك، قد لا تدعم [بعض المتصفحات](https://caniuse.com/webgpu) WebGPU لخيار الجهاز.
- **MiniMax** - يتطلب مفتاح API من [MiniMax](https://www.minimax.io/). راجع صفحة [MiniMax TTS](./MiniMaxTTS.md) للحصول على إرشادات الإعداد.
- **Novel** - يتطلب اشتراكًا مدفوعًا في NovelAI، يتم إنشاؤه بواسطة محرك TTS الخاص بـ NovelAI
- **OpenAI** - يتطلب مفتاح API مدفوع، يستخدم نماذج TTS الخاصة بـ OpenAI.
- **Pollinations** - وصول مجاني إلى نماذج OpenAI TTS، ولكن مع حد للمعدل. [الموقع](https://pollinations.ai/).
- **Silero** - مجاني، يعمل على جهاز الكمبيوتر الخاص بك، قد تختلف الجودة بشكل كبير. يتطلب تثبيت [خادم API مخصص](https://github.com/ouoertheo/silero-api-server) أو Extras API (مهمل).
- **System** - يستخدم محرك TTS الخاص بنظام التشغيل لديك، إذا كان موجودًا. قد تختلف الجودة بشكل كبير حسب نظام التشغيل.
- **XTTS** - مجاني، يتطلب تثبيت خادم API مخصص. راجع صفحة [XTTS](./XTTS.md) للحصول على إرشادات الإعداد.

### مربعات الاختيار

- **Enabled** - تشغيل/إيقاف تشغيل TTS
- **Auto Generation** - يتيح لـ TTS البدء في التشغيل تلقائيًا عند دخول رسالة جديدة إلى الدردشة
- **Only narrate "quotes"** - يحد من تشغيل TTS ليشمل فقط النص داخل `"علامات الاقتباس"`. سيشمل هذا `*"الاقتباسات" داخل سطور النجمة*` (اسم المتغير الداخلي = `narrate_quoted_only`)
- **Ignore \*text, even "quotes", inside asterisks\*** - لن يقوم TTS بتشغيل أي نص داخل `*النجوم*`، حتى "الاقتباسات" (اسم المتغير الداخلي = `narrate_dialogues_only`)
- *وجود كل من مربعي الاختيار "فقط اقرأ الاقتباسات" و "تجاهل النجوم" محددين سيؤدي إلى قراءة TTS فقط "الاقتباسات" التي ليست في النجوم، وتجاهل كل شيء آخر.*
- **Narrate only the translated text** - سيجعل هذا TTS يقرأ النص المترجم فقط.

بالنظر إلى النص المثال: `*Cohee approaches you with a faint "nya"* "Good evening, senpai", she says.`
إليك جدول يوضح كيف سيتم تعديل النص بناءً على حالات boolean لـ **Ignore \*text, even "quotes", inside asterisks\*** و **Only narrate "quotes"**:

| **Ignore \*text, even "quotes", inside asterisks\*** 	 | **Only narrate "quotes"**	 | **Output**                                                                |
|:-------------------------------------------------------|:---------------------------|:--------------------------------------------------------------------------|
| Disabled                                               | 	Disabled	                 | Cohee approaches you with a faint "nya" "Good evening, senpai", she says. |
| Disabled                                               | Enabled	                   | "nya"... "Good evening, senpai"                                           |
| Enabled	                                               | Disabled	                  | "Good evening, senpai", she says.                                         |
| Enabled	                                               | Enabled	                   | "Good evening, senpai"                                                    |

### أشرطة التمرير

ستتغير هذه اعتمادًا على API الذي تحدده.

### الأزرار

- **Apply** - يجب النقر فوق هذا بعد تعيين TTS API وبعد تحرير خريطة الصوت.
- **Refresh** - يعيد تحميل قائمة الأصوات من TTS API المحدد.
- **Available voices** - يحمل نافذة منبثقة بجميع الأصوات المتاحة لـ API المحدد، ويتيح لك معاينتها بحوارات عينة.

## استخدام TTS

1. انقر فوق خانة الاختيار "Enable"، وإلا فلن يحدث شيء أبدًا.
2. انقر فوق خانة الاختيار "Auto-generation" إذا كنت تريد أن يبدأ TTS تلقائيًا في كل مرة تصل فيها رسالة جديدة في الدردشة.
3. اختياريًا، انقر فوق رمز مكبر الصوت داخل الجزء العلوي الأيمن من أي رسالة للتشغيل عند الطلب.
4. انقر فوق زر "Stop" في الجزء السفلي الأيمن (الموجود داخل قائمة العصا السحرية) لإيقاف أي تشغيل.

### خريطة الصوت

يجب عليك توفير خريطة صوت لـ TTS لاستخدامها، وإلا فلن تعرف ما هي الأصوات التي يجب استخدامها لكل شخصية. لإعداد خريطة الصوت، افتح أولاً محادثة مع شخصية تريد تعيين صوت لها و/أو حدد شخصية مستخدم لتعيين صوت لها، ثم حدد صوتًا مدرجًا بواسطة مزود TTS من القائمة المنسدلة. إذا لم تشاهد قائمة بالأصوات و/أو الشخصيات، فتأكد من تكوين مزود TTS الخاص بك بشكل صحيح وانقر فوق "Refresh". تتطلب بعض المزودين (مثل OpenAI-compatible أو NovelAI) منك ملء قائمة الصوت يدويًا.
