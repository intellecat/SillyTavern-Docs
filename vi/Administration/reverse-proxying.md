---
label: Reverse proxying
order: -50
icon: server
route: /usage/st-reverse-proxy-guide/
---

!!!danger Lưu Ý
Phần này **không** đề cập đến reverse proxy OpenAI/Claude. Phần này chỉ đề cập đến **HTTP/HTTPS Reverse Proxies**.
!!!

Termux có khó thiết lập không? Bạn có mệt mỏi với việc cập nhật và cài đặt ST trên mọi thiết bị bạn có không? Muốn tổ chức các cuộc trò chuyện và nhân vật của bạn? Thì bạn đã may mắn. Hướng dẫn này _hy vọng_ sẽ bao gồm cách lưu trữ SillyTavern trên PC của bạn, nơi bạn có thể kết nối từ bất kỳ đâu và chat với bot của bạn trên cùng một PC mà bạn sử dụng để chạy các model AI!

!!!warning Cảnh Báo
Hướng dẫn này **không dành** cho người mới bắt đầu. Đây sẽ là một hướng dẫn rất kỹ thuật.
!!!

## Cảnh Báo Công Bằng

!!!info Dành Cho Người Dùng Windows
Hướng dẫn này không dành cho người dùng Windows. Chúng tôi khuyên bạn nên sử dụng Linux VM hoặc WSL2 để làm theo hướng dẫn này.
!!!

!!!info Dành Cho Người Dùng Linux
Bạn phải có kiến thức trước về

- Lệnh console Linux
- DNS Records
- Địa chỉ IP công cộng
- [Docker](https://www.docker.com)

!!!

**Bạn sẽ phải mua một domain cho mình và cấu hình một `CNAME` cho trang SillyTavern của bạn. Chúng tôi đề xuất thêm hoặc mua domain trên [Cloudflare](https://www.cloudflare.com) vì hướng dẫn này sẽ bao gồm cách thực hiện điều này với Cloudflare.**

## Cài Đặt

### Linux (Bare-Metal SillyTavern)

Đối với Linux, chúng tôi sẽ reverse proxy SillyTavern thông qua [Traefik](https://traefik.io/traefik/). Có các tùy chọn khác như _NGINX_ hoặc _Caddy_, nhưng cho hướng dẫn này, chúng tôi sẽ sử dụng Traefik vì đó là cái chúng tôi tự sử dụng.

1. Lấy IP riêng của máy tính của bạn bằng cách sử dụng `ifconfig` hoặc từ router của bạn.
   !!!info Mẹo
   Nên đặt IP riêng của bạn thành IP tĩnh. Tham khảo hướng dẫn sử dụng router của bạn hoặc Google để cấu hình IP tĩnh.
   !!!
2. Lấy IP công cộng của modem của bạn bằng cách Google `what's my ip`.
   !!!info Về IP Công Cộng
   Hầu hết các mạng dân cư/gia đình sử dụng **IP động** được làm mới sau nhiều tháng sử dụng. Nếu bạn có IP động, hãy sử dụng DDClient hoặc nhớ kiểm tra và thay đổi IP công cộng của bạn thường xuyên trên Cloudflare Dashboard.
   !!!
3. Cài đặt Docker bằng cách làm theo hướng dẫn cài đặt Docker [tại đây](https://docs.docker.com/engine/install/).
   !!!danger Lưu Ý
   **Không** cài đặt Docker Desktop.
   !!!
4. Làm theo các bước trong **Manage Docker as a non-root user** trong hướng dẫn sau cài đặt Docker [tại đây](https://docs.docker.com/engine/install/linux-postinstall/).
5. Đi đến thư mục root của bạn trong Linux và tạo một thư mục mới có tên `docker`.
    ```sh
    cd /
    sudo mkdir docker && cd docker
    ```
6. Thực thi `chown`, thay thế _<USER>_ bằng tên người dùng Linux của bạn để đặt quyền trong thư mục docker.
    ```sh
    sudo chown -R <USER>:<USER> .
    ```
7. Tạo một thư mục bên trong thư mục _docker_, đó là `secrets` và bên trong _secrets_ là `cloudflare`.
    ```sh
    mkdir secrets && mkdir secrets/cloudflare
    ```
8. Tạo một thư mục bên trong thư mục _docker_, đó là `appdata` và bên trong _appdata_ là `traefik`. Sau đó vào thư mục `appdata/traefik`.
    ```sh
    mkdir appdata && mkdir appdata/traefik
    cd appdata/traefik
    ```
9. Tạo tệp _acme.json_ bằng cách sử dụng `touch` và đặt quyền của nó thành 600.
    ```sh
    touch acme.json
    chmod 600 acme.json
    ```
10. Sử dụng `nano` hoặc trình chỉnh sửa tương tự, tạo một tệp có tên _traefik.yml_ và dán nội dung sau. Thay thế email mẫu bằng email của bạn, sau đó lưu tệp.
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
11. Quay lại thư mục `docker`.
    ```sh
    cd /docker
    ```
12. Sử dụng `nano` hoặc trình chỉnh sửa tương tự, tạo một tệp có tên _docker-compose.yaml_ và dán nội dung sau. Lưu tệp sau đó.

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

13. Đăng nhập vào Cloudflare và nhấp vào Domain của bạn, tiếp theo là **Get your API token**.
14. Nhấp vào _Create Token_ sau đó _Create Custom Token_ và đảm bảo bạn cấp cho token của mình các quyền sau.
    !!!info Quyền Token
    **Zone -> DNS -> Edit**

    **Zone -> Zone -> Read**
    !!!

    Nhấp vào _Continue to summary_ tiếp theo là _Create Token._

15. Sao chép Token Key được cung cấp cho bạn và lưu trữ nó ở nơi an toàn.
16. `cd` vào `secrets/cloudflare` và sử dụng `nano` hoặc trình chỉnh sửa tương tự, tạo một tệp có tên **CF_DNS_API_KEY** và dán khóa của bạn vào bên trong.
17. Quay lại trang domain của bạn và vào **DNS**. Tạo một record mới bằng cách sử dụng **Add record** và tạo hai khóa loại _A_ như những khóa bên dưới. Thay thế `PUBLIC_IP` bằng IP công cộng của riêng bạn, sau đó nhấp _Save_.

    | Type | Name (required) | Target (required) | Proxy Status | TTL  |
    |------|-----------------|-------------------|--------------|------|
    | A    | DOMAIN.com      | PUBLIC_IP         | Proxied      | Auto |
    | A    | www             | PUBLIC_IP         | Proxied      | Auto |

18. Tạo một record khác của loại **`CNAME`**, sau đó nhấp _Save_. Đây là một ví dụ về cách nó sẽ xuất hiện trên dashboard Cloudflare.

    | Type  | Name (required) | Target (required) | Proxy Status | TTL |
    |-------|-----------------|-------------------|--------------|-----|
    | CNAME | silly           | DOMAIN.com        | Proxied      | N/A |

19. `cd` vào _appdata/traefik_ và sử dụng `nano` hoặc trình chỉnh sửa tương tự, tạo một tệp có tên _config.yml_ và dán nội dung sau. Thay thế `PRIVATE_IP` bằng IP riêng bạn đã lấy được, và `silly.DOMAIN.com` bằng tên subdomain và trang domain của bạn, sau đó lưu tệp.

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

20. Chạy Docker Compose bằng các lệnh sau:
    ```sh
    cd /docker
    docker compose up -d
    ```
21. Vào thư mục SillyTavern của bạn và chỉnh sửa `config.yaml` để bật chế độ listen và xác thực cơ bản, trong khi vô hiệu hóa `whitelistMode`.

    ```yaml
    listen: yes
    whitelistMode: false
    basicAuthMode: true
    ```

    !!!warning Mẹo
    Đảm bảo thay đổi tên người dùng và mật khẩu mặc định thành thứ gì đó mạnh mà bạn có thể nhớ.
    !!!

    Hoặc để sử dụng các tài khoản SillyTavern làm tên người dùng và mật khẩu:

    ```yaml
    basicAuthMode: true
    enableUserAccounts: true
    perUserBasicAuth: true
    ```

    !!!warning Mẹo
    Trước khi bật perUserBasicAuth, hãy đảm bảo bạn có thiết lập đa người dùng hợp lệ với mật khẩu hoạt động.
    !!!

22. Đợi vài phút, sau đó mở trang domain bạn đã tạo cho ST. Cuối cùng, bạn sẽ có thể mở SillyTavern từ bất kỳ đâu bạn đi chỉ với một URL và một tài khoản.
    !!!info Mẹo
    Nếu không có gì xảy ra sau vài phút, hãy kiểm tra log container của Traefik để tìm bất kỳ lỗi có thể có nào.
    !!!
23. Tận hưởng! :D

### Linux (Docker SillyTavern)

!!!warning Lưu Ý
Lưu ý rằng chúng tôi chạy SillyTavern trên bare-metal thay vì Docker. Đây là một ý tưởng sơ bộ về những gì chúng tôi sẽ làm trên Docker với các Docker container khác mà chúng tôi có xu hướng sử dụng với ST.
!!!

1. Làm theo Bước 1-11 của **Linux (Bare-Metal SillyTavern)**.
2. Đăng nhập vào Cloudflare và nhấp vào Domain của bạn, tiếp theo là **Get your API token**.
3. Nhấp vào _Create Token_ sau đó _Create Custom Token_ và đảm bảo bạn cấp cho token của mình các quyền sau.
   !!!info Quyền Token
   **Zone -> DNS -> Edit**

    **Zone -> Zone -> Read**
    !!!

    Nhấp vào _Continue to summary_ tiếp theo là _Create Token._

4. Sao chép Token Key được cung cấp cho bạn và lưu trữ nó ở nơi an toàn.
5. `cd` vào `secrets/cloudflare` và sử dụng `nano` hoặc trình chỉnh sửa tương tự, tạo một tệp có tên **CF_DNS_API_KEY** và dán khóa của bạn vào bên trong.
6. Quay lại trang domain của bạn và vào **DNS**. Tạo một record mới bằng cách sử dụng **Add record** và tạo hai khóa loại _A_ như những khóa bên dưới. Thay thế `PUBLIC_IP` bằng IP công cộng của riêng bạn và domain ví dụ bằng domain của bạn, sau đó nhấp _Save_.

    | Type | Name (required) | Target (required) | Proxy Status | TTL  |
    |------|-----------------|-------------------|--------------|------|
    | A    | DOMAIN.com      | PUBLIC_IP         | Proxied      | Auto |
    | A    | www             | PUBLIC_IP         | Proxied      | Auto |

7. Tạo một record khác của loại **`CNAME`**, sau đó nhấp _Save_. Đây là một ví dụ về cách nó sẽ xuất hiện trên dashboard Cloudflare.

    | Type  | Name (required) | Target (required) | Proxy Status | TTL |
    |-------|-----------------|-------------------|--------------|-----|
    | CNAME | silly           | DOMAIN.com        | Proxied      | N/A |

8. Git clone SillyTavern vào thư mục `docker`.
    ```sh
    cd /docker && git clone https://github.com/SillyTavern/SillyTavern
    ```
9. Sử dụng `nano` hoặc trình chỉnh sửa tương tự, tạo một tệp có tên _docker-compose.yaml_ và dán nội dung sau. Thay thế `silly.DOMAIN.com` bằng subdomain bạn đã thêm ở trên, sau đó lưu tệp.

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

10. Chạy Docker Compose bằng các lệnh sau:
    ```sh
    docker compose up -d
    ```
11. Dừng container Docker SillyTavern.
    ```sh
    docker compose stop sillytavern
    ```
12. Vào thư mục SillyTavern của bạn (`appdata/sillytavern/config`) và chỉnh sửa `config.yaml` để bật chế độ listen và xác thực cơ bản, trong khi vô hiệu hóa `whitelistMode`.

    ```yaml
    listen: yes
    whitelistMode: false
    basicAuthMode: true
    ```

    !!!warning Mẹo
    Đảm bảo thay đổi tên người dùng và mật khẩu mặc định thành thứ gì đó mạnh mà bạn có thể nhớ.
    !!!

13. Khởi động lại container Docker SillyTavern.
    ```sh
    docker compose up -d sillytavern
    ```
14. Đợi vài phút, sau đó mở trang domain bạn đã tạo cho ST. Cuối cùng, bạn sẽ có thể mở SillyTavern từ bất kỳ đâu bạn đi chỉ với một URL và một tài khoản.
    !!!info Mẹo
    Nếu không có gì xảy ra sau vài phút, hãy kiểm tra log container của Traefik để tìm bất kỳ lỗi có thể có nào.
    !!!
15. Tận hưởng! :D

## Cập Nhật DNS Cloudflare của bạn

[**DDClient**](https://ddclient.net/) cho phép bạn đồng bộ hóa IP công cộng của mình với Cloudflare trong trường hợp ISP của bạn thay đổi nó, cho phép bạn tiếp tục truy cập instance ST của mình như thể không có gì xảy ra.
