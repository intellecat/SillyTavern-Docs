---
order: 50
icon: package
expanded: true
route: /installation/
---

# التثبيت

اتبع دليل التثبيت المناسب لنظام التشغيل الخاص بك:

* [Windows](/Installation/Windows.md)
* [Linux و Mac](/Installation/LinuxMacOS.md)
* [Android](/Installation/Android.md)
* [Docker](/Installation/Docker.md)

## الفروع

يتم تطوير SillyTavern باستخدام نظام ذو فرعين لضمان تجربة سلسة لجميع المستخدمين.

* `release` -🌟 **موصى به لمعظم المستخدمين.** هذا هو الفرع الأكثر استقراراً والموصى به، ويتم تحديثه فقط عند إصدار الإصدارات الرئيسية. مناسب لغالبية المستخدمين. عادةً ما يتم تحديثه مرة واحدة شهرياً.
* `staging` - ⚠️ **غير موصى به للاستخدام العادي.** يحتوي هذا الفرع على أحدث الميزات، ولكن كن حذراً فقد يتعطل في أي وقت. مخصص للمستخدمين المتقدمين والمهتمين فقط. يتم تحديثه عدة مرات يومياً.

## الوضع العام / المستقل

هناك وضعان لتشغيل SillyTavern يختلفان في كيفية التعامل مع مسارات التكوين والبيانات.

* **الوضع المستقل** (افتراضي) - يستخدم ملف `config.yaml` ومجلد `data` في دليل الخادم. سيتم تقييد جميع البيانات بمسار التثبيت. هذا هو الوضع الموصى به لمعظم المستخدمين.
* **الوضع العام** - يستخدم المسارات على مستوى النظام للتكوين والبيانات. هذا مفيد لتثبيت SillyTavern كحزمة أو عندما تريد مشاركة نفس التكوين والبيانات عبر عدة تثبيتات.

!!!info
التثبيتات التي تتم باستخدام [حزمة npm الرسمية](https://www.npmjs.com/package/sillytavern) (مثل `npx sillytavern@latest`) سيتم تشغيلها في الوضع العام افتراضياً.
!!!

### مسارات البيانات

مسارات **الوضع المستقل** نسبية إلى دليل تثبيت SillyTavern:

* **مسار التكوين**: `./config.yaml`
* **جذر البيانات**: `./data/`

مسارات **الوضع العام** تعتمد على نظام التشغيل:

* **Linux**: `~/.local/share/SillyTavern/config.yaml` (أو `$XDG_DATA_HOME/SillyTavern/config.yaml`) و `~/.local/share/SillyTavern/data/` (أو `$XDG_DATA_HOME/SillyTavern/data/`)
* **Windows**: `%APPDATA%\SillyTavern\config.yaml` و `%APPDATA%\SillyTavern\data\`
* **MacOS**: `~/Library/Application Support/SillyTavern/config.yaml` و `~/Library/Application Support/SillyTavern/data/`

### كيفية التشغيل في الوضع العام

!!!warning
لا يمكن تجاوز `dataRoot` و `configPath` باستخدام [وسائط CLI](../Administration/config-yaml.md#command-line-arguments) أو [config.yaml](../Administration/config-yaml.md) عند التشغيل في الوضع العام.
!!!

1. قم بتمرير وسيط `--global` إلى أمر بدء تشغيل الخادم (مثل `node server.js --global`).
2. قم بتمرير وسيط `--global` إلى سكريبت بدء التشغيل (مثل `Start.bat --global` أو `./start.sh --global`).
3. استخدم سكريبت `start:global` في ملف `package.json` (مثل `npm run start:global`).
