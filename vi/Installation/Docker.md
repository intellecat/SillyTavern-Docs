---
# icon: container
label: Docker
route: /installation/docker/
---

# Cài đặt Docker

!!!
Các hướng dẫn này giả định bạn đã cài đặt Docker, có thể truy cập dòng lệnh để cài đặt containers, và quen thuộc với hoạt động chung của chúng.
!!!

## Sử dụng GitHub Container Registry

Sử dụng image được xây dựng sẵn là cách nhanh nhất và dễ nhất để bắt đầu với SillyTavern trong Docker. Bạn có thể kéo image mới nhất từ GitHub Container Registry.

### Docker Compose (khuyến nghị)

Tải tệp `docker-compose.yml` từ [GitHub Repository](https://github.com/SillyTavern/SillyTavern/blob/release/docker/docker-compose.yml) và chạy lệnh sau trong thư mục chứa tệp đó. Lệnh này sẽ kéo image release mới nhất từ GitHub Container Registry và khởi động container, tự động tạo các volumes cần thiết.

```sh
docker compose up
```

Bạn có thể chỉnh sửa tệp và áp dụng tùy chỉnh bổ sung để phù hợp với nhu cầu của bạn:

- Cổng mặc định là 8000. Bạn có thể thay đổi nó bằng cách sửa đổi phần `ports`.
- Thay đổi tag `image` thành `staging` nếu bạn muốn sử dụng nhánh phát triển thay vì bản phát hành ổn định.
- Nếu bạn muốn điều chỉnh cấu hình máy chủ bằng biến môi trường, kiểm tra trang [Biến môi trường](/Administration/config-yaml.md#environment-variables).

### Docker CLI (nâng cao)

Bạn sẽ cần hai ánh xạ thư mục bắt buộc và một ánh xạ cổng để cho phép SillyTavern hoạt động. Trong lệnh, thay thế các lựa chọn của bạn ở các vị trí sau:

#### Biến Container

##### Ánh xạ Volume

- `CONFIG_PATH` - Thư mục nơi các tệp cấu hình SillyTavern sẽ được lưu trữ trên máy chủ của bạn
- `DATA_PATH` - Thư mục nơi dữ liệu người dùng SillyTavern (bao gồm nhân vật) sẽ được lưu trữ trên máy chủ của bạn
- `PLUGINS_PATH` - (tùy chọn) Thư mục nơi các plugin máy chủ SillyTavern sẽ được lưu trữ trên máy chủ của bạn
- `EXTENSIONS_PATH` - (tùy chọn) Thư mục nơi các tiện ích mở rộng UI toàn cục sẽ được lưu trữ trên máy chủ của bạn

##### Ánh xạ Cổng

- `PUBLIC_PORT` - Cổng để hiển thị lưu lượng. Đây là bắt buộc, vì bạn sẽ truy cập instance từ bên ngoài container máy ảo của nó. KHÔNG hiển thị điều này ra internet mà không triển khai dịch vụ riêng biệt cho bảo mật.

##### Cài đặt bổ sung

- `SILLYTAVERN_VERSION` - Trên [trang GitHub Packages](https://github.com/SillyTavern/SillyTavern/pkgs/container/sillytavern) bạn sẽ thấy danh sách các phiên bản image được gắn thẻ. Tag image "latest" sẽ giữ cho bạn cập nhật với bản phát hành hiện tại. Bạn cũng có thể sử dụng "staging" trỏ đến image hàng đêm của nhánh tương ứng.

#### Chạy container

1. Mở Dòng lệnh của bạn
2. Chạy lệnh sau trong thư mục nơi bạn muốn lưu trữ các tệp cấu hình và dữ liệu:

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
Theo mặc định, container sẽ chạy ở chế độ foreground. Nếu bạn muốn chạy nó ở chế độ background, thêm cờ `-d` vào lệnh `docker run`.
!!!

## Xây dựng Docker Image

!!!info
Phần sau giả định bạn đã cài đặt SillyTavern trong thư mục không phải root (không phải admin). Nếu bạn cài đặt SillyTavern trong thư mục root, bạn có thể phải chạy một số lệnh này với quyền quản trị viên [`sudo`, `doas`, Command Prompt (Administrator)].
!!!

Nếu bạn muốn tự xây dựng Docker image, bạn có thể làm như vậy bằng cách làm theo các bước sau. Điều này hữu ích nếu bạn muốn tùy chỉnh image hoặc sử dụng nó cho mục đích phát triển.

### Linux

1. Cài đặt Docker bằng cách làm theo hướng dẫn cài đặt Docker [tại đây](https://docs.docker.com/engine/install/).
   !!!danger
   **Không** cài đặt Docker Desktop.
   !!!
2. Làm theo các bước trong **Manage Docker as a non-root user** trong [Hướng dẫn Sau cài đặt](https://docs.docker.com/engine/install/linux-postinstall/) của Docker.
3. Cài đặt [Git](https://git-scm.com/download/linux) bằng trình quản lý gói của bạn.

    - Debian (Ubuntu/Pop! OS/etc.)

        ```sh
        sudo apt install git
        ```

    - Arch Linux (Manjaro/EndeavourOS/etc.)

        ```sh
        sudo pacman -S git
        ```

    - Fedora, Red Hat Enterprise Linux (RHEL), etc.
        ```sh
        sudo dnf install git
        ```

4. Clone repository SillyTavern.

    - Release (Nhánh ổn định)

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    - Staging (Nhánh phát triển)
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

5. Thực thi `docker compose` bằng cách chạy lệnh sau trong thư mục Docker.

    ```sh
    docker compose up -d
    ```

6. Mở trình duyệt mới và truy cập [http://localhost:8000](http://localhost:8000). Bạn sẽ thấy SillyTavern tải trong vài giây.

### Windows

!!!warning Về Docker trên Windows
Sử dụng Docker trên Windows **_thực sự_** phức tạp. Bạn không chỉ cần kích hoạt _Windows Subsystem for Linux_ trong _Turn Windows features on or off_, mà còn phải cấu hình hệ thống của bạn cho Virtualization (Intel VT-d/AMD SVM) khác nhau tùy theo nhà sản xuất PC hoặc bo mạch chủ. Đôi khi, tùy chọn này không có trên một số hệ thống.

Rất khuyến khích bạn cài đặt SillyTavern bằng cách làm theo hướng dẫn [Windows](/Installation/Windows.md) của chúng tôi. Phần này là ý tưởng _sơ bộ_ về cách thực hiện trên Windows.
!!!

1.  Cài đặt Docker Desktop bằng cách làm theo hướng dẫn cài đặt Docker [tại đây](https://docs.docker.com/desktop/setup/install/windows-install/).
2.  Cài đặt [Git for Windows](https://git-scm.com/download/win).
3.  Clone repository SillyTavern.

    -   Release (Nhánh ổn định)

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    -   Staging (Nhánh phát triển)
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

4.  Thực thi `docker compose` bằng cách chạy lệnh sau trong thư mục Docker.

    ```sh
    docker compose up -d
    ```

5.  Mở trình duyệt mới và truy cập [http://localhost:8000](http://localhost:8000). Bạn sẽ thấy SillyTavern tải trong vài giây.

### macOS

!!!
Mặc dù macOS tương tự như Linux, nó không có Docker Engine. Bạn sẽ phải cài đặt Docker Desktop tương tự như Windows.
Bạn cũng sẽ cần cài đặt [Homebrew](https://brew.sh/) để cài đặt Git trên Mac của bạn. Phần này là ý tưởng _sơ bộ_ về cách thực hiện trên macOS.
!!!

1.  Cài đặt Docker Desktop bằng cách làm theo hướng dẫn cài đặt Docker [tại đây](https://docs.docker.com/desktop/setup/install/mac-install/).
2.  Cài đặt `git` bằng Homebrew.

    ```sh
    brew install git
    ```

3.  Clone repository SillyTavern.

    -   Release (Nhánh ổn định)

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    -   Staging (Nhánh phát triển)
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

4.  Thực thi `docker compose` bằng cách chạy lệnh sau trong thư mục Docker.

    ```sh
    docker compose up -d
    ```

5.  Mở trình duyệt mới và truy cập [http://localhost:8000](http://localhost:8000). Bạn sẽ thấy SillyTavern tải trong vài giây.

## Cấu hình SillyTavern

Tệp cấu hình của SillyTavern (config.yaml) sẽ nằm trong thư mục `config`. Cấu hình tệp config không khác gì cấu hình nó mà không có Docker, tuy nhiên bạn sẽ cần chạy `nano` hoặc trình soạn thảo code với quyền quản trị viên để lưu các thay đổi của bạn.

!!!warning
Đừng quên khởi động lại Docker container cho SillyTavern để áp dụng các thay đổi của bạn! Đảm bảo bạn thực thi lệnh này trong thư mục `docker`.

```sh
docker compose restart sillytavern
```

!!!

## Định vị dữ liệu người dùng

Thư mục data của SillyTavern sẽ nằm trong thư mục `data`. Sao lưu các tệp của bạn sẽ dễ dàng thực hiện, tuy nhiên, khôi phục hoặc thêm nội dung vào đó có thể yêu cầu bạn làm như vậy với quyền quản trị viên.

## Chạy Server Plugins

Chạy các plugin như [HoYoWiki-Scraper-TS](https://github.com/Bronya-Rand/HoYoWiki-Scraper-TS) hoặc [SillyTavern-Fandom-Scraper](https://github.com/SillyTavern/SillyTavern-Fandom-Scraper) trong Docker không khác gì chạy nó trên hệ thống của bạn mà không có Docker, tuy nhiên chúng ta sẽ cần thực hiện một sửa đổi nhỏ cho script Docker Compose để làm như vậy.

!!! Note
Nếu bạn đã thấy thư mục _plugins_ trong thư mục `docker`, bạn có thể bỏ qua Bước 1-2.
!!!

1. Sử dụng `nano` hoặc trình soạn thảo code, mở _docker-compose.yml_ và thêm dòng sau bên dưới `volumes`.

    ```sh
        volumes:
            - "./config:/home/node/app/config"
            - "./data:/home/node/app/data"
            - "./plugins:/home/node/app/plugins"
    ```

2. Tạo một thư mục mới trong thư mục `docker` có tên _plugins_.
3. Làm theo hướng dẫn của plugin về cách cài đặt plugin.
4. Sử dụng `nano` hoặc trình soạn thảo code với quyền quản trị viên, mở _config.yaml_ (trong thư mục `config`) và bật `enableServerPlugins`

    ```sh
    enableServerPlugins: true
    ```

5. Khởi động lại Docker container.

    ```sh
    docker compose restart sillytavern
    ```

## Vấn đề phổ biến với Docker

### Vấn đề quyền SELinux với Volumes được gắn kết

Các bản phân phối Linux có bật SELinux (như RHEL, CentOS, Fedora, v.v.) có thể ngăn các Docker containers truy cập các volumes được gắn kết do chính sách bảo mật. Điều này có thể dẫn đến lỗi từ chối quyền khi container cố gắng đọc hoặc ghi vào các thư mục được gắn kết.

Hai hậu tố `:z` hoặc `:Z` có thể được thêm vào mount volume. Các hậu tố này yêu cầu Docker gắn nhãn lại các đối tượng tệp trên các volumes được chia sẻ.

- Tùy chọn `z` được sử dụng khi nội dung volume sẽ được chia sẻ giữa các containers.
- Tùy chọn `Z` được sử dụng khi nội dung volume chỉ nên được sử dụng bởi container hiện tại.

Ví dụ:

```yaml
# docker-compose.yml
volumes:
  ## Shared volume
  - ./config:/home/node/app/config:z
  ## Private volume
  - ./data:/home/node/app/data:Z
```

### Bị cấm bởi Whitelist

!!!
Các IP gateway của Docker nên được đưa vào danh sách trắng tự động nếu giá trị cấu hình [whitelistDockerHosts](/Administration/config-yaml.md#ip-whitelisting) được đặt thành `true`.

Nếu bạn vẫn không thể truy cập SillyTavern, hãy làm theo hướng dẫn dưới đây để cập nhật danh sách trắng theo cách thủ công.
!!!

1. Thực thi lệnh Docker sau để lấy IP của Docker container SillyTavern của bạn.

    ```sh
    docker network inspect docker_default
    ```

    Bạn sẽ nhận được một số đầu ra tương tự như sau.

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

    Sao chép IP bạn thấy trong _Gateway_ vì điều này sẽ quan trọng.

2. Chạy trình soạn thảo văn bản bạn chọn với quyền quản trị viên, đi đến `config` và mở `config.yaml`.

    Trong trình soạn thảo của bạn, đi xuống phần `whitelist`. Bạn sẽ thấy một cái gì đó tương tự như sau.

    ```yaml
    whitelist:
        - 127.0.0.1
    ```

    Thêm một dòng mới bên dưới _127.0.0.1_ và đặt IP bạn đã sao chép từ Docker. Nó sẽ trông giống như sau sau đó.

    ```yaml
    whitelist:
        - 127.0.0.1
        - 172.18.0.1
    ```

    Lưu tệp và thoát khỏi trình soạn thảo văn bản.

    !!!info
    Lưu ý rằng nếu bạn cấu hình mạng Docker như một bridge, bạn cũng có thể thêm các địa chỉ IP bên ngoài vào danh sách trắng như thường lệ.
    !!!

3. Khởi động lại Docker Container để áp dụng cấu hình mới.

    ```sh
    docker compose restart sillytavern
    ```
