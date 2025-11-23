---
order: 112
route: /installation/st-1.12.0-migration-guide/
---

# دليل الترحيل 1.12.0

يتضمن SillyTavern 1.12.0 (الاسم الكودي تحديث "Neo Server") عدة تغييرات حاسمة قد تؤثر على طريقة استخدامك لـ SillyTavern.

سيعدك هذا الدليل للتحديث ويوفر بعض الإرشادات الإضافية.

## تحديث تخزين البيانات

يغير 1.12.0 طريقة تعامل SillyTavern مع بيانات المستخدم.

في السابق، كانت جميع البيانات الدائمة مخزنة مع جزء الواجهة الأمامية في دليل `/public`، مما أدى إلى إرباك ونقاط فشل محتملة، بالإضافة إلى جعل الحاويات وتثبيت التطبيقات على مستوى النظام أمراً صعباً للغاية.

### ما الذي تغير؟

تم نقل جميع المعلومات الدائمة من `/public` مثل الإعدادات والدردشات (القائمة الكاملة أدناه) إلى دليل منفصل بمسار قابل للتكوين، مما يجعله محمولاً ومستقلاً عن خادم الويب نفسه. عند الحاجة لأغراض التوافق، على سبيل المثال، لاستضافة الامتدادات، وبطاقات الشخصيات بالحجم الكامل، وتحميلات صور المستخدم، وما إلى ذلك، تم إعداد إعادة توجيه ذكية لاستضافة ملفات المستخدم تلقائياً من دليل البيانات.

### تعيين جذر البيانات

يمكنك توفير مسار مطلق أو نسبي (إلى دليل مستودع ST) لجذر البيانات إما عن طريق `config.yaml` أو عن طريق بدء الخادم باستخدام وسيط وحدة التحكم `--dataRoot`.

> مثال YAML

```yaml
# -- DATA CONFIGURATION --
# دليل جذر لتخزين بيانات المستخدم
dataRoot: C:\Users\Harry\Documents\ST-Data
```

> مثال وحدة التحكم

```bash
node server.js --dataRoot="/Users/harry/ST-Data"
# OR
npm run start -- --dataRoot="/Users/harry/ST-Data"
```

مسار جذر البيانات الافتراضي هو `./data`، مما يعني دليل `data` في مستودع SillyTavern.

!!!info ملاحظة
يجب أن يكون مسار جذر البيانات إما **مسار مطلق كامل** أو **مسار نسبي كامل**. _لا يمكنك_ استخدام اختصارات المسار مثل `~` أو `%APP_DATA%`، حيث يتم حلها بواسطة shell، وليس نظام التشغيل.
!!!

### الترحيل

#### **مهم!** قبل أن نبدأ

1. **فقط إذا كنت تريد نقل dataRoot من الموقع الافتراضي. وإلا تخطَّ هذا الجزء.** قم بتعيين جذر البيانات _قبل_ تشغيل الخادم لأول مرة بعد سحب التحديث. قم بتشغيل `npm install` لملء `config.yaml` بقيمة جديدة، أو مرر وسيط وحدة التحكم.
2. سيتم ترحيل جميع البيانات إلى حساب `default-user`. راجع المزيد عن [المستخدمين](#users) أدناه.

#### التثبيتات بدون حاويات (bare metal)

لا داعي لفعل أي شيء! يجب أن يتعامل الترحيل التلقائي مع كل شيء نيابةً عنك عند بدء خادم ST واكتشافه لتنسيق التخزين القديم (عن طريق فحص وجود دليل `/public/characters`).

عند نقل أي ملفات، سيتم إنشاء نسخة احتياطية تلقائية في دليل `/backups/_migration/YYYY-MM-DD` (محلل إلى التاريخ الحالي)، ولكن من الجيد دائماً إجراء نسخ احتياطي يدوي كامل قبل تشغيل الترحيل.

#### التثبيتات المحوسبة (Docker)

ترحيل البيانات في وحدات تخزين Docker أصعب قليلاً ولكنه واضح تماماً. بينما تم تحديث `docker-compose.yml` المقدم مع المستودع ليعكس التغييرات، قد تحتاج إلى ضبط سير العمل/النشر المخصص الخاص بك.

**الخطوة 1.** أنشئ وحدة تخزينية جديدة، وقم بتثبيتها على مسار "/home/node/app/data" داخل الحاوية. لا تزل وحدة التخزين `config`.

```yaml
volumes:
    - "./config:/home/node/app/config"
    - "./data:/home/node/app/data"
```

**الخطوة 2.** انقل كل شيء ما عدا ملف `config.yaml` من وحدة التخزين `config` إلى دليل فرعي `default-user` من وحدة التخزين `data`.

**الخطوة 3.** أعد بناء الحاوية وابدأها.

!!!info ملاحظة
لم تعد هناك حاجة للروابط الرمزية بين دليل `/public` ووحدة تخزين `config` ولم يتم بناؤها في حاوية Docker!
!!!

#### ما الذي يجب ترحيله؟

الملفات والأدلة التالية تخضع لترحيل البيانات. بافتراض التكوين الافتراضي، يتم توفير المسارات قبل وبعد في الجدول أدناه.

| قبل                                    | بعد                                  |
|----------------------------------------|--------------------------------------|
| /secrets.json                          | /data/default-user/secrets.json      |
| /thumbnails                            | /data/default-user/thumbnails        |
| /vectors                               | /data/default-user/vectors           |
| /public/settings.json                  | /data/default-user/settings.json     |
| /public/stats.json                     | /data/default-user/stats.json        |
| /public/assets                         | /data/default-user/assets            |
| /public/backgrounds                    | /data/default-user/backgrounds       |
| /public/characters                     | /data/default-user/characters        |
| /public/chats                          | /data/default-user/chats             |
| /public/context                        | /data/default-user/context           |
| /public/scripts/extensions/third-party | /data/default-user/extensions        |
| /public/group chats                    | /data/default-user/group chats       |
| /public/groups                         | /data/default-user/groups            |
| /public/instruct                       | /data/default-user/instruct          |
| /public/KoboldAI Settings              | /data/default-user/KoboldAI Settings |
| /public/movingUI                       | /data/default-user/movingUI          |
| /public/NovelAI Settings               | /data/default-user/NovelAI Settings  |
| /public/OpenAI Settings                | /data/default-user/OpenAI Settings   |
| /public/QuickReplies                   | /data/default-user/QuickReplies      |
| /public/TextGen Settings               | /data/default-user/TextGen Settings  |
| /public/themes                         | /data/default-user/themes            |
| /public/worlds                         | /data/default-user/worlds            |
| /default/content/content.log           | /data/default-user/content.log       |

## المستخدمون

يضيف 1.12.0 القدرة (الاختيارية تماماً) على إنشاء إعداد متعدد المستخدمين على نفس الخادم، مما يسمح لعدة مستخدمين باستخدام مثيلات SillyTavern الخاصة بهم المعزولة تماماً حتى في نفس الوقت. يمكن أيضاً حماية حسابات المستخدمين بكلمة مرور لطبقة إضافية من الخصوصية.

يُرجى الرجوع إلى وثائق [المستخدمين](/Administration/multi-user.md) لمزيد من المعلومات.
