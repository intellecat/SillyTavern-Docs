---
label: Reverse proxying
order: -50
icon: server
route: /usage/st-reverse-proxy-guide/
---

!!!danger Nota
Esta seção **não** se refere a proxies reversos OpenAI/Claude. Isso se refere exclusivamente a **Proxies Reversos HTTP/HTTPS**.
!!!

O Termux é confuso de configurar? Você está cansado de atualizar e instalar o ST em cada dispositivo que possui? Quer organização de seus chats e personagens? Bem, você está com sorte. Este guia _esperançosamente_ cobrirá como hospedar o SillyTavern no seu PC onde você pode se conectar de qualquer lugar e conversar com seus bots no mesmo PC que você usa para executar modelos de IA!

!!!warning Aviso
Este guia **não é destinado** a iniciantes. Será muito técnico.
!!!

## Aviso Importante

!!!info Para Usuários Windows
Este guia não é para usuários Windows. Recomendamos usar uma VM Linux ou WSL2 para seguir este guia.
!!!

!!!info Para Usuários Linux
Você deve ter conhecimento prévio de

- Comandos do console Linux
- Registros DNS
- Endereços IP públicos
- [Docker](https://www.docker.com)

!!!

**Você terá que comprar um domínio para si mesmo e configurar um `CNAME` para sua página SillyTavern. Sugerimos adicionar ou comprar o domínio no [Cloudflare](https://www.cloudflare.com) pois este guia cobrirá como fazer isso com o próprio Cloudflare.**

## Instalação

### Linux (SillyTavern Bare-Metal)

Para Linux, faremos proxy reverso do SillyTavern através do [Traefik](https://traefik.io/traefik/). Existem outras opções como _NGINX_ ou _Caddy_, mas para este guia, usaremos o Traefik pois é o que usamos.

1. Obtenha o IP privado do seu computador usando `ifconfig` ou do seu roteador.
   !!!info Dica
   É recomendado definir seu IP privado como um IP estático. Consulte o manual do seu roteador ou o Google para configurar IPs estáticos.
   !!!
2. Obtenha seu IP público do seu modem pesquisando no Google `what's my ip`.
   !!!info Sobre IPs Públicos
   A maioria das redes residenciais/domésticas usa **IPs dinâmicos** que são renovados após meses de uso. Se você tem um IP dinâmico, use DDClient ou lembre-se de verificar e alterar seu IP público de vez em quando no painel do Cloudflare.
   !!!
3. Instale o Docker seguindo o guia de instalação do Docker [aqui](https://docs.docker.com/engine/install/).
   !!!danger Nota
   **Não** instale o Docker Desktop.
   !!!
4. Siga as etapas em **Manage Docker as a non-root user** no guia pós-instalação do Docker [aqui](https://docs.docker.com/engine/install/linux-postinstall/).
5. Vá para sua pasta raiz no Linux e crie uma nova pasta chamada `docker`.
    ```sh
    cd /
    sudo mkdir docker && cd docker
    ```
6. Execute `chown`, substituindo _<USER>_ pelo seu nome de usuário Linux para definir as permissões na pasta docker.
    ```sh
    sudo chown -R <USER>:<USER> .
    ```
7. Crie uma pasta dentro da pasta _docker_, sendo `secrets` e dentro de _secrets_ sendo `cloudflare`.
    ```sh
    mkdir secrets && mkdir secrets/cloudflare
    ```
8. Crie uma pasta dentro da pasta _docker_, sendo `appdata` e dentro de _appdata_ sendo `traefik`. Entre na pasta `appdata/traefik` depois.
    ```sh
    mkdir appdata && mkdir appdata/traefik
    cd appdata/traefik
    ```
9. Crie um arquivo _acme.json_ usando `touch` e defina as permissões dele para 600.
    ```sh
    touch acme.json
    chmod 600 acme.json
    ```
10. Usando `nano` ou um editor similar, crie um arquivo chamado _traefik.yml_ e cole o seguinte. Substitua o email de exemplo pelo seu próprio, depois salve o arquivo.
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
11. Retorne para a pasta `docker`.
    ```sh
    cd /docker
    ```
12. Usando `nano` ou um editor similar, crie um arquivo chamado _docker-compose.yaml_ e cole o seguinte. Salve o arquivo depois.

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

13. Faça login no Cloudflare e clique no seu domínio, seguido de **Get your API token**.
14. Clique em _Create Token_ e depois _Create Custom Token_ e certifique-se de dar ao seu token as seguintes permissões.
    !!!info Permissões do Token
    **Zone -> DNS -> Edit**

    **Zone -> Zone -> Read**
    !!!

    Clique em _Continue to summary_ seguido de _Create Token._

15. Copie a chave do token dada a você e armazene-a em algum lugar seguro.
16. Use `cd` para ir para `secrets/cloudflare` e usando `nano` ou um editor similar, crie um arquivo chamado **CF_DNS_API_KEY** e cole sua chave dentro.
17. Retorne para sua página de domínio e vá para **DNS**. Crie um novo registro usando **Add record** e crie duas chaves do tipo _A_ como as abaixo. Substitua `PUBLIC_IP` pelo seu próprio IP público, depois clique em _Save_.

    | Type | Name (required) | Target (required) | Proxy Status | TTL  |
    |------|-----------------|-------------------|--------------|------|
    | A    | DOMAIN.com      | PUBLIC_IP         | Proxied      | Auto |
    | A    | www             | PUBLIC_IP         | Proxied      | Auto |

18. Crie outro registro do tipo **`CNAME`**, depois clique em _Save_. Aqui está um exemplo de como deve aparecer no painel do Cloudflare.

    | Type  | Name (required) | Target (required) | Proxy Status | TTL |
    |-------|-----------------|-------------------|--------------|-----|
    | CNAME | silly           | DOMAIN.com        | Proxied      | N/A |

19. Use `cd` para ir para _appdata/traefik_ e usando `nano` ou um editor similar, crie um arquivo chamado _config.yml_ e cole o seguinte. Substitua `PRIVATE_IP` pelo IP privado que você obteve, e `silly.DOMAIN.com` pelo nome do seu subdomínio e página de domínio, depois salve o arquivo.

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

20. Execute o Docker Compose usando os seguintes comandos:
    ```sh
    cd /docker
    docker compose up -d
    ```
21. Vá para sua pasta SillyTavern e edite `config.yaml` para habilitar o modo listen e autenticação básica, enquanto desabilita `whitelistMode`.

    ```yaml
    listen: yes
    whitelistMode: false
    basicAuthMode: true
    ```

    !!!warning Dica
    Certifique-se de alterar o nome de usuário e senha padrão para algo forte que você possa lembrar.
    !!!

    Ou para usar as contas do SillyTavern como nomes de usuário e senhas:

    ```yaml
    basicAuthMode: true
    enableUserAccounts: true
    perUserBasicAuth: true
    ```

    !!!warning Dica
    Antes de habilitar perUserBasicAuth, certifique-se de ter uma configuração multiusuário válida com senhas funcionando.
    !!!

22. Aguarde alguns minutos, depois abra sua página de domínio que você criou para o ST. No final, você deve ser capaz de abrir o SillyTavern de qualquer lugar que você vá apenas com uma URL e uma conta.
    !!!info Dica
    Se nada acontecer após vários minutos, verifique os logs do container do Traefik para possíveis erros.
    !!!
23. Aproveite! :D

### Linux (SillyTavern Docker)

!!!warning Nota
Note que executamos o SillyTavern em bare-metal sobre Docker. Esta é uma ideia aproximada do que faríamos no Docker com outros containers Docker que tendemos a usar com o ST.
!!!

1. Siga os Passos 1-11 de **Linux (SillyTavern Bare-Metal)**.
2. Faça login no Cloudflare e clique no seu domínio, seguido de **Get your API token**.
3. Clique em _Create Token_ e depois _Create Custom Token_ e certifique-se de dar ao seu token as seguintes permissões.
   !!!info Permissões do Token
   **Zone -> DNS -> Edit**

    **Zone -> Zone -> Read**
    !!!

    Clique em _Continue to summary_ seguido de _Create Token._

4. Copie a chave do token dada a você e armazene-a em algum lugar seguro.
5. Use `cd` para ir para `secrets/cloudflare` e usando `nano` ou um editor similar, crie um arquivo chamado **CF_DNS_API_KEY** e cole sua chave dentro.
6. Retorne para sua página de domínio e vá para **DNS**. Crie um novo registro usando **Add record** e crie duas chaves do tipo _A_ como as abaixo. Substitua `PUBLIC_IP` pelo seu próprio IP público e o domínio de exemplo pelo seu domínio, depois clique em _Save_.

    | Type | Name (required) | Target (required) | Proxy Status | TTL  |
    |------|-----------------|-------------------|--------------|------|
    | A    | DOMAIN.com      | PUBLIC_IP         | Proxied      | Auto |
    | A    | www             | PUBLIC_IP         | Proxied      | Auto |

7. Crie outro registro do tipo **`CNAME`**, depois clique em _Save_. Aqui está um exemplo de como deve aparecer no painel do Cloudflare.

    | Type  | Name (required) | Target (required) | Proxy Status | TTL |
    |-------|-----------------|-------------------|--------------|-----|
    | CNAME | silly           | DOMAIN.com        | Proxied      | N/A |

8. Clone o SillyTavern via Git na pasta `docker`.
    ```sh
    cd /docker && git clone https://github.com/SillyTavern/SillyTavern
    ```
9. Usando `nano` ou um editor similar, crie um arquivo chamado _docker-compose.yaml_ e cole o seguinte. Substitua `silly.DOMAIN.com` pelo subdomínio que você adicionou acima, depois salve o arquivo.

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

10. Execute o Docker Compose usando os seguintes comandos:
    ```sh
    docker compose up -d
    ```
11. Pare o container Docker do SillyTavern.
    ```sh
    docker compose stop sillytavern
    ```
12. Vá para sua pasta SillyTavern (`appdata/sillytavern/config`) e edite `config.yaml` para habilitar o modo listen e autenticação básica, enquanto desabilita `whitelistMode`.

    ```yaml
    listen: yes
    whitelistMode: false
    basicAuthMode: true
    ```

    !!!warning Dica
    Certifique-se de alterar o nome de usuário e senha padrão para algo forte que você possa lembrar.
    !!!

13. Inicie o container Docker do SillyTavern novamente.
    ```sh
    docker compose up -d sillytavern
    ```
14. Aguarde alguns minutos, depois abra sua página de domínio que você criou para o ST. No final, você deve ser capaz de abrir o SillyTavern de qualquer lugar que você vá apenas com uma URL e uma conta.
    !!!info Dica
    Se nada acontecer após vários minutos, verifique os logs do container do Traefik para possíveis erros.
    !!!
15. Aproveite! :D

## Atualizando seu DNS do Cloudflare

[**DDClient**](https://ddclient.net/) permite que você sincronize seu IP público com o Cloudflare na situação em que seu ISP o altere, permitindo que você continue acessando sua instância ST como se nada tivesse acontecido.
