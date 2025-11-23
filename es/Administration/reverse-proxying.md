---
label: Proxy inverso
order: -50
icon: server
route: /usage/st-reverse-proxy-guide/
---

!!!danger Nota
Esta sección **no** se refiere a proxies inversos de OpenAI/Claude. Esto se refiere exclusivamente a **Proxies Inversos HTTP/HTTPS**.
!!!

¿Es confuso configurar Termux? ¿Estás cansado de actualizar e instalar ST en cada dispositivo que tienes? ¿Quieres organizar tus chats y personajes? Estás de suerte. Esta guía _con suerte_ cubrirá cómo alojar SillyTavern en tu PC donde puedes conectarte desde cualquier lugar y chatear con tus bots en la misma PC que usas para ejecutar modelos de IA!

!!!warning Advertencia
Esta guía **no está destinada** a principiantes. Será muy técnica.
!!!

## Advertencia Importante

!!!info Para Usuarios de Windows
Esta guía no es para usuarios de Windows. Recomendamos usar una VM de Linux o WSL2 para seguir esta guía.
!!!

!!!info Para Usuarios de Linux
Debes tener conocimiento previo de

- Comandos de consola de Linux
- DNS Records
- Direcciones IP públicas
- [Docker](https://www.docker.com)

!!!

**Tendrás que comprar un dominio para ti y configurar un `CNAME` para tu página de SillyTavern. Sugerimos agregar o comprar el dominio en [Cloudflare](https://www.cloudflare.com) ya que esta guía cubrirá cómo hacer esto con Cloudflare.**

## Instalación

### Linux (SillyTavern Bare-Metal)

Para Linux, haremos proxy inverso de SillyTavern a través de [Traefik](https://traefik.io/traefik/). Hay otras opciones como _NGINX_ o _Caddy_, pero para esta guía, usaremos Traefik ya que es lo que usamos nosotros mismos.

1. Obtén la IP privada de tu computadora usando `ifconfig` o desde tu router.
   !!!info Consejo
   Se recomienda configurar tu IP privada como IP estática. Consulta el manual de tu router o Google para configurar IPs estáticas.
   !!!
2. Obtén tu IP pública de tu módem buscando en Google `what's my ip`.
   !!!info Acerca de las IPs Públicas
   La mayoría de las redes residenciales/domésticas usan **IPs Dinámicas** que se renuevan después de meses de uso. Si tienes una IP dinámica, usa DDClient o recuerda verificar y cambiar tu IP pública de vez en cuando en el Panel de Cloudflare.
   !!!
3. Instala Docker siguiendo la guía de instalación de Docker [aquí](https://docs.docker.com/engine/install/).
   !!!danger Nota
   **No** instales Docker Desktop.
   !!!
4. Sigue los pasos en **Manage Docker as a non-root user** en la guía de post-instalación de Docker [aquí](https://docs.docker.com/engine/install/linux-postinstall/).
5. Ve a tu carpeta raíz en Linux y crea una nueva carpeta llamada `docker`.
    ```sh
    cd /
    sudo mkdir docker && cd docker
    ```
6. Ejecuta `chown`, reemplazando _<USER>_ con tu nombre de usuario de Linux para establecer los permisos en la carpeta docker.
    ```sh
    sudo chown -R <USER>:<USER> .
    ```
7. Crea una carpeta dentro de la carpeta _docker_, llamada `secrets` y dentro de _secrets_ una llamada `cloudflare`.
    ```sh
    mkdir secrets && mkdir secrets/cloudflare
    ```
8. Crea una carpeta dentro de la carpeta _docker_, llamada `appdata` y dentro de _appdata_ una llamada `traefik`. Entra en la carpeta `appdata/traefik` después.
    ```sh
    mkdir appdata && mkdir appdata/traefik
    cd appdata/traefik
    ```
9. Crea un archivo _acme.json_ usando `touch` y establece sus permisos a 600.
    ```sh
    touch acme.json
    chmod 600 acme.json
    ```
10. Usando `nano` o un editor similar, crea un archivo llamado _traefik.yml_ y pega lo siguiente. Reemplaza el correo electrónico de plantilla con el tuyo, luego guarda el archivo.
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
11. Regresa a la carpeta `docker`.
    ```sh
    cd /docker
    ```
12. Usando `nano` o un editor similar, crea un archivo llamado _docker-compose.yaml_ y pega lo siguiente. Guarda el archivo después.

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

13. Inicia sesión en Cloudflare y haz clic en tu Dominio, seguido de **Get your API token**.
14. Haz clic en _Create Token_ luego en _Create Custom Token_ y asegúrate de darle a tu token los siguientes permisos.
    !!!info Permisos del Token
    **Zone -> DNS -> Edit**

    **Zone -> Zone -> Read**
    !!!

    Haz clic en _Continue to summary_ seguido de _Create Token._

15. Copia la clave del Token que se te proporcionó y guárdala en un lugar seguro.
16. Usa `cd` para ir a `secrets/cloudflare` y usando `nano` o un editor similar, crea un archivo llamado **CF_DNS_API_KEY** y pega tu clave dentro.
17. Regresa a la página de tu dominio y ve a **DNS**. Crea un nuevo registro usando **Add record** y crea dos claves tipo _A_ como las de abajo. Reemplaza `PUBLIC_IP` con tu propia IP pública, luego haz clic en _Save_.

    | Tipo | Nombre (requerido) | Destino (requerido) | Estado del Proxy | TTL  |
    |------|---------------------|---------------------|------------------|------|
    | A    | DOMAIN.com          | PUBLIC_IP           | Proxied          | Auto |
    | A    | www                 | PUBLIC_IP           | Proxied          | Auto |

18. Crea otro registro del tipo **`CNAME`**, luego haz clic en _Save_. Aquí hay un ejemplo de cómo debería aparecer en el panel de Cloudflare.

    | Tipo  | Nombre (requerido) | Destino (requerido) | Estado del Proxy | TTL |
    |-------|--------------------|---------------------|------------------|-----|
    | CNAME | silly              | DOMAIN.com          | Proxied          | N/A |

19. Usa `cd` para ir a _appdata/traefik_ y usando `nano` o un editor similar, crea un archivo llamado _config.yml_ y pega lo siguiente. Reemplaza `PRIVATE_IP` con la IP privada que obtuviste, y `silly.DOMAIN.com` con el nombre de tu subdominio y página de dominio, luego guarda el archivo.

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

20. Ejecuta Docker Compose usando los siguientes comandos:
    ```sh
    cd /docker
    docker compose up -d
    ```
21. Ve a tu carpeta de SillyTavern y edita `config.yaml` para habilitar el modo escucha y autenticación básica, mientras deshabilitas `whitelistMode`.

    ```yaml
    listen: yes
    whitelistMode: false
    basicAuthMode: true
    ```

    !!!warning Consejo
    Asegúrate de cambiar el nombre de usuario y contraseña predeterminados a algo fuerte que puedas recordar.
    !!!

    O para usar las cuentas de SillyTavern como nombres de usuario y contraseñas:

    ```yaml
    basicAuthMode: true
    enableUserAccounts: true
    perUserBasicAuth: true
    ```

    !!!warning Consejo
    Antes de habilitar perUserBasicAuth asegúrate de tener una configuración válida de múltiples usuarios con contraseñas funcionando.
    !!!

22. Espera unos minutos, luego abre la página de dominio que creaste para ST. Al final, deberías poder abrir SillyTavern desde donde sea que vayas solo con una URL y una cuenta.
    !!!info Consejo
    Si no sucede nada después de varios minutos, revisa los registros del contenedor de Traefik para detectar posibles errores.
    !!!
23. ¡Disfruta! :D

### Linux (Docker SillyTavern)

!!!warning Nota
Ten en cuenta que ejecutamos SillyTavern en bare-metal sobre Docker. Esta es una idea aproximada de lo que haríamos en Docker con otros contenedores Docker que tendemos a usar con ST.
!!!

1. Sigue los Pasos 1-11 de **Linux (SillyTavern Bare-Metal)**.
2. Inicia sesión en Cloudflare y haz clic en tu Dominio, seguido de **Get your API token**.
3. Haz clic en _Create Token_ luego en _Create Custom Token_ y asegúrate de darle a tu token los siguientes permisos.
   !!!info Permisos del Token
   **Zone -> DNS -> Edit**

    **Zone -> Zone -> Read**
    !!!

    Haz clic en _Continue to summary_ seguido de _Create Token._

4. Copia la clave del Token que se te proporcionó y guárdala en un lugar seguro.
5. Usa `cd` para ir a `secrets/cloudflare` y usando `nano` o un editor similar, crea un archivo llamado **CF_DNS_API_KEY** y pega tu clave dentro.
6. Regresa a la página de tu dominio y ve a **DNS**. Crea un nuevo registro usando **Add record** y crea dos claves tipo _A_ como las de abajo. Reemplaza `PUBLIC_IP` con tu propia IP pública y el dominio de ejemplo con tu dominio, luego haz clic en _Save_.

    | Tipo | Nombre (requerido) | Destino (requerido) | Estado del Proxy | TTL  |
    |------|---------------------|---------------------|------------------|------|
    | A    | DOMAIN.com          | PUBLIC_IP           | Proxied          | Auto |
    | A    | www                 | PUBLIC_IP           | Proxied          | Auto |

7. Crea otro registro del tipo **`CNAME`**, luego haz clic en _Save_. Aquí hay un ejemplo de cómo debería aparecer en el panel de Cloudflare.

    | Tipo  | Nombre (requerido) | Destino (requerido) | Estado del Proxy | TTL |
    |-------|--------------------|---------------------|------------------|-----|
    | CNAME | silly              | DOMAIN.com          | Proxied          | N/A |

8. Clona con Git SillyTavern en la carpeta `docker`.
    ```sh
    cd /docker && git clone https://github.com/SillyTavern/SillyTavern
    ```
9. Usando `nano` o un editor similar, crea un archivo llamado _docker-compose.yaml_ y pega lo siguiente. Reemplaza `silly.DOMAIN.com` con el subdominio que agregaste arriba, luego guarda el archivo después.

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

10. Ejecuta Docker Compose usando los siguientes comandos:
    ```sh
    docker compose up -d
    ```
11. Detén el contenedor Docker de SillyTavern.
    ```sh
    docker compose stop sillytavern
    ```
12. Ve a tu carpeta de SillyTavern (`appdata/sillytavern/config`) y edita `config.yaml` para habilitar el modo escucha y autenticación básica, mientras deshabilitas `whitelistMode`.

    ```yaml
    listen: yes
    whitelistMode: false
    basicAuthMode: true
    ```

    !!!warning Consejo
    Asegúrate de cambiar el nombre de usuario y contraseña predeterminados a algo fuerte que puedas recordar.
    !!!

13. Inicia el contenedor Docker de SillyTavern nuevamente.
    ```sh
    docker compose up -d sillytavern
    ```
14. Espera unos minutos, luego abre la página de dominio que creaste para ST. Al final, deberías poder abrir SillyTavern desde donde sea que vayas solo con una URL y una cuenta.
    !!!info Consejo
    Si no sucede nada después de varios minutos, revisa los registros del contenedor de Traefik para detectar posibles errores.
    !!!
15. ¡Disfruta! :D

## Actualizando tu DNS de Cloudflare

[**DDClient**](https://ddclient.net/) te permite sincronizar tu IP pública con Cloudflare en la situación de que tu ISP la cambie, permitiéndote continuar accediendo a tu instancia de ST como si nada hubiera pasado.
