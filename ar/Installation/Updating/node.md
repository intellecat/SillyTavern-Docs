---
order: -50
route: /installation/updating/node/
---

# كيفية تحديث Node.js

من المهم الحفاظ على تحديث بيئة تشغيل Node.js الخاصة بك لأسباب أمنية وأداء. فيما يلي خطوات تحديث Node.js اعتماداً على نظام التشغيل الخاص بك.

نوصي باستخدام أحدث إصدار دعم طويل الأمد (LTS)، والذي يمكنك العثور عليه على [الموقع الرسمي لـ Node.js](https://nodejs.org/en/about/previous-releases).

## كيفية التحقق من إصدار Node.js الحالي الخاص بك

1. افتح terminal أو موجه الأوامر الخاص بك.
2. اكتب الأمر التالي واضغط على Enter:

```bash
node -v
```

## nvm (Node Version Manager) - متعدد المنصات

إذا كنت تستخدم `nvm`:

1. افتح terminal الخاص بك.
2. اكتب الأمر التالي:

[**Unix/Linux/macOS:**](https://github.com/nvm-sh/nvm)

```bash
nvm install --lts
nvm use --lts
```

[**Windows:**](https://github.com/coreybutler/nvm-windows)

```bash
nvm install lts
nvm use lts
```

## Windows - التثبيت العادي

1. انتقل إلى [صفحة تنزيل Node.js](https://nodejs.org/en/download/).
2. قم بتنزيل مثبت Windows لإصدار LTS.
3. قم بتشغيل المثبت واتبع التعليمات لإكمال التثبيت.

## Windows - SillyTavern Launcher

إذا قمت بالتثبيت باستخدام SillyTavern Launcher:

1. افتح SillyTavern Launcher.
2. انتقل إلى `Toolbox / App Installer / Core Utilities / Install Node.js`.

**أو:**

قم بذلك يدوياً باستخدام winget في PowerShell:

```powershell
winget install --id=OpenJS.NodeJS.LTS  -e
```

## Android - Termux

1. افتح تطبيق Termux.
2. اكتب الأوامر التالية:

```bash
pkg update
pkg upgrade nodejs-lts
```

لا تنس قبول أي مطالبات قد تظهر أثناء عملية التحديث بالضغط على `Y` على لوحة المفاتيح الافتراضية.

## macOS - التثبيت العادي

1. انتقل إلى [صفحة تنزيل Node.js](https://nodejs.org/en/download/).
2. قم بتنزيل مثبت macOS لإصدار LTS.
3. قم بتشغيل ملف `.pkg` واتبع التعليمات لإكمال التثبيت.

## macOS - Homebrew

إذا كان لديك Homebrew مثبتاً، يمكنك تحديث Node.js باستخدام الأوامر التالية:

```bash
brew update
brew upgrade node
```

## Linux - مدير الحزم

تعتمد طريقة تحديث Node.js على Linux على توزيعتك.

لكن نظراً لأن إصدار Node.js في المستودعات الرسمية قد لا يكون الأحدث، نوصي باستخدام [Node Version Manager (nvm)](https://github.com/nvm-sh/nvm) أو [مستودع NodeSource](https://github.com/nodesource/distributions).

## Docker

لا حاجة لاتخاذ إجراء. صورة Docker المسبقة البناء التي نوفرها تم تجميعها مع أحدث إصدار من Node.js.
