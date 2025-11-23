---
label: 업데이트
icon: repo-pull
order: -1
expanded: false
route: /installation/updating/
---

# SillyTavern 업데이트 방법

아래에서 사용 중인 OS를 찾아 ST 업데이트 지침을 따르세요.

!!! 설치 지침은 [설치](/Installation/index.md) 페이지를 참조하세요.

이 가이드는 이미 SillyTavern을 설치하고 최소한 한 번 실행했다고 가정합니다.
!!!

----

## Linux/Termux 또는 MacOS

git를 통해 설치했으므로 SillyTavern 디렉터리 내에서 'git pull'만 하면 됩니다.

- `cd SillyTavern`으로 올바른 폴더로 들어갑니다.
- `git pull`로 업데이트를 가져옵니다.
- `./start.sh` 또는 `bash start.sh`로 ST를 시작합니다.

----

## Windows

>먼저 SillyTavern 설치 기본 폴더에 있는 `UpdateAndStart.bat`를 사용해 보세요.

실패하면 여기로 돌아와서 계속 읽으세요.

### 방법 1 - GIT

항상 사용자에게 'git'를 사용하여 설치하는 것을 권장합니다. 이유는 다음과 같습니다:

`git clone`을 통해 설치한 경우 업데이트하려면 [ST 폴더의 명령줄에서](https://www.google.com/search?q=how+to+open+command+prompt+in+a+folder) `git pull`을 입력하기만 하면 됩니다.
또는 명령 프롬프트에 문제가 있고 GitHub Desktop이 설치된 경우 `Repository` 메뉴를 사용하고 `Pull`을 선택할 수 있습니다.

업데이트가 자동으로 안전하게 적용됩니다.

#### "도와주세요 원래 Zip으로 설치했는데 이제 Git 설치로 전환하고 싶습니다"

현명한 길을 선택하셨습니다.

Zip을 통해 설치했으므로 git을 사용하여 새로 설치해야 합니다.

다행히도 그렇게 하는 방법에 대한 [지침](/Installation/Windows.md)이 있습니다.

git을 사용하여 다른 폴더에 새 SillyTavern을 설치했으면 이 페이지로 돌아와서 아래 'Zip 업데이트' 지침의 **4단계**로 진행하세요.

### 방법 2 - ZIP

zip을 통해 설치하고 싶다면 업데이트를 위한 번거로운 프로세스는 다음과 같습니다:

1. 새 릴리스 zip을 다운로드합니다.
2. 현재 ST 설치 외부의 폴더에 압축을 풉니다.
3. OS에 대한 일반적인 설정 절차를 수행하여 NodeJS 요구 사항을 설치합니다.

4. 이전 ST 설치에서 필요에 따라(*) 다음 파일/폴더를 복사합니다:

    (*) '필요에 따라' = "해당 폴더와 관련된 사용자 정의 콘텐츠를 만든 경우".

    #### >=1.12.0 업데이트

    `/data` 디렉터리와 `config.yaml` 파일을 한 설치에서 다른 설치로 복사합니다. 보존하려는 서버 전체 확장("모든 사용자"용으로 설치)이 있는 경우 `/public/scripts/extensions/third-party` 디렉터리도 복사합니다.

    #### <1.12.0에서 >1.12.0으로 업데이트

    1.12.0에는 자동 마이그레이션 절차가 포함되어 있습니다. 아래 단계는 마이그레이션이 중단되거나 오류가 발생한 경우에*만* 필요합니다.

5. 업데이트된 서버 설치를 최소한 한 번 실행하여 `/data/default-user` 디렉터리를 만듭니다.
6. 필요에 따라 이전 `/public`에서 새 `/data/default-user`로 파일을 전송합니다.

    폴더는 필수가 아니므로 필요한 것만 복사하세요.

    **참고: 전체 /PUBLIC/ 폴더를 복사하지 마세요**

    그렇게 하면 새 설치가 중단되고 새 기능이 나타나지 않을 수 있습니다.

    ```plaintext
    Assets
    Backgrounds
    Characters
    Chats
    Context
    Groups
    Group chats
    Instruct
    movingUI
    KoboldAI Settings
    NovelAI Settings
    OpenAI Settings
    QuickReplies
    TextGen Settings (textgen = ooba)
    Themes
    User Avatars
    Worlds
    User
    settings.json
    secrets.json <---- 이것은 /public/이 아닌 기본 폴더에 있습니다
    ```

7. 해당 폴더/파일이 복사되면 새 설치의 /data/default-user 폴더(secrets.json은 폴더 루트에)에 붙여넣습니다.
8. OS에 적합한 방법으로 SillyTavern을 다시 시작하고 제대로 되었기를 바랍니다.
9. 모든 것이 표시되면 이전 ST 폴더를 안전하게 삭제할 수 있습니다.

### 일반적인 업데이트 문제

#### "There are unresolved conflicts in the working directory."

이는 원격 저장소에서 변경된 기본 파일(예: 설정 프리셋)을 수정했음을 의미합니다.

이 문제를 해결하려면 터미널에서 다음을 실행하세요. 파괴적일 수 있으므로 주의해서 사용하세요. 필요한 경우 백업을 확보하세요.

```bash
git merge --abort
git reset --hard
git pull --rebase --autostash
```

#### 파일 변경으로 인해 git pull이 방지됨

- SillyTavern 시스템 파일을 변경하면 `git pull`이 작동하지 않을 수 있습니다.
- 때때로 업데이트에서 중요한 파일을 변경해야 할 수도 있으며, 이로 인해 동일한 문제가 발생할 수 있습니다.
- 일반적으로 기본 프리셋 파일 또는 `package-lock.json`입니다.
- 이 경우 파일을 다른 폴더로 이동하거나 파일을 삭제한 다음 `git pull`을 수행할 수 있습니다.
- 또 다른 해결책은 `git pull --rebase --autostash` 사용입니다

#### 서버 시작 시 Error: Cannot find module "***"

- 이는 SillyTavern이 새 npm 패키지 요구 사항을 추가했음을 의미합니다.
- SillyTavern 디렉터리에서 `npm install`을 실행하여 이 문제를 해결하세요. 제공된 Start.bat 및 start.sh 스크립트는 자동으로 수행합니다.
- 도움이 되지 않았나요? node_modules 폴더를 제거하세요

**Windows**

```bash
rmdir /s /q node_modules
npm cache clean --force
npm install
```

**Unix/Linux**

```bash
rm -rf node_modules
npm cache clean --force
npm install
```

## Docker

1. 터미널 창을 열고 docker 디렉터리로 이동합니다 `cd SillyTavern/docker`
2. `docker compose down`으로 컨테이너를 삭제합니다
3. 캐시에서 SillyTavern docker 이미지를 삭제합니다 `docker rmi ghcr.io/sillytavern/sillytavern:latest` (staging 브랜치를 대상으로 하는 경우 `sillytavern:latest`를 `sillytavern:staging`으로 바꿉니다.)
4. `sudo docker compose up -d`로 컨테이너를 다시 빌드합니다

모든 것이 원활하게 진행되면 docker가 이미지를 다시 다운로드하기 시작하고 곧 실행됩니다. 문제가 발생하면 이 가이드의 다음 섹션을 참조하세요.

### 일반적인 업데이트 문제
#### Docker를 사용하는데 업데이트 후 모든 데이터가 사라졌습니다!

1.12.0에 도입된 새 데이터 모델에 대한 볼륨 매핑을 업데이트하려면 [Docker 컨테이너용 마이그레이션 가이드](/Installation/Updating/ST-1.12.0-Migration-Guide.md#containerized-docker-installs)를 따라야 합니다

#### docker 명령 실행 시 권한이 거부됨

이것은 Linux 문제이며 권한이 제대로 설정되지 않았음을 의미합니다. 이 문제를 해결하는 두 가지 방법이 있습니다:

1. **쉬운 방법**: 사용자에게 sudo 액세스 권한이 있는 경우 명령 앞에 `sudo`를 붙이기만 하면 됩니다(예: `sudo docker compose down`)
2. **적절한 방법**: 권한을 수정합니다. 이것은 사용하는 Linux 버전에 따라 다릅니다. 이 문제를 해결하는 데 도움이 되는 온라인 가이드가 많이 있습니다.
