---
icon: report
order: 170
expanded: false
route: /ko/usage/chatting/
---

# 채팅

[API에 연결](/Usage/API_Connections/index.md)되어 있으면 화면 하단의 채팅 바에 입력하여 AI에 메시지를 보냅니다. 그런 다음 <i class="fa-solid fa-paper-plane"></i> **Send**를 클릭하거나 Enter를 누릅니다.
![Chat bar](/static/chatbox.png)

AI는 대화를 계속하는 메시지로 응답합니다.

![Chat message](/static/chatmessage.png)

이제 다음을 수행할 수 있습니다:

* **다른 메시지 보내기**
* **응답 스와이프**: 메시지의 <i class="fa-solid fa-chevron-right"></i> **Swipe** 버튼을 클릭하여 다른 응답을 생성합니다.
* **메시지 편집**: 메시지의 <i class="fa-solid fa-pencil"></i> **Edit** 버튼을 클릭하여 [메시지 콘텐츠를 편집](#edit-message-content)합니다.
* **메시지 작업**: 메시지의 <i class="fa-solid fa-ellipsis"></i> **Message actions** 버튼을 클릭하여 [번역](../../extensions/Translation.md), 이미지 생성, 스토리 분기와 같은 [메시지 옵션](#message-actions-panel)을 더 표시합니다.
* **채팅 옵션**: 채팅 바 옆의 <i class="fa-solid fa-bars"></i> **Options** 버튼을 클릭하여 작가 노트 및 채팅 파일 관리와 같은 [채팅 옵션](#chat-options-panel)을 더 표시합니다.

!!! 편집 및 스와이프
다르게 말했으면 좋겠다고 생각되면 메시지를 편집한 다음 AI의 응답을 스와이프하여 새 응답을 얻을 수 있습니다.
!!!

!!! 키보드 단축키
**Right** 화살표 키를 사용하여 스와이프하고, **Up** 화살표 키를 사용하여 채팅의 마지막 메시지를 편집할 수도 있습니다. 더 많은 핫키를 보려면 채팅에서 `/help hotkeys` [슬래시 명령](/Usage/Chatting/slashcommands.md)을 사용하거나 [HotKeys](/Usage/Chatting/hotkeys.md) 페이지를 확인하세요.
!!!

## 메시지 작업 패널

메시지의 줄임표(•••) 버튼을 통해 개별 채팅 메시지를 관리합니다.

채팅의 모든 메시지에 대해 이러한 옵션을 표시하려면 사용자 설정에서 [Expand Message Actions](/Usage/User_Settings/uicustomization.md#theme-toggles) 설정을 활성화하세요.

### 핵심 기능

* <i class="fa-solid fa-language"></i> **Translate**: 메시지를 다른 언어로 변환
* <i class="fa-solid fa-paintbrush"></i> **Generate Image**: 메시지 콘텐츠에서 [이미지 생성](/extensions/Stable-Diffusion.md)
* <i class="fa-solid fa-bullhorn"></i> **Narrate**: [텍스트 음성 변환](/extensions/TTS.md)
* <i class="fa-solid fa-square-poll-horizontal"></i> **Prompt**: 생성 프롬프트 및 토큰 사용량 보기

### 메시지 가시성

* <i class="fa-solid fa-eye"></i> **Included**: AI가 이 메시지를 봅니다; 클릭하여 제외
* <i class="fa-solid fa-eye-slash"></i> **Excluded**: AI가 이 메시지를 보지 않습니다; 클릭하여 포함

### 콘텐츠 관리

* <i class="fa-solid fa-paperclip"></i> **Embed**: [파일 또는 이미지 첨부](/Usage/Characters/data-bank.md#about-documents)
* <i class="fa-solid fa-flag-checkered"></i> **Checkpoint**: 스토리 체크포인트 생성
* <i class="fa-solid fa-flag"></i> **Checkpoint Navigation**: 클릭하여 체크포인트 채팅 열기, Shift+클릭하여 기존 체크포인트 업데이트
* <i class="fa-solid fa-code-branch"></i> **Branch**: 대체 스토리 경로 시작
* <i class="fa-solid fa-copy"></i> **Copy**: 메시지 텍스트 복사
* <i class="fa-solid fa-pencil"></i> **Edit**: 메시지 콘텐츠 편집

## 메시지 콘텐츠 편집

채팅 메시지를 <i class="fa-solid fa-pencil"></i> **Edit**할 때 나타나는 메시지 조작 도구의 간단한 패널입니다.

### 핵심 작업

* <i class="fa-solid fa-check"></i> **Confirm**: 메시지 변경 사항 저장
* <i class="fa-solid fa-xmark"></i> **Cancel**: 메시지 변경 사항 취소

### 메시지 작업

* <i class="fa-solid fa-copy"></i> **Copy**: 메시지 콘텐츠 복제
* <i class="fa-solid fa-trash-can"></i> **Delete**: 메시지 제거

### 메시지 위치

* <i class="fa-solid fa-chevron-up"></i> **Move Up**: 채팅에서 메시지를 위로 이동
* <i class="fa-solid fa-chevron-down"></i> **Move Down**: 채팅에서 메시지를 아래로 이동

참고: 채팅 기록에서 메시지 위치에 따라 이동 컨트롤이 비활성화될 수 있습니다.

## 채팅 옵션 패널

채팅 인터페이스의 왼쪽 하단에 있는 <i class="fa-solid fa-bars"></i> **Options** 버튼을 통해 채팅 설정 및 작업을 관리합니다.

### 디스플레이 컨트롤

* <i class="fa-lg fa-solid fa-times"></i> **Close chat**: 현재 채팅 세션 종료
* <i class="fa-lg fa-solid fa-cog"></i> **Toggle Panels**: [인터페이스 패널](/Usage/index.md#control-panels) 표시/숨기기

### 생성 설정

* <i class="fa-lg fa-solid fa-note-sticky"></i> **[Author's Note](/Usage/Characters/Author's-Note.md)**: 사용자 정의 컨텍스트 지시사항
* <i class="fa-lg fa-solid fa-scale-balanced"></i> **[CFG Scale](/Usage/Prompts/CFG.md)**: 응답 창의성 조정
* <i class="fa-lg fa-solid fa-pie-chart"></i> **[Token Probabilities](#token-probabilities-panel)**: 토큰 생성 통계 보기

### 채팅 탐색

* <i class="fa-lg fa-solid fa-left-long"></i> **Back to parent chat**: 메인 대화로 돌아가기
* <i class="fa-lg fa-solid fa-flag"></i> **Save checkpoint**: 스토리 체크포인트 생성
* <i class="fa-lg fa-solid fa-people-arrows"></i> **Convert to group**: [그룹 채팅](/Usage/Characters/groupchats.md)으로 변환

### 채팅 관리

* <i class="fa-lg fa-solid fa-comments"></i> **Start new chat**: 새 대화 시작
* <i class="fa-lg fa-solid fa-address-book"></i> **Manage chat files**: 가져오기, 내보내기, 이름 바꾸기와 같은 [채팅 파일 작업](/Usage/Characters/chatfilemanagement.md)

### 메시지 컨트롤

* <i class="fa-lg fa-solid fa-trash-can"></i> **Delete messages**: 여러 메시지 선택 및 제거
* <i class="fa-lg fa-solid fa-repeat"></i> **Regenerate**: 새 응답 생성
* <i class="fa-lg fa-solid fa-user-secret"></i> **Impersonate**: AI가 사용자로 메시지 작성
* <i class="fa-lg fa-solid fa-arrow-right"></i> **Continue**: 마지막 메시지 확장

참고: 컨텍스트 및 채팅 상태에 따라 일부 옵션이 숨겨질 수 있습니다.

## 토큰 확률 패널

토큰 확률 패널을 사용하면 텍스트 생성을 위한 AI의 샘플링 프로세스를 살펴볼 수 있습니다. AI가 작성한 내용뿐만 아니라 텍스트의 각 지점에서 고려한 다른 옵션도 보여줍니다.

열려면 <i class="fa-solid fa-bars" title="Burger Menu icon"></i> **Chat Options** 패널에서 <i class="fa-solid fa-pie-chart"></i> **Token Probabilities** 버튼을 클릭합니다.

![Example message](/static/token-probs/fling-msg.png){ width=500}

![Token probabilities display for example message](/static/token-probs/fling-probs.png){ width=500}

생성된 텍스트의 토큰(단어, 구두점 또는 형식 문자)을 클릭하면 패널에 AI가 해당 위치에서 고려한 대체 토큰과 확률 점수가 표시됩니다. 이것은 AI의 "사고 프로세스"에 대한 통찰력을 제공하고 응답이 취할 수 있었던 다른 방향을 보여줍니다. 이러한 대안을 살펴보면 여러 가능성이 높은 옵션이 있었는지 또는 단일 명확한 선택이 있었는지 이해하는 데 도움이 될 수 있습니다.

![Alternative tokens and probabilities](/static/token-probs/fling-probs-logprob.png){ width=500}

AI가 다르게 선택했어야 한다고 생각하는 토큰이 보이면 대안을 선택하면 해당 지점부터 메시지가 재생성되어 다른 응답을 얻을 수 있습니다.

### 리롤링

특정 토큰을 변경하고 응답을 재생성하면 변경된 토큰 이전의 새 응답 부분은 원래 응답과 동일합니다. 이 부분은 회색으로 표시됩니다. 생성되지 않았으므로 이 부분에 대한 확률 정보가 없습니다.

대체 토큰을 기반으로 생성될 수 있었던 다른 응답을 보고 싶을 수 있습니다.

회색 부분을 클릭하여 생성을 "리롤"하여 텍스트의 새 변형을 얻을 수 있습니다. 회색 부분의 어느 부분을 클릭하든 전체 회색 부분을 유지하고 전체 흰색/착색 부분을 재생성합니다.

회색 부분의 토큰을 Ctrl을 누른 상태로 클릭하면 클릭한 토큰까지 회색 부분을 유지하고 나머지 텍스트를 재생성합니다. 이 경우 대체 토큰의 선택을 유지할 수 없습니다.

### 컨트롤

**토큰 디스플레이**:

* 생성된 텍스트는 개별 토큰으로 분할됩니다
* 각 토큰은 상호작용이 가능하며, 토큰을 클릭하여 AI가 고려한 대안을 볼 수 있습니다
* 토큰은 시각적 도움으로 착색되지만 이것이 확률을 나타내지는 않습니다
* 특수 문자(공백, 줄 바꿈)는 눈에 띄게 표시됩니다

**토큰 선택**:

* 토큰을 클릭하여 대안 보기
* 대안을 클릭하여 토큰을 교체하고 응답 재생성
* 토큰 위로 마우스를 가져가면 원시 로그 확률 점수를 볼 수 있습니다

**창 컨트롤**:

* <i class="fa-solid fa-grip"></i> 패널 재배치를 위한 드래그 핸들(MovingUI만 해당)
* <i class="fa-solid fa-window-maximize"></i> 패널 크기 최대화/복원
* <i class="fa-solid fa-circle-chevron-up"></i> 패널 콘텐츠 확장/축소
* <i class="fa-solid fa-circle-xmark"></i> 패널 닫기

### 가용성

이 기능을 활성화하려면 [User Settings](/Usage/User_Settings/index.md#chatmessage-handling)에서 **Request token probabilities**를 선택해야 합니다.

토큰 확률은 가장 최근 메시지에만 사용 가능하며 채팅에 저장되지 않습니다. 메시지에 대한 토큰 확률 정보를 더 이상 사용할 수 없는 경우 패널에 이를 나타내는 메시지가 표시됩니다.

Smooth Streaming을 사용할 때는 토큰 확률을 사용할 수 없습니다.

토큰 확률은 모든 API에서 사용할 수 없습니다. 토큰 확률을 지원하지 않는 API를 사용하는 경우 패널은 열리지만 정보를 표시하지 않습니다.

#### Text Completion
* **LlamaCPP**: 사용 가능
* **Text Generation WebUI** (oobabooga): 사용 가능
* **TabbyAPI**: 사용 가능
* **NovelAI**: 사용 가능
* **KoboldCPP**: 사용 가능
* **Ollama**: 사용할 수 없는 것으로 보임
* **OpenRouter Text**: 사용할 수 없는 것으로 보임

#### Chat Completion
* **OpenAI** 또는 **Custom**: 사용 가능하지만 리롤링은 지원되지 않음
* **Anthropic**: 사용할 수 없는 것으로 보임
* **Google AI Studio**: 사용할 수 없는 것으로 보임
* **OpenRouter Chat**: 사용할 수 없는 것으로 보임
