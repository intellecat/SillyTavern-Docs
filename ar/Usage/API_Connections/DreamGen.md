---
route: /usage/api-connections/dreamgen/
---

# DreamGen

DreamGen هو تطبيق وواجهة API للعب الأدوار وكتابة القصص المدعومة بالذكاء الاصطناعي. لديهم طبقة مجانية، بالإضافة إلى اشتراك مدفوع يتيح وصولاً شهرياً غير محدود إلى نماذج توليد النصوص الداخلية عالية الجودة المصنوعة خصيصاً لغرض لعب الأدوار وكتابة القصص القابلة للتوجيه بالذكاء الاصطناعي. أنشئ حساباً للبدء: <https://dreamgen.com/>.

يتم إعادة تعيين الرصيد (المجاني) في بداية كل شهر تقويمي. راجع [التسعير](https://dreamgen.com/pricing) لمعرفة تكلفة الرصيد لكل نموذج و[الاستخدام](https://dreamgen.com/account/usage) لمعرفة رصيدك المتبقي.

## الاتصال بـ DreamGen

### الحصول على مفتاح API

انتقل إلى صفحة [مفاتيح DreamGen API](https://dreamgen.com/account/api-keys) وانقر على زر "New API Key". تأكد من نسخ مفتاح API إلى الحافظة الخاصة بك.

![إنشاء مفتاح DreamGen API جديد](/static/dreamgen/dreamgen_api_keys_new.jpg)
![نسخ مفتاح DreamGen API](/static/dreamgen/dreamgen_api_keys_copy.jpg)

### الاتصال

1. انتقل إلى إعدادات اتصال SillyTavern.
2. حدد API: Text Completion
3. حدد نوع API: DreamGen
4. أدخل مفتاح API
5. (اختياري) اختر نموذجاً

![الاتصال بـ DreamGen](/static/dreamgen/dreamgen_st_connection.png)

## النماذج

يقدم DreamGen API عدة نماذج بأحجام مختلفة.

- Lucid Max (في API يسمى `lucid-v1-max` أو `lucid-v1-extra-large`)
- Lucid Base (في API يسمى `lucid-v1-base` أو `lucid-v1-medium`) -- يتوافق مع الوزن المتاح [Lucid V1 Nemo](https://dreamgen.com/docs/models/lucid-v1/huggingface).

يستخدم Lucid Base رصيداً أقل بكثير وأسرع، بينما Lucid Max أكثر إبداعاً وقادر على التعامل مع تعليمات وروايات أكثر تعقيداً.

## الإعدادات

تستخدم نماذج DreamGen Lucid V1 امتداداً لقالب دردشة Llama 3 محسّن للعب الأدوار والكتابة. تعمل بشكل أفضل مع مطالبة نظام محددة.

نوصي بشدة بالبدء بأحد هذه الإعدادات المسبقة الرئيسية:

- تأكد من تمكين وضع التعليمات وحدد جميع مربعات الاختيار عند الاستيراد.
- [إعداد DreamGen Lucid V1 Role-Play المسبق](https://dreamgen.com/docs/models/lucid-v1/sillytavern/master-preset/role-play)
- [إعداد DreamGen Lucid V1 Story المسبق](https://dreamgen.com/docs/models/lucid-v1/sillytavern/master-preset/story)

تأتي هذه الإعدادات المسبقة مع دعم مدمج لـ `/sys` لإرسال التعليمات إلى النموذج. يمكنك استخدامها لتوجيه الحبكة أو التحكم في تصرفات الشخصية.

![تم تحديد الإعداد المسبق DreamGen](/static/dreamgen/dreamgen_st_preset.png)

موارد أخرى:

- [**دليل DreamGen + SillyTavern التفصيلي**](https://dreamgen.com/docs/models/lucid-v1/sillytavern)
- [توثيق تفصيلي لتنسيق مطالبة Lucid V1](https://dreamgen.com/docs/models/lucid-v1).
- [عرض توضيحي DreamGen + SillyTavern للعب الأدوار](https://imgur.com/a/dreamgen-lucid-sillytavern-roleplay-demo-bhzQpto)
- [عرض توضيحي DreamGen + SillyTavern لكتابة القصص](https://imgur.com/a/dreamgen-lucid-sillytavern-writing-demo-JLv5iO3)
- [نصائح لإنشاء سيناريوهاتك الخاصة](https://v2.dreamgen.com/docs/scenario-editor)

## الأسئلة الشائعة

### كيف يمكنني جعل الردود أطول أو أقصر؟

يمكنك تعيين `Last Assistant Prefix` في إعدادات التنسيق المسبقة الخاصة بك.

للرسائل الطويلة:

```txt
<|start_header_id|>user<|end_header_id|>

The next message is from {{char}} and is at least 100 words long<|eot_id|><|start_header_id|>writer character {{char}}<|end_header_id|>

```

للرسائل القصيرة:

```txt
<|start_header_id|>user<|end_header_id|>

The next message is from {{char}} and is at most 50 words long<|eot_id|><|start_header_id|>writer character {{char}}<|end_header_id|>

```

تأكد من الحفاظ على جميع الأسطر الجديدة، بما في ذلك الاثنان في النهاية.

![بادئة الرسالة الطويلة](/static/dreamgen/dreamgen_st_long_response_prefix.png)

يمكنك أيضاً تضمين وصف أسلوب الكتابة في بطاقتك أو مطالبة النظام، على سبيل المثال:

```txt
## Style

<your description>
```

راجع [توثيق "Style"](https://v2.dreamgen.com/docs/scenario-editor#style) لمعرفة المزيد ورؤية بعض الأمثلة.

### كيف يمكنني توجيه لعب الأدوار / القصة؟

استخدم خيار `/sys` لإرسال التعليمات إلى النموذج. بعض الأمثلة:

> The inkeeper offers Daria and the others a pint of ale.

> The next message is from Draco and should be at least 200 words, focusing on his inner conflict about the decision.

[شاهده في العمل.](https://imgur.com/a/dreamgen-lucid-sillytavern-roleplay-demo-bhzQpto)
