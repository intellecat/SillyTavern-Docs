---
label: 리버스 프록시
order: -50
icon: server
route: /usage/st-reverse-proxy-guide/
---

!!!danger 참고
이 섹션은 OpenAI/Claude 리버스 프록시를 **의미하지 않습니다**. 이것은 **HTTP/HTTPS 리버스 프록시**만을 다룹니다.
!!!

Termux 설정이 혼란스러우신가요? 모든 기기에서 ST를 업데이트하고 설치하는 것에 지치셨나요? 채팅과 캐릭터를 정리하고 싶으신가요? 다행히도 이 가이드는 AI 모델을 실행하는 데 사용하는 동일한 PC에서 어디서나 연결하여 봇과 채팅할 수 있는 PC에서 SillyTavern을 호스팅하는 방법을 _바라건대_ 다룰 것입니다!

!!!warning 경고
이 가이드는 초보자를 위한 것이 **아닙니다**. 매우 기술적입니다.
!!!

## 공정한 경고

!!!info Windows 사용자용
이 가이드는 Windows 사용자를 위한 것이 아닙니다. 이 가이드를 따르려면 Linux VM 또는 WSL2를 사용하는 것이 좋습니다.
!!!

!!!info Linux 사용자용
다음에 대한 사전 지식이 있어야 합니다

- Linux 콘솔 명령
- DNS 레코드
- 공용 IP 주소
- [Docker](https://www.docker.com)

!!!

**자신을 위한 도메인을 구매하고 SillyTavern 페이지에 대한 `CNAME`을 구성해야 합니다. 이 가이드에서는 Cloudflare를 사용하는 방법을 다룰 것이므로 [Cloudflare](https://www.cloudflare.com)에서 도메인을 추가하거나 구매하는 것을 권장합니다.**

## 설치

### Linux (베어메탈 SillyTavern)

Linux의 경우 [Traefik](https://traefik.io/traefik/)을 통해 SillyTavern을 리버스 프록시합니다. _NGINX_ 또는 _Caddy_와 같은 다른 옵션도 있지만, 이 가이드에서는 우리가 직접 사용하는 Traefik을 사용합니다.

1. `ifconfig` 또는 라우터에서 컴퓨터의 사설 IP를 가져옵니다.
   !!!info 팁
   사설 IP를 고정 IP로 설정하는 것이 좋습니다. 고정 IP를 구성하려면 라우터 설명서를 참조하거나 Google을 검색하십시오.
   !!!
2. Google에서 `what's my ip`를 검색하여 모뎀의 공용 IP를 가져옵니다.
   !!!info 공용 IP에 대하여
   대부분의 가정/주거용 네트워크는 몇 달 사용 후 갱신되는 **동적 IP**를 사용합니다. 동적 IP가 있는 경우 DDClient를 사용하거나 Cloudflare 대시보드에서 공용 IP를 가끔 확인하고 변경하는 것을 기억하십시오.
   !!!
3. [여기](https://docs.docker.com/engine/install/)에 있는 Docker 설치 가이드에 따라 Docker를 설치합니다.
   !!!danger 참고
   Docker Desktop을 설치하지 **마십시오**.
   !!!
4. [여기](https://docs.docker.com/engine/install/linux-postinstall/)에 있는 Docker 설치 후 가이드에서 **Manage Docker as a non-root user** 단계를 따르십시오.
5. Linux의 루트 폴더로 이동하여 `docker`라는 새 폴더를 만듭니다.
    ```sh
    cd /
    sudo mkdir docker && cd docker
    ```
6. _<USER>_를 Linux 사용자 이름으로 바꿔서 `chown`을 실행하여 docker 폴더의 권한을 설정합니다.
    ```sh
    sudo chown -R <USER>:<USER> .
    ```
7. _docker_ 폴더 안에 `secrets` 폴더를 만들고 _secrets_ 안에 `cloudflare` 폴더를 만듭니다.
    ```sh
    mkdir secrets && mkdir secrets/cloudflare
    ```
8. _docker_ 폴더 안에 `appdata` 폴더를 만들고 _appdata_ 안에 `traefik` 폴더를 만듭니다. 그런 다음 `appdata/traefik` 폴더로 들어갑니다.
    ```sh
    mkdir appdata && mkdir appdata/traefik
    cd appdata/traefik
    ```
9. `touch`를 사용하여 _acme.json_ 파일을 만들고 권한을 600으로 설정합니다.
    ```sh
    touch acme.json
    chmod 600 acme.json
    ```
10. `nano` 또는 유사한 편집기를 사용하여 _traefik.yml_이라는 파일을 만들고 다음을 붙여넣습니다. 템플릿 이메일을 자신의 이메일로 바꾼 다음 파일을 저장합니다.
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
11. `docker` 폴더로 돌아갑니다.
    ```sh
    cd /docker
    ```
12. `nano` 또는 유사한 편집기를 사용하여 _docker-compose.yaml_이라는 파일을 만들고 다음을 붙여넣습니다. 그런 다음 파일을 저장합니다.

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

13. Cloudflare에 로그인하고 도메인을 클릭한 다음 **Get your API token**을 클릭합니다.
14. _Create Token_을 클릭한 다음 _Create Custom Token_을 클릭하고 토큰에 다음 권한을 부여합니다.
    !!!info 토큰 권한
    **Zone -> DNS -> Edit**

    **Zone -> Zone -> Read**
    !!!

    _Continue to summary_를 클릭한 다음 _Create Token_을 클릭합니다.

15. 제공된 토큰 키를 복사하여 안전한 곳에 보관합니다.
16. `secrets/cloudflare`로 `cd`하고 `nano` 또는 유사한 편집기를 사용하여 **CF_DNS_API_KEY**라는 파일을 만들고 키를 안에 붙여넣습니다.
17. 도메인 페이지로 돌아가서 **DNS**로 이동합니다. **Add record**를 사용하여 새 레코드를 만들고 아래와 같은 두 개의 _A_ 유형 키를 만듭니다. `PUBLIC_IP`를 자신의 공용 IP로 바꾼 다음 _Save_를 클릭합니다.

    | Type | Name (required) | Target (required) | Proxy Status | TTL  |
    |------|-----------------|-------------------|--------------|------|
    | A    | DOMAIN.com      | PUBLIC_IP         | Proxied      | Auto |
    | A    | www             | PUBLIC_IP         | Proxied      | Auto |

18. **`CNAME`** 유형의 다른 레코드를 만든 다음 _Save_를 클릭합니다. Cloudflare 대시보드에 표시되는 방법의 예는 다음과 같습니다.

    | Type  | Name (required) | Target (required) | Proxy Status | TTL |
    |-------|-----------------|-------------------|--------------|-----|
    | CNAME | silly           | DOMAIN.com        | Proxied      | N/A |

19. _appdata/traefik_으로 `cd`하고 `nano` 또는 유사한 편집기를 사용하여 _config.yml_이라는 파일을 만들고 다음을 붙여넣습니다. `PRIVATE_IP`를 얻은 사설 IP로, `silly.DOMAIN.com`을 하위 도메인 및 도메인 페이지 이름으로 바꾼 다음 파일을 저장합니다.

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

20. 다음 명령을 사용하여 Docker Compose를 실행합니다:
    ```sh
    cd /docker
    docker compose up -d
    ```
21. SillyTavern 폴더로 이동하여 `config.yaml`을 편집하여 listen 모드와 기본 인증을 활성화하고 `whitelistMode`를 비활성화합니다.

    ```yaml
    listen: yes
    whitelistMode: false
    basicAuthMode: true
    ```

    !!!warning 팁
    기본 사용자 이름과 비밀번호를 기억할 수 있는 강력한 것으로 변경하십시오.
    !!!

    또는 SillyTavern 계정을 사용자 이름과 비밀번호로 사용하려면:

    ```yaml
    basicAuthMode: true
    enableUserAccounts: true
    perUserBasicAuth: true
    ```

    !!!warning 팁
    perUserBasicAuth를 활성화하기 전에 작동하는 비밀번호가 있는 유효한 다중 사용자 설정이 있는지 확인하십시오.
    !!!

22. 몇 분 기다린 다음 ST용으로 만든 도메인 페이지를 엽니다. 결국 하나의 URL과 하나의 계정만으로 어디를 가든 SillyTavern을 열 수 있어야 합니다.
    !!!info 팁
    몇 분이 지나도 아무 일도 일어나지 않으면 Traefik의 컨테이너 로그를 확인하여 가능한 오류를 찾으십시오.
    !!!
23. 즐기세요! :D

### Linux (Docker SillyTavern)

!!!warning 참고
우리는 Docker보다 베어메탈에서 SillyTavern을 실행합니다. 이것은 우리가 ST와 함께 사용하는 다른 Docker 컨테이너와 함께 Docker에서 무엇을 할 것인지에 대한 대략적인 아이디어입니다.
!!!

1. **Linux (베어메탈 SillyTavern)**의 1-11단계를 따르십시오.
2. Cloudflare에 로그인하고 도메인을 클릭한 다음 **Get your API token**을 클릭합니다.
3. _Create Token_을 클릭한 다음 _Create Custom Token_을 클릭하고 토큰에 다음 권한을 부여합니다.
   !!!info 토큰 권한
   **Zone -> DNS -> Edit**

    **Zone -> Zone -> Read**
    !!!

    _Continue to summary_를 클릭한 다음 _Create Token_을 클릭합니다.

4. 제공된 토큰 키를 복사하여 안전한 곳에 보관합니다.
5. `secrets/cloudflare`로 `cd`하고 `nano` 또는 유사한 편집기를 사용하여 **CF_DNS_API_KEY**라는 파일을 만들고 키를 안에 붙여넣습니다.
6. 도메인 페이지로 돌아가서 **DNS**로 이동합니다. **Add record**를 사용하여 새 레코드를 만들고 아래와 같은 두 개의 _A_ 유형 키를 만듭니다. `PUBLIC_IP`를 자신의 공용 IP로, 예제 도메인을 자신의 도메인으로 바꾼 다음 _Save_를 클릭합니다.

    | Type | Name (required) | Target (required) | Proxy Status | TTL  |
    |------|-----------------|-------------------|--------------|------|
    | A    | DOMAIN.com      | PUBLIC_IP         | Proxied      | Auto |
    | A    | www             | PUBLIC_IP         | Proxied      | Auto |

7. **`CNAME`** 유형의 다른 레코드를 만든 다음 _Save_를 클릭합니다. Cloudflare 대시보드에 표시되는 방법의 예는 다음과 같습니다.

    | Type  | Name (required) | Target (required) | Proxy Status | TTL |
    |-------|-----------------|-------------------|--------------|-----|
    | CNAME | silly           | DOMAIN.com        | Proxied      | N/A |

8. `docker` 폴더에 SillyTavern을 Git clone합니다.
    ```sh
    cd /docker && git clone https://github.com/SillyTavern/SillyTavern
    ```
9. `nano` 또는 유사한 편집기를 사용하여 _docker-compose.yaml_이라는 파일을 만들고 다음을 붙여넣습니다. `silly.DOMAIN.com`을 위에 추가한 하위 도메인으로 바꾼 다음 파일을 저장합니다.

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

10. 다음 명령을 사용하여 Docker Compose를 실행합니다:
    ```sh
    docker compose up -d
    ```
11. SillyTavern Docker 컨테이너를 중지합니다.
    ```sh
    docker compose stop sillytavern
    ```
12. SillyTavern 폴더(`appdata/sillytavern/config`)로 이동하여 `config.yaml`을 편집하여 listen 모드와 기본 인증을 활성화하고 `whitelistMode`를 비활성화합니다.

    ```yaml
    listen: yes
    whitelistMode: false
    basicAuthMode: true
    ```

    !!!warning 팁
    기본 사용자 이름과 비밀번호를 기억할 수 있는 강력한 것으로 변경하십시오.
    !!!

13. SillyTavern Docker 컨테이너를 다시 시작합니다.
    ```sh
    docker compose up -d sillytavern
    ```
14. 몇 분 기다린 다음 ST용으로 만든 도메인 페이지를 엽니다. 결국 하나의 URL과 하나의 계정만으로 어디를 가든 SillyTavern을 열 수 있어야 합니다.
    !!!info 팁
    몇 분이 지나도 아무 일도 일어나지 않으면 Traefik의 컨테이너 로그를 확인하여 가능한 오류를 찾으십시오.
    !!!
15. 즐기세요! :D

## Cloudflare DNS 업데이트

[**DDClient**](https://ddclient.net/)를 사용하면 ISP가 공용 IP를 변경하는 상황에서 공용 IP를 Cloudflare에 동기화할 수 있어 아무 일도 없었던 것처럼 ST 인스턴스에 계속 액세스할 수 있습니다.
