---
order: 100
route: /usage/core-concepts/advancedformatting/
---

# Advanced Formatting

توفر الإعدادات المتوفرة في هذا القسم مزيداً من التحكم في استراتيجية [بناء prompt](index.md)، بشكل أساسي لـ Text Completion APIs.

معظم الإعدادات في هذه اللوحة لا تنطبق على Chat Completions APIs حيث يتم التحكم فيها بواسطة نظام prompt manager بدلاً من ذلك.

+++ Text Completion APIs
* [System Prompt](#system-prompt)
* [Context Template](#context-template)
* [Tokenizer](#tokenizer)
* [Custom Stopping Strings](#custom-stopping-strings)
+++ Chat Completion APIs
* System Prompt: غير قابل للتطبيق، استخدم [Prompt Manager](prompt-manager.md)
* Context Template: غير قابل للتطبيق، استخدم [Prompt Manager](prompt-manager.md)
* [Tokenizer](#tokenizer)
* [Custom Stopping Strings](#custom-stopping-strings)
+++

## إعادة تعيين Templates

يمكنك استعادة templates الافتراضية إلى حالتها الأصلية. يمكن القيام بذلك إما من خلال واجهة المستخدم أو عن طريق حذف ملفات البيانات ذات الصلة يدوياً.

### إعادة التعيين عبر واجهة المستخدم

1. افتح قائمة **<i class="fa-solid fa-font"></i> Advanced Formatting**.
2. اختر template الذي تريد إعادة تعيينه.
3. انقر على زر **<i class="fa-solid fa-recycle"></i> Restore current template**.
4. أكد الإجراء عند المطالبة.

### إعادة التعيين اليدوية

!!!
تأكد من تعيين إعداد `skipContentCheck` إلى `false` في [config.yaml](/Administration/config-yaml.md#data-configuration)، وإلا فلن يتم تشغيل فحص المحتوى.
!!!

1. انتقل إلى دليل بيانات المستخدم الخاص بك (راجع [Data paths](/Installation/index.md#data-paths) للحصول على التفاصيل).
2. احذف ملف `content.log` من جذر دليل بيانات المستخدم الخاص بك. يتتبع هذا الملف الملفات الافتراضية المنسوخة للمستخدم الخاص بك.
3. احذف ملفات template JSON من الأدلة الفرعية ذات الصلة (`context`، `instruct`، `sysprompt`، إلخ).
4. أعد تشغيل خادم SillyTavern. سيقوم التطبيق بإعادة ملء المحتوى الافتراضي، واستعادة أي templates افتراضية محذوفة.

## Templates المحددة بواسطة Backend

!!! ينطبق على: Text Completion APIs
غير قابل للتطبيق على Chat Completion APIs حيث تستخدم منشئ prompt مختلف.
!!!

توفر بعض مصادر Text Completion القدرة على اختيار templates الموصى بها تلقائياً من قبل مؤلف النموذج. يعمل هذا من خلال مقارنة hash لـ chat template المحدد في ملف `tokenizer_config.json` للنموذج مع أحد templates SillyTavern الافتراضية.

1. يجب تمكين خيار **<i class="fa-solid fa-bolt"></i> Derive templates** في قائمة **<i class="fa-solid fa-font"></i> Advanced Formatting**. يمكن تطبيق هذا على Context أو Instruct أو كليهما.
2. يجب اختيار backend مدعوم كمصدر Text Completion. حالياً فقط llama.cpp و KoboldCpp يدعمان اشتقاق templates.
3. يجب أن يبلغ النموذج بشكل صحيح عن بيانات التعريف الخاصة به عند إنشاء الاتصال بـ API. إذا لم ينجح هذا، حاول تحديث backend إلى أحدث إصدار.
4. يجب أن يتطابق hash chat template المبلغ عنه مع أحد [templates SillyTavern المعروفة](https://github.com/SillyTavern/SillyTavern/blob/release/public/scripts/chat-templates.js). يغطي هذا فقط templates الافتراضية، مثل Llama 3 و Gemma 2 و Mistral V7، إلخ.
5. إذا تطابق hash، سيتم تحديد template تلقائياً إذا كان موجوداً في قائمة templates (أي لم يتم إعادة تسميته أو حذفه).

## System Prompt

!!! ينطبق على: Text Completion APIs
للإعدادات المكافئة في Chat Completion APIs، استخدم [Prompt Manager](prompt-manager.md). الـ **Main Prompt** هو ما يعادل System Prompt في Chat Completion APIs.
!!!

يحدد System Prompt التعليمات العامة للنموذج لاتباعها. فهو يحدد النغمة والسياق للمحادثة. على سبيل المثال، يخبر النموذج بأن يتصرف كمساعد ذكاء اصطناعي، أو شريك في الكتابة، أو شخصية خيالية.

الـ System Prompt هو جزء من [Story String](context-template.md#story-string) وعادة ما يكون الجزء الأول من prompt الذي يتلقاه النموذج.

راجع [دليل prompting](index.md#main-prompt-system-prompt) لمعرفة المزيد عن System Prompt.

## Context Template

!!! ينطبق على: Text Completion APIs
للإعدادات المكافئة في Chat Completion APIs، استخدم [Prompt Manager](prompt-manager.md).
!!!

عادةً، تتطلب نماذج الذكاء الاصطناعي منك توفير بيانات الشخصية لها بطريقة محددة. يتضمن SillyTavern قائمة من قواعد التحويل المُعدة مسبقاً لنماذج مختلفة، ولكن يمكنك تخصيصها بالطريقة التي تريدها.

يتم شرح خيارات هذا القسم في [Context Template](context-template.md).

## Tokenizer

الـ tokenizer هو أداة تقسم جزءاً من النص إلى وحدات أصغر تسمى tokens. يمكن أن تكون هذه tokens كلمات فردية أو حتى أجزاء من كلمات، مثل البادئات أو اللاحقات أو علامات الترقيم. القاعدة العامة هي أن token واحد يتوافق عموماً مع 3~4 أحرف من النص.

يتم شرح خيارات هذا القسم في [Tokenizer](tokenizer.md).

## Custom Stopping Strings

يقبل مصفوفة متسلسلة بـ JSON من stopping strings. مثال: `["\n", "\nUser:", "\nChar:"]`. إذا لم تكن متأكداً من التنسيق، استخدم [online JSON validator](https://jsonlint.com/). إذا **انتهى** إخراج النموذج بأي من stop strings، فسيتم إزالتها من الإخراج.

الـ APIs المدعومة:

1. KoboldAI Classic (الإصدارات 1.2.2 وأعلى) أو KoboldCpp
2. AI Horde
3. Text Completion APIs: Text Generation WebUI (ooba)، Tabby، Aphrodite، Mancer، TogetherAI، Ollama، إلخ.
4. NovelAI
5. OpenAI (بحد أقصى 4 strings) و APIs المتوافقة
6. OpenRouter (كل من Text و Chat Completion)
7. Claude
8. Google AI Studio
9. MistralAI

## Start Reply With

!!! ملاحظة
بشكل افتراضي، لن يتم عرض بادئة Start Reply With في الرسالة الناتجة. فعّل "Show reply prefix in chat" لعرضها.
!!!

### Text Completion APIs

يملأ مسبقاً السطر الأخير من prompt، مما يجبر النموذج على الاستمرار من تلك النقطة. هذا مفيد لفرض محتوى، مثل الدفع نحو [Model Reasoning](/Usage/Prompts/reasoning.md) مع البادئة المحددة:

```txt
<think>
Sure!
```

### Chat Completion APIs

يضيف رسالة بدور assistant إلى نهاية prompt. بالنسبة لبعض نماذج backend، هذا يعادل الملء المسبق لاستجابة النموذج، لكن البعض قد لا يدعم ذلك على الإطلاق وسيفشل بخطأ تحقق. إذا لم تكن متأكداً، اترك هذا الحقل فارغاً.
