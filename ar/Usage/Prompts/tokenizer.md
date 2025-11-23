---
order: 70
route: /usage/prompts/tokenizer/
---

# Tokenizer

الـ tokenizer هو أداة تقسم جزءاً من النص إلى وحدات أصغر تسمى tokens. يمكن أن تكون هذه tokens كلمات فردية أو حتى أجزاء من كلمات، مثل البادئات أو اللاحقات أو علامات الترقيم. القاعدة العامة هي أن token واحد يتوافق عموماً مع 3~4 أحرف من النص.

يوفر SillyTavern خيار "Best match" الذي يحاول مطابقة tokenizer باستخدام القواعد التالية اعتماداً على مزود API المستخدم.

Text Completion APIs **(قابل للتجاوز)**:

1. NovelAI Clio: NerdStash tokenizer.
2. NovelAI Kayra: NerdStash v2 tokenizer.
3. Text Completion: API tokenizer (إذا كان مدعوماً) أو Llama tokenizer.
4. KoboldAI Classic / AI Horde: Llama tokenizer.
5. KoboldCpp: model API tokenizer.

إذا حصلت على نتائج غير دقيقة أو ترغب في التجربة، يمكنك تعيين _override tokenizer_ لـ SillyTavern لاستخدامه أثناء تشكيل طلب إلى AI backend:

1. None. يُقدر كل token بـ ~3.3 أحرف، مقرب لأعلى إلى أقرب عدد صحيح. **جرب هذا إذا تم قطع prompts الخاصة بك عند أطوال context عالية.** يُستخدم هذا النهج بواسطة KoboldAI Lite.
2. Llama tokenizer. يُستخدم بواسطة عائلة نماذج Llama 1/2: Vicuna، Hermes، Airoboros، إلخ. **اختر إذا كنت تستخدم نموذج Llama 1/2.**
3. Llama 3 tokenizer. يُستخدم بواسطة نماذج Llama 3/3.1. **اختر إذا كنت تستخدم نموذج Llama 3/3.1.**
4. NerdStash tokenizer. يُستخدم بواسطة نموذج Clio من NovelAI. **اختر إذا كنت تستخدم نموذج Clio.**
5. NerdStash v2 tokenizer. يُستخدم بواسطة نموذج Kayra من NovelAI. **اختر إذا كنت تستخدم نموذج Kayra.**
6. Mistral V1 tokenizer. يُستخدم بواسطة عائلة نماذج Mistral الأقدم ومشتقاتها. **اختر إذا كنت تستخدم نموذج Mistral أقدم.**
7. Mistral Nemo tokenizer. يُستخدم بواسطة عائلة نماذج Mistral Nemo ومشتقاتها. **اختر إذا كنت تستخدم نموذج Mistral Nemo/Pixtral.**
8. Yi tokenizer. يُستخدم بواسطة نماذج Yi. **اختر إذا كنت تستخدم نموذج Yi.**
9. Gemma tokenizer. يُستخدم بواسطة نماذج Gemini/Gemma. **اختر إذا كنت تستخدم نموذج Gemma.**
10. DeepSeek tokenizer. يُستخدم بواسطة نماذج DeepSeek (مثل R1). **اختر إذا كنت تستخدم نموذج DeepSeek.**
11. API tokenizer. يستعلم API التوليد للحصول على عدد الرموز مباشرة من النموذج. backends المعروف دعمها: Text Generation WebUI (ooba)، koboldcpp، TabbyAPI، Aphrodite API. **اختر إذا كنت تستخدم backend مدعوم.**

Chat Completion APIs **(غير قابل للتجاوز)**:

1. OpenAI: tokenizer معتمد على النموذج عبر [tiktoken](https://github.com/openai/tiktoken).
2. Claude: tokenizer معتمد على النموذج عبر [WebTokenizers](https://github.com/mlc-ai/tokenizers-cpp).
3. OpenRouter: Llama، Mistral، Gemma، Yi tokenizers لنماذجهم المعنية.
4. Google AI Studio: Gemma tokenizer.
5. AI21 API: Jamba tokenizer (يتطلب تنزيل لمرة واحدة).
6. Cohere API: Command-R أو Command-A tokenizer (يتطلب تنزيل لمرة واحدة).
7. MistralAI API: Mistral V1 أو V3 tokenizer (يتطلب تنزيل لمرة واحدة).
8. DeepSeek API: DeepSeek tokenizer (يتطلب تنزيل لمرة واحدة).
9. Fallback tokenizer: GPT-3.5 turbo tokenizer.

#### Tokenizers إضافية

لا يتم تضمين هذه tokenizers في التثبيت الافتراضي بسبب حجمها. يلزم تنزيل لمرة واحدة عند استخدامها لأول مرة.

1. Qwen2 tokenizer.
2. Command-R / Command-A tokenizers. يُستخدم بواسطة مصدر Cohere في Chat Completion.
3. Mistral V3 (Nemo) tokenizer. يُستخدم بواسطة مصدر MistralAI في Chat Completion (نماذج Nemo و Pixtral).
4. DeepSeek (deepseek-chat) tokenizer. يُستخدم بواسطة مصدر DeepSeek في Chat Completion.

إذا كنت لا تريد استخدام تنزيلات الإنترنت، يوجد خيار إلغاء الاشتراك في config.yaml: `enableDownloadableTokenizers`. اضبط على `false` لتعطيل التنزيلات.

يمكنك أيضاً تنزيل tokenizers يدوياً من مستودع [SillyTavern-Tokenizers](https://github.com/SillyTavern/SillyTavern-Tokenizers). قم بتنزيل ملفات JSON ووضعها في دليل `_cache` الفرعي لجذر بياناتك، المسار هو `./data/_cache` بشكل افتراضي. أنشئ دليل `_cache` إذا لم يكن موجوداً. بعد ذلك، أعد تشغيل خادم SillyTavern لإعادة تهيئة tokenizers.

إذا لم يتم تخزين نموذج tokenizer المطلوب مؤقتاً وتم تعطيل التنزيلات، فسيتم استخدام tokenizer احتياطي (Llama 3) للعد.

### Token Padding

!!! ينطبق على: Text Completion APIs
سيستخدم SillyTavern دائماً tokenizer المطابق لنماذج Chat Completion، لذلك لا حاجة لـ token padding.
!!!

ما لم يستخدم SillyTavern tokenizer مقدم من backend API البعيد الذي يشغل النموذج، فإن جميع أعداد الرموز المفترضة أثناء توليد prompt يتم تقديرها بناءً على نوع [tokenizer](#tokenizer) المحدد.

نظراً لأن نتائج tokenization يمكن أن تكون غير دقيقة على أحجام context قريبة من الحد الأقصى المحدد للنموذج، فقد يتم قص أو إسقاط بعض أجزاء prompt، مما قد يؤثر سلباً على تماسك تعريفات الشخصية.

لمنع هذا، يخصص SillyTavern جزءاً من حجم context كـ padding لتجنب إضافة المزيد من عناصر المحادثة أكثر مما يمكن للنموذج استيعابه. إذا وجدت أن بعض أجزاء prompt يتم قصها حتى مع تحديد tokenizer الأكثر مطابقة، اضبط padding حتى لا يتم اقتطاع الوصف.

يمكنك إدخال قيم سلبية لـ reverse padding، مما يسمح بتخصيص أكثر من الحد الأقصى المحدد لعدد الرموز.
