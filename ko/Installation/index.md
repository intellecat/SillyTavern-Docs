---
order: 50
icon: package
expanded: true
route: /ko/installation/
label: 설치
---

# 설치

사용 중인 플랫폼에 맞는 설치 가이드를 따라주세요:

* [Windows](/Installation/Windows.md)
* [Linux and Mac](/Installation/LinuxMacOS.md)
* [Android](/Installation/Android.md)
* [Docker](/Installation/Docker.md)

## 브랜치

SillyTavern은 모든 사용자에게 원활한 경험을 제공하기 위해 2개의 브랜치 시스템을 사용하여 개발되고 있습니다.

* `release` -🌟 **대부분의 사용자에게 권장됩니다.** 가장 안정적이고 권장되는 브랜치로, 주요 릴리스가 푸시될 때만 업데이트됩니다. 대부분의 사용자에게 적합합니다. 일반적으로 한 달에 한 번 업데이트됩니다.
* `staging` - ⚠️ **일반적인 사용에는 권장되지 않습니다.** 이 브랜치는 최신 기능을 가지고 있지만, 언제든지 문제가 발생할 수 있으므로 주의하세요. 파워 유저 및 열성 사용자만을 위한 브랜치입니다. 하루에 여러 번 업데이트됩니다.

## Global / Standalone 모드

SillyTavern을 실행하는 두 가지 모드가 있으며, 구성 및 데이터 경로를 처리하는 방식이 다릅니다.

* **Standalone 모드** (기본값) - 서버 디렉터리에 있는 `config.yaml` 파일과 `data` 디렉터리를 사용합니다. 모든 데이터는 설치 경로 내에 제한됩니다. 대부분의 사용자에게 권장되는 모드입니다.
* **Global 모드** - 시스템 전체 경로를 구성 및 데이터에 사용합니다. SillyTavern을 패키지로 설치하거나 여러 설치 간에 동일한 구성 및 데이터를 공유하려는 경우 유용합니다.

!!!info
[공식 npm 패키지](https://www.npmjs.com/package/sillytavern)를 사용하여 설치한 경우 (예: `npx sillytavern@latest`) 기본적으로 global 모드에서 실행됩니다.
!!!

### 데이터 경로

**Standalone 모드** 경로는 SillyTavern 설치 디렉터리를 기준으로 합니다:

* **Config 경로**: `./config.yaml`
* **Data 루트**: `./data/`

**Global 모드** 경로는 OS에 따라 다릅니다:

* **Linux**: `~/.local/share/SillyTavern/config.yaml` (또는 `$XDG_DATA_HOME/SillyTavern/config.yaml`) 및 `~/.local/share/SillyTavern/data/` (또는 `$XDG_DATA_HOME/SillyTavern/data/`)
* **Windows**: `%APPDATA%\SillyTavern\config.yaml` 및 `%APPDATA%\SillyTavern\data\`
* **MacOS**: `~/Library/Application Support/SillyTavern/config.yaml` 및 `~/Library/Application Support/SillyTavern/data/`

### Global 모드로 실행하는 방법

!!!warning
global 모드에서 실행할 때는 [CLI 인수](../Administration/config-yaml.md#command-line-arguments) 또는 [config.yaml](../Administration/config-yaml.md)로 `dataRoot`와 `configPath`를 오버라이드할 수 없습니다.
!!!

1. 서버 시작 명령에 `--global` 인수를 전달합니다 (예: `node server.js --global`).
2. 셸 시작 스크립트에 `--global` 인수를 전달합니다 (예: `Start.bat --global` 또는 `./start.sh --global`).
3. `package.json` 파일의 `start:global` 스크립트를 사용합니다 (예: `npm run start:global`).
