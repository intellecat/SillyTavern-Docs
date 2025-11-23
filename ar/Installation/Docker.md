---
# icon: container
label: Docker
route: /installation/docker/
---

# تثبيت Docker

!!!
تفترض هذه التعليمات أنك قمت بتثبيت Docker، وأنك قادر على الوصول إلى سطر الأوامر لتثبيت الحاويات، ومعتاد على عملها العام.
!!!

## استخدام GitHub Container Registry

استخدام صورة مسبقة البناء هو أسرع وأسهل طريقة للبدء مع SillyTavern في Docker. يمكنك سحب أحدث صورة من GitHub Container Registry.

### Docker Compose (موصى به)

قم بتنزيل ملف `docker-compose.yml` من [مستودع GitHub](https://github.com/SillyTavern/SillyTavern/blob/release/docker/docker-compose.yml) وقم بتشغيل الأمر التالي في الدليل الذي يوجد فيه الملف. سيؤدي هذا إلى سحب أحدث صورة إصدار من GitHub Container Registry وبدء الحاوية، مع إنشاء الوحدات التخزينية اللازمة تلقائياً.

```sh
docker compose up
```

يمكنك تحرير الملف وتطبيق تخصيصات إضافية لتناسب احتياجاتك:

- المنفذ الافتراضي هو 8000. يمكنك تغييره عن طريق تعديل قسم `ports`.
- قم بتغيير وسم `image` إلى `staging` إذا كنت تريد استخدام فرع التطوير بدلاً من الإصدار المستقر.
- إذا كنت تريد ضبط تكوين الخادم باستخدام متغيرات البيئة، راجع صفحة [متغيرات البيئة](/Administration/config-yaml.md#environment-variables).

### Docker CLI (متقدم)

ستحتاج إلى تعيينين إلزاميين للدليل وتعيين منفذ للسماح لـ SillyTavern بالعمل. في الأمر، استبدل اختياراتك في الأماكن التالية:

#### متغيرات الحاوية

##### تعيينات الوحدات التخزينية

- `CONFIG_PATH` - الدليل الذي سيتم تخزين ملفات تكوين SillyTavern فيه على جهاز المضيف الخاص بك
- `DATA_PATH` - الدليل الذي سيتم تخزين بيانات مستخدم SillyTavern فيه (بما في ذلك الشخصيات) على جهاز المضيف الخاص بك
- `PLUGINS_PATH` - (اختياري) الدليل الذي سيتم تخزين إضافات خادم SillyTavern فيه على جهاز المضيف الخاص بك
- `EXTENSIONS_PATH` - (اختياري) الدليل الذي سيتم تخزين امتدادات واجهة المستخدم العامة فيه على جهاز المضيف الخاص بك

##### تعيينات المنافذ

- `PUBLIC_PORT` - المنفذ لعرض حركة المرور عليه. هذا إلزامي، حيث ستصل إلى المثيل من خارج حاوية الجهاز الافتراضية الخاصة به. لا تعرض هذا على الإنترنت بدون تنفيذ خدمة منفصلة للأمان.

##### إعدادات إضافية

- `SILLYTAVERN_VERSION` - في [صفحة GitHub Packages](https://github.com/SillyTavern/SillyTavern/pkgs/container/sillytavern) سترى قائمة بإصدارات الصور المميزة. وسم الصورة "latest" سيبقيك محدثاً بالإصدار الحالي. يمكنك أيضاً استخدام "staging" الذي يشير إلى الصورة الليلية للفرع المعني.

#### تشغيل الحاوية

1. افتح سطر الأوامر الخاص بك
2. قم بتشغيل الأمر التالي في مجلد حيث تريد تخزين ملفات التكوين والبيانات:

```bash
SILLYTAVERN_VERSION="latest"
PUBLIC_PORT="8000"
CONFIG_PATH="./config"
DATA_PATH="./data"
PLUGINS_PATH="./plugins"
EXTENSIONS_PATH="./extensions"

docker run \
  --name="sillytavern" \
  -p "$PUBLIC_PORT:8000/tcp" \
  -v "$CONFIG_PATH:/home/node/app/config:rw" \
  -v "$DATA_PATH:/home/node/app/data:rw" \
  -v "$EXTENSIONS_PATH:/home/node/app/public/scripts/extensions/third-party:rw" \
  -v "$PLUGINS_PATH:/home/node/app/plugins:rw" \
  ghcr.io/sillytavern/sillytavern:"$SILLYTAVERN_VERSION"
```

!!!tip
بشكل افتراضي، ستعمل الحاوية في المقدمة. إذا كنت تريد تشغيلها في الخلفية، أضف علامة `-d` إلى أمر `docker run`.
!!!

## بناء صورة Docker

!!!info
يفترض القسم التالي أنك قمت بتثبيت SillyTavern في مجلد غير جذر (غير مسؤول). إذا قمت بتثبيت SillyTavern في مجلد جذر، فقد تحتاج إلى تشغيل بعض هذه الأوامر بصلاحيات المسؤول [`sudo`، `doas`، Command Prompt (Administrator)].
!!!

إذا كنت تريد بناء صورة Docker بنفسك، يمكنك القيام بذلك باتباع هذه الخطوات. هذا مفيد إذا كنت تريد تخصيص الصورة أو استخدامها لأغراض التطوير.

### Linux

1. قم بتثبيت Docker باتباع دليل تثبيت Docker [هنا](https://docs.docker.com/engine/install/).
   !!!danger
   **لا** تقم بتثبيت Docker Desktop.
   !!!
2. اتبع الخطوات في **إدارة Docker كمستخدم غير جذر** في [دليل ما بعد التثبيت](https://docs.docker.com/engine/install/linux-postinstall/) الخاص بـ Docker.
3. قم بتثبيت [Git](https://git-scm.com/download/linux) باستخدام مدير الحزم الخاص بك.

    - Debian (Ubuntu/Pop! OS/إلخ.)

        ```sh
        sudo apt install git
        ```

    - Arch Linux (Manjaro/EndeavourOS/إلخ.)

        ```sh
        sudo pacman -S git
        ```

    - Fedora، Red Hat Enterprise Linux (RHEL)، إلخ.
        ```sh
        sudo dnf install git
        ```

4. استنسخ مستودع SillyTavern.

    - Release (الفرع المستقر)

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    - Staging (فرع التطوير)
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

5. قم بتنفيذ `docker compose` عن طريق تشغيل الأمر التالي داخل مجلد Docker.

    ```sh
    docker compose up -d
    ```

6. افتح متصفحاً جديداً وانتقل إلى [http://localhost:8000](http://localhost:8000). يجب أن ترى SillyTavern يتم تحميله في بضع لحظات.

### Windows

!!!warning بخصوص Docker على Windows
استخدام Docker على Windows **_معقد حقاً_**. لا تحتاج فقط إلى تفعيل _Windows Subsystem for Linux_ ضمن _Turn Windows features on or off_، ولكن أيضاً تكوين نظامك للمحاكاة الافتراضية (Intel VT-d/AMD SVM) والتي تختلف من مصنع PC إلى مصنع PC (أو مصنع اللوحة الأم). في بعض الأحيان، لا يتوفر هذا الخيار في بعض الأنظمة.

يُنصح بشدة بتثبيت SillyTavern باتباع دليل [Windows](/Installation/Windows.md) الخاص بنا. هذا القسم هو فكرة _تقريبية_ عن كيفية القيام بذلك على Windows.
!!!

1.  قم بتثبيت Docker Desktop باتباع دليل تثبيت Docker [هنا](https://docs.docker.com/desktop/setup/install/windows-install/).
2.  قم بتثبيت [Git for Windows](https://git-scm.com/download/win).
3.  استنسخ مستودع SillyTavern.

    -   Release (الفرع المستقر)

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    -   Staging (فرع التطوير)
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

4.  قم بتنفيذ `docker compose` عن طريق تشغيل الأمر التالي داخل مجلد Docker.

    ```sh
    docker compose up -d
    ```

5.  افتح متصفحاً جديداً وانتقل إلى [http://localhost:8000](http://localhost:8000). يجب أن ترى SillyTavern يتم تحميله في بضع لحظات.

### macOS

!!!
على الرغم من أن macOS مشابه لـ Linux، إلا أنه لا يحتوي على Docker Engine. سيتعين عليك تثبيت Docker Desktop بشكل مشابه لـ Windows.
ستحتاج أيضاً إلى تثبيت [Homebrew](https://brew.sh/) من أجل تثبيت Git على جهاز Mac الخاص بك. هذا القسم هو فكرة _تقريبية_ عن كيفية القيام بذلك على macOS.
!!!

1.  قم بتثبيت Docker Desktop باتباع دليل تثبيت Docker [هنا](https://docs.docker.com/desktop/setup/install/mac-install/).
2.  قم بتثبيت `git` باستخدام Homebrew.

    ```sh
    brew install git
    ```

3.  استنسخ مستودع SillyTavern.

    -   Release (الفرع المستقر)

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    -   Staging (فرع التطوير)
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

4.  قم بتنفيذ `docker compose` عن طريق تشغيل الأمر التالي داخل مجلد Docker.

    ```sh
    docker compose up -d
    ```

5.  افتح متصفحاً جديداً وانتقل إلى [http://localhost:8000](http://localhost:8000). يجب أن ترى SillyTavern يتم تحميله في بضع لحظات.

## تكوين SillyTavern

سيتواجد ملف تكوين SillyTavern (config.yaml) داخل مجلد `config`. لا يجب أن يختلف تكوين ملف التكوين عن تكوينه بدون Docker، ولكن ستحتاج إلى تشغيل `nano` أو محرر أكواد بصلاحيات المسؤول لحفظ التغييرات.

!!!warning
لا تنس إعادة تشغيل حاوية Docker الخاصة بـ SillyTavern من أجل تطبيق التغييرات! تأكد من تنفيذ هذا الأمر داخل مجلد `docker`.

```sh
docker compose restart sillytavern
```

!!!

## تحديد موقع بيانات المستخدم

سيكون مجلد بيانات SillyTavern داخل مجلد `data`. يجب أن يكون من السهل إجراء نسخ احتياطي لملفاتك، ومع ذلك، قد يتطلب الاستعادة أو إضافة محتوى فيه القيام بذلك بصلاحيات المسؤول.

## تشغيل إضافات الخادم

تشغيل الإضافات مثل [HoYoWiki-Scraper-TS](https://github.com/Bronya-Rand/HoYoWiki-Scraper-TS) أو [SillyTavern-Fandom-Scraper](https://github.com/SillyTavern/SillyTavern-Fandom-Scraper) داخل Docker لا يختلف عن تشغيله على نظامك بدون Docker، ولكن سنحتاج إلى إجراء تعديل طفيف على سكريبت Docker Compose من أجل القيام بذلك.

!!! ملاحظة
إذا كنت ترى بالفعل مجلد _plugins_ داخل مجلد `docker`، يمكنك تخطي الخطوات 1-2.
!!!

1. باستخدام `nano` أو محرر أكواد، افتح _docker-compose.yml_ وأضف السطر التالي أسفل `volumes`.

    ```sh
        volumes:
            - "./config:/home/node/app/config"
            - "./data:/home/node/app/data"
            - "./plugins:/home/node/app/plugins"
    ```

2. أنشئ مجلداً جديداً داخل مجلد `docker` يسمى _plugins_.
3. اتبع تعليمات الإضافة الخاصة بك لتثبيت الإضافة.
4. باستخدام `nano` أو محرر أكواد بصلاحيات المسؤول، افتح _config.yaml_ (داخل مجلد `config`) وفعّل `enableServerPlugins`

    ```sh
    enableServerPlugins: true
    ```

5. أعد تشغيل حاوية Docker.

    ```sh
    docker compose restart sillytavern
    ```

## المشاكل الشائعة مع Docker

### مشاكل أذونات SELinux مع الوحدات التخزينية المثبتة

قد تمنع توزيعات Linux التي تم تمكين SELinux فيها (مثل RHEL وCentOS وFedora وما إلى ذلك) حاويات Docker من الوصول إلى الوحدات التخزينية المثبتة بسبب سياسات الأمان. يمكن أن يؤدي هذا إلى أخطاء رفض الإذن عندما تحاول الحاوية القراءة أو الكتابة إلى الأدلة المثبتة.

يمكن إضافة لاحقتين `:z` أو `:Z` إلى تثبيت الوحدة التخزينية. تخبر هذه اللواحق Docker بإعادة وضع علامات على كائنات الملفات على الوحدات التخزينية المشتركة.

- يتم استخدام خيار `z` عندما سيتم مشاركة محتوى الوحدة التخزينية بين الحاويات.
- يتم استخدام خيار `Z` عندما يجب استخدام محتوى الوحدة التخزينية بواسطة الحاوية الحالية فقط.

مثال:

```yaml
# docker-compose.yml
volumes:
  ## وحدة تخزينية مشتركة
  - ./config:/home/node/app/config:z
  ## وحدة تخزينية خاصة
  - ./data:/home/node/app/data:Z
```

### محظور بواسطة القائمة البيضاء

!!!
يجب أن تتم إضافة عناوين IP الخاصة ببوابة Docker تلقائياً إلى القائمة البيضاء إذا تم تعيين قيمة تكوين [whitelistDockerHosts](/Administration/config-yaml.md#ip-whitelisting) على `true`.

إذا كنت لا تزال غير قادر على الوصول إلى SillyTavern، فاتبع التعليمات أدناه لتحديث القائمة البيضاء يدوياً.
!!!

1. قم بتنفيذ أمر Docker التالي للحصول على IP الخاص بحاوية SillyTavern Docker الخاصة بك.

    ```sh
    docker network inspect docker_default
    ```

    يجب أن تتلقى نوعاً من المخرجات مشابهة لما يلي أدناه.

    ```json
    [
        {
            "Name": "docker_default",
            "IPAM": {
                "Config": [
                    {
                        "Subnet": "172.18.0.0/16",
                        "Gateway": "172.18.0.1"
                    }
                ]
            }
        }
    ]
    ```

    انسخ عنوان IP الذي تراه في _Gateway_ لأن هذا سيكون مهماً.

2. باستخدام محرر نصوص من اختيارك بصلاحيات المسؤول، انتقل إلى `config` وافتح `config.yaml`.

    داخل محررك، انتقل لأسفل إلى قسم `whitelist`. يجب أن ترى شيئاً مشابهاً لما يلي أدناه.

    ```yaml
    whitelist:
        - 127.0.0.1
    ```

    أضف سطراً جديداً أسفل _127.0.0.1_ وضع عنوان IP الذي نسخته من Docker. يجب أن يبدو شيئاً مشابهاً لما يلي بعد ذلك.

    ```yaml
    whitelist:
        - 127.0.0.1
        - 172.18.0.1
    ```

    احفظ الملف واخرج من محرر النصوص.

    !!!info
    لاحظ أنه إذا قمت بتكوين شبكة Docker كجسر، يمكنك أيضاً إضافة عناوين IP الخارجية إلى القائمة البيضاء كالمعتاد.
    !!!

3. أعد تشغيل حاوية Docker لتطبيق التكوين الجديد.

    ```sh
    docker compose restart sillytavern
    ```
