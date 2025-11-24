---
order: -30
icon: server
route: /for-contributors/server-plugins/
---

# Server Plugins

تسمح هذه الإضافات بإضافة وظائف يستحيل تحقيقها باستخدام إضافات واجهة المستخدم وحدها، مثل إنشاء نقاط نهاية API جديدة أو استخدام حزم Node.JS غير المتاحة في بيئة المتصفح.

الإضافات موجودة في دليل `plugins` الخاص بـ SillyTavern ويتم تحميلها عند بدء تشغيل الخادم، ولكن *فقط* إذا تم تعيين `enableServerPlugins` على `true` في ملف `config.yaml`.

!!!warning تحذير
 **Server Plugins غير معزولة. هذا يعني أنها يمكن أن تحصل على وصول إلى نظام الملفات بالكامل، أو تقدم مجموعة واسعة من الثغرات الأمنية بطريقة لا تستطيع إضافات واجهة المستخدم العادية القيام بها. قم بتثبيت الإضافات الخاصة بالخادم فقط من المطورين الذين تثق بهم!**
!!!

للحصول على قائمة بجميع الإضافات الرسمية للخادم، راجع قائمة منظمة GitHub: <https://github.com/search?q=topic%3Aplugin+org%3ASillyTavern&type=Repositories>

## أنواع الإضافات

### الملفات

ملف JavaScript قابل للتنفيذ بامتداد ".js" (لوحدات CommonJS) أو ".mjs" (لوحدات ES) يحتوي على وحدة تُصدّر دالة `init`. تقبل هذه الدالة موجه Express (تم إنشاؤه خصيصاً للإضافة الخاصة بك) كوسيطة وتُرجع Promise.

يجب أن تُصدّر الوحدة أيضاً كائن `info` يحتوي على معلومات حول الإضافة (سلاسل `id`، `name`، و `description`). سيوفر هذا معلومات حول الإضافة للمُحمِّل.

يمكنك تسجيل المسارات عبر الموجه الذي سيتم تسجيله تحت مسار `/api/plugins/{id}/{route}`. على سبيل المثال، `router.get('/foo')` للإضافة `example` ستنتج مساراً مثل هذا: `/api/plugins/example/foo`.

يمكن للإضافة أيضاً *اختيارياً* تصدير دالة `exit` التي تقوم بالتنظيف عند إيقاف تشغيل الخادم. يجب ألا تحتوي على وسيطات ويجب أن تُرجع Promise.

عقد TypeScript لصادرات الإضافة:

```ts
interface PluginInfo {
    id: string;
    name: string;
    description: string;
}

interface Plugin {
    init: (router: Router) => Promise<void>;
    exit: () => Promise<void>;
    info: PluginInfo;
}
```

انظر أدناه لمثال على إضافة "Hello world!":

```js
/**
 * تهيئة الإضافة.
 * @param {import('express').Router} router موجه Express
 * @returns {Promise<any>} Promise يتم حلها عند تهيئة الإضافة
 */
async function init(router) {
    // قم بالتهيئة هنا...
    router.get('/foo', req, res, function () {
       res.send('bar');
    });
    console.log('Example plugin loaded!');
    return Promise.resolve();
}

async function exit() {
    // قم ببعض التنظيف هنا...
    return Promise.resolve();
}

module.exports = {
    init,
    exit,
    info: {
        id: 'example',
        name: 'Example',
        description: 'My cool plugin!',
    },
};
```

### الأدلة

يمكنك تحميل إضافة من دليل فرعي في دليل `plugins` بإحدى الطرق التالية (حسب الأسبقية):

1. ملف `package.json` يحتوي على مسار إلى ملف قابل للتنفيذ في حقل "main".
2. ملف `index.js` لوحدات CommonJS.
3. ملف `index.mjs` لوحدات ES.

يجب أن يُصدّر الملف الناتج دالة `init` وكائن `info` بنفس المتطلبات كما هو الحال للملفات الفردية.

مثال على إضافة دليل (مع ملف `index.js`): <https://github.com/SillyTavern/SillyTavern-DiscordRichPresence-Server>

### التجميع

من الأفضل استخدام أداة تجميع (مثل Webpack أو Browserify) التي ستحزم جميع المتطلبات في ملف واحد. تأكد من تعيين "Node" كهدف للبناء.

مستودع قالب للإضافات باستخدام Webpack و TypeScript: <https://github.com/SillyTavern/Plugin-WebpackTemplate>
