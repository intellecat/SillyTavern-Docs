---
order: -50
route: /installation/updating/node/
---

# Node.js 업데이트 방법

보안 및 성능상의 이유로 Node.js 런타임을 최신 상태로 유지하는 것이 중요합니다. 아래는 운영 체제에 따라 Node.js를 업데이트하는 단계입니다.

[Node.js 공식 웹사이트](https://nodejs.org/en/about/previous-releases)에서 찾을 수 있는 최신 장기 지원(LTS) 버전을 사용하는 것이 좋습니다.

## 현재 Node.js 버전을 확인하는 방법

1. 터미널 또는 명령 프롬프트를 엽니다.
2. 다음 명령을 입력하고 Enter를 누릅니다:

```bash
node -v
```

## nvm (Node Version Manager) - 크로스 플랫폼

`nvm`을 사용하는 경우:

1. 터미널을 엽니다.
2. 다음 명령을 입력합니다:

[**Unix/Linux/macOS:**](https://github.com/nvm-sh/nvm)

```bash
nvm install --lts
nvm use --lts
```

[**Windows:**](https://github.com/coreybutler/nvm-windows)

```bash
nvm install lts
nvm use lts
```

## Windows - 일반 설치

1. Node.js [다운로드 페이지](https://nodejs.org/en/download/)로 이동합니다.
2. LTS 버전용 Windows 설치 프로그램을 다운로드합니다.
3. 설치 프로그램을 실행하고 프롬프트에 따라 설치를 완료합니다.

## Windows - SillyTavern Launcher

SillyTavern Launcher를 사용하여 설치한 경우:

1. SillyTavern Launcher를 엽니다.
2. `Toolbox / App Installer / Core Utilities / Install Node.js`로 이동합니다.

**또는:**

PowerShell에서 winget을 사용하여 수동으로 수행합니다:

```powershell
winget install --id=OpenJS.NodeJS.LTS  -e
```

## Android - Termux

1. Termux 앱을 엽니다.
2. 다음 명령을 입력합니다:

```bash
pkg update
pkg upgrade nodejs-lts
```

가상 키보드에서 `Y`를 눌러 업데이트 프로세스 중에 나타날 수 있는 프롬프트를 수락하는 것을 잊지 마세요.

## macOS - 일반 설치

1. Node.js [다운로드 페이지](https://nodejs.org/en/download/)로 이동합니다.
2. LTS 버전용 macOS 설치 프로그램을 다운로드합니다.
3. `.pkg` 파일을 실행하고 프롬프트에 따라 설치를 완료합니다.

## macOS - Homebrew

Homebrew가 설치된 경우 다음 명령으로 Node.js를 업데이트할 수 있습니다:

```bash
brew update
brew upgrade node
```

## Linux - 패키지 관리자

Linux에서 Node.js를 업데이트하는 방법은 배포판에 따라 다릅니다.

그러나 공식 저장소의 Node.js 버전이 최신이 아닐 수 있으므로 [Node Version Manager (nvm)](https://github.com/nvm-sh/nvm) 또는 [NodeSource 저장소](https://github.com/nodesource/distributions)를 사용하는 것이 좋습니다.

## Docker

조치가 필요하지 않습니다. 우리가 제공하는 사전 빌드된 Docker 이미지는 최신 버전의 Node.js로 컴파일됩니다.
