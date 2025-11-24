---
icon: gear
label: التثبيت المحلي
route: /extensions/extras/installation/
---

# تثبيت Extras

تحتوي هذه الصفحة على تعليمات لتثبيت SillyTavern Extras على جهازك المحلي.

!!! متوقف
تم إيقاف مشروع Extras في أبريل 2024 ولن يتلقى أي تحديثات أو وحدات جديدة. الغالبية العظمى من الوحدات متاحة بشكل أصلي في تطبيق SillyTavern الرئيسي. لا يزال بإمكانك تثبيته واستخدامه ولكن لا تتوقع الحصول على دعم فوري إذا واجهت أي مشاكل.
!!!

يمكن أن يكون التثبيت المحلي لـ Extras صعبًا أو مستحيلًا على نظام التشغيل الخاص بك (خاصة Termux).

## استخدم [Official Extras Colab](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb) الرسمي

* سهل الإعداد
* مجاني الاستخدام
* لا يتطلب أرصدة GPU من Colab (استخدم خيارات `use_cpu`)
* راجع [صفحة دليل Colab](/extensions/Extras/Installation.md#running-extras-in-colab) للحصول على التفاصيل.

### تشغيل Extras في Colab

* افتح [Official Extras Colab](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb) الرسمي
* حدد خيارات "Extra" المطلوبة
* حدد `use_cpu` لتشغيل Extras بدون الحاجة إلى رصيد GPU
  * سيؤدي هذا إلى جعل Stable Diffusion أبطأ، ولكن كل شيء آخر سيعمل بشكل طبيعي
* غير مطلوب، ولكن موصى به: حدد خيار `secure` لإنشاء مفتاح API لحماية مثيلك المشترك.
* انقر فوق زر Start على اليسار (يبدو مثل زر 'تشغيل' مثلث)
* انتظر حتى ينتهي من تحميل كل شيء
* ابحث عن رابط `trycloudflare.com` في أسفل المخرجات. تجاهل رابط localhost، لن يعمل (لقد حاولنا!).
* سيبدأ بالنص `Running on`
* انسخ رابط API URL المدرج أسفل هذا السطر. (**لا تنسخ عنوان URL 'localhost'، استخدم العنوان الآخر**)
* ابدأ SillyTavern مع دعم الإضافات: (قم بتعيين `enableExtensions` إلى `true` في ملف `config.yaml` الخاص بك إذا لزم الأمر)
* انتقل إلى قائمة Extensions في SillyTavern (انقر فوق أيقونة 'الكتل المكدسة' في أعلى الصفحة).
* الصق رابط API URL في المربع في الأعلى. (**وليس في مربع API Key**)
* إذا لم تقم بتمكين خيار `secure`، تأكد من أن مربع API Key فارغ تمامًا عند استخدام colab الرسمي.
* إذا قمت بتمكين خيار `secure`، الصق مفتاح API الذي تم إنشاؤه في مربع API Key.
* سيظهر مفتاح API في مخرجات وحدة التحكم في colab، على سبيل المثال: `Your API key is fee2f3f559`
* انقر فوق "Connect"

---

## طرق التثبيت المحلي

### MiniConda (موصى به)

يُوصى بهذه الطريقة لأن Conda تنشئ 'بيئة افتراضية' لحزم متطلبات Extras للعيش فيها، بحيث لا تؤثر على إعداد Python على مستوى النظام.

1. قم بتثبيت [Miniconda](https://docs.conda.io/en/latest/miniconda.html)

    _(مهم!) اقرأ [كيفية استخدام Conda](https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html)_

2. قم بتثبيت [git](https://git-scm.com/downloads)

    _(الأشخاص الذين قاموا بتثبيت SillyTavern باستخدام git في البداية يمكنهم تخطي هذه الخطوة!)_

    بعد تثبيت كليهما...

    اكتب/الصق الأوامر أدناه `واحدًا تلو الآخر` في `نافذة موجه أوامر CONDA` واضغط على `Enter` بعد كل واحد.

3. أنشئ بيئة Conda جديدة (دعنا نسميها `extras`):

    `conda create -n extras`

4. قم بتنشيط البيئة الجديدة

    `conda activate extras` (يجب أن ترى `(extras)` تظهر على الجانب الأيسر من موجه الأوامر الخاص بك)

5. قم بتثبيت حزم النظام المطلوبة (سيستغرق هذا بعض الوقت)

    `conda install python=3.11 git`

6. استنسخ مستودع Extras على GitHub

    `git clone https://github.com/SillyTavern/SillyTavern-extras`

7. انتقل إلى مستودع Extras المستنسخ

    `cd SillyTavern-extras`

8. قم بتثبيت متطلبات Extras باستخدام **واحد** من الأوامر التالية (سيستغرق وقتًا، مرة أخرى):

   * `pip install -r requirements.txt` - للميزات الأساسية
   * `pip install -r requirements-rvc.txt` - لاستنساخ الصوت في الوقت الفعلي
   * `pip install -r requirements-coqui.txt` - لـ Coqui TTS (غير موصى به)

    راجع صفحة [المشاكل الشائعة](/extensions/Extras/Installation.md#extras-install-common-problems) إذا حصلت على أخطاء في هذه الخطوة!

9. انظر أدناه 'تشغيل Extras بعد التثبيت'

---

### التثبيت على مستوى النظام

هذا أسهل، ولكنه سيؤثر على تثبيت Python على مستوى النظام.

يمكن أن يتسبب هذا في تعارضات إذا كنت تعمل مع العديد من برامج Python التي لها متطلبات مختلفة.

إذا كانت هذه هي المرة الأولى التي تتعامل فيها مع أي شيء متعلق بـ Python، فلن يكون ذلك مشكلة.

1. قم بتثبيت Python 3.11: <https://www.python.org/downloads/release/python-3115/>
2. قم بتثبيت git: <https://git-scm.com/downloads>
3. افتح نافذة موجه الأوامر وانتقل إلى مجلد لديك فيه أذونات وصول كاملة.
4. استنسخ المستودع: `git clone https://github.com/SillyTavern/SillyTavern-extras`، اضغط Enter.
5. بعد انتهاء الاستنساخ، اكتب `cd SillyTavern-extras`، اضغط Enter.
6. اكتب `python -m pip install -r requirements.txt`
7. انظر أدناه 'تشغيل Extras بعد التثبيت'

---

## تشغيل Extras بعد التثبيت

### تأكد من تمكين الإضافات

1. افتح الملف المسمى `config.yaml` في محرر نصوص. يقع الملف في مجلد التثبيت الأساسي لـ ST.
2. ابحث عن السطر الذي يحتوي على `enableExtensions`.
3. تأكد من أن هذا السطر يحتوي على `true`، وليس `false`.

### حدد الوحدة التي ستستخدمها

(يجب القيام بذلك مرة واحدة فقط)

* يتم دائمًا بدء Extras باستخدام سطر أوامر Python.
* `python server.py` هو الحد الأدنى، لكنه لا يقوم بتمكين أي وحدات مفيدة.
* لتمكين الوحدات يجب استخدام معدل `--enable-modules=`، مع قائمة مفصولة بفواصل من أسماء الوحدات

مثال: `python server.py --enable-modules=caption,summarize,classify`

سيؤدي هذا إلى تمكين وصف الصور، وملخص الدردشة، وتعبيرات الشخصية المحدثة المباشرة.

فيما يلي جدول يصف كل وحدة.

| الاسم         | الوصف                                                         |
|--------------|---------------------------------------------------------------|
| `caption`    | وصف الصور                                                     |
| `summarize`  | تلخيص النص                                                    |
| `classify`   | تصنيف المشاعر النصية                                          |
| `sd`         | توليد صور Stable Diffusion                                    |
| `silero-tts` | [خادم Silero TTS](https://github.com/ouoertheo/silero-api-server) |
| `edge-tts`   | [عميل Microsoft Edge TTS](https://github.com/rany2/edge-tts)      |
| `chromadb`   | خادم تخزين المتجهات                                           |
| `coqui-tts`  | Coqui TTS                                                     |
| `rvc`        | استنساخ الصوت في الوقت الفعلي                                 |

* حدد الوحدات التي تريد إضافتها إلى سطر أوامر Python الخاص بك.
* سيتم استخدامها في الخطوة التالية.

**ملاحظة: يجب ألا تكون هناك `مسافات على الإطلاق في قائمة وحدات أوامر Python الخاصة بك!`**

### ابدأ خادم Extras

بينما لا تزال في نافذة موجه الأوامر داخل مجلد تثبيت Extras...

1. تأكد من أن بيئة conda نشطة (إذا كنت قد استخدمت طريقة تثبيت Conda)
2. اكتب `activate extras` إذا لم تكن البيئة نشطة.
3. اكتب `python server.py --enable-modules=YOUR,SELECTED,MODULE,LIST,HERE`
4. سيتم تحميل خادم extras.
5. بعد فترة سيعرض لك عنوان URL في النهاية. بالنسبة للتثبيتات المحلية، يكون هذا افتراضيًا `http://localhost:5100`.
6. انسخ رابط API URL.

### اربط ST بخادم Extras

1. ابدأ خادم SillyTavern، واعرض واجهة SillyTavern في متصفحك.
2. افتح لوحة Extensions (عبر أيقونة 'الكتل المكدسة' في أعلى الصفحة)
3. الصق رابط API URL في مربع الإدخال.
4. انقر فوق `Connect`.

لتشغيل Extras مرة أخرى، ما عليك سوى تنشيط البيئة وتشغيل هذه الأوامر في موجه الأوامر.

`conda activate extras`، اضغط Enter.
`python server.py`، اضغط Enter.

تأكد من إضافة الخيارات الإضافية لـ server.py (انظر أدناه) التي يتطلبها إعدادك.

## إنشاء ملف .bat لبدء التشغيل السهل

هذا اختياري ويطبق فقط على Windows، ولكن يجب أن يكون شيء مشابه ممكنًا على MacOS.

1. اعرض سطح مكتب Windows الخاص بك
2. انقر بزر الماوس الأيمن، وحدد `New`، ثم انقر فوق `Text Document`
3. سيظهر ملف جديد على سطح المكتب، يطلب منك اسمًا.
4. قم بتسمية الملف `STExtras.txt`
5. افتح الملف الذي تم إنشاؤه حديثًا في محرر نصوص.
6. الصق الكود التالي فيه:

    ```
    cd C:\_your_\_full_\_Extras_\_folder_\_path_\
    call conda activate extras
    python server.py --enable-modules=YOUR,SELECTED,MODULE,LIST,HERE,WITH,NO,SPACES
    call conda deactivate
    pause
    ```

7. استبدل مسار المجلد المؤقت بمسار مجلد تثبيت Extras الفعلي الخاص بك.
8. استبدل سطر أوامر python بسطر الأوامر الفعلي الخاص بك
9. احفظ الملف باسم جديد `STExtras.bat` (استخدم `File` >> `Save As` في معظم محررات النصوص)

يمكنك الآن ببساطة النقر المزدوج فوق ملف .bat هذا لبدء Extras بسهولة.

إذا كنت تريد تغيير قائمة الوحدات (أو أي معدلات سطر أوامر أخرى لخادم extras)، ما عليك سوى تحرير أمر python داخل ملف .bat.

## مشاكل التثبيت الشائعة لـ Extras

يسرد هذا القسم الأسئلة والمشاكل الشائعة التي تمت مواجهتها أثناء تثبيت SillyTavern Extras.

### خطأ: تعذر استيراد وحدة 'talkinghead' على Linux

يتطلب تثبيت حزمة إضافية لأنها لا يتم تثبيتها تلقائيًا بسبب عدم التوافق مع Colab. قم بتشغيل هذا بعد تثبيت المتطلبات الأخرى:

`pip install wxpython`

### لا يمكن لخادم Extras الاتصال بـ Stable Diffusion Web UI من AUTOMATIC1111

> Could not connect to remote SD backend at <http://127.0.0.1:7860>! Disabling SD module...

**تأكد من أن webui-user.bat الذي تبدأ به Stable Diffusion يحتوي على خيار سطر أوامر --api في متغير COMMANDLINE_ARGS.**

ابحث واستبدل هذا السطر في "webui-user.bat" الخاص بك: `set COMMANDLINE_ARGS=--api`

![كيف يجب أن يبدو](/static/extensions/sd-user.png)

إذا تم تعطيل وضع API لـ SD Web UI، فلن يتمكن خادم Extras من إجراء اتصال ولن تتمكن من توليد الصور!

#### لا يزال لا يعمل؟

تأكد من أنك تبدأ كل شيء بالترتيب الصحيح، في انتظار انتهاء كل برنامج من التحميل قبل الانتقال إلى الخطوة التالية:

1. Stable Diffusion Web UI
2. SillyTavern Extras
3. SillyTavern

لا يمكن لخادم extras إعادة الاتصال بـ Stable Diffusion API إذا تم تحميله بعد ذلك.

### خطأ في بناء عجلة hnswlib عند تثبيت ChromaDB

> ERROR: Could not build wheels for hnswlib, which is required to install pyproject.toml-based projects

قبل تثبيت وحدة ChromaDB يجب عليك أولاً القيام `بواحد مما يلي`:

* قم بتثبيت أدوات بناء Visual C++: <https://visualstudio.microsoft.com/visual-cpp-build-tools/>
* قم بتثبيت حزمة `hnswlib` باستخدام conda: `conda install -c conda-forge hnswlib`

---

### خطأ عند تثبيت متطلبات Python على Mac

> ERROR: No matching distribution found for torch==2.0.0+cu117

لا يدعم Mac CUDA، لذلك يجب تثبيت حزم torch بدون دعم CUDA.

قم بتثبيت المتطلبات باستخدام ملف `requirements-silicon.txt` بدلاً من ذلك.

---

### وحدات مفقودة؟

* يجب عليك تحديد قائمة بأسماء الوحدات في سطر أوامر Python الخاص بك، باستخدام معدل `--enable-modules`.
* راجع قسم [الوحدات](/extensions/Extras/Installation.md#decide-which-module-to-use).

---

### ما الغرض من مربع API Key؟

* يتم استخدام مربع API Key في لوحة Extensions في SillyTavern فقط عندما:
  * قمت بإنشاء ملف نصي باسم `api_key.txt` في مجلد تثبيت Extras، والذي يحتوي على 'كلمة مرور' Extras التي اخترتها.
  * بدأت extras بوسيط سطر الأوامر `--secure`.
* هذا يجعل API Extras 'محمية بكلمة مرور'، بحيث يمكن فقط للمستخدمين الذين لديهم هذا المفتاح في مربع API Key الخاص بهم الوصول إليه.
* هذا مفيد بشكل أساسي للأشخاص الذين يريدون إنشاء نشر عام خاص بهم لـ Extras (colab، إلخ).
* لا ينبغي للمستخدمين الذين يقومون بتشغيل Extras على أجهزة الكمبيوتر الخاصة بهم للاستخدام الشخصي كتابة أي شيء في مربع API Key.

### ماذا عن الهاتف المحمول/Android/Termux؟ 🤔

* هناك بعض الأشخاص في المجتمع حققوا نجاحًا في تشغيل Extras على هواتفهم عبر Ubuntu على Termux.
* ومع ذلك، لم يتم إنشاء Extras مع وضع دعم الأجهزة المحمولة في الاعتبار.
* لن يتم توفير دعم للأشخاص الذين يقومون بتشغيل Extras على أجهزة Android الخاصة بهم.
* وجه جميع أسئلتك إلى منشئ الدليل المرتبط أدناه بدلاً من ذلك.

#### ❗ هذا غير مدعوم

<https://rentry.org/STAI-Termux#downloading-and-running-tai-extras>
