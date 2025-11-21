---
label: リバースプロキシ
order: -50
icon: server
route: /usage/st-reverse-proxy-guide/
---

!!!danger 注意
このセクションは、OpenAI/Claudeリバースプロキシを指すものでは**ありません**。これは**HTTP/HTTPSリバースプロキシ**のみを指します。
!!!

Termuxのセットアップが混乱していますか? 所有しているすべてのデバイスでSTを更新およびインストールすることに疲れていますか? チャットとキャラクターの整理が必要ですか? あなたは幸運です。このガイドでは、どこからでも接続でき、AIモデルを実行するために使用するのと同じPC上のボットとチャットできるPC上でSillyTavernをホストする方法を_うまくいけば_カバーします!

!!!warning 警告
このガイドは初心者向けでは**ありません**。これは非常に技術的です。
!!!

## 公正な警告

!!!info Windowsユーザー向け
このガイドはWindowsユーザー向けではありません。このガイドに従うには、Linux VMまたはWSL2を使用することをお勧めします。
!!!

!!!info Linuxユーザー向け
次の事前知識が必要です

- Linuxコンソールコマンド
- DNSレコード
- パブリックIPアドレス
- [Docker](https://www.docker.com)

!!!

**自分用のドメインを購入し、SillyTavernページ用の`CNAME`を設定する必要があります。このガイドはCloudflare自体でこれを行う方法をカバーするため、[Cloudflare](https://www.cloudflare.com)でドメインを追加または購入することをお勧めします。**

## インストール

### Linux (Bare-Metal SillyTavern)

Linuxの場合、[Traefik](https://traefik.io/traefik/)を介してSillyTavernをリバースプロキシします。_NGINX_や_Caddy_などの他のオプションもありますが、このガイドでは、私たち自身が使用しているTraefikを使用します。

1. `ifconfig`またはルーターからコンピューターのプライベートIPを取得します。
   !!!info ヒント
   プライベートIPを静的IPに設定することをお勧めします。静的IPを設定するには、ルーターのマニュアルまたはGoogleを参照してください。
   !!!
2. `what's my ip`をGoogleで検索して、モデムのパブリックIPを取得します。
   !!!info パブリックIPについて
   ほとんどの住宅/ホームネットワークは、数か月の使用後に更新される**動的IP**を使用しています。動的IPがある場合は、DDClientを使用するか、CloudflareダッシュボードでパブリックIPを時々確認して変更することを忘れないでください。
   !!!
3. [こちら](https://docs.docker.com/engine/install/)のDockerインストールガイドに従ってDockerをインストールします。
   !!!danger 注意
   Docker Desktopをインストール**しない**でください。
   !!!
4. [こちら](https://docs.docker.com/engine/install/linux-postinstall/)のDockerインストール後ガイドの**Manage Docker as a non-root user**の手順に従います。
5. Linuxのルートフォルダに移動し、`docker`という名前の新しいフォルダを作成します。
    ```sh
    cd /
    sudo mkdir docker && cd docker
    ```
6. `chown`を実行し、_<USER>_をLinuxユーザー名に置き換えて、dockerフォルダの権限を設定します。
    ```sh
    sudo chown -R <USER>:<USER> .
    ```
7. _docker_フォルダ内に`secrets`フォルダを作成し、_secrets_内に`cloudflare`フォルダを作成します。
    ```sh
    mkdir secrets && mkdir secrets/cloudflare
    ```
8. _docker_フォルダ内に`appdata`フォルダを作成し、_appdata_内に`traefik`フォルダを作成します。その後、`appdata/traefik`フォルダに入ります。
    ```sh
    mkdir appdata && mkdir appdata/traefik
    cd appdata/traefik
    ```
9. `touch`を使用して_acme.json_ファイルを作成し、その権限を600に設定します。
    ```sh
    touch acme.json
    chmod 600 acme.json
    ```
10. `nano`または類似のエディタを使用して、_traefik.yml_という名前のファイルを作成し、以下を貼り付けます。テンプレートメールを自分のものに置き換えてから、ファイルを保存します。
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
                    #disablePropagationCheck: true  # uncomment this if you have issues pulling certificates through cloudflare, By setting this flag to true disables the need to wait for the propagation of the TXT record to all authoritative name servers.
                    resolvers:
                        - "1.1.1.1:53"
                        - "1.0.0.1:53"
    ```
11. `docker`フォルダに戻ります。
    ```sh
    cd /docker
    ```
12. `nano`または類似のエディタを使用して、_docker-compose.yaml_という名前のファイルを作成し、以下を貼り付けます。その後、ファイルを保存します。

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

13. Cloudflareにログインし、ドメインをクリックしてから、**Get your API token**をクリックします。
14. _Create Token_をクリックしてから_Create Custom Token_をクリックし、トークンに次の権限を与えてください。
    !!!info トークンの権限
    **Zone -> DNS -> Edit**

    **Zone -> Zone -> Read**
    !!!

    _Continue to summary_をクリックしてから_Create Token_をクリックします。

15. 与えられたトークンキーをコピーし、安全な場所に保存します。
16. `secrets/cloudflare`に`cd`し、`nano`または類似のエディタを使用して、**CF_DNS_API_KEY**という名前のファイルを作成し、その中にキーを貼り付けます。
17. ドメインページに戻り、**DNS**に移動します。**Add record**を使用して新しいレコードを作成し、以下のような2つの_A_タイプキーを作成します。`PUBLIC_IP`を自分のパブリックIPに置き換えてから、_Save_をクリックします。

    | Type | Name (required) | Target (required) | Proxy Status | TTL  |
    |------|-----------------|-------------------|--------------|------|
    | A    | DOMAIN.com      | PUBLIC_IP         | Proxied      | Auto |
    | A    | www             | PUBLIC_IP         | Proxied      | Auto |

18. **`CNAME`**タイプの別のレコードを作成してから、_Save_をクリックします。Cloudflareダッシュボードに表示される方法の例を次に示します。

    | Type  | Name (required) | Target (required) | Proxy Status | TTL |
    |-------|-----------------|-------------------|--------------|-----|
    | CNAME | silly           | DOMAIN.com        | Proxied      | N/A |

19. _appdata/traefik_に`cd`し、`nano`または類似のエディタを使用して、_config.yml_という名前のファイルを作成し、以下を貼り付けます。`PRIVATE_IP`を取得したプライベートIPに、`silly.DOMAIN.com`をサブドメインとドメインページの名前に置き換えてから、ファイルを保存します。

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

20. 次のコマンドを使用してDocker Composeを実行します:
    ```sh
    cd /docker
    docker compose up -d
    ```
21. SillyTavernフォルダに移動し、`config.yaml`を編集してlistenモードとbasic authenticationを有効にし、`whitelistMode`を無効にします。

    ```yaml
    listen: yes
    whitelistMode: false
    basicAuthMode: true
    ```

    !!!warning ヒント
    覚えられる強力なユーザー名とパスワードにデフォルトのユーザー名とパスワードを変更してください。
    !!!

    または、SillyTavernアカウントをユーザー名とパスワードとして使用するには:

    ```yaml
    basicAuthMode: true
    enableUserAccounts: true
    perUserBasicAuth: true
    ```

    !!!warning ヒント
    perUserBasicAuthを有効にする前に、機能するパスワードを持つ有効なマルチユーザー設定があることを確認してください。
    !!!

22. 数分待ってから、ST用に作成したドメインページを開きます。最終的には、1つのURLと1つのアカウントだけで、どこからでもSillyTavernを開くことができるはずです。
    !!!info ヒント
    数分後に何も起こらない場合は、Traefikのコンテナログで可能性のあるエラーを確認してください。
    !!!
23. お楽しみください! :D

### Linux (Docker SillyTavern)

!!!warning 注意
私たちはDocker上ではなく、bare-metal上でSillyTavernを実行していることに注意してください。これは、STで使用する傾向がある他のDockerコンテナでDockerで行うであろうことの大まかなアイデアです。
!!!

1. **Linux (Bare-Metal SillyTavern)**のステップ1-11に従います。
2. Cloudflareにログインし、ドメインをクリックしてから、**Get your API token**をクリックします。
3. _Create Token_をクリックしてから_Create Custom Token_をクリックし、トークンに次の権限を与えてください。
   !!!info トークンの権限
   **Zone -> DNS -> Edit**

    **Zone -> Zone -> Read**
    !!!

    _Continue to summary_をクリックしてから_Create Token_をクリックします。

4. 与えられたトークンキーをコピーし、安全な場所に保存します。
5. `secrets/cloudflare`に`cd`し、`nano`または類似のエディタを使用して、**CF_DNS_API_KEY**という名前のファイルを作成し、その中にキーを貼り付けます。
6. ドメインページに戻り、**DNS**に移動します。**Add record**を使用して新しいレコードを作成し、以下のような2つの_A_タイプキーを作成します。`PUBLIC_IP`を自分のパブリックIPに、例のドメインを自分のドメインに置き換えてから、_Save_をクリックします。

    | Type | Name (required) | Target (required) | Proxy Status | TTL  |
    |------|-----------------|-------------------|--------------|------|
    | A    | DOMAIN.com      | PUBLIC_IP         | Proxied      | Auto |
    | A    | www             | PUBLIC_IP         | Proxied      | Auto |

7. **`CNAME`**タイプの別のレコードを作成してから、_Save_をクリックします。Cloudflareダッシュボードに表示される方法の例を次に示します。

    | Type  | Name (required) | Target (required) | Proxy Status | TTL |
    |-------|-----------------|-------------------|--------------|-----|
    | CNAME | silly           | DOMAIN.com        | Proxied      | N/A |

8. `docker`フォルダにSillyTavernをgit cloneします。
    ```sh
    cd /docker && git clone https://github.com/SillyTavern/SillyTavern
    ```
9. `nano`または類似のエディタを使用して、_docker-compose.yaml_という名前のファイルを作成し、以下を貼り付けます。`silly.DOMAIN.com`を上記で追加したサブドメインに置き換えてから、ファイルを保存します。

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

10. 次のコマンドを使用してDocker Composeを実行します:
    ```sh
    docker compose up -d
    ```
11. SillyTavern Dockerコンテナを停止します。
    ```sh
    docker compose stop sillytavern
    ```
12. SillyTavernフォルダ(`appdata/sillytavern/config`)に移動し、`config.yaml`を編集してlistenモードとbasic authenticationを有効にし、`whitelistMode`を無効にします。

    ```yaml
    listen: yes
    whitelistMode: false
    basicAuthMode: true
    ```

    !!!warning ヒント
    覚えられる強力なユーザー名とパスワードにデフォルトのユーザー名とパスワードを変更してください。
    !!!

13. SillyTavern Dockerコンテナを再度起動します。
    ```sh
    docker compose up -d sillytavern
    ```
14. 数分待ってから、ST用に作成したドメインページを開きます。最終的には、1つのURLと1つのアカウントだけで、どこからでもSillyTavernを開くことができるはずです。
    !!!info ヒント
    数分後に何も起こらない場合は、Traefikのコンテナログで可能性のあるエラーを確認してください。
    !!!
15. お楽しみください! :D

## CloudflareのDNSの更新

[**DDClient**](https://ddclient.net/)を使用すると、ISPがパブリックIPを変更した場合に、パブリックIPをCloudflareに同期できます。これにより、何も起こらなかったかのようにSTインスタンスに引き続きアクセスできます。
