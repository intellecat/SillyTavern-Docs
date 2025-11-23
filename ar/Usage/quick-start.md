---
order: 190
icon: rocket
route: /usage/quick-start/
---

# البداية السريعة

!!!light
أنا في حيرة. فقط أعطني أسهل وأسرع طريقة يمكنني بها بدء استخدام SillyTavern. -- *مجهول*
!!!

يمكنك البدء مع SillyTavern في بضع دقائق فقط. إليك طريقتان سهلتان للبدء:

* يمكنك [استخدام AI Horde](#quick-start-with-ai-horde) مجانًا. AI Horde هي خدمة ذكاء اصطناعي تحركها المجتمع توفر الوصول إلى مجموعة متنوعة من نماذج الذكاء الاصطناعي.

* إذا كان لديك حساب OpenAI أو تريد التسجيل للحصول على واحد، يمكنك [استخدام OpenAI](#quick-start-with-openai).

## البداية السريعة مع AI Horde

1. اتبع [دليل التثبيت](/Installation/index.md) لتثبيت وبدء SillyTavern.

2. في شاشة الإعداد الأولية لـ SillyTavern، أدخل اسمًا لشخصيتك الأساسية. سيتم استخدام هذا الاسم في المحادثة.

   ![This is an optional caption](/static/quick-start/1_name.png)
3. انقر على زر API Connections في الشريط العلوي.

   ![This is an optional caption](/static/quick-start/2_api_conn.png)
4. أدخل مفتاح API لـ AI Horde. يمكنك استخدام `0000000000` الآن، أو الحصول على مفتاح مجاني من [AI Horde](https://aihorde.net/).

   ![This is an optional caption](/static/quick-start/3_horde_key.png)
5. حدد بعض نماذج الذكاء الاصطناعي لاستخدامها. فقط اختر بعضًا من الأعلى. يمكنك دائمًا تغييرها لاحقًا.

   ![This is an optional caption](/static/quick-start/4_horde_models.png)
6. أغلق نافذة API Connections. أدخل رسالة في مربع المحادثة في الأسفل واضغط Enter.

   ![This is an optional caption](/static/quick-start/5_msg.png)
7. سيستجيب الذكاء الاصطناعي في بضع لحظات. يمكنك مواصلة [المحادثة](/Usage/Chatting/index.md) معه. نجاح!

   ![This is an optional caption](/static/quick-start/6_success.png)

## البداية السريعة مع OpenAI

### تثبيت SillyTavern

اتبع [دليل التثبيت](/Installation/index.md) لتثبيت وبدء SillyTavern.

### الحصول على الوصول إلى OpenAI

1. قم بالتسجيل في OpenAI.
2. انتقل إلى <https://platform.openai.com>
3. انقر على أيقونة حسابك في الزاوية العلوية اليمنى، ثم View API Keys.
4. انقر على "Create new secret key". انسخه في مكان ما على الفور. **لا تشارك هذا المفتاح. من لديه يمكنه استخدام حسابك لاستخدام GPT على نفقتك.**

### تكوين SillyTavern لاستخدام API الخاص بك

1. في الشريط العلوي لـ SillyTavern، انقر على API Connections.
2. تحت API، حدد Chat Completion (OpenAI).
3. تحت Chat Completion Source، حدد OpenAI.
4. الصق مفتاح API الذي حفظته في الخطوة السابقة.
5. انقر على زر Connect. تأكد من أنه يقول Valid.
6. بشكل افتراضي، سيستخدم SillyTavern GPT-4 Turbo. يمكنك اختيار نموذج مختلف، ولكن ثقف نفسك بشأن التسعير.

### اختبار إعدادك

1. في الشريط العلوي لـ SillyTavern، انقر على Character Management في أقصى اليمين.
2. حدد شخصية موجودة مثل Seraphina.
3. في مربع النص في الأسفل، اكتب شيئًا لـ Seraphina، ثم اضغط Enter أو انقر على زر Send.

إذا فعلت كل شيء بشكل صحيح، بعد بضع ثوانٍ، يجب أن تستجيب Seraphina.
