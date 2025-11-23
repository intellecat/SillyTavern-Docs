---
route: /extensions/rvc/
---

# Retrieval-based Voice Conversion (RVC)

سيرشدك هذا الدليل خلال استخدام RVC، وهي تقنية تسمح بنقل ميزات الصوت من مقطع صوتي إلى آخر، مما يمكّن الأصوات من التحدث بنبرات وأساليب مختلفة.

هل استمتعت بمقاطع الفيديو الشهيرة تلك "Presidents Play X"؟ تم إنشاؤها باستخدام RVC. مع إضافة RVC، يمكنك جعل شخصيات SillyTavern الخاصة بك تتحدث بأي صوت تريده، سواء كان أنمي أو فيلماً أو حتى صوتك الفريد الخاص.

RVC ليس TTS: إنه أشبه بتحويل الكلام إلى كلام. يأخذ مقطعاً صوتياً كمدخل. في الخلفية، ما يفعله RVC هو العمل جنباً إلى جنب مع إضافة TTS في SillyTavern: ينتظر TTS لإنشاء ملف صوتي (والذي كان TTS سيفعله بغض النظر عما إذا كنت تستخدم RVC أم لا)، ثم سيقوم RVC بإجراء مرور ثانٍ يأخذ ملف TTS الصوتي ويحوله إلى الصوت المستنسخ من تكوين RVC الخاص بك.

## إعداد RVC

يدعم RVC في SillyTavern عدة مصادر API التي تقوم بتحويل الصوت:

* [rvc-python](https://github.com/daswer123/rvc-python)
* [SillyTavern Extras](https://github.com/SillyTavern/SillyTavern-Extras) (مهمل)

### المتطلبات الأساسية الشائعة

قبل البدء، تأكد من استيفاء المتطلبات الأساسية التالية.

#### ffmpeg

تأكد من وجود ملف `ffmpeg` الثنائي في متغير بيئة PATH الخاص بك. تُستخدم هذه الأداة لتحويل الصوت الوارد.

**Windows**:

* استخدم Toolbox في برنامج SillyTavern Launcher النصي لتثبيت ffmpeg تلقائياً: <https://github.com/SillyTavern/SillyTavern-Launcher>
* أو قم بتنزيل البناء هنا: <https://www.gyan.dev/ffmpeg/builds/>
* كيفية تعديل متغير PATH: <https://www.architectryan.com/2018/03/17/add-to-the-path-on-windows-10/>
* لاختبار ما إذا كنت قد فعلت الأشياء بشكل صحيح، افتح موجه الأوامر وقم بتشغيل ```ffmpeg```. يجب أن يطبع إصدار ffmpeg والمعلومات.

**Linux**:

قم بتثبيت ffmpeg باستخدام مدير الحزم الخاص بك.

```shell
# Debian/Ubuntu
sudo apt install ffmpeg
# Arch Linux
sudo pacman -S ffmpeg
# Fedora
sudo dnf install ffmpeg
```

**macOS**:

قم بتثبيت ffmpeg باستخدام [Homebrew](https://brew.sh/):

```shell
brew install ffmpeg
```

#### تأكد من تمكين TTS وأنه يعمل

يعتمد RVC على TTS، تحتاج إلى تمكين إضافة TTS. يجب أن يعمل TTS الخاص بك بشكل صحيح بالفعل ويسرد دردشاتك قبل محاولة إضافة RVC إلى المزيج!

يرجى ملاحظة أن:

* محرك System TTS لا يدعم تحويل الصوت على الإطلاق.
* سينتظر Streaming TTS انتهاء دفق الصوت قبل التحويل.

#### تثبيت الإضافة

قم بتثبيت إضافة "RVC" من قائمة "Download Extensions & Assets" في لوحة الإضافات (أيقونة الكتل المكدسة).

#### تمكين RVC في SillyTavern*

في SillyTavern، انتقل إلى **Extensions** > **RVC** وقم بتمكينه.

#### اختيار المصدر

في إعدادات الإضافة، اختر مصدر RVC لاستخدامه. ثم انتقل إلى تعليمات التثبيت الخاصة بالمصدر.

### إعداد rvc-python

#### 1. تثبيت الحزمة

اتبع تعليمات التثبيت من صفحة GitHub: [تثبيت rvc-python](https://github.com/daswer123/rvc-python?tab=readme-ov-file#installation). يُوصى باتباع تعليمات تثبيت CUDA إذا كان لديك GPU من Nvidia.

إذا كنت تواجه مشكلات عند التثبيت على Windows (مثل فشل خطوة بناء fairseq)، فتأكد من تثبيت البرامج التالية على جهاز الكمبيوتر الخاص بك:

* [Windows 10 SDK](https://developer.microsoft.com/en-us/windows/downloads/windows-sdk/)
* [Visual Studio Build Tools 2022](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2022)

#### 2. إعداد النماذج

أنشئ دليلاً لتخزين نماذج RVC. افتراضياً يسمى `rvc_models` ويتم التقاطه من دليلك الحالي عند بدء تشغيل الخادم. كل نموذج هو مجلد فرعي (سيكون اسمه مرئياً في واجهة المستخدم) يجب أن يحتوي على ملفات `.pth` (مطلوب) و`.index` (اختياري).

اقرأ المزيد: [إدارة نموذج rvc-python](https://github.com/daswer123/rvc-python?tab=readme-ov-file#model-management)

#### 3. بدء خادم API

ابدأ خادم API عن طريق تشغيل الأمر التالي:

```shell
python -m rvc_python api -p 5050 -l -md models_path
```

**الوسيطات:**

* `5050` - يعين منفذ استماع للخادم. قم بالتغيير إذا كنت تريد الاستضافة على منفذ مختلف.
* `models_path` - يعين مساراً للنماذج. قم بإزالته إذا كنت تريد استخدام دليل `rvc_models` الافتراضي.
* `-l` - يعين الخادم للاستماع على جميع واجهات الشبكة. قم بالإزالة للاستماع فقط على localhost.

#### 4. الاتصال بالخادم

* في إعدادات إضافة RVC، قم بتعيين **rvc-python API URL** المناسب. افتراضياً، سيكون `http://localhost:5050`.
* حدد مربع الاختيار **Use CUDA** إذا قمت بتثبيت rvc-python لدعم تسريع CUDA.
* اضغط على "Refresh" لتحميل قائمة بالأصوات المتاحة.

#### 5. تكوين خريطة الصوت

**تحدد خريطة الصوت إعدادات تحويل الصوت لكل شخصية أو شخصية مستخدم.**

* لإعداد خريطة صوت، اختر اسم شخصيتك أو شخصيتك من القائمة المنسدلة "Character"، ثم اختر "Voice" RVC، ثم انقر على Apply.
* اختيارياً، يمكنك أيضاً تكوين إعدادات أخرى ذات صلة مثل تصحيح طبقة الصوت أو الترشيح.
* إذا قمت بكل شيء بشكل صحيح، فستعرض منطقة تصحيح Voice Map شيئاً مثل 'Betty:MyVoice(rvpme)'.

### إعداد SillyTavern Extras

#### 1. إعداد ملفات نموذج RVC

* في متصفح الملفات، انتقل إلى: `\SillyTavern-extras\data\models\rvc`.
* أنشئ مجلداً فرعياً مثل 'Betty' وضع ملفات `.pth` و`.index` فيه. (تلميح: يمكنك تنزيل ملفات الصوت من https://voice-models.com، تأكد من أن اسم الصوت يقول إنه RVPME.)

#### 2. تثبيت المتطلبات

قم بتثبيت المتطلبات الضرورية باستخدام الأمر:

```shell
pip install -r requirements-rvc.txt`
```

#### 3. تشغيل SillyTavern-extras مع تمكين RVC

قم بتشغيل SillyTavern-extras مع تمكين وحدة RVC. يفترض مثال الاستدعاء هذا أنك استخدمت Edge TTS الذي يأتي مثبتاً مسبقاً مع SillyTavern-extras:

```shell
python server.py --enable-modules=rvc,edge-tts
```

اختيارياً، قد ترغب في تشغيل RVC على GPU الخاص بك إذا كان لديك واحد قادر، عن طريق إضافة ```--cuda``` إلى أمر بدء التشغيل. بناءً على اختبار سريع، كان استخدام VRAM 3.4GB لسرد 50 رمزاً (~36 كلمة)، و7.6GB لـ 200 رمز (~150 كلمة).

#### 4. إعداد تعيين الصوت

أنشئ خريطة صوت لـ RVC. قم بتعيين شخصيتك على اسم شخصية SillyTavern المطلوب، وقم بتعيين Voice على مجلد RVC الذي أنشأته في الخطوة 1، ثم انقر على Apply. إذا قمت بالأشياء بشكل صحيح، فستعرض Voice Map شيئاً مثل 'Betty:MyVoice(rvpme)'.

#### 5. تحديد استخراج طبقة الصوت

* اختر "rmvpe" كطريقة استخراج طبقة الصوت.
* إذا كان لديك مشكلة مع "rmvpe" جرب طرقاً أخرى (على سبيل المثال، "harvest" أو "torchcrepe").

#### 6. (اختياري) تكوين RVC لحفظ أجيالك في ملف

إذا كنت ترغب في أغراض الاختبار أو استكشاف الأخطاء وإصلاحها في حفظ صوت RVC المُنشأ، أضف ```--rvc-save-file``` إلى أمر بدء التشغيل الخاص بك. سيحفظ هذا الجيل الأخير تحت `SillyTavern-extras/data/tmp/rvc_output.wav`:

```shell
python server.py --enable-modules=rvc,edge-tts --rvc-save-file
```

#### صوت ديناميكي قائم على التعبير

##### 1. تكوين نماذج RVC

في مجلد نموذج RVC الخاص بك، احصل على ملفات `.pth` و`.index` منفصلة لكل تعبير مصنف (على سبيل المثال، anger، fear، joy، love، sadness، surprise).

##### 2. تمكين الوحدات

قم بتمكين كل من وحدات RVC و classify:

```shell
python server.py --enable-modules=rvc,classify
```

##### 3. استخدام وحدة RVC

الإعداد المتبقي مشابه لاستخدام وحدة RVC وحدها (كما هو موضح أعلاه).

## تدريب نموذج RVC الخاص بك

### استخدام RVC Easy Menu بواسطة Deffcolony (Windows فقط)

تثبيت وتشغيل Mangio-RVC تلقائياً: https://github.com/deffcolony/rvc-easy-menu

#### 1. استنساخ المستودع

استنسخ المستودع إلى الموقع المطلوب:

```shell
git clone https://github.com/deffcolony/rvc-easy-menu.git
```

#### 2. تشغيل RVC-Launcher.bat

* افتح ملف `RVC-Launcher.bat`.
* اختر الخيار 1 لتثبيت RVC.

#### 3. إكمال التثبيت

عندما يُطلب منك ذلك، قم بتثبيت الحزم والتبعيات المطلوبة.

#### 4. فتح WebUI لتدريب الصوت

بعد التثبيت، اختر الخيار 2 لفتح WebUI لتدريب الصوت.

### Mangio-RVC: تدريب نموذج صوت

***إعداد مجموعة البيانات***:

**1. إعداد الصوت**:

* ضع الصوت الذي تريد تدريبه في مجلد `datasets`.
* تأكد من أن الصوت خالٍ من ضوضاء الخلفية - يلزم فقط الصوت الخام.
* الصوت الأطول يجعل جودة الإخراج أفضل.

***تدريب WebUI***:

**1. الوصول إلى علامة تبويب التدريب**:

* انقر على علامة تبويب التدريب في WebUI.

**2. تكوين التجربة**:

* أدخل اسم تجربة (على سبيل المثال، `my-epic-voice-model`).
* قم بتعيين الإصدار إلى v2.

**3. معالجة البيانات واستخراج الميزات**:

* انقر على "Process data" و"Feature extraction".
* قم بتعيين "Save frequency" إلى 50.

**4. معاملات التدريب**:

* قم بتعيين "Total training epochs" إلى 300.
* انقر على "Train feature index" و"Train model".
