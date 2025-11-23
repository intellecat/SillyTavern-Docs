---
order: 112
route: /installation/st-1.12.0-migration-guide/
---

# 1.12.0 마이그레이션 가이드

SillyTavern 1.12.0 (코드명 "Neo Server" 업데이트)에는 SillyTavern 사용 방식에 영향을 미칠 수 있는 몇 가지 중요한 변경 사항이 포함되어 있습니다.

이 가이드는 업데이트를 준비하고 추가 지침을 제공합니다.

## 데이터 저장소 업데이트

1.12.0은 SillyTavern이 사용자 데이터를 처리하는 방식을 변경합니다.

이전에는 모든 영구 데이터가 `/public` 디렉터리의 프론트엔드 부분과 함께 저장되어 혼란과 잠재적인 실패 지점을 만들었으며 컨테이너화 및 시스템 전체 앱 설치를 상당히 어렵게 만들었습니다.

### 무엇이 변경되었나요?

설정 및 채팅과 같은 `/public`의 모든 영구 정보(아래 전체 목록)는 구성 가능한 경로가 있는 별도의 디렉터리로 이동되어 웹 서버 자체와 독립적이고 이식 가능합니다. 호환성 목적으로 필요한 경우(예: 확장 호스팅, 전체 크기 캐릭터 카드, 사용자 이미지 업로드 등) 데이터 디렉터리에서 사용자 파일을 자동으로 호스팅하도록 스마트 리디렉션이 설정되었습니다.

### 데이터 루트 설정

`config.yaml` 또는 `--dataRoot` 콘솔 인수로 서버를 시작하여 데이터 루트에 절대 또는 상대(ST 저장소 디렉터리에 대해) 경로를 제공할 수 있습니다.

> YAML 예시

```yaml
# -- DATA CONFIGURATION --
# 사용자 데이터 저장을 위한 루트 디렉터리
dataRoot: C:\Users\Harry\Documents\ST-Data
```

> 콘솔 예시

```bash
node server.js --dataRoot="/Users/harry/ST-Data"
# 또는
npm run start -- --dataRoot="/Users/harry/ST-Data"
```

기본 데이터 루트 경로는 `./data`이며, 이는 SillyTavern 저장소의 `data` 디렉터리를 의미합니다.

!!!info 참고
데이터 루트 경로는 **전체 절대** 또는 **전체 상대** 경로여야 합니다. 이러한 경로는 운영 체제가 아닌 셸에서 해석되므로 `~` 또는 `%APP_DATA%`와 같은 경로 바로 가기를 사용할 수 _없습니다_.
!!!

### 마이그레이션

#### **중요!** 시작하기 전에

1. **기본 위치에서 dataRoot를 이동하려는 경우에만 해당됩니다. 그렇지 않으면 이 부분을 건너뜁니다.** 업데이트를 가져온 후 서버를 처음 실행하기 _전에_ 데이터 루트를 설정합니다. `npm install`을 실행하여 `config.yaml`이 새 값으로 채워지도록 하거나 콘솔 인수를 전달합니다.
2. 모든 데이터는 `default-user` 계정으로 마이그레이션됩니다. 아래의 [사용자](#users)에서 자세히 알아보세요.

#### 컨테이너리스 (베어 메탈) 설치

아무것도 할 필요가 없습니다! ST 서버를 시작하고 이전 저장소 형식을 감지하면(`/public/characters` 디렉터리의 존재를 확인하여) 자동 마이그레이션이 모든 것을 처리합니다.

파일을 이동하면 `/backups/_migration/YYYY-MM-DD` (현재 날짜로 해석됨) 디렉터리에 자동 백업이 생성되지만, 마이그레이션을 실행하기 전에 전체 수동 백업을 만드는 것이 항상 좋은 습관입니다.

#### 컨테이너화 (Docker) 설치

Docker 볼륨의 데이터를 마이그레이션하는 것은 약간 까다롭지만 매우 간단합니다. 저장소와 함께 제공되는 `docker-compose.yml`이 변경 사항을 반영하도록 업데이트되었지만 사용자 정의 워크플로/배포를 조정해야 할 수 있습니다.

**1단계.** 새 볼륨을 만들고 컨테이너 내의 "/home/node/app/data" 경로에 마운트합니다. `config` 볼륨을 제거하지 마세요.

```yaml
volumes:
    - "./config:/home/node/app/config"
    - "./data:/home/node/app/data"
```

**2단계.** `config` 볼륨에서 `config.yaml` 파일을 제외한 모든 항목을 `data` 볼륨의 `default-user` 하위 디렉터리로 이동합니다.

**3단계.** 컨테이너를 다시 빌드하고 시작합니다.

!!!info 참고
`/public` 디렉터리와 `config` 볼륨 사이의 소프트 링크는 더 이상 필요하지 않으며 Docker 컨테이너에 내장되어 있지 않습니다!
!!!

#### 무엇을 마이그레이션하나요?

다음 파일 및 디렉터리는 데이터 마이그레이션 대상입니다. 기본 구성을 가정하면 이전 및 이후 경로가 아래 표에 제공됩니다.

| 이전                                 | 이후                                |
|----------------------------------------|--------------------------------------|
| /secrets.json                          | /data/default-user/secrets.json      |
| /thumbnails                            | /data/default-user/thumbnails        |
| /vectors                               | /data/default-user/vectors           |
| /public/settings.json                  | /data/default-user/settings.json     |
| /public/stats.json                     | /data/default-user/stats.json        |
| /public/assets                         | /data/default-user/assets            |
| /public/backgrounds                    | /data/default-user/backgrounds       |
| /public/characters                     | /data/default-user/characters        |
| /public/chats                          | /data/default-user/chats             |
| /public/context                        | /data/default-user/context           |
| /public/scripts/extensions/third-party | /data/default-user/extensions        |
| /public/group chats                    | /data/default-user/group chats       |
| /public/groups                         | /data/default-user/groups            |
| /public/instruct                       | /data/default-user/instruct          |
| /public/KoboldAI Settings              | /data/default-user/KoboldAI Settings |
| /public/movingUI                       | /data/default-user/movingUI          |
| /public/NovelAI Settings               | /data/default-user/NovelAI Settings  |
| /public/OpenAI Settings                | /data/default-user/OpenAI Settings   |
| /public/QuickReplies                   | /data/default-user/QuickReplies      |
| /public/TextGen Settings               | /data/default-user/TextGen Settings  |
| /public/themes                         | /data/default-user/themes            |
| /public/worlds                         | /data/default-user/worlds            |
| /default/content/content.log           | /data/default-user/content.log       |

## 사용자

1.12.0은 동일한 서버에서 다중 사용자 설정을 생성할 수 있는 (완전히 선택적인) 기능을 추가하여 여러 사용자가 동시에 자신의 완전히 격리된 SillyTavern 인스턴스를 사용할 수 있도록 합니다. 사용자 계정은 추가 개인 정보 보호 계층을 위해 암호로 보호될 수도 있습니다.

자세한 내용은 [사용자](/Administration/multi-user.md) 문서를 참조하세요.
