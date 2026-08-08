---
title: 다중 사용자 모드
icon: people
order: -10
route: /ko/administration/multi-user/
---

다중 사용자 모드를 사용하면 여러 사람이 하나의 SillyTavern 서버를 사용할 수 있습니다. 각 사용자는 자신만의 설정, 확장 프로그램 및 데이터를 가지고 있습니다. 사용자 계정은 비밀번호로 보호할 수도 있습니다.

!!!warning
사용자 비밀번호는 다중 사용자 설정의 사용자 간에 기본적인 개인정보 보호를 제공합니다. 이것은 보안 기능이 아니며 그렇게 간주되어서는 안 됩니다. 모든 사용자 데이터(채팅 기록, API 키 및 기타 민감한 정보 포함)는 서버에 일반 텍스트로 저장됩니다. 서버의 파일 시스템에 액세스할 수 있는 모든 사람이 보고 수정할 수 있습니다. **공개 서버에서 또는 신뢰할 수 없는 사용자와 함께 SillyTavern을 사용하지 마십시오.**
!!!

## 구성

다중 사용자 모드를 활성화하고 사용하려면 `config.yaml` 파일을 편집하십시오:

```yaml
# Enable multi-user mode
enableUserAccounts: true
# Enable discreet login mode: hides user list on the login screen
enableDiscreetLogin: true
```

1. 사용자 계정 설정이 비활성화되면 사용자 데이터 저장에 `default-user` 폴백 관리자 계정이 사용됩니다.
2. 신중한 로그인 설정이 비활성화되면 활성 사용자 목록이 로그인 화면에 표시됩니다. 활성화되면 사용자가 핸들을 수동으로 입력해야 합니다.

!!!info
`enableUserAccounts`가 `false`로 설정된 경우 사용자 데이터 제공에 사용되므로 사용자 목록에서 `default-user` 계정을 _삭제_할 수 없습니다. 그러나 목록에서 숨기고 로그인을 허용하지 않도록 _비활성화_할 수 있습니다.
!!!

## 사용자 핸들

핸들은 사용자의 고유 식별자입니다. 소문자, 숫자 및 대시만 포함할 수 있습니다.

사용자 데이터 디렉터리 경로는 다음 패턴을 사용합니다: `%DATA_ROOT%/%USER_HANDLE%`.

유효한 사용자 핸들의 예:

- default-user
- juan555
- flux-the-cat
- cool-guy1337

## 역할

- **Admin** - 다른 사용자를 관리(생성, 삭제, 수정)할 수 있습니다. 모든 사용자를 위한 확장 프로그램을 설치할 수 있습니다.
- **User** - 다른 사용자를 관리할 수 없습니다. 자신만을 위한 확장 프로그램만 설치할 수 있습니다.

관리자 패널 액세스 권한을 제외하고 두 사용자 역할은 기능적으로 동일하며 제한 없이 SillyTavern 기능의 전체 범위를 사용할 수 있습니다. 사용자 권한 구현은 TBD입니다.

모든 사용자 계정은 먼저 일반 사용자로 생성된 다음 필요한 경우 관리자로 승격될 수 있습니다.

### 로그인 화면

사용할 사용자 계정을 선택할 수 있습니다. `enableDiscreetLogin` 구성 값에 따라 두 가지 스타일이 있습니다.

활성 사용자가 하나만 있고 비밀번호로 보호되지 않은 경우 로그인 화면은 우회되어 표시되지 않습니다.

### 사용자 프로필

상단 메뉴 바의 "User settings" 패널 아래에 있는 "Account" 버튼을 사용하여 계정 자체 관리 메뉴에 액세스할 수 있습니다.

1. Display name - 로그인 화면에 사용되며 변경할 수 있습니다. 페르소나와 상관관계가 없으며 AI API에 표시되지 않습니다. 여전히 원하는 만큼 많은 페르소나를 사용할 수 있습니다.
2. Profile picture - 로그인 화면에 사용됩니다. 사용자 지정 사진, 기본 페르소나 사진(설정된 경우) 또는 마지막으로 사용한 페르소나를 사용할 수 있습니다.
3. Password - 잠금 아이콘은 계정 보호 상태를 반영합니다(열린 잠금 = 비밀번호 없음). "Change Password" 버튼을 사용하여 비밀번호를 설정, 변경 또는 제거할 수 있습니다.
4. Settings Snapshots - 스냅샷을 생성하거나 복원할 수 있는 기능과 함께 `settings.json` 파일의 백업에 액세스하고 검토합니다.
5. Download Backup - 사용자 데이터 폴더의 아카이브를 다운로드합니다.
6. Reset Settings - 다른 데이터(캐릭터, 채팅)는 그대로 두고 공장 기본 설정으로 재설정합니다.

## 비밀번호 복구

1. 로그인 화면에서 비밀번호를 복구할 수 있습니다. 일회성 복구 코드(4자리로 구성)를 얻으려면 서버 콘솔에 액세스해야 합니다.
2. 또는 SillyTavern 서버의 유틸리티 스크립트를 사용하여 사용자 핸들을 제공하여 비밀번호를 재설정할 수 있습니다.

```txt
Usage: node recover.js [account] (password)
Example: node recover.js admin SecurePassword
```

## 콘텐츠 스캐폴딩

사용자를 위한 사용자 지정 콘텐츠를 추가하려면 콘텐츠 스캐폴딩 기능을 사용할 수 있습니다. 이 기능을 사용하면 서버가 시작될 때 각 사용자의 데이터 디렉터리로 복사될 파일 집합을 정의할 수 있습니다.

이 기능이 작동하려면 `/default/scaffold` 디렉터리에 `index.json` 파일을 만들어야 합니다. 구문은 기본 콘텐츠와 동일합니다. 모든 파일 경로는 `/default/scaffold` 디렉터리를 기준으로 해야 하며 하위 디렉터리를 사용하여 파일을 구성할 수 있습니다.

스캐폴드된 파일은 기본 파일보다 먼저 복사되므로 동일한 파일 이름을 가진 기본 파일(프리셋/설정/등)을 재정의합니다.

!!!tip
모든 사용자 데이터 디렉터리에는 스캐폴드 및 기본 디렉터리에서 복사된 모든 파일을 나열하는 `content.log` 파일이 있습니다. 다음 재시작 시 서버가 콘텐츠를 다시 동기화하도록 하려면 이 파일을 제거하십시오.
!!!

### 인식된 콘텐츠 유형

| Type                          | Value                |
|-------------------------------|----------------------|
| settings.json                 | `'settings'`         |
| Character card                | `'character'`        |
| Character sprites             | `'sprites'`          |
| Background image              | `'background'`       |
| World Info file               | `'world'`            |
| Persona avatar                | `'avatar'`           |
| UI theme                      | `'theme'`            |
| ComfyUI workflow              | `'workflow'`         |
| KoboldAI Classic preset       | `'kobold_preset'`    |
| Chat Completion preset        | `'openai_preset'`    |
| NovelAI preset                | `'novel_preset'`     |
| Text Completion preset        | `'textgen_preset'`   |
| Instruct Mode template        | `'instruct'`         |
| Context Formatting template   | `'context'`          |
| MovingUI preset               | `'moving_ui'`        |
| Quick Replies set             | `'quick_replies'`    |
| System Prompt template        | `'sysprompt'`        |
| Reasoning Formatting template | `'reasoning'`        |

### 예제 (`/default/scaffold/index.json`)

```json
[
    {
        "filename": "themes/Midnight.json",
        "type": "theme"
    },
    {
        "filename": "backgrounds/city.png",
        "type": "background"
    },
    {
        "filename": "characters/Charlie.png",
        "type": "character"
    }
]
```
