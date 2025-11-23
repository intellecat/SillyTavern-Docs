---
route: /extensions/captioning/
templating: false
---

# Image Captioning

يتيح Image Captioning لـ SillyTavern إنشاء أوصاف نصية تلقائياً للصور المستخدمة في الدردشات.

استخدم Image Captioning عندما تريد أن تتمكن شخصية الذكاء الاصطناعي الخاصة بك من "رؤية" والاستجابة للمحتوى المرئي في محادثاتك.

- إنشاء تعليقات توضيحية للصور التي تحملها أو تلصقها في الرسائل
- إضافة سياق للصور الموجودة في سجل الدردشة
- استخدام مصادر متنوعة للتوليد، بما في ذلك النماذج المحلية وواجهات برمجة التطبيقات السحابية والشبكات الجماعية

هناك خيارات لا تتطلب إعداداً أو أموالاً أو GPU. وهناك أيضاً خيارات تتطلب بعضاً أو كل هذه الأشياء. اختر الخيار الذي يناسب احتياجاتك ومواردك.

إضافة image captioning مدمجة في SillyTavern ولا تحتاج إلى تثبيت منفصل.

## البداية السريعة

1. الإعداد:
    - افتح لوحة **Image Captioning** في لوحة **<i class="fa-solid fa-cubes"></i> Extensions**
    - اختر مصدر التعليق التوضيحي (على الأرجح "Local" أو "Multimodal")
    - بالنسبة لـ "Multimodal" تأكد من إعداد الاتصال في علامة التبويب **<i class="fa-solid fa-plug"></i> API Connections**
2. إنشاء تعليق توضيحي:
    - اختر "**Generate Caption**" من القائمة المنبثقة **<i class="fa-solid fa-magic-wand-sparkles"></i> Extensions**
    - حدد ملف صورة عندما يُطلب منك ذلك
    - انتظر إنشاء التعليق التوضيحي
3. المراجعة والإرسال:
    - سيتم إدراج الصورة المزودة بتعليق توضيحي في رسالتك
    - شاهد التعليق التوضيحي باستخدام تلميح أداة الصورة
    - انقر **<i class="fa-solid fa-paper-plane"></i> Send** لمعرفة ما تفكر فيه شخصيتك حول الصورة!

## عناصر تحكم اللوحة

### اختيار المصدر

اختر مصدر image captioning. الخيارات المدعومة:

| المصدر                           | الوصف                                                                                                                                                                                  |
|----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Multimodal](#multimodal-source) | **سحابي**: OpenAI، Anthropic، Google، MistralAI، وغيرهم. <br>**محلي**: Ollama، llama.cpp، KoboldCpp، Text Generation WebUI، وvLLM. <br>يدعم المطالبات المخصصة حتى تتمكن من طرح الأسئلة على صورك. |
| [Local](#local-source)           | يستخدم [transformers.js](https://huggingface.co/docs/transformers.js/en/index) يعمل محلياً داخل خادم SillyTavern الخاص بك. بدون إعداد!                                                   |
| Horde                            | يستخدم شبكة [AI Horde](https://aihorde.net/)، شبكة موزعة جماعية لنماذج توليد الصور. لا شيء للتنزيل أو التكوين أو الدفع. أوقات استجابة متغيرة.                                              |
| Extras                           | تم إيقاف مشروع Extras في أبريل 2024 ولا يتم صيانته أو دعمه.                                                                                                                             |

### تكوين التعليق التوضيحي
- **Caption Prompt**: أدخل مطالبة مخصصة للتعليق التوضيحي. المطالبة الافتراضية هي "What's in this image?"
- **Ask every time**: تبديل لطلب مطالبة مخصصة لكل تعليق توضيحي للصورة

### قالب الرسالة
- **Message Template**: تخصيص قالب رسالة التعليق التوضيحي. استخدم ماكرو `{{caption}}` لإدراج التعليق التوضيحي المُنشأ. القالب الافتراضي هو `[{{user}} sends {{char}} a picture that contains: {{caption}}]`

### التعليق التوضيحي التلقائي
- **Automatically caption images**: تبديل لتمكين التعليق التوضيحي التلقائي للصور الملصقة أو المرفقة بالرسائل
- **Edit captions before saving**: تبديل للسماح بتحرير التعليقات التوضيحية قبل حفظها

## وضع التعليقات التوضيحية على الصور

جميع طرق وضع التعليقات التوضيحية على الصور في SillyTavern:

* اختر "**Generate Caption**" من القائمة المنبثقة **<i class="fa-solid fa-magic-wand-sparkles"></i> Extensions** وحدد ملف صورة عندما يُطلب منك ذلك
* انقر على أيقونة <i class="fa-solid fa-envelope-open-text"></i> **Caption** في أعلى صورة موجودة بالفعل في رسالة
* الصق صورة مباشرة في إدخال الدردشة مع تمكين [التعليق التوضيحي التلقائي](#auto-captioning)
* أرفق ملف صورة برسالة باستخدام زر <i class="fa-solid fa-paperclip"></i> **Embed File or Image** في إجراءات الرسالة.
* أرسل رسالة بصورة مضمنة
* استخدم [أمر slash](#slash-command-caption) `/caption`

## التعليق التوضيحي التلقائي
تتيح لك ميزة التعليق التوضيحي التلقائي إنشاء تعليقات توضيحية تلقائياً للصور عند إضافتها إلى الدردشة، دون تشغيل عملية التعليق التوضيحي يدوياً في كل مرة.

للتمكين، حدد مربع الاختيار "Automatically caption images" في لوحة Image Captioning. يمكنك أيضاً اختيار تحرير التعليقات التوضيحية قبل حفظها عن طريق تحديد مربع "Edit captions before saving".

بمجرد التمكين، سيتم تشغيل التعليق التوضيحي التلقائي في السيناريوهات التالية:

- عند لصق صورة مباشرة في إدخال الدردشة.
- عند إرفاق ملف صورة برسالة.
- عند إرسال رسالة بصورة مضمنة.

سيستخدم النظام مصدر التعليق التوضيحي المحدد (Local أو Extras أو Horde أو Multimodal) والإعدادات المكونة لإنشاء تعليق توضيحي للصورة.

### تحرير التعليقات التوضيحية قبل الحفظ (وضع التحسين)

إذا قمت بتمكين خيار "Edit captions before saving":
1. بعد إضافة صورة، ستظهر نافذة منبثقة مع التعليق التوضيحي المُنشأ.
2. يمكنك مراجعة وتحرير التعليق التوضيحي حسب الحاجة.
3. انقر "OK" لتطبيق التعليق التوضيحي، أو "Cancel" لإلغاء التعليق التوضيحي دون حفظ.

### إرسال التعليق التوضيحي
سيتم إدراج التعليق التوضيحي المُنشأ (والمحرر اختيارياً) تلقائياً في المطالبة باستخدام قالب الرسالة الذي قمت بتكوينه. افتراضياً، سيتم إرساله بهذا التنسيق:

```
[BaronVonUser sends Seraphina a picture that contains: ...]
```


## أمر Slash: /caption
توفر الإضافة أمر slash `/caption` للاستخدام في صندوق الدردشة أو في البرامج النصية.

### الاستخدام

```
/caption [quiet=true|false]? [mesId=number]? [prompt]
```

- `prompt` (اختياري): مطالبة مخصصة لنموذج التعليق التوضيحي. مدعوم فقط من قبل مصادر multimodal.
- `quiet=true|false`: إذا تم تعيينه على true، يتم منع إرسال رسالة مع تعليق توضيحي إلى الدردشة. الافتراضي هو false.
- `mesId=number`: يحدد معرف رسالة لوضع تعليق توضيحي على صورة من رسالة موجودة بدلاً من تحميل صورة جديدة.

إذا لم يتم توفير `mesId`، سيطالبك الأمر بتحميل صورة. عندما يكون `quiet` false (افتراضي)، سيتم إرسال رسالة جديدة بالصورة المزودة بتعليق توضيحي إلى الدردشة. يمكن استخدام التعليق التوضيحي المُنشأ كمدخل لأوامر أخرى.

### الأمثلة
وضع تعليق توضيحي على صورة جديدة بالإعدادات الافتراضية:

```
/caption
```

وضع تعليق توضيحي على صورة جديدة بمطالبة مخصصة:

```
/caption Describe the main colours and shapes in this image
```

وضع تعليق توضيحي على صورة من الرسالة رقم 5 دون إرسال رسالة جديدة:

```
/caption mesId=5 quiet=true
```

وضع تعليق توضيحي على صورة من الرسالة رقم 10 بمطالبة مخصصة ثم [إنشاء صورة جديدة](/extensions/Stable-Diffusion.md) بناءً على التعليق التوضيحي:

```
/caption mesId=10 Describe this image using comma-separated keywords | /imagine
```

## المصدر المحلي

يمكنك تغيير النموذج في [config.yaml](/Administration/config-yaml.md#extensions-configuration). المفتاح يسمى `extensions.models.captioning`. أدخل معرف نموذج Hugging Face الذي تريد استخدامه. الافتراضي هو `Xenova/vit-gpt2-image-captioning`.

يمكنك استخدام أي نموذج يدعم image captioning (`VisionEncoderDecoderModel` أو خط أنابيب "image-to-text"). يجب أن يكون النموذج متوافقاً مع مكتبة transformers.js. أي أنه يحتاج إلى أوزان ONNX. ابحث عن النماذج ذات علامات `ONNX` و`image-to-text`، أو التي تحتوي على مجلد يسمى `onnx` مليء بملفات `.onnx`.

## مصدر Multimodal

### التكوين العام

- **Model**: اختر النموذج لـ image captioning. تختلف الخيارات بناءً على API المحددة.
- **Allow reverse proxy**: تبديل للسماح باستخدام بروكسي عكسي إذا كان معرفاً وصالحاً (OpenAI، Anthropic، Google، Mistral، xAI)

تتم إدارة مفاتيح API وعناوين URL للنقاط الطرفية لمصادر التعليق التوضيحي في لوحة [API Connections](/Usage/API_Connections/index.md). قم بإعداد الاتصال في API Connections أولاً، ثم حدده كمصدر للتعليقات التوضيحية في Captioning.

!!!warning قم بإعداده في لوحة API Connections أولاً
مرة أخيرة: قم بتكوين مفتاح API/العنوان/المنفذ في **<i class="fa-solid fa-plug"></i> API Connections** واستخدم الاتصال في Captioning.

يمكنك استخدام Claude للدردشات و Google AI Studio لـ image captioning، أو ما شابه. قم بإعدادهما *كليهما* في علامة التبويب 'API Connections' أولاً. ثم اقلب مصدر Chat Completion إلى Claude ومصدر Captioning إلى Google AI Studio.
!!!

بالنسبة لمعظم الخلفيات المحلية، ستحتاج إلى تعيين بعض الخيارات في الخلفية النموذجية بدلاً من SillyTavern. إذا كانت الخلفية الخاصة بك يمكنها تشغيل نموذج واحد فقط في كل مرة ولا تدعم التبديل التلقائي، فلديك عدة خيارات لاستخدام نماذج مختلفة للدردشة والتعليق التوضيحي:

1. **النقاط الطرفية الثانوية:** استخدم ميزة النقطة الطرفية الثانوية (راجع قسم [النقاط الطرفية الثانوية](#secondary-endpoints) أدناه) للاتصال بخادم API مختلف للتعليق التوضيحي
2. **أنواع اتصال متعددة:** اتصل بالخلفية الخاصة بك باستخدام أوضاع Text Completion و Chat Completion في API Connections - يمنحك هذا اتصالين منفصلين بنفس نوع الخلفية

### المصادر

لاستخدام أحد مصادر التعليق التوضيحي هذه، حدد Multimodal في القائمة المنسدلة Source.

* "أريد أفضل تعليق توضيحي ممكن، ولا مانع لدي من الدفع مقابله": Anthropic
* "لا أريد دفع أي شيء أو تشغيل أي شيء": الطبقة المجانية من Google AI Studio
* "أريد وضع تعليقات توضيحية على الصور محلياً وأن يعمل فقط": Ollama
* "أريد الحفاظ على حلم الذكاء الاصطناعي المحلي حياً": [KoboldCpp](#koboldcpp)
* "أريد الشكوى عندما لا يعمل": ~~Extras~~

| مزود API                          | الوصف                                                                                                                                                                       |
|-----------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AI/ML API                         | سحابي، مدفوع، نماذج GPT و Claude و Gemini متنوعة مع قدرات الرؤية                                                                                                          |
| Claude                            | سحابي، مدفوع، جميع نماذج Claude مع قدرات الرؤية                                                                                                                           |
| Cohere                            | سحابي، مدفوع، Aya Vision 8B / 32B                                                                                                                                          |
| Custom (OpenAI-compatible)        | لواجهات برمجة التطبيقات المتوافقة مع OpenAI المخصصة، يستخدم النموذج المكون حالياً في علامة تبويب API Connections                                                              |
| Electron Hub                      | سحابي، مدفوع، نماذج متنوعة مع قدرات الرؤية.                                                                                                                               |
| Google AI Studio                  | سحابي، طبقة مجانية ثم مدفوعة، Gemini Flash/Pro                                                                                                                            |
| Google Vertex AI                  | سحابي، طبقة مجانية، Gemini Flash/Pro                                                                                                                                      |
| Groq                              | سحابي، llama-4 scout/maverick                                                                                                                                             |
| KoboldCpp                         | محلي، يجب تكوين النموذج في KoboldCpp                                                                                                                                      |
| llama.cpp                         | محلي، يجب تكوين النموذج في llama.cpp                                                                                                                                      |
| MistralAI                         | سحابي، مدفوع، pixtral-large، pixtral-12B، magistral، mistral-large، إلخ.                                                                                                   |
| Moonshot AI                       | سحابي، مدفوع، moonshot-vision                                                                                                                                            |
| NanoGPT                           | سحابي، مدفوع، نماذج GPT/Claude/Google متنوعة مع قدرات الرؤية                                                                                                              |
| Ollama                            | محلي، يمكن التبديل بين النماذج المتاحة وتنزيل [نماذج رؤية إضافية](https://ollama.com/search?c=vision) داخل Captioning بعد التكوين في API Connections                          |
| OpenAI                            | سحابي، مدفوع، GPT-4 Vision، 4-turbo، 4o، 4o-mini                                                                                                                         |
| OpenRouter                        | سحابي، مدفوع (ربما خيارات مجانية)، نماذج كثيرة، اختر من المتاح داخل Captioning بعد التكوين في API connections                                                                |
| Pollinations                      | سحابي، مجاني                                                                                                                                                             |
| Text Generation WebUI (oobabooga) | محلي، يجب تكوين النموذج في ooba                                                                                                                                           |
| vLLM                              | محلي                                                                                                                                                                      |
| xAI (Grok)                        | سحابي، مدفوع، grok-vision                                                                                                                                                |

### النقاط الطرفية الثانوية

افتراضياً، يستخدم مصدر Multimodal النقطة الطرفية الأساسية المكونة في علامة تبويب API Connections.
يمكنك أيضاً إعداد نقطة طرفية ثانوية خصيصاً للتعليق التوضيحي متعدد الوسائط.

- افتح لوحة **Image Captioning** في لوحة **<i class="fa-solid fa-cubes"></i> Extensions**.
- حدد "Multimodal" كمصدر للتعليق التوضيحي ومزود API مفضل.
- أدخل عنوان URL صالحاً للنقطة الطرفية الثانوية في حقل "Secondary captioning endpoint URL".
- حدد مربع "Use secondary URL" لتمكين النقطة الطرفية الثانوية.

!!!tip
لا تُلحق `/v1` أو `/chat/completions` بنهاية عنوان URL. ستتعامل الإضافة مع ذلك تلقائياً.
!!!

هذا مدعوم فقط من قبل واجهات برمجة التطبيقات التالية:

- KoboldCpp
- llama.cpp
- Ollama
- Text Generation WebUI (oobabooga)
- vLLM

### أدلة خاصة بالمصدر

#### KoboldCpp

للحصول على معلومات عامة حول تثبيت واستخدام [KoboldCpp](https://github.com/LostRuins/koboldcpp)، راجع [وثائق KoboldCpp](https://github.com/LostRuins/koboldcpp/wiki).

لاستخدام KoboldCpp للتعليق التوضيحي متعدد الوسائط:

* احصل على نموذج قادر على التعامل مع multimodal، مدرب لمعالجة مطالبات النص والصورة في نفس الوقت.
* احصل أيضاً على إسقاطات multimodal للنموذج. تسمح هذه الأوزان للنموذج بفهم كيفية ارتباط أجزاء النص والصورة من الإدخال ببعضها البعض.
* قم بتحميل النموذج والإسقاطات في واجهة إطلاق KoboldCpp أو واجهة سطر الأوامر.

النموذج الأصلي والكلاسيكي المحلي متعدد الوسائط هو LLaVA. ملفات بتنسيق GGUF للنموذج والإسقاطات متاحة من [Mozilla/llava-v1.5-7b-llamafile](https://huggingface.co/Mozilla/llava-v1.5-7b-llamafile). لتحميلها من سطر الأوامر، حدد النموذج والإسقاطات باستخدام علامات `--model` و`--mmproj`. على سبيل المثال:

```shell
./koboldcpp \
--model="models/llava-v1.5-7b-Q4_K.gguf" \
--mmproj="models/ llava-v1.5-7b-mmproj-Q4_0.gguf" \
... other flags ...
```

بعض ضبطات LLaVA الدقيقة التي يمكنك تجربتها: [xtuner/llava-llama-3-8b-v1_1-gguf](https://huggingface.co/xtuner/llava-llama-3-8b-v1_1-gguf)، [xtuner/llava-phi-3-mini-gguf](https://huggingface.co/xtuner/llava-phi-3-mini-gguf).

يمكنك استخدام إسقاطات multimodal للنموذج الأساسي الذي تم بناء الضبط الدقيق الخاص بك منه. الإسقاطات لبعض النماذج الأساسية الشائعة متاحة من [koboldcpp/mmproj](https://huggingface.co/koboldcpp/mmproj/tree/main).
