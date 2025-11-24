---
route: /extensions/translation/
templating: false
---

# ترجمة الدردشة

## نظرة عامة

تتيح إضافة ترجمة الدردشة الترجمة الفورية لرسائل الدردشة بين
اللغات المختلفة باستخدام مزودي ترجمة متنوعين. وهي تدعم أوضاع الترجمة اليدوية
والتلقائية.

![Character message translated from English to Chinese using 'Translate Message/翻譯訊息' message action button](../static/extensions/translation/sensei.png)

+++ English
!["Translate Chat", "Translate Input"](../static/extensions/translation/wand-menu-en.png)
+++ 简体中文
!["翻译聊天", "翻译输入"](../static/extensions/translation/wand-menu-zh-cn.png)
+++ 繁體中文
!["翻譯聊天內容", "翻譯輸入內容"](../static/extensions/translation/wand-menu-zh-tw.png)
+++ 한국어
!["채팅 번역하기", "입력 번역하기"](../static/extensions/translation/wand-menu-ko.png)
+++ Русский
!["Перевести чат", "Перевести моё сообщение"](../static/extensions/translation/wand-menu-ru.png)
+++

## الاستخدام

جميع الطرق لترجمة رسائل الدردشة:

**<i class="fa-solid fa-language"></i> زر Translate Chat** في قائمة **<i class="fa-solid fa-magic-wand-sparkles"></i>
Extensions**

- يترجم سجل الدردشة بالكامل دفعة واحدة

**<i class="fa-solid fa-keyboard"></i> زر Translate Input** في قائمة **<i class="fa-solid fa-magic-wand-sparkles"></i>
Extensions**

- يترجم نص الإدخال الحالي فقط
- مفيد قبل إرسال رسالة

**<i class="fa-solid fa-language"></i> أيقونة Translate Message** في شريط أدوات **<i class="fa-solid fa-ellipsis"></i> Message
Actions**
لأي رسالة

- انقر لترجمة تلك الرسالة فقط
- انقر مرة أخرى للعودة إلى النص الأصلي

تكوين **Auto-mode** في درج **Chat Translation** بلوحة **<i class="fa-solid fa-cubes"></i>
Extensions**

- يترجم تلقائيًا مدخلات المستخدم أو ردود AI أو كليهما

أمر **/translate** المائل

- استخدم `/translate [target=language_code] text` لترجمة النص

## التكوين

خيارات التكوين متاحة في درج **Chat Translation** بلوحة **<i class="fa-solid fa-cubes"></i>
Extensions**.

#### المزود

- اختر [خدمة الترجمة](#translation-providers) المفضلة لديك
- انقر على أيقونة **<i class="fa-solid fa-key"></i> API Key**، إذا ظهرت، لإدخال مفتاح API
- انقر على أيقونة **<i class="fa-solid fa-link"></i> Custom URL**، إذا ظهرت، لإدخال عنوان URL مخصص لـ API

#### اللغة المستهدفة

اختر اللغة التي تريد كتابة رسائلك بها، أو قراءة ردود AI بها.

#### Auto-mode

تكوين سلوك الترجمة التلقائية.

- **None**: لا توجد ترجمة تلقائية
- **Translate responses**: يترجم تلقائيًا ردود AI إلى اللغة المستهدفة
- **Translate inputs**: يترجم تلقائيًا مدخلات المستخدم إلى الإنجليزية
- **Translate both**: يترجم كلاً من مدخلات المستخدم وردود AI

#### مسح الترجمات

زر **<i class="fa-solid fa-trash-can"></i> Clear Translations** يزيل جميع الترجمات من الرسائل في
الدردشة الحالية. يتم الاحتفاظ بالرسائل الأصلية.

### مثال على التكوين: الدردشة من الصينية إلى الإنجليزية

لإعداد سير عمل حيث يمكن لمستخدم يتحدث الصينية الدردشة بالصينية مع AI يعمل بالإنجليزية:

1. اضبط Auto-mode على "Translate both"
2. اضبط اللغة المستهدفة على "Chinese (Simplified)" أو "Chinese (Traditional)"
3. اختر مزود ترجمة مع كشف تلقائي جيد للغة (مثل Google أو DeepL)

سيؤدي هذا الإعداد إلى:

- ترجمة إدخال المستخدم الصيني إلى الإنجليزية لـ AI
- ترجمة ردود AI الإنجليزية مرة أخرى إلى الصينية للمستخدم

يعتمد هذا الإعداد على الكشف التلقائي للغة للإدخال. لمزيد من التحكم الدقيق، قد تتضمن التحديثات المستقبلية
اختيار اللغة المصدر بشكل صريح.

## مزودو الترجمة

**:icon-cloud:** قائم على السحابة
**<i class="fa-solid fa-link"></i>** محلي، عنوان URL مخصص
**<i class="fa-solid fa-key"></i>** يتطلب مفتاح API

| المزود                                                            | الموقع                                                                      | المميزات                                                                                               |
|---------------------------------------------------------------------|-------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| [Libre Translate](https://libretranslate.com/)                      | :icon-cloud: <i class="fa-solid fa-key"></i> <i class="fa-solid fa-link"></i> | بديل مستضاف ذاتيًا (AGPL-3.0) لخدمات الترجمة الاحتكارية، مع مستوى Pro مستضاف سحابيًا     |
| [Google Translate](https://cloud.google.com/translate)              | :icon-cloud:                                                                  | مستخدم على نطاق واسع، يدعم العديد من اللغات، دقة جيدة                                                    |
| [Lingva Translate](https://lingva.ml/)                              | <i class="fa-solid fa-link"></i>                                              | واجهة أمامية بديلة لـ Google Translate، مفتوح المصدر (AGPL-3.0)، يركز على الخصوصية                    |
| [DeepL](https://www.deepl.com/)                                     | :icon-cloud: <i class="fa-solid fa-key"></i>                                  | ترجمات عالية الجودة، خاصة للغات الأوروبية                                           |
| [DeepLX](https://github.com/OwO-Network/DeepLX)                     | <i class="fa-solid fa-link"></i>                                              | وكيل DeepL مستضاف ذاتيًا، مفتوح المصدر (MIT)، مجاني لكن وكالة DeepL Pro تتطلب مفتاح API من DeepL         |
| [Bing Translator](https://www.bing.com/translator)                  | :icon-cloud:                                                                  | خدمة ترجمة Microsoft، تتكامل مع خدمات Azure                                        |
| [OneRing Translator](https://github.com/janvarev/OneRingTranslator) | <i class="fa-solid fa-link"></i>                                              | واجهة أمامية مستضافة ذاتيًا لـ Google Translate ومزودين آخرين، يركز على الخصوصية، مفتوح المصدر (AGPL-3.0) |
| [Yandex Translate](https://translate.yandex.com/)                   | :icon-cloud:                                                                  | جيد للروسية ولغات أوروبا الشرقية                                                        |

### تكوين خاص بـ DeepL

- مستويات الرسمية متاحة للألمانية والفرنسية والإيطالية والإسبانية والهولندية واليابانية والروسية
- تكوين عبر `deepl.formality` في [config.yaml](/Administration/config-yaml.md#deepl-configuration)

## الأوامر المائلة

استخدم أمر `/translate` للترجمات السريعة. الصيغة: `/translate [target=language_code] text`. إذا لم يتم توفير اللغة المستهدفة، فسيتم استخدام القيمة من إعدادات الإضافة.

### الاستخدام الأساسي

ترجمة النص إلى اللغة المستهدفة الحالية وإظهاره في نافذة منبثقة:

```
/translate Welcome to the Tavern | /echo
```

![Popup in Chinese (Simplified), '欢迎来到酒馆/Welcome to the Tavern'](../static/extensions/translation/welcome-tavern.png)

ترجمة النص إلى الإسبانية وإضافته إلى الدردشة:

```
/translate target=es Hello world | /send
```

![User message in Spanish, 'Hola Mundo/Hello world'](/static/extensions/translation/hola-mundo.png)

### الاختبار، ترجمة خط الأنابيب، الترجمة المحلية

مطالبة المستخدم بإدخال رسالة ولغة، ترجمة الرسالة إلى تلك اللغة، ثم إعادة ترجمتها إلى
اللغة المستهدفة المكونة وإظهار كلتا الترجمتين في نافذة منبثقة. يستخدم هذا المثال أوامر `/input` و `/buttons`
لجمع إدخال المستخدم:

```shell
/input default="Hello, world!" <span data-i18n="Test Message">Sample text</span> |
/let key=input ||
/buttons labels=["zh-CN", "zh-TW", "es", "hu", "en"] <span data-i18n="UI Language">Language</span> |
/let key=lang ||
/translate target={{var::lang}} {{var::input}} | /let key=tx_target |
/translate | /let key=tx_orig ||
/echo escapeHtml=false cssClass=wider_dialogue_popup
<b data-i18n="Test Message">Test message</b>: {{var::input}} <br/>
<b data-i18n="Output">Output</b> ({{var::lang}}): {{var::tx_target}} <br/>
<b data-i18n="Output">Output</b> (<span data-i18n="ext_translate_target_lang">target language</span>): {{var::tx_orig}} <br/>
```

هذا مفيد للتحقق من جودة الترجمة إلى لغة لا تتحدثها، قبل كتابتها
في مكان مهم.

![Popup, 'Welcome to the Tavern/欢迎来到酒馆/welcome to the pub', en, zh-CN, en](../static/extensions/translation/welcome-tavern-en-cn.png)
![Popup, 'My hovercraft is full of eels/我的氣墊船裡裝滿了鰻魚/My hovercraft is filled with eels', en, zh-TW, en](../static/extensions/translation/eels-out-zh-tw.png)

يتم عرض عناصر التحكم بـ UI في اللغة المحلية الحالية، بغض النظر عن اللغة المستهدفة المكونة.

| `/input`                                                                                        | `/buttons`                                                                          |
|-------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|
| ![Input dialog, '发送测试消息/Send Test Message'](../static/extensions/translation/eels-input-zh.png) | ![Buttons dialog, '语言/Language'](../static/extensions/translation/eels-lang-zh.png) |

![Popup, '我的氣墊船裡裝滿了鰻魚/My hovercraft is full of eels', zh-TW -> en -> zh-TW](../static/extensions/translation/eels-out-tw-en.png)

الكشف التلقائي للغة الإدخال فعال نسبيًا في الأمثلة التالية:

![Popup, '(My hovercraft is full of eels)/A légpárnás hajóm tele van angolnával/我的氣墊船裡裝滿了鰻魚', zh-TW -> hu -> zh-TW](../static/extensions/translation/eels-out-tw-hu.png)
![Popup, '我的氣墊船裡裝滿了鰻魚/Mi aerodeslizador está lleno de anguilas/My hovercraft is full of eels', zh-TW -> es -> en](../static/extensions/translation/eels-out-tw-es-en.png)
![Popup, 'Il mio hovercraft è pieno di anguille/我的气垫船里装满了鳗鱼/My hovercraft is filled with eels', it -> zh-CN -> en](../static/extensions/translation/eels-out-it-zhCN-en.png)

## ملاحظات تقنية

- ترميز UTF-8 والأحرف الخاصة والرموز التعبيرية مدعومة
- يتعامل مع الرسائل الكبيرة عن طريق تقسيمها إلى أجزاء عند الحاجة
- يحتفظ بالتنسيق والصور المضمنة في الرسائل
- يخزن الترجمات مؤقتًا لتجنب استدعاءات API الزائدة

### لغة إدخال AI

`internal_language` يتحكم في اللغة التي يتم ترجمة رسائل المستخدم إليها تلقائيًا قبل إرسالها إلى AI. إنها
مشفرة إلى 'en' في الإعدادات الافتراضية ولا يمكن تغييرها من خلال UI. وبالتالي، فإن اللغة المستهدفة للترجمة
للرسائل *إلى AI* هي دائمًا الإنجليزية. أظهر الاختبار السابق أن أداء AI كان أفضل عند تلقي
رسائل إنجليزية، ولكن قد يتغير هذا مع تدريب المزيد من نماذج LLM على بيانات لغوية أكثر تنوعًا. أفترض أنه يمكن
تغيير `internal_language` في `settings.json` ومعرفة النتيجة.

### معالجة المتغيرات الصينية

تدعم الإضافة الصينية المبسطة والتقليدية، لكن ليس جميع مزودي الترجمة يفعلون ذلك. يعرض UI
هذه كـ 'Chinese (Simplified)' و 'Chinese (Traditional)' على التوالي، مع رموز اللغة 'zh-CN' و 'zh-TW'. يتم
تعيينها إلى رموز اللغة التالية لمزودي الترجمة:

* Libre Translate: 'zh-CN' إلى 'zh' و 'zh-TW' إلى 'zt'.
* DeepL و DeepLX: كلا المتغيرين إلى 'ZH'.
* Bing: 'zh-CN' إلى 'zh-Hans'، 'zh-TW' كما هو.
* المزودون الآخرون يستخدمون 'zh-CN' و 'zh-TW' كما هو مقدم.

### حدود طول النص

بعض المزودين لديهم حدود للأحرف لكل طلب:

- Yandex: 5000 حرف
- DeepLX: 1500 حرف
- Bing: 1000 حرف
- Google: 5000 حرف

النصوص الأطول يتم تقسيمها تلقائيًا إلى أجزاء للترجمة.
