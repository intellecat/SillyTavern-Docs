---
order: 120
icon: gear
route: /usage/user-settings/
---

# 사용자 설정


:::callout
**[UI 사용자 정의](uicustomization.md)**

선호도에 맞게 채팅 인터페이스의 테마, 모양 및 느낌을 변경하세요.
:::



:::callout
**[Visual Novel 모드](Visual-Novel.md)**

Doki Doki Literature Club 및 기타 유명한 VN 게임과 같은 비주얼 노벨처럼 스프라이트가 있는 캐릭터와 채팅하세요.
:::


## 일반 설정

전반적인 SillyTavern 경험에 영향을 미치는 핵심 설정입니다.

### UI Language

SillyTavern의 사용자 인터페이스는 여러 언어로 사용할 수 있습니다. 언어 선택기는 다음 옵션을 제공합니다:
* **Default**: 사용 가능한 경우 시스템 언어를 사용합니다
* **English**: 시스템 설정과 관계없이 영어 UI를 강제합니다
* 드롭다운을 통해 사용할 수 있는 기타 언어

참고: 이 설정은 사용자 인터페이스 텍스트에만 영향을 미칩니다. AI 대화 번역은 [Chat Translation](../../extensions/Translation.md) 확장 프로그램을 사용하세요.

### 소프트웨어 버전

현재 SillyTavern 버전이 오른쪽 상단 모서리에 표시됩니다. 이 정보는 다음에 필수적입니다:
* 문제 해결
* 확장 프로그램과의 호환성 보장
* 업데이트가 사용 가능한지 확인

SillyTavern을 최신 버전으로 업데이트하려면 [Updating](/Installation/Updating) 문서를 참조하세요.

### 계정 관리

SillyTavern 사용자 계정을 제어하고, 설정 및 사용자 데이터를 백업하며, [다중 사용자 모드](/Administration/multi-user.md)에서 사용자 역할 및 권한을 관리합니다.

#### <i class="fa-fw fa-solid fa-user-shield"></i> 계정

계정 대화상자에서 프로필 정보를 보고 편집하고, 비밀번호를 변경하며, 계정 설정을 관리할 수 있습니다.

**프로필 정보**

* 표시 이름(연필 아이콘을 통해 편집 가능)
* 사용자 아바타([페르소나](/Usage/personas.md)를 사용하여 변경 가능)
* 계정 핸들
* 사용자 역할
* 계정 생성 날짜
* 비밀번호 상태(잠금/잠금 해제 아이콘은 보호를 나타냄)

**계정 작업**

* **Settings Snapshots**: 사용자 설정의 백업 생성, 관리 및 복원
* **Download Backup**: 모든 사용자 데이터의 전체 백업 내보내기
* **Change Password**: 계정 보안 자격 증명 업데이트

**위험 영역**

주의해서 사용해야 하는 중요한 계정 작업:
* **Reset Settings**: 모든 설정을 공장 기본값으로 복원
* **Reset Everything**: 완전한 계정 삭제 및 공장 재설정

#### <i class="fa-fw fa-solid fa-user-tie"></i> 관리자 패널

!!! 적용 대상: [다중 사용자 모드](/Administration/multi-user.md)

다중 계정 기능을 사용하려면 config.yaml에서 `enableUserAccounts`를 true로 설정해야 합니다.
!!!

**Manage Users**를 선택하여 기존 사용자 계정을 보고 관리합니다.

##### 사용자 프로필

- 사용자 정의 아바타 관리(업로드/제거)
- 표시 이름 및 핸들
- 역할 및 상태 정보
- 계정 생성 날짜
- 비밀번호 보호 상태

##### 계정 컨트롤

- <i class="fa-fw fa-solid fa-pencil"></i> 표시 이름 편집
- <i class="fa-fw fa-solid fa-check"></i> 계정 활성화
- <i class="fa-fw fa-solid fa-ban"></i> 계정 비활성화
- <i class="fa-fw fa-solid fa-arrow-up"></i> 관리자로 승격
- <i class="fa-fw fa-solid fa-arrow-down"></i> 일반 사용자로 강등

##### 관리 작업

- <i class="fa-fw fa-solid fa-download"></i> 사용자 데이터 백업 다운로드
- <i class="fa-fw fa-solid fa-key"></i> 사용자 비밀번호 변경
- <i class="fa-fw fa-solid fa-trash"></i> 계정 삭제

##### 새 사용자

**New User**를 선택하여 새 사용자 계정을 만듭니다.

* Display Name* (예: "John Snow")
* User Handle* (소문자, 숫자 및 대시만 허용)
* Password (선택 사항)
* Password Confirmation

새 사용자를 만들면 사용자의 핸들을 폴더 이름으로 사용하여 /data/ 디렉토리에 하위 폴더가 자동으로 생성됩니다.

#### <i class="fa-fw fa-solid fa-right-from-bracket"></i> 로그아웃

!!! 적용 대상: [다중 사용자 모드](/Administration/multi-user.md)
!!!

현재 세션에서 로그아웃합니다.

### 설정 검색

특정 설정을 빠르게 찾는 데 도움이 되는 편리한 검색 바:
* 키워드를 입력하여 User Settings의 모든 곳에서 설정을 필터링하고 강조 표시
* 설정 이름 및 설명을 검색
* 복잡한 설정을 더 효율적으로 탐색하는 데 도움

## UI 테마

선호도에 맞게 채팅 인터페이스의 모양을 변경합니다.

<i class="fa-fw fa-solid fa-user-gear" title="User Settings icon"></i> **User Settings**의 이 섹션 설정에 대한 자세한 내용은 [UI 사용자 정의](uicustomization.md#ui-theme)를 참조하세요.

## 캐릭터 처리

* **Char List Subheader**: [<i class="fa-fw fa-solid fa-address-card" title="Characters icon"></i> Characters](/Usage/Characters/characterdesign.md) 목록의 캐릭터 이름 아래에 표시할 추가 정보를 선택합니다:
    - Character Version
    - Created by
* **Import Card Tags**: 캐릭터 카드를 가져올 때 태그를 처리하는 방법을 제어합니다:
    - Ask - 각 가져오기에 대한 대화상자 표시
    - None - 태그를 가져오지 않음
    - All - 모든 태그 가져오기
    - Existing - 이미 존재하는 태그만 가져오기
* **Advanced Character Search**: 활성화하면 퍼지 매칭을 사용하고 이름뿐만 아니라 모든 캐릭터 데이터 필드를 검색합니다.
* **Prefer Char. Prompt**: 활성화하면 사용 가능한 경우 캐릭터 카드의 System Prompt 재정의를 사용합니다.
* **Prefer Char. Instructions**: 활성화하면 사용 가능한 경우 캐릭터 카드의 Post-History Instructions 재정의를 사용합니다.
* **Never resize avatars**: 가져온 캐릭터 이미지의 자르기/크기 조정을 방지합니다. 비활성화하면 이미지 크기가 512x768로 조정됩니다.
* **Show avatar filenames**: 캐릭터 목록에 캐릭터 아바타의 실제 파일 이름을 표시합니다.
* **Spoiler Free Mode**: 편집기 패널에서 스포일러 버튼 뒤에 캐릭터 정의를 숨깁니다.

## 기타

* **Reload Chat**: 현재 채팅을 다시 로드하고 다시 그립니다.
* **[Debug Menu](#debug-menu)**: 디버깅 옵션에 액세스합니다.
* **Smooth Streaming**: 텍스트를 글자별로 표시하여 스트리밍된 생성을 부드럽게 합니다. 속도 제어 슬라이더를 포함합니다.
* **Stream Fade-In**: 스트리밍된 텍스트에 페이드인 효과를 적용합니다. Smooth Streaming과 함께 또는 별도로 사용할 수 있습니다.
* **[Message Sound](uicustomization.md#message-sound)**: 메시지 생성이 완료되면 소리를 재생합니다.
    - **Background Sound Only**: 브라우저 탭이 포커스되지 않은 경우에만 소리를 재생합니다.
* **Relaxed API URLs**: API URL에 대한 형식 요구 사항을 줄입니다.
* **Lorebook Import Dialog**: 내장된 로어가 있는 캐릭터를 가져올 때 World Info/Lorebook에 대한 가져오기 대화상자를 표시합니다.
* **Auto-select Input Text**: 클릭하면 특정 입력 필드의 텍스트를 자동으로 선택합니다.
* **Markdown Hotkeys**: markdown 형식화를 위한 키보드 단축키를 활성화합니다.
* **Restore User Input**: 페이지를 새로 고칠 때 저장되지 않은 사용자 입력을 보존합니다.
* **MovingUI**: 드래그하여 UI 요소를 재배치할 수 있습니다(PC만 해당).
    - <i class="fa-solid fa-recycle" title="Reset icon"></i> **Reset** 버튼으로 기본 위치 복원
    - UI 레이아웃 저장/로드를 위한 프리셋 시스템

## 채팅/메시지 처리

### 메시지 디스플레이 설정

채팅 인터페이스에서 메시지가 로드되고 표시되는 방법을 제어합니다. 이러한 설정은 전반적인 채팅 경험 및 성능에 영향을 미칩니다.
* **# Messages to Load**: 페이지네이션 전에 로드할 채팅 기록 메시지 수 (0 = 모두)
* **Streaming FPS**: 스트리밍된 텍스트의 업데이트 속도 (5-100 FPS)
* **Example Messages Behavior**:
    - Gradual push-out
    - Always include examples
    - Never include examples

### 입력 및 응답 컨트롤

메시지가 전송되는 방법과 AI가 응답을 계속하는 방법을 결정하는 설정입니다.
* **Enter to Send**: Disabled, Automatic (PC) 또는 Enabled 중 선택
* **"Send" to Continue**: Send 버튼을 사용하여 AI 응답을 계속합니다
* **Quick "Continue" button**: AI의 마지막 메시지를 확장하는 버튼을 표시합니다
* **Quick "Impersonate" button**: 단일 메시지 캐릭터 가장을 위한 버튼을 표시합니다
* **Swipes**: 대체 AI 응답을 위한 화살표 버튼을 표시합니다 (PC 및 모바일)
* **Gestures**: 생성을 위한 스와이프 제스처를 활성화합니다 (모바일만 해당)

### 자동 관리

채팅 흐름 및 콘텐츠를 관리하는 데 도움이 되는 자동화된 기능입니다.
* **Auto-load Last Chat**: 시작 시 가장 최근 채팅을 자동으로 로드합니다
* **Auto-scroll Chat**: 가장 최근 메시지로 자동 스크롤합니다
* **Auto-save Message Edits**: 확인 없이 메시지 편집을 저장합니다
* **Confirm message deletion**: 메시지를 삭제하기 전에 메시지를 표시합니다
* **Auto-fix Markdown**: markdown 형식을 자동으로 수정합니다

#### 자동 스와이프

구성 가능한 기준에 따라 AI 메시지를 자동으로 거부하고 재생성합니다.
* **Enable Auto-swipe**: 자동 스와이프 기능의 마스터 토글
* **Minimum generated message length**: 메시지가 이 값보다 짧으면 자동 스와이프를 트리거합니다
* **Blacklisted words**: 자동 스와이프를 트리거할 수 있는 단어 목록, 쉼표로 구분
* **Blacklisted word count to swipe**: 자동 스와이프를 트리거하기 위해 감지되어야 하는 블랙리스트 단어의 최소 수

#### 자동 계속

모델이 특정 길이에 도달하기 전에 중지한 경우 응답을 자동으로 계속합니다.

이를 통해 AI가 여러 부분으로 긴 응답을 작성할 수 있으므로 짧은 [응답 길이 설정](/Usage/Common-Settings.md#response-tokens)을 사용하면서도 긴 응답을 얻을 수 있습니다.

AI가 다른 방식으로 작성했을 것보다 더 많이 작성하게 하지는 않습니다. AI가 "완료"로 간주하는 메시지를 계속하도록 요청하는 것은 일반적으로 작동하지 않습니다. 다른 아이디어는 [How to make the AI write more?](/Usage/faq.md#how-to-make-the-ai-write-more)를 참조하세요.

* **Enable Auto-continue**: 자동 계속의 마스터 토글
* **Allow for Chat Completion APIs**: Chat Completion API 엔드포인트에 대한 자동 계속 기능을 활성화합니다
* **Target length (tokens)**: 토큰 단위의 원하는 메시지 길이 - 메시지가 이 값보다 짧으면 계속을 트리거합니다 (0-1024)

### 메시지 형식화 및 디스플레이

메시지가 형식화되는 방법과 표시되는 콘텐츠를 제어합니다.
* **Forbid External Media**: 외부 도메인의 내장된 미디어를 차단합니다
* **Show {\{char}}: in responses**: 생성된 경우 응답에서 캐릭터 이름 접두사를 유지합니다
* **Show {\{user}}: in responses**: 생성된 경우 응답에서 사용자 이름 접두사를 유지합니다
* **Show tags in responses**: (일부) HTML 태그가 응답에서 HTML로 표시되도록 허용합니다
* **Relax message trim in Groups**: 그룹 채팅에서 AI가 다른 캐릭터를 대신 말하도록 허용하며, 응답 생성을 중지하지 않습니다
* **Show group chat queue**: 그룹 채팅에 대한 캐릭터 목록에 응답 순서를 표시합니다
* **Pin greeting message styles**: 지연 로딩으로 인해 메시지가 언로드된 경우에도 인사말의 스타일 태그를 항상 렌더링합니다.

### 프롬프트 검사 및 디버깅

* **Log prompts to console**: 브라우저 콘솔에 프롬프트를 출력합니다
* **Request token probabilities**: API에서 AI 응답에 대한 토큰 확률을 요청합니다. 사용 가능한 경우 <i class="fa-solid fa-bars" title="Burger Menu icon"></i> [Token Probabilities](../../Usage/Chatting/index.md#token-probabilities-panel)에서 볼 수 있습니다.

### AutoComplete

- 자동 숨기기 세부 정보
- 매칭 스타일 (Starts with/Includes/Fuzzy)
- 비주얼 스타일 (Theme/Dark/Light)
- 키보드 선택 옵션
- 글꼴 크기 조정
- 너비 컨트롤

## STscript 설정

[STscript 파서](/For_Contributors/st-script.md#parser-flags)에 대한 구성 옵션입니다.

### STRICT_ESCAPING

* 파이프는 인용된 값에서 이스케이프될 필요가 없습니다.
* 기호 앞의 백슬래시를 이스케이프하여 기능적인 기호가 뒤따르는 리터럴 백슬래시를 제공할 수 있습니다.

자세한 내용은 [Strict Escaping](/For_Contributors/st-script.md#strict-escaping)을 참조하세요.

### REPLACE_GETVAR

변수 값에 매크로로 해석될 수 있는 텍스트가 포함된 경우 이중 대체를 피하는 데 도움이 됩니다.

자세한 내용은 [Replace Variable Macros](/For_Contributors/st-script.md#replace-variable-macros)를 참조하세요.

## 정리 메뉴

정리 메뉴는 SillyTavern 설치에서 불필요한 파일을 식별하고 제거하는 데 도움이 되는 데이터 유지 관리 도구를 제공합니다. 이 기능은 데이터 디렉토리를 정리하는 데 도움이 되며 상당한 디스크 공간을 확보할 수 있습니다.

!!! warning "중요 경고"
정리 도구는 파일을 영구적으로 삭제합니다. **이 작업은 취소할 수 없습니다!**

`/data/user/files/` 및 `/data/user/images/` 디렉토리에 대한 수동 업로드는 채팅 메시지 또는 Data Bank 항목과 연결되지 않은 경우 삭제됩니다.

확실하지 않은 경우 정리 메뉴를 사용하기 전에 데이터를 백업하세요.
!!!

### 정리 사용 방법

1. **Miscellaneous** 섹션 아래의 **Clean-Up** 버튼을 클릭합니다
2. **Scan**을 클릭하여 설치를 분석합니다. 데이터 디렉토리의 크기에 따라 시간이 걸릴 수 있습니다
3. 발견된 파일 범주를 검토합니다
4. **View**를 사용하여 삭제하기 전에 파일 내용을 미리 봅니다
5. **Download**를 사용하여 삭제하기 전에 파일을 저장합니다
6. 필요에 따라 개별 파일 또는 전체 범주를 삭제합니다

### 정리 범주

정리 도구는 느슨한 파일을 다음 범주로 스캔합니다:

#### Files

* **발견 내용**: 채팅 메시지 또는 Data Bank 항목과 연결되지 않은 파일
* **위치**: `/data/<user-handle>/user/files/`
* **위험**: ⚠️ **채팅에서 참조되지 않는 수동 업로드를 삭제합니다**
* **정리 시기**: 참조되지 않은 파일이 필요하지 않은 경우 삭제해도 안전합니다

#### Images

* **발견 내용**: 채팅 메시지와 연결되지 않은 이미지
* **위치**: `/data/<user-handle>/user/images/`
* **위험**: ⚠️ **채팅에서 참조되지 않는 수동 업로드를 삭제합니다**
* **정리 시기**: 참조되지 않은 이미지가 필요하지 않은 경우 삭제해도 안전합니다

#### Chats

* **발견 내용**: 삭제된 캐릭터와 연결된 채팅 파일
* **위치**: `data/<user-handle>/chats/`
* **위험**: ⚠️ **고아 채팅이 영구적으로 손실됩니다**
* **정리 시기**: 의도적으로 캐릭터를 삭제했고 채팅 기록이 더 이상 필요하지 않은 경우 삭제해도 안전합니다

#### Group Chats

* **발견 내용**: 삭제된 그룹과 연결된 채팅 파일
* **위치**: `data/<user-handle>/group chats/`
* **위험**: ⚠️ **고아 그룹 채팅이 영구적으로 손실됩니다**
* **정리 시기**: 의도적으로 그룹을 삭제했고 채팅 기록이 더 이상 필요하지 않은 경우 삭제해도 안전합니다

#### Avatar Thumbnails

* **발견 내용**: 누락되거나 삭제된 캐릭터의 아바타 썸네일
* **위치**: `data/<user-handle>/thumbnails/avatar`
* **위험**: ✅ **삭제해도 안전** - 썸네일은 필요할 때 자동으로 재생성됩니다
* **정리 시기**: 항상 정리해도 안전하며 공간을 확보하는 데 도움이 됩니다

#### Background Thumbnails

* **발견 내용**: 누락되거나 삭제된 배경의 썸네일
* **위치**: `data/<user-handle>/thumbnails/bg`
* **위험**: ✅ **삭제해도 안전** - 썸네일은 필요할 때 자동으로 재생성됩니다
* **정리 시기**: 항상 정리해도 안전하며 공간을 확보하는 데 도움이 됩니다

#### Chat Backups

* **발견 내용**: 자동으로 생성된 채팅 백업
* **위치**: `data/<user-handle>/backups/chat_*`
* **위험**: ⚠️ **백업 파일이 영구적으로 손실됩니다**
* **정리 시기**: 최근 백업을 유지하는 것을 고려하지만 이전 백업은 안전하게 삭제할 수 있습니다

#### Settings Backups

* **발견 내용**: 자동으로 생성된 설정 백업
* **위치**: `data/<user-handle>/backups/settings_*`
* **위험**: ⚠️ **설정 백업 파일이 영구적으로 손실됩니다**
* **정리 시기**: 최근 백업을 유지하는 것을 고려하지만 이전 백업은 안전하게 삭제할 수 있습니다

## 디버그 메뉴

!!!warning 이러한 기능은 고급 사용자만을 위한 것입니다.

결과를 완전히 이해하지 못하면 사용하지 마세요.
!!!

디버그 메뉴는 문제 해결, 유지 관리 및 개발 목적을 위한 기능을 제공합니다. 이러한 기능은 SillyTavern 설치에 상당한 영향을 미칠 수 있으므로 주의해서 사용해야 합니다.

확장 프로그램이 디버그 기능을 추가할 수 있으므로 사용 가능한 옵션은 설치한 확장 프로그램에 따라 다릅니다.

### 번역 및 로케일 기능
* **Get missing translations**: 현재 로케일(영어가 선택된 경우 모든 로케일)에서 누락된 번역을 분석하고 브라우저 콘솔에 결과를 출력합니다
* **Apply locale**: 선택한 로케일을 다시 적용하여 현재 언어 설정을 강제로 새로 고칩니다
### 캐시 및 저장소 관리
* **Clear WebSearch cache**: 로컬 캐시에서 저장된 모든 검색 결과를 제거합니다
* **Purge all vector indices**: 모든 소스에서 저장된 모든 벡터를 완전히 제거합니다
* **Reset token cache**: 저장된 토큰 수를 지우고 모든 채팅의 완전한 재토큰화를 강제합니다
* **Delete itemized prompts**: 로컬 저장소에서 모든 항목화된 프롬프트를 제거합니다
### 데이터 및 통계
* **Refresh Stat File**: 기존 채팅 데이터를 사용하여 통계 파일을 다시 빌드합니다
* **Backfill token counters**: 현재 채팅의 모든 메시지에 대한 토큰 수를 다시 계산합니다
    - 다른 tokenizers를 가진 모델 간에 전환할 때 유용합니다
    - 완료 후 채팅 다시 로드를 트리거합니다
    - 시각적 변경만, 채팅 콘텐츠를 수정하지 않습니다
### API 및 확장 프로그램 테스트
* **Change Mancer base URL**: Mancer API 서버의 기본 URL을 수정합니다
* **Test WebSearch extension**: 현재 설정을 사용하여 테스트 검색을 수행합니다
* **Send a generation request**: 현재 선택된 API를 사용하여 텍스트 생성을 테스트합니다
### 시스템 및 디버그 도구
* **Force onboarding**: 온보딩 프로세스를 다시 시작합니다
* **Toggle event tracing**: 디버깅을 위한 이벤트 추적을 활성화/비활성화합니다
* **Copy ST setup**: [진행 중] 버그 리포트를 위해 시스템 구성 데이터를 클립보드에 복사합니다

각 기능은 설명 아래의 "Execute" 버튼을 사용하여 실행할 수 있습니다. 일부 작업은 취소할 수 없으므로 이러한 도구를 사용하기 전에 데이터를 백업하는 것을 고려하세요.
