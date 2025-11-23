---
order: -20
icon: file-added
templating: false
route: /for-contributors/writing-extensions/
---

# UI Extensions

إضافات واجهة المستخدم توسع وظائف SillyTavern من خلال الربط بأحداثه و API. تعمل في سياق المتصفح ولديها وصول غير مقيد عملياً إلى DOM وواجهات برمجة تطبيقات JavaScript وسياق SillyTavern. يمكن للإضافات تعديل واجهة المستخدم واستدعاء واجهات برمجة التطبيقات الداخلية والتفاعل مع بيانات الدردشة. يشرح هذا الدليل كيفية إنشاء إضافاتك الخاصة (مطلوب معرفة JavaScript).

!!!tip هل تريد فقط تثبيت الإضافات؟
اذهب هنا: [Extensions](../extensions/index.md).
!!!

لتوسيع وظائف خادم Node.js، راجع صفحة [Server Plugins](./Server-Plugins.md).

**لا تستطيع كتابة JavaScript؟**

* اعتبر [STscript](./st-script.md) بديلاً أبسط لكتابة إضافة كاملة.
* اذهب عبر [دورة MDN](https://developer.mozilla.org/en-US/docs/Learn/JavaScript) وعد عندما تنتهي.

## تقديم الإضافات

تريد المساهمة بإضافاتك إلى [مستودع المحتوى الرسمي](https://github.com/SillyTavern/SillyTavern-Content)؟ اتصل بنا!

للتأكد من أن جميع الإضافات آمنة وسهلة الاستخدام، لدينا بعض المتطلبات:

1. يجب أن تكون إضافتك مفتوحة المصدر ولها رخصة libre (راجع [Choose a License](https://choosealicense.com/licenses/)). إذا لم تكن متأكداً، فإن AGPLv3 خيار جيد.
2. يجب أن تكون الإضافات متوافقة مع أحدث إصدار من SillyTavern. يرجى أن تكون مستعداً لتحديث إضافتك إذا تغير شيء في الأساس.
3. يجب أن تكون الإضافات موثقة جيداً. يتضمن ذلك ملف README بتعليمات التثبيت وأمثلة الاستخدام وقائمة بالميزات.
4. لن يتم قبول الإضافات التي لها متطلب إضافة خادم للعمل.

## أمثلة

شاهد أمثلة حية لإضافات SillyTavern البسيطة:

* <https://github.com/city-unit/st-extension-example> - قالب إضافة أساسي. يعرض إنشاء manifest، واستيرادات النصوص المحلية، وإضافة لوحة إعدادات واجهة المستخدم، واستخدام إعدادات الإضافة الدائمة.
* <https://github.com/search?q=topic%3Aextension+org%3ASillyTavern&type=Repositories> - قائمة بجميع إضافات SillyTavern الرسمية على GitHub.

## التجميع

يمكن للإضافات أيضاً الاستفادة من التجميع لعزل نفسها عن بقية الوحدات واستخدام أي تبعيات من NPM، بما في ذلك أطر عمل واجهة المستخدم مثل Vue و React، إلخ.

* <https://github.com/SillyTavern/Extension-WebpackTemplate> - مستودع قالب لإضافة باستخدام TypeScript و Webpack (بدون React).
* <https://github.com/SillyTavern/Extension-ReactTemplate> - مستودع قالب لإضافة أساسية باستخدام React و Webpack.

لاستخدام الاستيرادات النسبية من الحزمة، قد تحتاج إلى إنشاء غلاف استيراد. هذا مثال لـ Webpack:

```js
/**
 * استيراد عضو من وحدة بواسطة URL، تجاوز webpack.
 * @param {string} url URL للاستيراد منه
 * @param {string} what اسم العضو للاستيراد
 * @param {any} defaultValue القيمة الاحتياطية
 * @returns {Promise<any>} العضو المستورد
 */
export async function importFromUrl(url, what, defaultValue = null) {
    try {
        const module = await import(/* webpackIgnore: true */ url);
        if (!Object.hasOwn(module, what)) {
            throw new Error(`No ${what} in module`);
        }
        return module[what];
    } catch (error) {
        console.error(`Failed to import ${what} from ${url}: ${error}`);
        return defaultValue;
     }
}

// استيراد وظيفة من وحدة 'script.js'
const generateRaw = await importFromUrl('/script.js', 'generateRaw');
```

## manifest.json

يجب أن يكون لكل إضافة مجلد في `data/<user-handle>/extensions` وملف `manifest.json`، والذي يحتوي على بيانات وصفية حول الإضافة ومساراً إلى ملف برنامج JS النصي الذي هو نقطة الدخول للإضافة.

يتم تثبيت الإضافات القابلة للتنزيل في مجلد `/scripts/extensions/third-party` عند التقديم عبر HTTP، لذا يجب استخدام الاستيرادات النسبية بناءً على ذلك. لتسهيل التطوير المحلي، اعتبر وضع مستودع إضافتك في مجلد `/scripts/extensions/third-party` (خيار "Install for all users").

```json
{
    "display_name": "The name of the extension",
    "loading_order": 1,
    "requires": [],
    "optional": [],
    "dependencies": [],
    "js": "index.js",
    "css": "style.css",
    "author": "Your name",
    "version": "1.0.0",
    "homePage": "https://github.com/your/extension",
    "auto_update": true,
    "minimum_client_version": "1.0.0",
    "i18n": {
        "de-de": "i18n/de-de.json"
    }
}
```

### حقول Manifest

* `display_name` مطلوب. يتم عرضه في قائمة "Manage Extensions".
* `loading_order` اختياري. رقم أعلى يتم تحميله لاحقاً.
* `js` هو مرجع ملف JS الرئيسي ومطلوب.
* `css` هو مرجع ملف نمط اختياري.
* `author` مطلوب. يجب أن يحتوي على اسم المؤلف (المؤلفين) أو معلومات الاتصال.
* `auto_update` يتم تعيينه على `true` إذا كان يجب تحديث الإضافة تلقائياً عند تغيير إصدار حزمة ST.
* `i18n` هو كائن اختياري يحدد اللغات المدعومة وملفات JSON المقابلة لها (انظر أدناه).
* `dependencies` هو مصفوفة اختيارية من السلاسل تحدد **الإضافات** الأخرى التي تعتمد عليها هذه الإضافة.
* `generate_interceptor` هو سلسلة اختيارية تحدد اسم دالة عامة يتم استدعاؤها على طلبات توليد النص.
* `minimum_client_version` هو سلسلة اختيارية تحدد الحد الأدنى لإصدار SillyTavern المطلوب لتعمل هذه الإضافة.

### التبعيات

يمكن للإضافات أيضاً أن تعتمد على إضافات SillyTavern الأخرى. لن يتم تحميل الإضافة إذا كانت أي من هذه التبعيات مفقودة أو معطلة.

يتم تحديد التبعيات بـ **اسم مجلدها** كما يظهر في دليل `public/extensions`.

أمثلة:

* الإضافات المدمجة: `"vectors"`، `"caption"`
* إضافات الطرف الثالث: `"third-party/Extension-WebLLM"`، `"third-party/Extension-Mermaid"`

### الحقول المهجورة

* `requires` هو مصفوفة اختيارية من السلاسل تحدد وحدات **Extras modules** المطلوبة. لن يتم تحميل الإضافة إذا لم توفر واجهة برمجة تطبيقات Extras المتصلة جميع الوحدات المدرجة.
* `optional` هو مصفوفة اختيارية من السلاسل تحدد وحدات **Extras modules** الاختيارية. ستظل الإضافة تُحمّل إذا كانت هذه مفقودة، ويجب على الإضافة التعامل مع غيابها بشكل صحيح.

للتحقق من الوحدات المقدمة حالياً بواسطة واجهة برمجة تطبيقات Extras المتصلة، قم باستيراد مصفوفة `modules` من `scripts/extensions.js`.

## البرمجة النصية

### استخدام getContext

دالة `getContext()` في كائن `SillyTavern` العام تمنحك الوصول إلى سياق SillyTavern، وهو مجموعة من جميع كائنات حالة التطبيق الرئيسية والوظائف المفيدة والأدوات المساعدة.

```js
const context = SillyTavern.getContext();
context.chat; // سجل الدردشة - قابل للتعديل
context.characters; // قائمة الشخصيات
context.characterId; // فهرس الشخصية الحالية
context.groups; // قائمة المجموعات
context.groupId; // معرف المجموعة الحالية
// والكثير...
```

يمكنك العثور على القائمة الكاملة للخصائص والوظائف المتاحة في [كود مصدر SillyTavern](https://github.com/SillyTavern/SillyTavern/blob/staging/public/scripts/st-context.js).

!!!
إذا كانت أي من الوظائف/الخصائص مفقودة في `getContext`، يرجى الاتصال بالمطورين أو إرسال طلب سحب لنا!
!!!

### المكتبات المشتركة

معظم مكتبات npm المستخدمة داخلياً بواسطة واجهة SillyTavern الأمامية مشتركة في خاصية `libs` لكائن `SillyTavern` العام.

* `lodash` - مكتبة الأدوات المساعدة. [المستندات](https://lodash.com/).
* `localforage` - مكتبة تخزين المتصفح. [المستندات](https://localforage.github.io/localForage/).
* `Fuse` - مكتبة بحث غامض. [المستندات](https://www.fusejs.io/).
* `DOMPurify` - مكتبة تنقية HTML. [المستندات](https://github.com/cure53/DOMPurify).
* `Handlebars` - مكتبة القوالب. [المستندات](https://handlebarsjs.com/).
* `moment` - مكتبة معالجة التاريخ/الوقت. [المستندات](http://momentjs.com/).
* `showdown` - مكتبة محول Markdown. [المستندات](https://showdownjs.com/).

يمكنك العثور على القائمة الكاملة للمكتبات المُصدّرة في [كود مصدر SillyTavern](https://github.com/SillyTavern/SillyTavern/blob/staging/public/lib.js).

**مثال:** استخدام مكتبة DOMPurify.

```js
const { DOMPurify } = SillyTavern.libs;

const sanitizedHtml = DOMPurify.sanitize('<script>"dirty HTML"</script>');
```

### ملاحظة TypeScript

إذا كنت تريد الوصول إلى الإكمال التلقائي لجميع الطرق في كائن `SillyTavern` العام (وربما تريد ذلك)، بما في ذلك `getContext()` و `libs`، يجب عليك إضافة إعلان وحدة TypeScript `.d.ts`. يجب أن يستورد هذا الإعلان الأنواع العامة من مصدر SillyTavern، اعتماداً على موقع إضافتك. فيما يلي مثال يعمل لكلا نوعي التثبيت: "all users" و "current user."

**global.d.ts** - ضع هذا الملف في جذر دليل إضافتك (بجوار `manifest.json`):

```ts
export {};

// 1. استيراد للإضافات ذات نطاق المستخدم
import '../../../../public/global';
// 2. استيراد للإضافات ذات نطاق الخادم
import '../../../../global';

// حدد أنواعاً إضافية إذا لزم الأمر...
declare global {
    // أضف إعلانات الأنواع العامة هنا
}
```

### الاستيراد من ملفات أخرى

!!!warning
استخدام الاستيرادات من كود SillyTavern غير موثوق ويمكن أن ينكسر في أي وقت إذا تغيرت البنية الداخلية لوحدات ST. يوفر `getContext` واجهة برمجة تطبيقات أكثر استقراراً.
!!!

ما لم تكن تبني إضافة مجمعة، يمكنك استيراد المتغيرات والوظائف من ملفات JS الأخرى.

على سبيل المثال، سينشئ مقتطف الكود هذا رداً من واجهة برمجة التطبيقات المحددة حالياً في الخلفية:

```js
import { generateQuietPrompt } from "../../../../script.js";

async function handleMessage(data) {
    const text = data.message;
    const translated = await generateQuietPrompt({ quietPrompt: text });
    // ...
}
```

## إدارة الحالة

### الإعدادات الدائمة

عندما تحتاج إضافة إلى الاحتفاظ بحالتها، يمكنها استخدام كائن `extensionSettings` من دالة `getContext()` لتخزين واسترداد البيانات. يمكن للإضافة تخزين أي بيانات قابلة للتسلسل JSON في كائن الإعدادات ويجب استخدام مفتاح فريد لتجنب التعارضات مع الإضافات الأخرى.

لإستمرار الإعدادات، استخدم دالة `saveSettingsDebounced()`، والتي ستحفظ الإعدادات على الخادم.

```js
const { extensionSettings, saveSettingsDebounced } = SillyTavern.getContext();

// حدد معرفاً فريداً لإضافتك
const MODULE_NAME = 'my_extension';

// حدد الإعدادات الافتراضية
const defaultSettings = Object.freeze({
    enabled: false,
    option1: 'default',
    option2: 5
});

// حدد دالة للحصول على الإعدادات أو تهيئتها
function getSettings() {
    // تهيئة الإعدادات إذا لم تكن موجودة
    if (!extensionSettings[MODULE_NAME]) {
        extensionSettings[MODULE_NAME] = structuredClone(defaultSettings);
    }

    // تأكد من وجود جميع المفاتيح الافتراضية (مفيد بعد التحديثات)
    for (const key of Object.keys(defaultSettings)) {
        if (!Object.hasOwn(extensionSettings[MODULE_NAME], key)) {
            extensionSettings[MODULE_NAME][key] = defaultSettings[key];
        }
    }

    return extensionSettings[MODULE_NAME];
}

// استخدم الإعدادات
const settings = getSettings();
settings.option1 = 'new value';

// احفظ الإعدادات
saveSettingsDebounced();
```

### البيانات الوصفية للدردشة

لربط بعض البيانات بدردشة معينة، يمكنك استخدام كائن `chatMetadata` من دالة `getContext()`. يسمح لك هذا الكائن بتخزين بيانات عشوائية مرتبطة بالدردشة، والتي يمكن أن تكون مفيدة لتخزين حالة خاصة بالإضافة.

لإستمرار البيانات الوصفية، استخدم دالة `saveMetadata()`، والتي ستحفظ البيانات الوصفية على الخادم.

!!!warning
لا تحفظ المرجع إلى `chatMetadata` في متغير طويل الأمد، حيث سيتغير المرجع عند تبديل الدردشة. استخدم دائماً `SillyTavern.getContext().chatMetadata` للوصول إلى البيانات الوصفية للدردشة الحالية.
!!!

```js
const { chatMetadata, saveMetadata } = SillyTavern.getContext();

// قم بتعيين بعض البيانات الوصفية للدردشة الحالية
chatMetadata['my_key'] = 'my_value';

// احصل على البيانات الوصفية للدردشة الحالية
const value = chatMetadata['my_key'];

// احفظ البيانات الوصفية على الخادم
await saveMetadata();
```

!!!tip
يتم إصدار حدث `CHAT_CHANGED` عند تبديل الدردشة، لذا يمكنك الاستماع إلى هذا الحدث لتحديث حالة إضافتك وفقاً لذلك. انظر المزيد في قسم [الاستماع إلى الأحداث](#listening-to-events).
!!!

### بطاقات الشخصيات

SillyTavern يدعم بالكامل [مواصفات بطاقات الشخصيات V2](https://github.com/malfoyslastname/character-card-spec-v2/blob/main/spec_v2.md)، والتي تسمح بتخزين بيانات عشوائية في بيانات JSON لبطاقة الشخصية.

هذا مفيد للإضافات التي تحتاج إلى تخزين بيانات إضافية مرتبطة بالشخصية وجعلها قابلة للمشاركة عند تصدير بطاقة الشخصية.

لكتابة بيانات إلى حقل بيانات [extensions](https://github.com/malfoyslastname/character-card-spec-v2/blob/main/spec_v2.md#extensions) في بطاقة الشخصية، استخدم دالة `writeExtensionField` من دالة `getContext()`. تأخذ هذه الدالة معرف شخصية، ومفتاح سلسلة، وقيمة للكتابة. يجب أن تكون القيمة قابلة للتسلسل JSON.

!!!warning غرابة قادمة
على الرغم من تسميته `characterId`، إلا أنه ليس معرفاً فريداً "حقيقياً" ولكنه بالأحرى فهرس الشخصية في مصفوفة `characters`.

يتم توفير فهرس الشخصية الحالية بواسطة خاصية `characterId` في السياق. إذا كنت تريد كتابة بيانات إلى الشخصية المحددة حالياً، استخدم `SillyTavern.getContext().characterId`. إذا كنت بحاجة إلى تخزين بيانات لشخصية أخرى، ابحث عن الفهرس من خلال البحث عن الشخصية في مصفوفة `characters`.

**تحذير: `characterId` هو `undefined` في الدردشات الجماعية أو عندما لا يتم تحديد شخصية!**
!!!

```js
const { writeExtensionField, characterId } = SillyTavern.getContext();

// اكتب بعض البيانات إلى بطاقة الشخصية
await writeExtensionField(characterId, 'my_extension_key', {
    someData: 'value',
    anotherData: 42
});

// اقرأ البيانات من بطاقة الشخصية
const character = SillyTavern.getContext().characters[characterId];
// يتم تخزين البيانات في كائن `extensions` لبيانات الشخصية
const myData = character.data?.extensions?.my_extension_key;
```

### إعدادات مسبقة للإعدادات

يمكن تخزين بيانات JSON العشوائية في الإعدادات المسبقة للإعدادات لأنواع API الرئيسية. سيتم تصديرها واستيرادها مع JSON الإعداد المسبق، بحيث يمكنك استخدامها لتخزين الإعدادات الخاصة بالإضافة للإعداد المسبق. أنواع API التالية تدعم امتدادات البيانات في الإعدادات المسبقة:

* Chat Completion
* Text Completion
* NovelAI
* KoboldAI / AI Horde

لقراءة أو كتابة البيانات، تحتاج أولاً إلى الحصول على مثيل PresetManager من السياق:

```js
const { getPresetManager } = SillyTavern.getContext();

// احصل على مدير الإعدادات المسبقة لنوع API الحالي
const pm = getPresetManager();

// اكتب البيانات إلى حقل امتداد الإعداد المسبق:
// - path: المسار إلى الحقل في بيانات الإعداد المسبق
// - value: القيمة للكتابة
// - name (اختياري): اسم الإعداد المسبق للكتابة إليه، الافتراضي للإعداد المسبق المحدد حالياً
await pm.writePresetExtensionField({ path: 'hello', value: 'world' });

// اقرأ البيانات من حقل امتداد الإعداد المسبق:
// - path: المسار إلى الحقل في بيانات الإعداد المسبق
// - name (اختياري): اسم الإعداد المسبق للقراءة منه، الافتراضي للإعداد المسبق المحدد حالياً
const value = pm.readPresetExtensionField({ path: 'hello' });
```

!!!tip
يتم إصدار أحداث `PRESET_CHANGED` و `MAIN_API_CHANGED` عند تغيير الإعداد المسبق أو تبديل API الرئيسي، لذا يمكنك الاستماع إلى هذه الأحداث لتحديث حالة إضافتك وفقاً لذلك. انظر المزيد في قسم [الاستماع إلى الأحداث](#listening-to-events).
!!!

## التدويل

!!!
للحصول على معلومات عامة حول تقديم الترجمات، راجع صفحة [Internationalization](/For_Contributors/i18n.md).
!!!

يمكن للإضافات توفير سلاسل إضافية محلية للاستخدام مع دالتي `t`، `translate` وخاصية `data-i18n` في قوالب HTML.

راجع قائمة اللغات المدعومة هنا (مفتاح `lang`): <https://github.com/SillyTavern/SillyTavern/blob/release/public/locales/lang.json>

### استدعاء `addLocaleData` المباشر

مرر رمز اللغة وكائناً بالترجمات إلى دالة `addLocaleData`. *لا* يُسمح بتجاوز المفاتيح الموجودة. إذا كان رمز اللغة الممرر ليس اللغة المختارة حالياً، سيتم تجاهل البيانات بصمت.

```js
SillyTavern.getContext().addLocaleData('fr-fr', { 'Hello': 'Bonjour' });
SillyTavern.getContext().addLocaleData('de-de', { 'Hello': 'Hallo' });
```

### عبر manifest الإضافة

أضف كائن i18n بقائمة اللغات المدعومة ومسارات ملفات JSON المقابلة لها (نسبة إلى دليل إضافتك) إلى manifest.

```json
{
  "display_name": "Foobar",
  "js": "index.js",
  // بقية الحقول
  "i18n": {
    "fr-fr": "i18n/french.json",
    "de-de": "i18n/german.json"
  }
}
```

## تسجيل أوامر الشرطة المائلة (الطريقة الجديدة)

بينما لا يزال `registerSlashCommand` موجوداً للتوافق مع الإصدارات السابقة، يجب الآن تسجيل أوامر الشرطة المائلة الجديدة من خلال `SlashCommandParser.addCommandObject()` لتوفير تفاصيل موسعة حول الأمر ومعاملاته للمحلل (وبالتالي للإكمال التلقائي ومساعدة الأمر).

```javascript
SlashCommandParser.addCommandObject(SlashCommand.fromProps({ name: 'repeat',
    callback: (namedArgs, unnamedArgs) => {
        return Array(namedArgs.times ?? 5)
            .fill(unnamedArgs.toString())
            .join(isTrueBoolean(namedArgs.space.toString()) ? ' ' : '')
        ;
    },
    aliases: ['example-command'],
    returns: 'the repeated text',
    namedArgumentList: [
        SlashCommandNamedArgument.fromProps({ name: 'times',
            description: 'number of times to repeat the text',
            typeList: ARGUMENT_TYPE.NUMBER,
            defaultValue: '5',
        }),
        SlashCommandNamedArgument.fromProps({ name: 'space',
            description: 'whether to separate the texts with a space',
            typeList: ARGUMENT_TYPE.BOOLEAN,
            defaultValue: 'off',
            enumList: ['on', 'off'],
        }),
    ],
    unnamedArgumentList: [
        SlashCommandArgument.fromProps({ description: 'the text to repeat',
            typeList: ARGUMENT_TYPE.STRING,
            isRequired: true,
        }),
    ],
    helpString: `
        <div>
            Repeats the provided text a number of times.
        </div>
        <div>
            <strong>Example:</strong>
            <ul>
                <li>
                    <pre><code class="language-stscript">/repeat foo</code></pre>
                    returns "foofoofoofoofoo"
                </li>
                <li>
                    <pre><code class="language-stscript">/repeat times=3 space=on bar</code></pre>
                    returns "bar bar bar"
                </li>
            </ul>
        </div>
    `,
}));
```

يمكن استخدام جميع الأوامر المسجلة في [STscript](/For_Contributors/st-script.md) بأي طريقة ممكنة.

## الأحداث

### الاستماع إلى الأحداث

استخدم `eventSource.on(eventType, eventHandler)` للاستماع إلى الأحداث:

```js
const { eventSource, event_types } = SillyTavern.getContext();

eventSource.on(event_types.MESSAGE_RECEIVED, handleIncomingMessage);

function handleIncomingMessage(data) {
    // معالجة الرسالة
}
```

أنواع الأحداث الرئيسية هي:

* `APP_READY`: التطبيق محمل بالكامل وجاهز للاستخدام. سيتم تفعيله تلقائياً في كل مرة يتم فيها إرفاق مستمع جديد بعد أن يصبح التطبيق جاهزاً.
* `MESSAGE_RECEIVED`: يتم إنشاء رسالة نموذج اللغة الكبير وتسجيلها في كائن `chat` ولكن لم يتم عرضها بعد في واجهة المستخدم.
* `MESSAGE_SENT`: يتم إرسال الرسالة من قبل المستخدم وتسجيلها في كائن `chat` ولكن لم يتم عرضها بعد في واجهة المستخدم.
* `USER_MESSAGE_RENDERED`: يتم عرض الرسالة المرسلة من قبل المستخدم في واجهة المستخدم.
* `CHARACTER_MESSAGE_RENDERED`: يتم عرض رسالة نموذج اللغة الكبير المُولّدة في واجهة المستخدم.
* `CHAT_CHANGED`: تم تبديل الدردشة (على سبيل المثال، التبديل إلى شخصية أخرى، أو تم تحميل دردشة أخرى).
* `GENERATION_AFTER_COMMANDS`: التوليد على وشك البدء بعد معالجة أوامر الشرطة المائلة.
* `GENERATION_STOPPED`: تم إيقاف التوليد من قبل المستخدم.
* `GENERATION_ENDED`: تم إكمال التوليد أو حدث خطأ.
* `SETTINGS_UPDATED`: تم تحديث إعدادات التطبيق.

يمكن العثور على الباقي [في المصدر](https://github.com/SillyTavern/SillyTavern/blob/staging/public/scripts/events.js).

!!!info بيانات الحدث
الطريقة التي يمرر بها كل حدث بياناته إلى المستمع ليست موحدة. بعض الأحداث لا تصدر أي بيانات؛ بعضها يمرر كائناً أو قيمة بدائية. يرجى الرجوع إلى كود المصدر حيث يتم إصدار الحدث لمعرفة البيانات التي يمررها، أو التحقق مع المصحح.
!!!

### إصدار الأحداث

يمكنك إنتاج أي أحداث تطبيق من الإضافات، بما في ذلك الأحداث المخصصة، عن طريق استدعاء `eventSource.emit(eventType, ...eventData)`:

```js
const { eventSource } = SillyTavern.getContext();

// يمكن أن يكون حقل event_types مدمجاً أو أي سلسلة.
const eventType = 'myCustomEvent';

// استخدم `await` للتأكد من اكتمال جميع معالجات الأحداث قبل الاستمرار في التنفيذ.
await eventSource.emit(eventType, { data: 'custom event data' });
```

## Prompt Interceptors

توفر Prompt Interceptors طريقة للإضافات لأداء أي نشاط مثل تعديل بيانات الدردشة أو إضافة حقن أو إحباط التوليد قبل تقديم طلب توليد نص.

يتم تشغيل Interceptors من إضافات مختلفة بالتسلسل. يتم تحديد الترتيب بواسطة حقل `loading_order` في ملفات `manifest.json` الخاصة بها. الإضافات ذات قيم `loading_order` الأقل تعمل أولاً. إذا لم يتم تحديد `loading_order`، يتم استخدام `display_name` كبديل احتياطي. إذا لم يتم تحديد أي منهما، فإن الترتيب غير محدد.

### تسجيل Interceptor

لتحديد prompt interceptor، أضف حقل `generate_interceptor` إلى ملف `manifest.json` لإضافتك. يجب أن تكون القيمة اسم دالة عامة سيتم استدعاؤها بواسطة SillyTavern.

```json
{
    "display_name": "My Interceptor Extension",
    "loading_order": 10, // يؤثر على ترتيب التنفيذ
    "generate_interceptor": "myCustomInterceptorFunction",
    // ... خصائص manifest الأخرى
}
```

### دالة Interceptor

دالة `generate_interceptor` هي دالة عامة سيتم استدعاؤها عند طلبات التوليد التي ليست تشغيلات تجريبية. يجب تحديدها في النطاق العام (على سبيل المثال، `globalThis.myCustomInterceptorFunction = async function(...) { ... }`) ويمكن أن ترجع `Promise` إذا كانت بحاجة إلى إجراء أي عمليات غير متزامنة.

تتلقى دالة interceptor الوسيطات التالية:

* `chat`: مصفوفة من كائنات الرسائل تمثل سجل الدردشة الذي سيتم استخدامه لبناء المطالبة. يمكنك تعديل هذه المصفوفة مباشرة (على سبيل المثال، إضافة أو إزالة أو تعديل الرسائل). يرجى ملاحظة أن الرسائل قابلة للتعديل، لذا فإن أي تغييرات تجريها على المصفوفة سوف تنعكس في سجل الدردشة الفعلي. إذا كنت تريد أن تكون التغييرات مؤقتة، استخدم `structuredClone` لإنشاء نسخة عميقة من كائن الرسالة.
* `contextSize`: رقم يشير إلى حجم السياق الحالي (بالرموز) المحسوب للتوليد القادم.
* `abort`: دالة تمنع، عند استدعائها، المتابعة في توليد النص. تقبل معاملاً منطقياً يمنع أي interceptors لاحقة من التشغيل إذا كان `true`.
* `type`: سلسلة تشير إلى نوع التوليد أو تفعيله (على سبيل المثال، `'quiet'`، `'regenerate'`، `'impersonate'`، `'swipe'`، إلخ). يساعد هذا interceptor على تطبيق المنطق بشكل مشروط بناءً على كيفية بدء التوليد.

**مثال على التنفيذ:**

```javascript
globalThis.myCustomInterceptorFunction = async function(chat, contextSize, abort, type) {
    // مثال: إضافة ملاحظة نظام قبل آخر رسالة مستخدم
    const systemNote = {
        is_user: false,
        name: "System Note",
        send_date: Date.now(),
        mes: "This was added by my extension!"
    };
    // أدرج قبل الرسالة الأخيرة
    chat.splice(chat.length - 1, 0, systemNote);
}
```

## توليد النص

يوفر SillyTavern عدة وظائف لتوليد النص في سياقات مختلفة باستخدام واجهة برمجة تطبيقات نموذج اللغة الكبير المختارة حالياً. تسمح لك هذه الوظائف بتوليد النص في سياق دردشة، أو توليد خام دون أي سياق، أو مع مخرجات منظمة.

### ضمن سياق دردشة

يتم استخدام دالة `generateQuietPrompt()` لتوليد النص في سياق دردشة مع مطالبة "هادئة" مضافة (تعليمات ما بعد السجل) في الخلفية (لا يتم عرض الناتج في واجهة المستخدم). هذا مفيد لتوليد النص دون مقاطعة تجربة المستخدم مع الحفاظ أيضاً على بيانات الدردشة والشخصية ذات الصلة سليمة، مثل توليد ملخص أو مطالبة صورة.

```js
const { generateQuietPrompt } = SillyTavern.getContext();

const quietPrompt = 'Generate a summary of the chat history.';

const result = await generateQuietPrompt({
    quietPrompt,
});
```

### التوليد الخام

يتم استخدام دالة `generateRaw()` لتوليد النص دون أي سياق دردشة. إنها مفيدة عندما تريد التحكم الكامل في عملية بناء المطالبة.

تقبل `prompt` كسلسلة Text Completion أو مصفوفة من كائنات Chat Completion، وتبني الطلب بتنسيق مناسب حسب نوع API المحدد، على سبيل المثال، التحويل بين أوضاع الدردشة/النص، وتطبيق تنسيق instruct، إلخ. يمكنك أيضاً تمرير `systemPrompt` و `prefill` إضافية إلى الدالة لمزيد من التحكم في عملية التوليد.

```js
const { generateRaw } = SillyTavern.getContext();

const systemPrompt = 'You are a helpful assistant.';
const prompt = 'Generate a story about a brave knight.';
const prefill = 'Once upon a time,';

/*
في وضع Chat Completion، سينتج مطالبة مثل هذه:
[
  {role: 'system', content: 'You are a helpful assistant.'},
  {role: 'user', content: 'Generate a story about a brave knight.'},
  {role: 'assistant', content: 'Once upon a time,'}
]
*/

/*
في وضع Text Completion (بدون instruct)، سينتج مطالبة مثل هذه:
"You are a helpful assistant.\nGenerate a story about a brave knight.\nOnce upon a time,"
*/

const result = await generateRaw({
    systemPrompt,
    prompt,
    prefill,
});
```

### Structured Outputs

!!!info
مدعوم حالياً فقط بواسطة واجهة برمجة تطبيقات Chat Completion. يختلف التوفر بناءً على المصدر والنموذج المحدد. إذا كان النموذج المحدد لا يدعم المخرجات المنظمة، فإن التوليد إما سيفشل أو سيعيد كائناً فارغاً (`'{}'`). راجع التوثيق لواجهة برمجة التطبيقات المحددة التي تستخدمها لمعرفة ما إذا كانت المخرجات المنظمة مدعومة.
!!!

يمكنك استخدام ميزة المخرجات المنظمة للتأكد من أن النموذج ينتج كائن JSON صالحاً يلتزم بـ [JSON Schema](https://json-schema.org/learn) المقدم. هذا مفيد للإضافات التي تتطلب بيانات منظمة، مثل تتبع الحالة، وتصنيف البيانات، إلخ.

لاستخدام المخرجات المنظمة، يجب تمرير كائن مخطط JSON إلى `generateRaw()` أو `generateQuietPrompt()`. سيولد النموذج بعد ذلك رداً يطابق المخطط، وسيتم إرجاعه ككائن JSON مسلسل.

!!!warning
لا يتم التحقق من صحة المخرجات مقابل المخطط، يجب عليك التعامل مع التحليل والتحقق من الناتج المُولّد بنفسك. إذا فشل النموذج في توليد كائن JSON صالح، فإن الدالة سترجع كائناً فارغاً (`'{}'`).

[Zod](https://zod.dev/json-schema) هي مكتبة شائعة لتوليد والتحقق من مخططات JSON. لن يتم تغطية استخدامها هنا.
!!!

```js
const { generateRaw, generateQuietPrompt } = SillyTavern.getContext();

// حدد مخطط JSON للناتج المتوقع
const jsonSchema = {
    // مطلوب: اسم للمخطط
    name: 'StoryStateModel',
    // اختياري: وصف للمخطط
    description: 'A schema for a story state with location, plans, and memories.',
    // اختياري: سيتم استخدام المخطط في الوضع الصارم، مما يعني أنه سيُسمح فقط بالحقول المحددة في المخطط
    strict: true,
    // مطلوب: تعريف للمخطط
    value: {
        '$schema': 'http://json-schema.org/draft-04/schema#',
        'type': 'object',
        'properties': {
            'location': {
                'type': 'string'
            },
            'plans': {
                'type': 'string'
            },
            'memories': {
                'type': 'string'
            }
        },
        'required': [
            'location',
            'plans',
            'memories'
        ],
    },
};

const prompt = 'Generate a story state with location, plans, and memories. Output as a JSON object.';

const rawResult = await generateRaw({
    prompt,
    jsonSchema,
});

const quietResult = await generateQuietPrompt({
    quietPrompt: prompt,
    jsonSchema,
});
```

## تسجيل ماكرو مخصصة

يمكنك تسجيل ماكرو مخصصة يمكن استخدامها في أي مكان تدعم فيه استبدالات الماكرو، على سبيل المثال في حقول بطاقة الشخصية، وأوامر STscript، وقوالب المطالبات، إلخ.

لتسجيل ماكرو، استخدم دالة `registerMacro()` من كائن `SillyTavern.getContext()`. تقبل الدالة اسم ماكرو يجب أن يكون سلسلة فريدة، وسلسلة أو دالة ترجع سلسلة. سيتم استدعاء الدالة مع سلسلة `nonce` فريدة ستكون مختلفة بين كل استدعاء `substituteParams`.

```js
const { registerMacro } = SillyTavern.getContext();

// ماكرو سلسلة بسيط
registerMacro('fizz', 'buzz');
// ماكرو دالة
registerMacro('tomorrow', () => {
    return new Date(Date.now() + 24 * 60 * 60 * 1000).toLocaleDateString();
});
```

عندما لا تكون هناك حاجة إلى ماكرو مخصص، قم بإزالته باستخدام دالة `unregisterMacro()`:

```js
const { unregisterMacro } = SillyTavern.getContext();

// إلغاء تسجيل ماكرو 'fizz'
unregisterMacro('fizz');
```

**تفاصيل مهمة وقيود معروفة بخصوص الماكرو المخصصة:**

1. حالياً، يتم دعم ماكرو استبدال السلاسل البسيطة فقط. نحن نعمل على إضافة دعم للماكرو الأكثر تعقيداً في المستقبل.
2. الماكرو التي تستخدم دوال لتوفير قيمة *يجب* أن تكون متزامنة. إرجاع `Promise` لن يعمل.
3. لا تحتاج إلى لف اسم الماكرو بأقواس معقوفة مزدوجة (`{{ }}`) عند تسجيله. سيقوم SillyTavern بذلك نيابة عنك.
4. نظراً لأن الماكرو هي استبدالات تعبير عادي بسيط، فإن تسجيل الكثير من الماكرو سيسبب مشاكل في الأداء، لذا استخدمها باعتدال.

## القيام بطلب Extras

!!!warning
واجهة برمجة تطبيقات Extras مهجورة. لا يُوصى باستخدامها في الإضافات الجديدة.
!!!

تسمح لك دالة `doExtrasFetch()` بتقديم طلبات إلى خادم واجهة برمجة تطبيقات SillyTavern Extras الخاص بك.

على سبيل المثال، لاستدعاء نقطة النهاية `/api/summarize`:

```js
import { getApiUrl, doExtrasFetch } from "../../extensions.js";

const url = new URL(getApiUrl());
url.pathname = '/api/summarize';

const apiResult = await doExtrasFetch(url, {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'Bypass-Tunnel-Reminder': 'bypass',
    },
    body: JSON.stringify({
        // جسم الطلب
    })
});
```

`getApiUrl()` يرجع URL الأساسي لخادم Extras.

دالة `doExtrasFetch()`:

* تضيف رؤوس `Authorization` و `Bypass-Tunnel-Reminder`
* تتعامل مع جلب النتيجة
* ترجع النتيجة (كائن الرد)

هذا يجعل من السهل استدعاء واجهة برمجة تطبيقات Extras من إضافتك.

يمكنك تحديد:

* طريقة الطلب: GET، POST، إلخ.
* رؤوس إضافية
* الجسم لطلبات POST
* أي خيارات جلب أخرى
