---
route: /usage/api-connections/google/
---

# Google Gemini

Gemini هو LLM متعدد الوسائط المتطور من Google، والمتاح من خلال عدة واجهات APIs، بما في ذلك Google Vertex AI و Google AI Studio (المعروف سابقاً باسم MakerSuite). سيساعدك هذا الدليل على إعداد اتصالات Gemini API في SillyTavern.

## Google AI Studio

AI Studio هو أسرع وأسهل طريقة لتجربة أحدث نماذج Google AI دون الحاجة إلى إعداد مشروع Google Cloud Platform (GCP). يوفر مفتاح API بسيطاً يمكنك استخدامه للوصول إلى نماذج Gemini.

### الخطوة 1: إنشاء مفتاح Google AI Studio

1. انتقل إلى صفحة [Google AI Studio](https://aistudio.google.com/apikey) وقم بتسجيل الدخول باستخدام حساب Google الخاص بك.
2. انقر على "Get API Key"، اقبل الشروط والأحكام.
3. انقر على "Create API Key" لإنشاء مفتاح API الخاص بك.
4. انسخ مفتاح API إلى الحافظة الخاصة بك.

### الخطوة 2: وضع مفتاح API في SillyTavern

1. في SillyTavern، انتقل إلى صفحة "API Connections".
2. حدد "Chat Completion" كنوع API.
3. حدد "Google AI Studio" من القائمة المنسدلة.
4. أدخل مفتاح API الذي نسخته سابقاً في مربع النص "API Key".
5. انقر على زر "Connect" لحفظ المفتاح.

يجب أن تكون الآن قادراً على استخدام Google AI Studio API مع SillyTavern.

## Google Vertex AI

Vertex AI هي خدمة مقدمة من Google Cloud Platform (GCP). توفر الوصول إلى نماذج ذكاء اصطناعي مختلفة، بما في ذلك سلسلة Gemini.

هناك عدة طرق يمكن من خلالها إعداد Vertex AI API، وقد تختلف النماذج المتاحة اعتماداً على الطريقة المستخدمة.

### حساب الخدمة

يتطلب Google Cloud Platform (GCP) حساب خدمة للوصول إلى Vertex AI، مفاتيح API البسيطة لن تعمل. سيتم إنشاء رمز من ملف JSON لحساب الخدمة، والذي سيُستخدم بعد ذلك لمصادقة الطلبات إلى Vertex AI API.

يمكنك إنشاء حساب خدمة باتباع هذه الخطوات:

**المتطلبات الأساسية:**

1. يجب أن يكون لديك حساب Google Cloud Platform (GCP).
2. يجب أن يكون لديك مشروع تم إنشاؤه داخل حساب GCP الخاص بك.
3. يجب تمكين الفوترة لذلك المشروع.

#### الخطوة 1: تمكين Vertex AI API

قبل أن يعمل مفتاحك، يجب تمكين API لمشروعك.

1. انتقل إلى Google Cloud Console: <https://console.cloud.google.com/>
2. تأكد من تحديد المشروع الصحيح في الشريط العلوي.
3. انتقل إلى صفحة Vertex AI API: <https://console.cloud.google.com/apis/library/aiplatform.googleapis.com>
4. إذا لم يكن ممكناً بالفعل، انقر على زر "Enable".

#### الخطوة 2: إنشاء حساب الخدمة

هذه هي الهوية التي سيتم استخدامها للوصول إلى Vertex AI API.

1. في Google Cloud Console، انتقل إلى صفحة "Service Accounts". يمكنك البحث عنها في شريط البحث العلوي أو استخدام هذا الرابط المباشر: <https://console.cloud.google.com/iam-admin/serviceaccounts>
2. حدد مشروع GCP الخاص بك وانقر على "+ CREATE SERVICE ACCOUNT".
3. اسم حساب الخدمة: أعطه اسماً وصفياً، مثل `my-vertex-ai-client`.
4. انقر على "CREATE AND CONTINUE".
5. منح هذا الحساب حق الوصول إلى المشروع: في القائمة المنسدلة "Role"، ابحث عن وحدد Vertex AI User. هذا الدور يمنح الأذونات اللازمة لتشغيل النماذج دون منح الكثير من الوصول.
6. انقر على "CONTINUE"، ثم انقر على "DONE".

#### الخطوة 3: إنشاء مفتاح JSON

هذا هو ملف "كلمة المرور" الذي تحتاجه. يحتوي على معلومات حساسة، لذا لا تشاركه أو تحمّله في أي مكان عام.

1. يجب أن تكون الآن مرة أخرى في قائمة حسابات الخدمة. ابحث عن الحساب الذي أنشأته للتو (على سبيل المثال، sillytavern-vertex-ai).
2. انقر على القائمة ذات النقاط الثلاث (⋮) على الجانب الأيمن البعيد من ذلك الصف وحدد "Manage keys".
3. انقر على "ADD KEY" -> "Create new key".
4. تأكد من تعيين نوع المفتاح على JSON.
5. انقر على "CREATE".

سيتم تنزيل ملف .json على الفور إلى جهاز الكمبيوتر الخاص بك. احتفظ به آمناً، لأنه لا يمكن استرداد هذا المفتاح إذا فُقد.

#### الخطوة 4: وضع محتوى JSON في SillyTavern

يحتوي ملف JSON الذي قمت بتنزيله على جميع المعلومات اللازمة للمصادقة مع Vertex AI API. سيبدو شيئاً مثل هذا:

```json
{
    "type": "service_account",
    "project_id": "your-gcp-project-name",
    "private_key_id": "...",
    "private_key": "-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n",
    "client_email": "sillytavern-vertex-ai@your-gcp-project-name.iam.gserviceaccount.com",
    "client_id": "...",
    "auth_uri": "https://accounts.google.com/o/oauth2/auth",
    "token_uri": "https://oauth2.googleapis.com/token",
    "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
    "client_x509_cert_url": "..."
}
```

1. افتح ملف .json الذي قمت بتنزيله للتو باستخدام محرر نصوص بسيط (مثل Notepad على Windows، أو TextEdit على Mac، أو VS Code).
2. حدد كل النص في الملف (Ctrl+A أو Cmd+A).
3. انسخ النص إلى الحافظة الخاصة بك (Ctrl+C أو Cmd+C).
4. في SillyTavern، انتقل إلى صفحة "API Connections"، حدد "Chat Completion" كنوع API، ثم حدد "Google Vertex AI" من القائمة المنسدلة. قم بتبديل طريقة المصادقة إلى "Service Account".
5. الصق المحتوى المنسوخ بالكامل في مربع النص "Service Account JSON Content".
6. انقر على زر "Validate JSON" للتأكد من أنك نسخته بشكل صحيح.
7. أخيراً، قم بالتمرير لأسفل وانقر على "Connect" في أسفل صفحة إعدادات API.

يجب أن تكون الآن قادراً على استخدام Google Vertex AI API مع SillyTavern.

### وضع Express

وضع Express هو أسرع طريقة للبدء في استخدام Generative AI على Google Cloud. يسمح لك باستخدام Gemini API دون الحاجة إلى إعداد حساب خدمة. بدلاً من ذلك، يمكنك استخدام مفتاح API مباشرة.

راجع التوثيق الرسمي لمزيد من التفاصيل: [نظرة عامة على Vertex AI في وضع express](https://cloud.google.com/vertex-ai/generative-ai/docs/start/express-mode/overview).

#### الخطوة 1: تأكد من أن حسابك مؤهل لوضع Express

يجب أن يكون لديك حساب Google لم يُستخدم سابقاً لإنشاء مشروع Google Cloud.
إذا كان لديك مشروع Google Cloud موجود (بما في ذلك النسخ التجريبية المجانية)، يمكنك إنشاء واحد جديد لهذا الغرض.

#### الخطوة 2: تنشيط وضع Vertex AI Express

1. انتقل إلى صفحة الويب التالية: [Vertex AI Studio](https://cloud.google.com/generative-ai-studio).
2. انقر على "Try it free".
3. اقبل الشروط والأحكام وقم بتسجيل الدخول باستخدام حساب Google الخاص بك.
4. اختر بلدك وانقر على "Agree & start free". انتظر حتى يكتمل الإعداد.

#### الخطوة 3: إنشاء مفتاح API

1. تحقق من أن Google Cloud Console الخاص بك يعمل في وضع Express. يجب أن ترى لافتة في الزاوية العلوية اليسرى من الصفحة.
2. انقر على رابط "API Keys" في الشريط الجانبي الأيسر.
3. انقر على زر "Create API Key".
4. سيتم إنشاء مفتاح API جديد. انسخ هذا المفتاح إلى الحافظة الخاصة بك.

#### الخطوة 4: وضع مفتاح API في SillyTavern

1. في SillyTavern، انتقل إلى صفحة "API Connections".
2. حدد "Chat Completion" كنوع API.
3. حدد "Google Vertex AI" من القائمة المنسدلة.
4. قم بتبديل طريقة المصادقة إلى "Express Mode (API Key)".
5. الصق مفتاح API الذي نسخته سابقاً في مربع النص "API Key".
6. انقر على زر "Connect" لحفظ المفتاح.

يجب أن تكون الآن قادراً على استخدام Google Vertex AI API في وضع Express مع SillyTavern.
