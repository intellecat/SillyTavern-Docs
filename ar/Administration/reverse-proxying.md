---
label: الوكيل العكسي
order: -50
icon: server
route: /usage/st-reverse-proxy-guide/
---

!!!danger ملاحظة
هذا القسم **لا** يشير إلى وكلاء OpenAI/Claude العكسيين. يشير هذا حصريًا إلى **وكلاء HTTP/HTTPS العكسية**.
!!!

هل إعداد Termux محير؟ هل سئمت من التحديث وتثبيت ST على كل جهاز لديك؟ تريد تنظيم دردشاتك وشخصياتك؟ حسنًا، أنت محظوظ. سيغطي هذا الدليل _نأمل_ كيفية استضافة SillyTavern على جهاز الكمبيوتر الخاص بك حيث يمكنك الاتصال من أي مكان والدردشة مع الروبوتات الخاصة بك على نفس الكمبيوتر الذي تستخدمه لتشغيل نماذج AI!

!!!warning تحذير
هذا الدليل **ليس مخصصًا** للمبتدئين. سيكون هذا تقنيًا جدًا.
!!!

## تحذير عادل

!!!info لمستخدمي Windows
هذا الدليل ليس لمستخدمي Windows. نوصي باستخدام جهاز افتراضي Linux أو WSL2 لمتابعة هذا الدليل.
!!!

!!!info لمستخدمي Linux
يجب أن يكون لديك معرفة مسبقة بـ

- أوامر وحدة تحكم Linux
- سجلات DNS
- عناوين IP العامة
- [Docker](https://www.docker.com)

!!!

**سيتعين عليك شراء نطاق لنفسك وتكوين `CNAME` لصفحة SillyTavern الخاصة بك. نقترح إضافة أو شراء النطاق على [Cloudflare](https://www.cloudflare.com) حيث سيغطي هذا الدليل كيفية القيام بذلك مع Cloudflare نفسه.**

## التثبيت

### Linux (SillyTavern على Bare-Metal)

بالنسبة لـ Linux، سنقوم بالوكيل العكسي لـ SillyTavern من خلال [Traefik](https://traefik.io/traefik/). هناك خيارات أخرى مثل _NGINX_ أو _Caddy_، ولكن في هذا الدليل، سنستخدم Traefik لأنه ما نستخدمه نحن أنفسنا.

1. احصل على IP الخاص لجهاز الكمبيوتر الخاص بك باستخدام `ifconfig` أو من جهاز التوجيه الخاص بك.
   !!!info نصيحة
   يوصى بتعيين IP الخاص بك إلى IP ثابت. راجع دليل جهاز التوجيه الخاص بك أو Google لتكوين عناوين IP الثابتة.
   !!!
2. احصل على IP العام للمودم الخاص بك عن طريق البحث في Google عن `what's my ip`.
   !!!info حول عناوين IP العامة
   تستخدم معظم الشبكات السكنية/المنزلية **عناوين IP الديناميكية** التي يتم تجديدها بعد أشهر من الاستخدام. إذا كان لديك IP ديناميكي، استخدم إما DDClient أو تذكر التحقق وتغيير IP العام الخاص بك من حين لآخر على لوحة تحكم Cloudflare.
   !!!
3. قم بتثبيت Docker باتباع دليل تثبيت Docker [هنا](https://docs.docker.com/engine/install/).
   !!!danger ملاحظة
   **لا** تقم بتثبيت Docker Desktop.
   !!!
4. اتبع الخطوات في **إدارة Docker كمستخدم غير جذر** في دليل ما بعد تثبيت Docker [هنا](https://docs.docker.com/engine/install/linux-postinstall/).
5. انتقل إلى المجلد الجذر في Linux وأنشئ مجلدًا جديدًا باسم `docker`.
    ```sh
    cd /
    sudo mkdir docker && cd docker
    ```
6. نفذ `chown`، مستبدلاً _<USER>_ باسم مستخدم Linux الخاص بك لتعيين الأذونات في مجلد docker.
    ```sh
    sudo chown -R <USER>:<USER> .
    ```
7. أنشئ مجلدًا داخل مجلد _docker_، وهو `secrets` وداخل _secrets_ يكون `cloudflare`.
    ```sh
    mkdir secrets && mkdir secrets/cloudflare
    ```
8. أنشئ مجلدًا داخل مجلد _docker_، وهو `appdata` وداخل _appdata_ يكون `traefik`. ادخل إلى مجلد `appdata/traefik` بعد ذلك.
    ```sh
    mkdir appdata && mkdir appdata/traefik
    cd appdata/traefik
    ```
9. أنشئ ملف _acme.json_ باستخدام `touch` وعيّن أذوناته إلى 600.
    ```sh
    touch acme.json
    chmod 600 acme.json
    ```
10. باستخدام `nano` أو محرر مشابه، أنشئ ملفًا باسم _traefik.yml_ والصق ما يلي. استبدل البريد الإلكتروني للقالب بالبريد الإلكتروني الخاص بك، ثم احفظ الملف.
    ```yml
    api:
        dashboard: true
        debug: true
        insecure: true
    entryPoints:
        http:
            address: ":80"
            http:
                redirections:
                    entryPoint:
                        to: https
                        scheme: https
        https:
            address: ":443"
    serversTransport:
        insecureSkipVerify: true
    providers:
        docker:
            endpoint: "unix:///var/run/docker.sock"
            exposedByDefault: false
        file:
            filename: /config.yml
            watch: true
    certificatesResolvers:
        cloudflare:
            acme:
                email: YOUR_CLOUDFLARE_EMAL@DOMAIN.com
                storage: acme.json
                dnsChallenge:
                    provider: cloudflare
                    #disablePropagationCheck: true  # قم بإلغاء التعليق على هذا إذا كانت لديك مشكلات في سحب الشهادات من خلال cloudflare، من خلال تعيين هذه العلامة على true يعطل الحاجة إلى انتظار نشر سجل TXT إلى جميع خوادم الأسماء الموثوقة.
                    resolvers:
                        - "1.1.1.1:53"
                        - "1.0.0.1:53"
    ```
11. ارجع إلى مجلد `docker`.
    ```sh
    cd /docker
    ```
12. باستخدام `nano` أو محرر مشابه، أنشئ ملفًا باسم _docker-compose.yaml_ والصق ما يلي. احفظ الملف بعد ذلك.

    ```yaml
    secrets:
        CF_DNS_API_KEY:
            file: ./secrets/cloudflare/CF_DNS_API_KEY

    services:
        traefik:
            image: traefik:latest
            container_name: traefik
            restart: unless-stopped
            secrets:
                - CF_DNS_API_KEY
            ports:
                - 80:80
                - 443:443
                - 8080:8080
            environment:
                CLOUDFLARE_DNS_API_TOKEN_FILE: /run/secrets/CF_DNS_API_KEY
                CLOUDFLARE_ZONE_API_TOKEN_FILE: /run/secrets/CF_DNS_API_KEY
            volumes:
                - /var/run/docker.sock:/var/run/docker.sock:ro
                - ./appdata/traefik/traefik.yml:/traefik.yml:ro
                - ./appdata/traefik/config.yml:/config.yml:ro
                - ./appdata/traefik/acme.json:/acme.json
                - /etc/localtime:/etc/localtime:ro

    networks:
        internal:
            driver: bridge
    ```

13. سجل الدخول إلى Cloudflare وانقر على نطاقك، متبوعًا بـ **Get your API token**.
14. انقر على _Create Token_ ثم _Create Custom Token_ وتأكد من منح الرمز الخاص بك الأذونات التالية.
    !!!info أذونات الرمز
    **Zone -> DNS -> Edit**

    **Zone -> Zone -> Read**
    !!!

    انقر على _Continue to summary_ متبوعًا بـ _Create Token._

15. انسخ مفتاح الرمز المعطى لك وقم بتخزينه في مكان آمن.
16. `cd` إلى `secrets/cloudflare` وباستخدام `nano` أو محرر مشابه، أنشئ ملفًا باسم **CF_DNS_API_KEY** والصق مفتاحك بداخله.
17. ارجع إلى صفحة نطاقك وانتقل إلى **DNS**. أنشئ سجلاً جديدًا باستخدام **Add record** وأنشئ مفتاحين من نوع _A_ مثل المفاتيح أدناه. استبدل `PUBLIC_IP` بـ IP العام الخاص بك، ثم انقر _Save_.

    | النوع | الاسم (مطلوب) | الهدف (مطلوب) | حالة الوكيل | TTL |
    |------|-----------------|-------------------|--------------|------|
    | A    | DOMAIN.com      | PUBLIC_IP         | Proxied      | Auto |
    | A    | www             | PUBLIC_IP         | Proxied      | Auto |

18. أنشئ سجلاً آخر من نوع **`CNAME`**، ثم انقر _Save_. هذا مثال على كيفية ظهوره على لوحة تحكم Cloudflare.

    | النوع  | الاسم (مطلوب) | الهدف (مطلوب) | حالة الوكيل | TTL |
    |-------|-----------------|-------------------|--------------|-----|
    | CNAME | silly           | DOMAIN.com        | Proxied      | N/A |

19. `cd` إلى _appdata/traefik_ وباستخدام `nano` أو محرر مشابه، أنشئ ملفًا باسم _config.yml_ والصق ما يلي. استبدل `PRIVATE_IP` بـ IP الخاص الذي حصلت عليه، و`silly.DOMAIN.com` باسم النطاق الفرعي وصفحة النطاق الخاصة بك، ثم احفظ الملف.

    ```yml
    http:
        routers:
            sillytavern:
                entryPoints:
                    - "https"
                rule: "Host(`silly.DOMAIN.com`)"
                middlewares:
                    - https-redirectscheme
                tls: {}
                service: sillytavern

        services:
            sillytavern:
                loadBalancer:
                    servers:
                        - url: "http://PRIVATE_IP:8000"
                    passHostHeader: true

        middlewares:
            https-redirectscheme:
                redirectScheme:
                    scheme: https
                    permanent: true
    ```

20. قم بتشغيل Docker Compose باستخدام الأوامر التالية:
    ```sh
    cd /docker
    docker compose up -d
    ```
21. انتقل إلى مجلد SillyTavern الخاص بك وحرر `config.yaml` لتمكين وضع الاستماع والمصادقة الأساسية، مع تعطيل `whitelistMode`.

    ```yaml
    listen: yes
    whitelistMode: false
    basicAuthMode: true
    ```

    !!!warning نصيحة
    تأكد من تغيير اسم المستخدم وكلمة المرور الافتراضيين إلى شيء قوي يمكنك تذكره.
    !!!

    أو لاستخدام حسابات SillyTavern كأسماء مستخدمين وكلمات مرور:

    ```yaml
    basicAuthMode: true
    enableUserAccounts: true
    perUserBasicAuth: true
    ```

    !!!warning نصيحة
    قبل تمكين perUserBasicAuth تأكد من أن لديك إعداد متعدد المستخدمين صالح مع كلمات مرور عاملة.
    !!!

22. انتظر بضع دقائق، ثم افتح صفحة النطاق التي أنشأتها لـ ST. في النهاية، يجب أن تكون قادرًا على فتح SillyTavern من أي مكان تذهب إليه فقط بعنوان URL واحد وحساب واحد.
    !!!info نصيحة
    إذا لم يحدث شيء بعد عدة دقائق، تحقق من سجلات الحاوية لـ Traefik بحثًا عن أي أخطاء محتملة.
    !!!
23. استمتع! :D

### Linux (SillyTavern على Docker)

!!!warning ملاحظة
لاحظ أننا نقوم بتشغيل SillyTavern على bare-metal عبر Docker. هذه فكرة تقريبية عما سنفعله على Docker مع حاويات Docker الأخرى التي نميل إلى استخدامها مع ST.
!!!

1. اتبع الخطوات 1-11 من **Linux (SillyTavern على Bare-Metal)**.
2. سجل الدخول إلى Cloudflare وانقر على نطاقك، متبوعًا بـ **Get your API token**.
3. انقر على _Create Token_ ثم _Create Custom Token_ وتأكد من منح الرمز الخاص بك الأذونات التالية.
   !!!info أذونات الرمز
   **Zone -> DNS -> Edit**

    **Zone -> Zone -> Read**
    !!!

    انقر على _Continue to summary_ متبوعًا بـ _Create Token._

4. انسخ مفتاح الرمز المعطى لك وقم بتخزينه في مكان آمن.
5. `cd` إلى `secrets/cloudflare` وباستخدام `nano` أو محرر مشابه، أنشئ ملفًا باسم **CF_DNS_API_KEY** والصق مفتاحك بداخله.
6. ارجع إلى صفحة نطاقك وانتقل إلى **DNS**. أنشئ سجلاً جديدًا باستخدام **Add record** وأنشئ مفتاحين من نوع _A_ مثل المفاتيح أدناه. استبدل `PUBLIC_IP` بـ IP العام الخاص بك والنطاق المثالي بنطاقك، ثم انقر _Save_.

    | النوع | الاسم (مطلوب) | الهدف (مطلوب) | حالة الوكيل | TTL  |
    |------|-----------------|-------------------|--------------|------|
    | A    | DOMAIN.com      | PUBLIC_IP         | Proxied      | Auto |
    | A    | www             | PUBLIC_IP         | Proxied      | Auto |

7. أنشئ سجلاً آخر من نوع **`CNAME`**، ثم انقر _Save_. هذا مثال على كيفية ظهوره على لوحة تحكم Cloudflare.

    | النوع  | الاسم (مطلوب) | الهدف (مطلوب) | حالة الوكيل | TTL |
    |-------|-----------------|-------------------|--------------|-----|
    | CNAME | silly           | DOMAIN.com        | Proxied      | N/A |

8. استنسخ Git لـ SillyTavern في مجلد `docker`.
    ```sh
    cd /docker && git clone https://github.com/SillyTavern/SillyTavern
    ```
9. باستخدام `nano` أو محرر مشابه، أنشئ ملفًا باسم _docker-compose.yaml_ والصق ما يلي. استبدل `silly.DOMAIN.com` بالنطاق الفرعي الذي أضفته أعلاه، واحفظ الملف بعد ذلك.

    ```yaml
    secrets:
        CF_DNS_API_KEY:
            file: ./secrets/cloudflare/CF_DNS_API_KEY

    services:
        traefik:
            image: traefik:latest
            container_name: traefik
            restart: unless-stopped
            secrets:
                - CF_DNS_API_KEY
            ports:
                - "80:80"
                - 443:443
                - 8080:8080
            environment:
                CLOUDFLARE_DNS_API_TOKEN_FILE: /run/secrets/CF_DNS_API_KEY
                CLOUDFLARE_ZONE_API_TOKEN_FILE: /run/secrets/CF_DNS_API_KEY
            volumes:
                - /var/run/docker.sock:/var/run/docker.sock:ro
                - ./appdata/traefik/traefik.yml:/traefik.yml:ro
                - ./appdata/traefik/config.yml:/config.yml:ro
                - ./appdata/traefik/acme.json:/acme.json
                - /etc/localtime:/etc/localtime:ro
        sillytavern:
            build: ./SillyTavern
            container_name: sillytavern
            hostname: sillytavern
            image: ghcr.io/sillytavern/sillytavern:latest
            volumes:
                - "./appdata/sillytavern/config:/home/node/app/config"
                - "./appdata/sillytavern/data:/home/node/app/data"
            restart: unless-stopped
            labels:
                - "traefik.enable=true"
                - "traefik.http.routers.sillytavern.entrypoints=http"
                - "traefik.http.routers.sillytavern.rule=Host(`silly.DOMAIN.com`)"
                - "traefik.http.middlewares.sillytavern-https-redirect.redirectscheme.scheme=https"
                - "traefik.http.routers.sillytavern.middlewares=sillytavern-https-redirect"
                - "traefik.http.routers.sillytavern-secure.entrypoints=https"
                - "traefik.http.routers.sillytavern-secure.rule=Host(`silly.DOMAIN.com`)"
                - "traefik.http.routers.sillytavern-secure.tls=true"
                - "traefik.http.routers.sillytavern-secure.service=sillytavern"
                - "traefik.http.services.sillytavern.loadbalancer.server.port=8000"

    networks:
        internal:
            driver: bridge
    ```

10. قم بتشغيل Docker Compose باستخدام الأوامر التالية:
    ```sh
    docker compose up -d
    ```
11. أوقف حاوية Docker الخاصة بـ SillyTavern.
    ```sh
    docker compose stop sillytavern
    ```
12. انتقل إلى مجلد SillyTavern الخاص بك (`appdata/sillytavern/config`) وحرر `config.yaml` لتمكين وضع الاستماع والمصادقة الأساسية، مع تعطيل `whitelistMode`.

    ```yaml
    listen: yes
    whitelistMode: false
    basicAuthMode: true
    ```

    !!!warning نصيحة
    تأكد من تغيير اسم المستخدم وكلمة المرور الافتراضيين إلى شيء قوي يمكنك تذكره.
    !!!

13. ابدأ تشغيل حاوية Docker الخاصة بـ SillyTavern مرة أخرى.
    ```sh
    docker compose up -d sillytavern
    ```
14. انتظر بضع دقائق، ثم افتح صفحة النطاق التي أنشأتها لـ ST. في النهاية، يجب أن تكون قادرًا على فتح SillyTavern من أي مكان تذهب إليه فقط بعنوان URL واحد وحساب واحد.
    !!!info نصيحة
    إذا لم يحدث شيء بعد عدة دقائق، تحقق من سجلات الحاوية لـ Traefik بحثًا عن أي أخطاء محتملة.
    !!!
15. استمتع! :D

## تحديث DNS الخاص بـ Cloudflare

يتيح لك [**DDClient**](https://ddclient.net/) مزامنة IP العام الخاص بك إلى Cloudflare في حالة قيام مزود خدمة الإنترنت الخاص بك بتغييره، مما يسمح لك بمواصلة الوصول إلى نسخة ST الخاصة بك كما لو لم يحدث شيء على الإطلاق.
