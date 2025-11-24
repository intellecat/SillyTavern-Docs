---
order: -10
icon: code-review
route: /for-contributors/function-calling/
---

# Function Calling

Function Calling يسمح بإضافة وظائف ديناميكية إلى إضافاتك من خلال السماح لنموذج اللغة الكبير باستخدام بيانات منظمة يمكنك بعد ذلك استخدامها لتفعيل وظيفة معينة في الإضافة.

## أمثلة لحالات الاستخدام

1. الاستعلام عن واجهات برمجة التطبيقات الخارجية للحصول على معلومات إضافية (الأخبار، الطقس، البحث على الويب، إلخ).
2. إجراء الحسابات أو التحويلات بناءً على إدخال المستخدم.
3. تخزين واستدعاء الذكريات أو الحقائق المهمة، بما في ذلك RAG واستعلامات قاعدة البيانات.
4. إدخال عشوائية حقيقية في المحادثة (رمي النرد، قلب العملات، إلخ).

## الإضافات المدعومة رسمياً التي تستخدم function calling

1. [Image Generation](/extensions/Stable-Diffusion.md) (مدمج) - إنشاء الصور بناءً على مطالبات المستخدم.
2. [Web Search](/extensions/WebSearch.md) - تفعيل البحث على الويب عن استعلام.
3. [RSS](https://github.com/SillyTavern/Extension-RSS/) - جلب آخر الأخبار من موجزات RSS.
4. [AccuWeather](https://github.com/SillyTavern/Extension-AccuWeather) - جلب معلومات الطقس من AccuWeather.
5. [D&D Dice](https://github.com/SillyTavern/Extension-Dice) - رمي النرد لألعاب D&D.

## المتطلبات المسبقة والقيود

1. هذه الميزة متاحة فقط لمصادر Chat Completion معينة: OpenAI، Claude، MistralAI، Groq، Cohere، OpenRouter، AI21، Google AI Studio، Google Vertex AI، DeepSeek، AI/ML API ومصادر Custom API.
2. واجهات برمجة تطبيقات Text Completion لا تدعم استدعاءات الوظائف، ولكن بعض الخوادم المستضافة محلياً مثل Ollama و TabbyAPI قد تعمل في وضع Custom OpenAI-compatible تحت Chat Completion.
3. يجب السماح صراحة بدعم استدعاء الوظائف من قبل المستخدم أولاً. يتم ذلك عن طريق تمكين خيار "Enable function calling" في لوحة AI Response Configuration.
4. لا يوجد ضمان بأن نموذج اللغة الكبير سيقوم بأي استدعاءات وظائف على الإطلاق. معظمها يتطلب "تفعيلاً" صريحاً من خلال المطالبة (على سبيل المثال، طلب المستخدم "رمي النرد"، "احصل على الطقس"، إلخ).
5. ليست كل المطالبات يمكنها تفعيل استدعاء أداة. الاستكمالات، التقمص، المطالبات الخلفية ('الهادئة') غير مسموح لها بتفعيل استدعاء أداة. لا يزال بإمكانها استخدام استدعاءات الأدوات الناجحة السابقة في ردودها.

## كيفية إنشاء أداة وظيفة

### التحقق من دعم الميزة

لتحديد ما إذا كانت ميزة استدعاء الأدوات الوظيفية مدعومة، يمكنك استدعاء `isToolCallingSupported` من كائن `SillyTavern.getContext()`. سيتحقق هذا مما إذا كانت واجهة برمجة التطبيقات الحالية تدعم استدعاء الأدوات الوظيفية وما إذا كانت ممكّنة في الإعدادات. إليك مثال على كيفية التحقق من دعم الميزة:

```ts
if (SillyTavern.getContext().isToolCallingSupported()) {
    console.log("Function tool calling is supported");
} else {
    console.log("Function tool calling is not supported");
}
```

### تسجيل وظيفة

لتسجيل أداة وظيفة، تحتاج إلى استدعاء دالة `registerFunctionTool` من كائن `SillyTavern.getContext()` وتمرير المعاملات المطلوبة. إليك مثال على كيفية تسجيل أداة وظيفة:

```ts
SillyTavern.getContext().registerFunctionTool({
    // الاسم الداخلي للأداة الوظيفية. يجب أن يكون فريداً.
    name: "myFunction",
    // اسم العرض للأداة الوظيفية. سيتم عرضه في واجهة المستخدم. (اختياري)
    displayName: "My Function",
    // وصف للأداة الوظيفية. يجب أن يصف ما تفعله الوظيفة ومتى يتم استخدامها.
    description: "My function description. Use when you need to do something.",
    // مخطط JSON لمعاملات الأداة الوظيفية. انظر: https://json-schema.org/
    parameters: {
        $schema: 'http://json-schema.org/draft-04/schema#',
        type: 'object',
        properties: {
            param1: {
                type: 'string',
                description: 'Parameter 1 description',
            },
            param2: {
                type: 'string',
                description: 'Parameter 2 description',
            },
        },
        required: [
            'param1', 'param2',
        ],
    },
    // الوظيفة التي يتم استدعاؤها عند تفعيل الأداة. يمكن أن تكون غير متزامنة.
    // إذا لم تكن النتيجة نصاً، سيتم تحويلها إلى JSON.
    action: async ({ param1, param2 }) => {
        // كود وظيفتك هنا
        console.log(`Function called with parameters: ${param1}, ${param2}`);
        return "Function result";
    },
    // وظيفة اختيارية لتنسيق رسالة الإشعار المعروضة عند استدعاء الوظيفة.
    // إذا تم إرجاع نص فارغ، لن يتم عرض رسالة الإشعار.
    formatMessage: ({ param1, param2 }) => {
        return `Function is called with: ${param1} and ${param2}`;
    },
    // وظيفة اختيارية ترجع قيمة منطقية تشير إلى ما إذا كان يجب تسجيل الأداة للمطالبة الحالية.
    // إذا لم يتم توفير وظيفة shouldRegister، سيتم تسجيل الأداة لكل مطالبة.
    shouldRegister: () => {
        return true;
    },
    // علم اختياري. إذا تم تعيينه على true، سيتم تنفيذ استدعاء الوظيفة، ولكن لن يتم تسجيل النتيجة في سجل الدردشة المرئي.
    stealth: false,
});
```

### إلغاء تسجيل وظيفة

لإلغاء تنشيط أداة وظيفة، تحتاج إلى استدعاء دالة `unregisterFunctionTool` من كائن `SillyTavern.getContext()` وتمرير اسم الأداة الوظيفية لتعطيلها. إليك مثال على كيفية إلغاء تسجيل أداة وظيفة:

```ts
SillyTavern.getContext().unregisterFunctionTool("myFunction");
```

## نصائح وحيل

1. استدعاءات الأدوات الناجحة يتم حفظها كجزء من السجل المرئي وسيتم عرضها في واجهة الدردشة، لذا يمكنك فحص المعاملات والنتائج الفعلية. إذا لم يكن ذلك مرغوباً، قم بتعيين علم `stealth: true` عند تسجيل أداة وظيفة.
2. إذا كنت لا تريد رؤية استدعاء الأداة في سجل الدردشة. إذا كنت تريد تنسيقها أو إخفائها باستخدام CSS مخصص، استهدف فئة `toolCall` على عناصر `.mes`، أي `.mes.toolCall { display: none; }` أو `.mes.toolCall { color: #999; }`.
