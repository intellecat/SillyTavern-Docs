---
# icon: container
label: Docker
route: /ko/installation/docker/
---

# Docker 설치

!!!
이 지침은 Docker를 설치했고, 컨테이너 설치를 위해 명령줄에 액세스할 수 있으며, 일반적인 작동에 익숙하다고 가정합니다.
!!!

## GitHub Container Registry 사용

사전 빌드된 이미지를 사용하는 것이 Docker에서 SillyTavern을 시작하는 가장 빠르고 쉬운 방법입니다. GitHub Container Registry에서 최신 이미지를 가져올 수 있습니다.

### Docker Compose (권장)

[GitHub Repository](https://github.com/SillyTavern/SillyTavern/blob/release/docker/docker-compose.yml)에서 `docker-compose.yml` 파일을 다운로드하고 파일이 있는 디렉터리에서 다음 명령을 실행합니다. 이렇게 하면 GitHub Container Registry에서 최신 릴리스 이미지를 가져오고 컨테이너를 시작하여 필요한 볼륨을 자동으로 생성합니다.

```sh
docker compose up
```

필요에 맞게 파일을 편집하고 추가 사용자 정의를 적용할 수 있습니다:

- 기본 포트는 8000입니다. `ports` 섹션을 수정하여 변경할 수 있습니다.
- 안정적인 릴리스 대신 개발 브랜치를 사용하려면 `image` 태그를 `staging`으로 변경하세요.
- 환경 변수를 사용하여 서버 구성을 조정하려면 [환경 변수](/Administration/config-yaml.md#environment-variables) 페이지를 확인하세요.

### Docker CLI (고급)

SillyTavern이 작동하려면 두 개의 필수 디렉터리 매핑과 포트 매핑이 필요합니다. 명령에서 다음 위치의 선택 사항을 바꿉니다:

#### 컨테이너 변수

##### 볼륨 매핑

- `CONFIG_PATH` - 호스트 머신에 SillyTavern 구성 파일이 저장될 디렉터리
- `DATA_PATH` - 호스트 머신에 SillyTavern 사용자 데이터(캐릭터 포함)가 저장될 디렉터리
- `PLUGINS_PATH` - (선택 사항) 호스트 머신에 SillyTavern 서버 플러그인이 저장될 디렉터리
- `EXTENSIONS_PATH` - (선택 사항) 호스트 머신에 전역 UI 확장이 저장될 디렉터리

##### 포트 매핑

- `PUBLIC_PORT` - 트래픽을 노출할 포트입니다. 가상 머신 컨테이너 외부에서 인스턴스에 액세스하므로 필수입니다. 보안을 위한 별도의 서비스를 구현하지 않고는 인터넷에 노출하지 마세요.

##### 추가 설정

- `SILLYTAVERN_VERSION` - [GitHub Packages 페이지](https://github.com/SillyTavern/SillyTavern/pkgs/container/sillytavern)에서 태그가 지정된 이미지 버전 목록을 볼 수 있습니다. "latest" 이미지 태그는 현재 릴리스를 최신 상태로 유지합니다. 각 브랜치의 나이틀리 이미지를 가리키는 "staging"도 사용할 수 있습니다.

#### 컨테이너 실행

1. 명령줄 열기
2. 구성 및 데이터 파일을 저장할 폴더에서 다음 명령을 실행합니다:

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
기본적으로 컨테이너는 포그라운드에서 실행됩니다. 백그라운드에서 실행하려면 `docker run` 명령에 `-d` 플래그를 추가하세요.
!!!

## Docker 이미지 빌드

!!!info
다음 섹션은 SillyTavern을 루트가 아닌(관리자가 아닌) 폴더에 설치했다고 가정합니다. SillyTavern을 루트 폴더에 설치한 경우 이러한 명령 중 일부를 관리자 권한 [`sudo`, `doas`, Command Prompt (Administrator)]으로 실행해야 할 수 있습니다.
!!!

Docker 이미지를 직접 빌드하려면 다음 단계를 따르세요. 이미지를 사용자 정의하거나 개발 목적으로 사용하려는 경우 유용합니다.

### Linux

1. [여기](https://docs.docker.com/engine/install/)의 Docker 설치 가이드에 따라 Docker를 설치합니다.
   !!!danger
   Docker Desktop을 설치**하지 마세요**.
   !!!
2. Docker [설치 후 가이드](https://docs.docker.com/engine/install/linux-postinstall/)의 **루트가 아닌 사용자로 Docker 관리** 단계를 따릅니다.
3. 패키지 관리자를 사용하여 [Git](https://git-scm.com/download/linux)을 설치합니다.

    - Debian (Ubuntu/Pop! OS/등)

        ```sh
        sudo apt install git
        ```

    - Arch Linux (Manjaro/EndeavourOS/등)

        ```sh
        sudo pacman -S git
        ```

    - Fedora, Red Hat Enterprise Linux (RHEL) 등
        ```sh
        sudo dnf install git
        ```

4. SillyTavern 저장소를 복제합니다.

    - Release (안정 브랜치)

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    - Staging (개발 브랜치)
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

5. Docker 폴더 내에서 다음 명령을 실행하여 `docker compose`를 실행합니다.

    ```sh
    docker compose up -d
    ```

6. 새 브라우저를 열고 [http://localhost:8000](http://localhost:8000)으로 이동합니다. 잠시 후 SillyTavern이 로드되는 것을 볼 수 있습니다.

### Windows

!!!warning Windows에서 Docker 사용에 관하여
Windows에서 Docker를 사용하는 것은 **_정말_** 복잡합니다. _Windows 기능 켜기/끄기_에서 _Windows Subsystem for Linux_를 활성화해야 할 뿐만 아니라, PC 제조업체 또는 PC 제조업체(또는 마더보드 제조업체)에 따라 다른 가상화(Intel VT-d/AMD SVM)를 위해 시스템을 구성해야 합니다. 때로는 일부 시스템에서 이 옵션이 없습니다.

[Windows](/Installation/Windows.md) 가이드를 따라 SillyTavern을 설치하는 것이 좋습니다. 이 섹션은 Windows에서 수행할 수 있는 방법에 대한 _대략적인_ 아이디어입니다.
!!!

1.  [여기](https://docs.docker.com/desktop/setup/install/windows-install/)의 Docker 설치 가이드에 따라 Docker Desktop을 설치합니다.
2.  [Git for Windows](https://git-scm.com/download/win)를 설치합니다.
3.  SillyTavern 저장소를 복제합니다.

    -   Release (안정 브랜치)

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    -   Staging (개발 브랜치)
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

4.  Docker 폴더 내에서 다음 명령을 실행하여 `docker compose`를 실행합니다.

    ```sh
    docker compose up -d
    ```

5.  새 브라우저를 열고 [http://localhost:8000](http://localhost:8000)으로 이동합니다. 잠시 후 SillyTavern이 로드되는 것을 볼 수 있습니다.

### macOS

!!!
macOS는 Linux와 유사하지만 Docker Engine이 없습니다. Windows와 유사하게 Docker Desktop을 설치해야 합니다.
Mac에 Git를 설치하려면 [Homebrew](https://brew.sh/)도 설치해야 합니다. 이 섹션은 macOS에서 수행할 수 있는 방법에 대한 _대략적인_ 아이디어입니다.
!!!

1.  [여기](https://docs.docker.com/desktop/setup/install/mac-install/)의 Docker 설치 가이드에 따라 Docker Desktop을 설치합니다.
2.  Homebrew를 사용하여 `git`을 설치합니다.

    ```sh
    brew install git
    ```

3.  SillyTavern 저장소를 복제합니다.

    -   Release (안정 브랜치)

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    -   Staging (개발 브랜치)
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

4.  Docker 폴더 내에서 다음 명령을 실행하여 `docker compose`를 실행합니다.

    ```sh
    docker compose up -d
    ```

5.  새 브라우저를 열고 [http://localhost:8000](http://localhost:8000)으로 이동합니다. 잠시 후 SillyTavern이 로드되는 것을 볼 수 있습니다.

## SillyTavern 구성

SillyTavern의 구성 파일(config.yaml)은 `config` 폴더 내에 있습니다. config 파일 구성은 Docker 없이 구성하는 것과 다르지 않지만, 변경 사항을 저장하려면 관리자 권한으로 `nano` 또는 코드 에디터를 실행해야 합니다.

!!!warning
변경 사항을 적용하려면 SillyTavern용 Docker 컨테이너를 다시 시작하는 것을 잊지 마세요! `docker` 폴더 내에서 이 명령을 실행해야 합니다.

```sh
docker compose restart sillytavern
```

!!!

## 사용자 데이터 찾기

SillyTavern의 data 폴더는 `data` 폴더 내에 있습니다. 파일 백업은 쉽게 할 수 있지만, 복원하거나 콘텐츠를 추가하려면 관리자 권한으로 수행해야 할 수 있습니다.

## 서버 플러그인 실행

Docker 내에서 [HoYoWiki-Scraper-TS](https://github.com/Bronya-Rand/HoYoWiki-Scraper-TS) 또는 [SillyTavern-Fandom-Scraper](https://github.com/SillyTavern/SillyTavern-Fandom-Scraper)와 같은 플러그인을 실행하는 것은 Docker 없이 시스템에서 실행하는 것과 다르지 않지만, 이를 위해 Docker Compose 스크립트를 약간 수정해야 합니다.

!!! Note
`docker` 폴더 내에 이미 _plugins_ 폴더가 보이면 1-2단계를 건너뛸 수 있습니다.
!!!

1. `nano` 또는 코드 에디터를 사용하여 _docker-compose.yml_을 열고 `volumes` 아래에 다음 줄을 추가합니다.

    ```sh
        volumes:
            - "./config:/home/node/app/config"
            - "./data:/home/node/app/data"
            - "./plugins:/home/node/app/plugins"
    ```

2. `docker` 폴더 내에 _plugins_라는 새 폴더를 만듭니다.
3. 플러그인 설치에 대한 플러그인의 지침을 따릅니다.
4. 관리자 권한으로 `nano` 또는 코드 에디터를 사용하여 (`config` 폴더 내의) _config.yaml_을 열고 `enableServerPlugins`를 활성화합니다

    ```sh
    enableServerPlugins: true
    ```

5. Docker 컨테이너를 다시 시작합니다.

    ```sh
    docker compose restart sillytavern
    ```

## 루트가 아닌 사용자 모드

기본적으로 컨테이너는 root로 실행됩니다. 마운트된 볼륨에서 생성된 파일이 특정 호스트 사용자 소유가 되도록 하려면(예를 들어 root 소유 파일을 방지하기 위해) 루트가 아닌 사용자 모드를 활성화할 수 있습니다.

### 옵션 1: PUID/PGID (권장)

컨테이너가 사용할 UID/GID로 `PUID` 및 `PGID` 환경 변수를 설정합니다. 진입점(entrypoint)이 필요한 디렉터리의 소유권을 업데이트한 다음 매핑된 사용자로 서버를 실행합니다.

Docker Compose 예시:

```yaml
services:
  sillytavern:
    environment:
      - PUID=1000
      - PGID=1000
```

Docker CLI 예시:

```bash
docker run \
  --name="sillytavern" \
  -e PUID=1000 \
  -e PGID=1000 \
  -p "$PUBLIC_PORT:8000/tcp" \
  -v "$CONFIG_PATH:/home/node/app/config:rw" \
  -v "$DATA_PATH:/home/node/app/data:rw" \
  -v "$EXTENSIONS_PATH:/home/node/app/public/scripts/extensions/third-party:rw" \
  -v "$PLUGINS_PATH:/home/node/app/plugins:rw" \
  ghcr.io/sillytavern/sillytavern:"$SILLYTAVERN_VERSION"
```

### 옵션 2: Docker `--user` 플래그

Docker의 `--user` 플래그를 사용하여 특정 사용자로 컨테이너를 실행할 수도 있습니다. 이 모드에서는 컨테이너가 권한을 자동으로 수정할 수 없으므로, 마운트된 볼륨이 제공한 UID/GID로 이미 쓰기 가능한 상태인지 확인하십시오.

```bash
docker run \
  --name="sillytavern" \
  --user 1000:1000 \
  -p "$PUBLIC_PORT:8000/tcp" \
  -v "$CONFIG_PATH:/home/node/app/config:rw" \
  -v "$DATA_PATH:/home/node/app/data:rw" \
  -v "$EXTENSIONS_PATH:/home/node/app/public/scripts/extensions/third-party:rw" \
  -v "$PLUGINS_PATH:/home/node/app/plugins:rw" \
  ghcr.io/sillytavern/sillytavern:"$SILLYTAVERN_VERSION"
```

## 컨테이너 상태 확인(Healthcheck)

Docker 이미지에는 SillyTavern 서버의 응답성을 모니터링하는 내장 상태 확인(healthcheck) 메커니즘이 포함되어 있습니다. 이는 응답하지 않는 컨테이너를 감지하고 자동으로 재시작하기 위해 Docker Compose, Kubernetes, Docker Swarm과 같은 컨테이너 오케스트레이션 시스템에 유용합니다.

### 작동 방식

상태 확인은 하트비트 파일 메커니즘을 사용합니다:

1. 활성화되면 SillyTavern 서버는 데이터 디렉터리의 `heartbeat.json` 파일에 주기적으로 타임스탬프를 기록합니다.
2. 상태 확인 스크립트(`src/healthcheck.js`)는 하트비트 파일이 존재하고 최근에 업데이트되었는지 확인합니다.
3. 하트비트 파일이 없거나 너무 오래된 경우(2번 이상의 간격을 놓친 경우) 컨테이너는 비정상(unhealthy) 상태로 표시됩니다.

### 구성

!!!warning
상태 확인 스크립트는 명령줄 인수를 통한 데이터 디렉터리 재정의를 지원하지 않습니다. 기본값인 `/home/node/app/data`에서 데이터 디렉터리를 변경하는 경우 `SILLYTAVERN_DATAROOT` 환경 변수가 그에 맞게 설정되어 있는지 확인하십시오.
!!!

상태 확인은 `SILLYTAVERN_HEARTBEATINTERVAL` 환경 변수(또는 config.yaml의 `heartbeatInterval`)로 제어됩니다. 이 값은 하트비트 기록 사이의 간격(초)을 지정합니다.

- **기본값:** `0` (비활성화)
- **권장값:** Docker 상태 확인을 사용할 때 `30`초

기본 `docker-compose.yml` 파일에는 하트비트가 활성화된 상태 확인 구성이 포함되어 있습니다:

```yaml
services:
  sillytavern:
    environment:
      - SILLYTAVERN_HEARTBEATINTERVAL=30
    healthcheck:
      test: ["CMD", "node", "src/healthcheck.js"]
      interval: 30s
      timeout: 10s
      start_period: 20s
      retries: 3
```

### 컨테이너 상태 확인하기

다음을 사용하여 컨테이너의 상태를 확인할 수 있습니다:

```sh
docker inspect --format='{{.State.Health.Status}}' sillytavern
```

또는 상태를 포함한 전체 컨테이너 상태를 확인합니다:

```sh
docker ps
```

`STATUS` 열에 가동 시간과 함께 `healthy`, `unhealthy`, 또는 `starting`이 표시됩니다.

### 상태 확인 비활성화

상태 확인 기능이 필요하지 않은 경우 다음과 같이 비활성화할 수 있습니다:

1. 환경 변수를 `0`으로 설정합니다:

    ```yaml
    environment:
      - SILLYTAVERN_HEARTBEATINTERVAL=0
    ```

2. `docker-compose.yml`에서 `healthcheck` 섹션을 제거하거나 주석 처리합니다.

## Docker의 일반적인 문제

### 마운트된 볼륨의 SELinux 권한 문제

SELinux가 활성화된 Linux 배포판(RHEL, CentOS, Fedora 등)은 보안 정책으로 인해 Docker 컨테이너가 마운트된 볼륨에 액세스하지 못하도록 할 수 있습니다. 이로 인해 컨테이너가 마운트된 디렉터리를 읽거나 쓰려고 할 때 권한 거부 오류가 발생할 수 있습니다.

볼륨 마운트에 두 개의 접미사 `:z` 또는 `:Z`를 추가할 수 있습니다. 이러한 접미사는 Docker에 공유 볼륨의 파일 객체에 레이블을 다시 지정하도록 지시합니다.

- `z` 옵션은 볼륨 콘텐츠가 컨테이너 간에 공유될 때 사용됩니다.
- `Z` 옵션은 볼륨 콘텐츠가 현재 컨테이너에서만 사용되어야 할 때 사용됩니다.

예시:

```yaml
# docker-compose.yml
volumes:
  ## 공유 볼륨
  - ./config:/home/node/app/config:z
  ## 프라이빗 볼륨
  - ./data:/home/node/app/data:Z
```

### 화이트리스트에 의해 금지됨

!!!warning Docker Desktop 대 Docker CE
[whitelistDockerHosts](/Administration/config-yaml.md#ip-whitelisting) 구성 옵션(기본적으로 활성화됨)은 `host.docker.internal` 및 `gateway.docker.internal` 호스트 이름을 확인하여 작동합니다. 이 호스트 이름은 **Docker Desktop(Windows/Mac)에서만 사용 가능**합니다. **Linux에서 Docker CE**를 사용하는 경우 이러한 호스트 이름이 확인되지 않으며 자동 화이트리스트 추가가 컨테이너 로그에 다음과 같은 오류와 함께 실패합니다:

```
Failed to resolve whitelist hostname host.docker.internal: getaddrinfo ENOTFOUND host.docker.internal
Failed to resolve whitelist hostname gateway.docker.internal: getaddrinfo ENOTFOUND gateway.docker.internal
```

이 경우 아래 설명된 대로 Docker 게이트웨이 IP를 화이트리스트에 수동으로 추가해야 합니다.
!!!

1. 다음 Docker 명령을 실행하여 SillyTavern Docker 컨테이너의 IP를 가져옵니다.

    ```sh
    docker network inspect docker_default
    ```

    다음과 유사한 출력을 받아야 합니다.

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

    _Gateway_에 표시되는 IP를 복사합니다. 이것은 중요합니다.

2. 관리자 권한으로 선택한 텍스트 에디터를 실행하여 `config`로 이동하여 `config.yaml`을 엽니다.

    에디터 내에서 `whitelist` 섹션으로 이동합니다. 다음과 유사한 것을 볼 수 있습니다.

    ```yaml
    whitelist:
        - 127.0.0.1
    ```

    _127.0.0.1_ 아래에 새 줄을 추가하고 Docker에서 복사한 IP를 입력합니다. 나중에 다음과 유사하게 보일 것입니다.

    ```yaml
    whitelist:
        - 127.0.0.1
        - 172.18.0.1
    ```

    파일을 저장하고 텍스트 에디터를 종료합니다.

    !!!info
    Docker 네트워크를 브리지로 구성한 경우 평소와 같이 외부 IP 주소를 화이트리스트에 추가할 수도 있습니다.
    !!!

3. 새 구성을 적용하려면 Docker 컨테이너를 다시 시작합니다.

    ```sh
    docker compose restart sillytavern
    ```
