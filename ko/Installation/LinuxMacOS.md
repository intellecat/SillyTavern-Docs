---
label: MacOS & Linux
order: 5
route: /ko/installation/linuxmacos/
---

# Linux/MacOS 설치

## 수동 Git 설치

MacOS / Linux의 경우 모든 작업은 터미널에서 수행됩니다.

1. git 및 nodeJS 설치 (방법은 OS에 따라 다릅니다)
2. 저장소 복제

   - Release 브랜치의 경우: `git clone https://github.com/SillyTavern/SillyTavern -b release`
   - Staging 브랜치의 경우: `git clone https://github.com/SillyTavern/SillyTavern -b staging`

3. `cd SillyTavern`으로 설치 폴더로 이동합니다.
4. 다음 명령 중 하나로 `start.sh` 스크립트를 실행합니다:

- `./start.sh`
- `bash start.sh`

## SillyTavern Launcher

### Linux 사용자

1. 선호하는 터미널을 열고 git을 설치합니다
2. 다음 명령으로 Sillytavern Launcher를 다운로드합니다: `git clone https://github.com/SillyTavern/SillyTavern-Launcher.git`
3. 다음 명령으로 SillyTavern-Launcher로 이동합니다: `cd SillyTavern-Launcher`
4. 다음 명령으로 설치 런처를 시작하고 설치할 항목을 선택합니다: `chmod +x install.sh && ./install.sh`
5. 설치 후 다음 명령으로 런처를 시작합니다: `chmod +x launcher.sh && ./launcher.sh`

### Mac 사용자

1. 터미널을 열고 다음 명령으로 brew를 설치합니다: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
2. 다음 명령으로 git을 설치합니다: `brew install git`
3. 다음 명령으로 Sillytavern Launcher를 다운로드합니다: `git clone https://github.com/SillyTavern/SillyTavern-Launcher.git`
4. 다음 명령으로 SillyTavern-Launcher로 이동합니다: `cd SillyTavern-Launcher`
5. 다음 명령으로 설치 런처를 시작하고 설치할 항목을 선택합니다: `chmod +x install.sh && ./install.sh`
6. 설치 후 다음 명령으로 런처를 시작합니다: `chmod +x launcher.sh && ./launcher.sh`
