---
label: Android (Termux)
route: /ko/installation/android-(termux)/
---

# Android (Termux) 설치

SillyTavern은 Termux를 사용하여 Android 기기에서 기본적으로 실행할 수 있습니다.

## Termux 설치

!!!tip
Google Play 스토어에서 Termux를 설치하지 마세요. 해당 버전은 더 이상 유지 관리되지 않습니다.
대신 F-Droid (권장) 또는 GitHub 릴리스를 사용하여 최신 버전을 받으세요.
!!!

1. [F-Droid](https://f-droid.org/en/packages/com.termux/) 또는 [GitHub 릴리스](https://github.com/termux/termux-app/releases)에서 Termux를 다운로드합니다.
2. 다운로드한 APK 파일을 설치합니다.
3. Termux를 열고 첫 번째 명령을 실행합니다:

   ```bash
   termux-change-repo
   ```

4. "Mirror group"을 선택하고 가장 가까운 서버를 선택합니다. 화면을 터치하거나 [Unexpected Keyboard](https://play.google.com/store/apps/details?id=juloo.keyboard2&hl=en)로 스와이프 제스처를 사용할 수 있습니다.
5. Termux 업데이트:

   ```bash
   pkg update && pkg upgrade
   ```

## 종속성 설치

필요한 패키지를 설치합니다:

```bash
pkg install git nodejs-lts nano
```

!!!warning
32비트 Android를 실행 중인 경우 추가 단계는 아래의 [일반적인 오류](#common-errors) 섹션을 참조하세요.
!!!

## SillyTavern 설치

SillyTavern 저장소 복제 ([브랜치 선택 방법](/Installation/index.md#branches)):

- **Release 브랜치:**

    ```bash
    git clone https://github.com/SillyTavern/SillyTavern -b release
    ```

- **Staging 브랜치:**

    ```bash
    git clone https://github.com/SillyTavern/SillyTavern -b staging
    ```

## SillyTavern 실행

SillyTavern을 실행하려면 복제된 디렉터리로 이동하여 시작 스크립트를 실행합니다:

```bash
cd ~/SillyTavern
bash start.sh
```

SillyTavern을 업데이트하려면 SillyTavern 디렉터리로 이동하여 실행합니다:

```bash
cd ~/SillyTavern
git pull --rebase --autostash
```

이 프로세스를 단순화하기 위한 바로 가기 생성은 아래의 [별칭](#optional-create-aliases) 섹션을 참조하세요.

## 일반적인 오류

### Unsupported platform: android arm LEtime-web

32비트 Android는 npm으로 설치할 수 없는 외부 종속성이 필요합니다.

다음 명령을 사용하여 설치합니다:

```bash
pkg install esbuild
```

그런 다음 위의 설치 단계를 진행합니다.

### 성능 조정

!!!info
성능 향상에 대한 일반적인 팁은 해당 [FAQ 섹션](/Usage/faq.md#performance-tips)을 참조하세요.
!!!

Android 기기의 하드웨어 제한으로 인해 더 나은 메모리, 스토리지 및 CPU 사용을 위해 다음 SillyTavern [config.yaml](/Administration/config-yaml.md) 설정을 조정할 수 있습니다:

```yaml
performance:
  # 필요할 때까지 모든 캐릭터 데이터 로딩 방지
  lazyLoadCharacters: true
  # 스토리지 사용량을 줄이기 위해 디스크 캐싱 비활성화
  useDiskCache: false
backups:
  chat:
    # 선택 사항: 스토리지 절약을 위해 자동 채팅 백업 비활성화
    enabled: false
```

!!!tip
Termux에 포함된 `nano` 텍스트 에디터를 사용하여 `config.yaml` 파일을 편집합니다: `nano ~/SillyTavern/config.yaml`
!!!

## 선택 사항: 별칭 생성

워크플로를 더 쉽게 만들기 위해 일반 명령에 대한 바로 가기를 만들 수 있습니다.

1. `.bashrc` 파일을 수정하기 위해 에디터를 엽니다:

   ```bash
   nano ~/.bashrc
   ```

2. 별칭을 만들기 위해 다음 줄을 추가합니다:

   ```bash
   # Termux 패키지 업데이트
   alias pkgup="pkg update && pkg upgrade"
   # SillyTavern 시작
   alias st='cd ~/SillyTavern && bash start.sh'
   # SillyTavern 업데이트
   alias stup='cd ~/SillyTavern && git pull --rebase --autostash'
   ```

3. 파일을 저장하고 에디터를 종료합니다 (nano에서는 `CTRL + X`를 누른 다음 `Y`를 누르고 `Enter`를 누릅니다).

4. 변경 사항을 적용하려면 다음을 실행합니다:

   ```bash
   source ~/.bashrc
   ```

이제 다음 명령을 사용할 수 있습니다:

- `st` - SillyTavern 시작
- `stup` - SillyTavern 업데이트
- `pkgup` - Termux 패키지 업데이트

## 추가 자료

!!!info
아래 링크된 가이드는 SillyTavern 팀에서 유지 관리하지 않습니다.
!!!

- ArroganceComplex#2659의 Termux에서 SillyTavern 가이드: <https://rentry.org/STAI-Termux>
- Material Files로 Termux 파일 액세스: <https://www.learntermux.tech/2020/10/Termux-File-Manager.html>
- Termux 프로세스 딥 슬립 방지: <https://wiki.termux.com/wiki/Termux-wake-lock>
