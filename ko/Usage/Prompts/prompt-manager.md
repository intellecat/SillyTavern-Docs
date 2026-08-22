---
order: 50
templating: false
route: /ko/usage/prompts/prompt-manager/
---

# Prompt Manager

Prompt Manager는 Chat Completion API에 대한 [프롬프트 구축](index.md) 전략에 대한 더 많은 제어를 제공하는 시스템입니다.

!!! 적용 대상: Chat Completion APIs
Text Completion API의 동등한 설정은 [Advanced Formatting](advancedformatting.md)을 사용하세요.
!!!

!!!tip 프리셋 이름 지정
프리셋이 캐릭터 카드 중 하나와 이름을 공유하면 해당 캐릭터와 채팅을 시작할 때 자동으로 선택됩니다. 이 동작을 피하려면 프리셋의 이름을 고유하게 지정하세요.
!!!

네비게이션 바의 "AI Response Configuration" 버튼을 클릭하여 Prompt Manager에 액세스합니다. Prompt Manager는 [common settings](/Usage/Common-Settings.md) 패널 아래에 있습니다.

## Quick Prompts Edit

**Main Prompt**, **Auxiliary Prompt** 및 **Post-History Instructions**와 같은 일반적인 프롬프트 섹션을 빠르게 편집할 수 있는 공간을 제공합니다. 이러한 프롬프트에 대한 자세한 내용은 [프롬프트 구축](index.md) 페이지에서 확인할 수 있습니다.

## Utility Prompts

이러한 프롬프트는 Chat Completion 모델이 전송되는 정보를 이해하거나 특정 유형의 상호작용 중에 특정 방식으로 행동하도록 지시하는 데 도움이 되도록 전송됩니다.

### Format Templates

!!!tip
형식 템플릿이 설정되지 않으면 정보가 래핑 없이 그대로 전송됩니다.
!!!

이것들은 [World Info](/Usage/worldinfo.md) 및 [캐릭터 카드](/Usage/Characters/characterdesign.md)에서 가져온 정보를 래핑하는 데 사용되는 문자열 템플릿입니다.

특수 마커를 사용하여 정보를 삽입할 위치를 나타냅니다:

- World Info 형식 템플릿의 경우 `{0}`.
- Scenario 형식 템플릿의 경우 `{{scenario}}`.
- Personality 형식 템플릿의 경우 `{{personality}}`.

### Group Nudge Prompt Template

그룹 채팅에서만 사용됩니다. 특정 캐릭터의 응답을 강제하기 위해 프롬프트 끝에 배치됩니다.

Group Nudge 기능을 비활성화하려면 이것을 비워 두세요.

### New Chat, New Group Chat, New Example Chat

이것들은 채팅 기록 앞과 각 [예시 대화](/Usage/Characters/characterdesign.md#examples-of-dialogue) 블록 앞에 전송되어 모델에 배경 정보가 끝나고 채팅 기록이 시작되는 위치를 알립니다.

- **New Chat:** 개별 채팅에 사용됩니다.
- **New Group Chat:** 그룹 채팅에 사용됩니다.
- **New Example Chat:** 예시 대화 블록에 사용됩니다.

이 기능을 비활성화하려면 이것들을 비워 두세요.

### Continue Nudge

Continue가 트리거될 때 모델에 무엇을 해야 하는지 지시하기 위해 프롬프트 끝에 전송됩니다. 예를 들어 Continue 버튼을 누르거나 STScript에 의해 트리거될 때입니다.

!!! Chat Completion 'Continues'
Chat Completion 모델은 **Text Completion** 모델과 다르게 Continues를 처리하며 Continue Nudge에 관계없이 항상 원활한 결과를 제공하지 않을 수 있습니다.
!!!

### Replace Empty Message

텍스트 상자가 비어 있고 **Send a message**를 누를 때 이 필드의 내용을 빈 메시지 대신 전송합니다.

## Character Names Behavior

모델이 메시지를 캐릭터와 연결하는 방법을 지시하는 다양한 전략을 제공합니다. Chat Completion 모델이 어떤 메시지가 어떤 캐릭터에 속하는지 결정하는 데 문제가 있는 경우 다른 전략을 선택해야 할 수 있습니다.

## Continue Postfix

Continue가 트리거되면 모델이 반환한 '계속된' 메시지는 선택한 Continue Postfix가 시작 부분에 앞에 추가됩니다. 예를 들어, 계속된 텍스트 앞에 공백을 추가할 수 있습니다.

## Additional Settings

### Wrap in Quotes

!!!warning
사용 중단된 옵션입니다. 대신 [Regex scripts](/extensions/Regex.md)를 선호하세요.
!!!

전송하기 전에 전체 사용자 메시지를 숨겨진 따옴표로 감쌉니다. 이것은 캐릭터가 말을 나타내기 위해 따옴표를 사용하지 않는 세션에 유용합니다. 세션에서 말을 나타내기 위해 따옴표를 사용하는 경우 이것을 선택 취소하세요.

### Continue Prefill

!!!warning
모든 Chat Completion 소스에서 작동하지 않을 수 있습니다.
!!!

Continue Nudge를 System 메시지 대신 Assistant 역할 메시지로 전송합니다. 이것이 활성화되면 Continue Nudge 프롬프트가 사용되지 않습니다.

### Squash system messages

!!!warning
사용 중단된 옵션입니다. 대신 [Prompt Post-Processing](/Usage/API_Connections/openai.md#prompt-post-processing)을 선호하세요.
!!!

연속된 System 메시지를 단일 결합 메시지로 결합합니다(예시 대화 제외).

### Enable web search

!!!
[Web Search extension](/extensions/WebSearch.md)과 혼동하지 마세요.
!!!

Chat Completion 백엔드에서 제공하는 웹 검색 기능을 활성화합니다. 프롬프트는 일반적으로 모델 제공자가 검색 결과로 풍부하게 하며 추가 비용이 발생할 수 있습니다.

### Enable function calling

[Function Calling](/For_Contributors/Function-Calling.md)을 참조하세요

### Send inline images, Send inline videos

!!!
[Image Captioning extension](/extensions/captioning.md)과 혼동하지 마세요.
!!!

Chat Completion 모델에 제출된 이미지 및 비디오를 처리하는 멀티모달 기능이 있는 경우, 이것이 그렇게 할 수 있는 능력을 토글합니다. 프롬프트에 미디어를 추가하려면 "Magic Wand" 메뉴의 **Attach A File** 옵션을 사용하세요.

### Request inline images

!!!
[Image Generation extension](/extensions/Stable-Diffusion.md)과 혼동하지 마세요.
!!!

모델이 이미지 첨부 파일을 반환하도록 허용합니다.

### Use system prompt

!!!
Google Gemini 및 Anthropic Claude 백엔드에서만 지원됩니다.

이 두 가지에 대해 매우 유사한 설정이 있지만 기술적으로 별도의 옵션이므로 별도로 구성할 수 있습니다.
!!!

비-시스템 역할(User/Assistant)이 있는 첫 번째 메시지까지의 모든 시스템 메시지를 병합하고 별도의 시스템 지시 필드로 전송합니다.

## Reasoning Settings

Chat Completion 모델이 reasoning을 사용하는 경우 이러한 설정은 가시성과 기능에 영향을 미칩니다.

### Request model reasoning

[Adding Reasoning: By Backend](/Usage/Prompts/reasoning.md#by-backend)를 참조하세요.

### Reasoning Effort

[Reasoning Effort](/Usage/Prompts/reasoning.md#reasoning-effort)를 참조하세요.

## "Prompts"

Prompt Manager는 Chat Completion 모델에 전송되는 프롬프트의 중추를 형성합니다. 무엇이 전송되는지뿐만 아니라 전송되는 *순서*도 제어합니다.

### The 'Prompts' Dropdown

현재 Chat Completion 프리셋이 포함하는 모든 (기본이 아닌) 프롬프트의 드롭다운 목록을 포함합니다. 이러한 프롬프트 중 하나가 나가는 메시지에 추가되려면 드롭다운 목록에서 선택한 다음 **Insert prompt** 버튼을 눌러 Prompt Manager에 추가해야 합니다. 이 드롭다운 목록에 추가할 새 프롬프트를 만들려면 **New prompt** 버튼을 누릅니다. 새 프롬프트가 작성되고 저장되면 드롭다운에 추가되고 삽입할 수 있습니다.

### Prompts List

이것은 Chat Completion 모델에 잠재적으로 전송되도록 선택된 프롬프트를 나열하는 드래그 앤 드롭 인터페이스입니다. 인터페이스의 **상단**에 가까이 배치된 프롬프트가 더 일찍 전송됩니다. 목록의 **하단**은 모델에 전송되는 **마지막 것**입니다(일반적으로 이것은 **Post-History Instructions**일 것입니다).

!!! '고정된' 프롬프트 = 기본 프롬프트
기본 프롬프트는 선택된 프롬프트 목록에서 제거할 수 없습니다. 여기에는 Main Prompt, World Info (before/after), Persona Description, Character Description, Character Personality, Scenario, Enhance Definitions, Auxiliary Prompt, Chat Examples, Chat History 및 Post-History Instructions가 포함됩니다. 이것들이 원하지 않는 경우 **'OFF'로 토글**할 수 있지만 완전히 제거하거나 삭제할 수는 없습니다.
!!!

## Editing a Prompt

프롬프트의 **연필 버튼**을 클릭하면 **Edit interface**로 이동합니다. 여기에서 프롬프트를 직접 편집할 수 있습니다.

!!! 변경 사항을 저장하세요!
Chat Completion 프리셋에서 이러한 프롬프트에 대한 변경 사항을 영구적으로 저장하려면 **Edit interface**의 오른쪽 하단에 있는 **Save** 버튼을 클릭하고 **AI Response Configuration** 섹션 상단에 있는 **Save** 버튼을 사용하여 프리셋 자체도 저장해야 합니다! 그렇지 않으면 변경한 내용이 Chat Completion 프리셋이 다른 것으로 전환될 때 손실됩니다.
!!!

### Name

프롬프트의 이름입니다. 이것은 Chat Completion 모델에 전송되지 않습니다. Prompt Manager 내에서 참조용일 뿐입니다.

### Role

프롬프트를 전송하는 역할입니다. System, AI Assistant 또는 User 중에서 선택할 수 있습니다.

### Triggers

이 프롬프트가 전송되는 생성 유형입니다. 아무것도 선택하지 않으면 프롬프트가 모든 생성 유형에 대해 전송됩니다. 하나 이상이 선택되면 프롬프트는 해당 특정 생성 유형에만 전송됩니다:

- **Normal:** 일반 메시지 생성 요청.
- **Continue:** Continue 버튼을 누를 때.
- **Impersonate:** Impersonate 버튼을 누를 때.
- **Swipe:** 스와이프로 생성이 트리거될 때.
- **Regenerate:** 단독 채팅에서 Regenerate 버튼을 누를 때.
- **Quiet:** 일반적으로 [extensions](/extensions/index.md) 또는 [STscript](/For_Contributors/st-script.md) 명령에 의해 트리거되는 백그라운드 생성 요청.

!!!
"Regenerate" 트리거는 다른 재생성 논리를 사용하므로 그룹 채팅에서 사용할 수 없습니다: 마지막 응답의 모든 메시지가 삭제되고 선택한 [Group reply strategy](/Usage/Characters/groupchats.md#reply-order-strategies)에 따라 "Normal" 생성 유형을 사용하여 메시지가 대기열에 추가됩니다.
!!!

### Position

Position이 **Relative**로 설정되면 이 프롬프트는 다른 모든 프롬프트와 함께 드래그 앤 드롭 인터페이스에 위치한 곳에 전송됩니다. **In-Chat**으로 설정되고 **Depth**가 지정되면 대신 선택한 Role로 **Chat History 내**에 전송되며 드래그 앤 드롭 인터페이스의 순서를 **무시**합니다.

### Depth

Position이 **In-Chat**으로 설정되면 이것은 프롬프트가 채팅 기록 내에서 얼마나 깊이 전송되는지를 정의합니다. 숫자가 높을수록 더 깊이 전송됩니다. 예를 들어, Depth 0은 마지막 채팅 메시지 이후에 전송되고, Depth 1은 마지막 채팅 메시지 앞에 전송되며, Depth 2는 마지막에서 두 번째 채팅 메시지 앞에 전송되는 식입니다.

### Order

!!!
동일한 Role 및 Depth를 가진 프롬프트는 함께 그룹화되고 Order 값으로 정렬됩니다.
순서는 다음과 같습니다(위에서 아래로): User, AI Assistant, System.
!!!

Position이 **In-Chat**으로 설정되면 이것은 채팅 기록 내에서 프롬프트가 전송되는 순서를 정의합니다. 숫자가 낮을수록 더 일찍 전송됩니다.

## Building Your Prompt: Tips and Tricks

효과적인 프롬프트를 작성하는 방법에 대한 자세한 내용은 SillyTavern 문서의 [프롬프트 구축](index.md) 섹션을 참조하세요. 이 정보는 Chat Completion 프리셋에 크게 적용될 수 있습니다.
