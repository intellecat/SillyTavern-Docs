---
label: MacOS و Linux
order: 5
route: /installation/linuxmacos/
---

# تثبيت Linux/MacOS

## التثبيت اليدوي عبر Git

بالنسبة لـ MacOS / Linux، سيتم تنفيذ كل هذه الخطوات في Terminal.

1. قم بتثبيت git و nodeJS (ستختلف طريقة القيام بذلك اعتماداً على نظام التشغيل الخاص بك)
2. استنسخ المستودع

   - لفرع Release: `git clone https://github.com/SillyTavern/SillyTavern -b release`
   - لفرع Staging: `git clone https://github.com/SillyTavern/SillyTavern -b staging`

3. `cd SillyTavern` للانتقال إلى مجلد التثبيت.
4. قم بتشغيل سكريبت `start.sh` بأحد هذه الأوامر:

- `./start.sh`
- `bash start.sh`

## SillyTavern Launcher

### لمستخدمي Linux
1. افتح terminal المفضل لديك وثبت git
2. قم بتنزيل Sillytavern Launcher باستخدام: `git clone https://github.com/SillyTavern/SillyTavern-Launcher.git`
3. انتقل إلى SillyTavern-Launcher باستخدام: `cd SillyTavern-Launcher`
4. ابدأ تشغيل مشغل التثبيت باستخدام: `chmod +x install.sh && ./install.sh` واختر ما تريد تثبيته
5. بعد التثبيت، ابدأ المشغل باستخدام: `chmod +x launcher.sh && ./launcher.sh`

### لمستخدمي Mac
1. افتح terminal وثبت brew باستخدام: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
2. ثم ثبت git باستخدام: `brew install git`
3. قم بتنزيل Sillytavern Launcher باستخدام: `git clone https://github.com/SillyTavern/SillyTavern-Launcher.git`
4. انتقل إلى SillyTavern-Launcher باستخدام: `cd SillyTavern-Launcher`
5. ابدأ تشغيل مشغل التثبيت باستخدام: `chmod +x install.sh && ./install.sh` واختر ما تريد تثبيته
6. بعد التثبيت، ابدأ المشغل باستخدام: `chmod +x launcher.sh && ./launcher.sh`
