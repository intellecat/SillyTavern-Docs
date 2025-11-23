---
order: 140
icon: typography
templating: false
route: /usage/prompts/
---

# 프롬프트

AI에 메시지를 보낼 때 작성하는 텍스트는 다른 텍스트와 결합되어 AI에 전송되는 단일 요청을 형성합니다. 이 결합된 텍스트를 "프롬프트" 또는 때때로 "요청" 또는 "컨텍스트"라고 합니다.

프롬프트에는 다양한 유형의 텍스트가 포함될 수 있습니다:

* AI가 응답을 생성하는 방법에 대한 [메인 지시사항](#main-prompt-system-prompt)
* AI가 맡아야 하는 [역할](/Usage/Characters/characterdesign.md)의 정의
* [사용자가 맡고 있는 역할](/Usage/personas.md)의 정의
* AI가 상호작용하는 ["세계"에 대한 정보](/Usage/worldinfo.md)
* [데이터 뱅크](/Usage/Characters/data-bank.md)의 관련 문서 또는 정보
* 과거 대화의 [요약](/extensions/Summarize.md)
* [웹 검색](/extensions/WebSearch.md) 또는 기타 [외부 데이터 소스](/For_Contributors/Function-Calling.md)의 결과
* 대화의 이전 메시지
* **AI에 대한 사용자의 메시지**
* AI가 응답을 생성하는 방법에 대한 [최종 지시사항](#post-history-instructions)

이것은 관리할 것이 많습니다! AI에 전송되는 요청을 구조화하고 수정하는 방법을 이해하는 데 도움이 되도록 SillyTavern은 프롬프트에 포함하고 싶을 수 있는 다양한 요소를 식별합니다. 그런 다음 AI와 상호작용하려는 방식에 맞는 것들을 포함하도록 프롬프트를 구조화할 수 있습니다.

이러한 요소 중 많은 부분은 변경할 섹션에서 설명됩니다. 예를 들어, AI가 맡았으면 하는 역할을 설명하려면 [캐릭터 디자인](/Usage/Characters/characterdesign.md)의 [설명](/Usage/Characters/characterdesign.md#personality-summary) 필드를 사용할 수 있습니다.

## 프롬프트 보기

AI에 전송된 최종 프롬프트를 읽는 것은 AI가 무엇을 들었는지, 그리고 왜 그러한 응답을 생성했는지 이해하는 데 매우 도움이 됩니다. 여러 방법으로 프롬프트를 볼 수 있습니다:

* AI의 응답 메시지에서 Prompt Itemization 아이콘 사용
* [Prompt Inspector](https://github.com/SillyTavern/Extension-PromptInspector) 확장 프로그램 사용
* SillyTavern을 실행 중인 터미널 창의 로그 확인
* 브라우저의 개발자 도구에서 콘솔 확인

## 프롬프트가 구성되는 방식 변경

프롬프트의 모든 부분을 올바른 방식으로 AI에 제시하는 것은 최상의 응답을 얻는 데 중요합니다. 프롬프트가 구성되는 방식을 제어할 수 있습니다.

+++ Text Completion APIs

Text Completion API에 대한 프롬프트 구성을 사용자 정의하려면 [Advanced Formatting](advancedformatting.md) 패널을 사용하세요.

+++ Chat Completion APIs

Chat Completion API에 대한 프롬프트 구성을 사용자 정의하려면 [Prompt Manager](prompt-manager.md)를 사용하세요.

+++

## 메인 프롬프트 (시스템 프롬프트)

메인 프롬프트(또는 시스템 프롬프트)는 모델이 따라야 할 일반 지시사항을 정의합니다. 대화의 톤과 컨텍스트를 설정합니다. 예를 들어, AI 어시스턴트, 작문 파트너 또는 가상 캐릭터로 행동하도록 모델에 지시합니다.

+++ Text Completion APIs

[시스템 프롬프트](advancedformatting.md#system-prompt)는 [Story String](context-template.md#story-string)의 일부이며 일반적으로 모델이 받는 프롬프트의 첫 번째 부분입니다.

+++ Chat Completion APIs

메인 프롬프트는 [Prompt Manager](prompt-manager.md)의 기본 프롬프트 중 하나입니다. 일반적으로 모델이 받는 컨텍스트의 첫 번째 메시지이며 시스템 역할에 귀속됩니다("sent by").

+++

기본 메인 프롬프트는 다음과 같습니다:

> Write \{\{char\}\}'s next reply in a fictional chat between \{\{char\}\} and \{\{user\}\}.

\{\{char\}\} 및 \{\{user\}\} 플레이스홀더는 대화에서 정의한 캐릭터 및 페르소나의 이름으로 대체됩니다.

메인 프롬프트에서 지원되는 [\{\{매크로\}\}](/Usage/Characters/macros.md) 태그를 사용하여 대화 간에 다를 수 있거나 대화가 진행됨에 따라 변경되는 정보를 포함할 수 있습니다.

### 메인 프롬프트 조정

기본 메인 프롬프트는 모델이 다음에 오는 캐릭터 및 페르소나 정보로 무엇을 해야 하는지, 과거 대화를 해석하는 방법, 그리고 어떤 종류의 응답을 생성해야 하는지 이해하는 데 도움이 됩니다. AI가 페르소나와의 대화에서 캐릭터로 작성하고 있다는 것을 확립하기 때문에 많은 상황에서 잘 작동하는 유연한 범용 프롬프트입니다.

그러나 필요에 맞게 메인 프롬프트를 조정할 수 있습니다. 다음은 메인 프롬프트를 조정하는 몇 가지 일반적인 이유입니다:

* **추가 지시사항 제공**: 예를 들어, AI가 추론을 설명하거나 특정 규칙을 따르거나 특정 주제를 피하도록 하려는 경우
* **AI의 역할 명확히 하기**: 예를 들어, AI가 내레이터, 스토리텔러 또는 가이드로 행동하도록 하려는 경우
* **대화의 컨텍스트 변경**: 예를 들어, AI가 AI 어시스턴트, 텍스트 어드벤처 게임 또는 작문 파트너인 것처럼 응답하도록 하려는 경우

!!! 시도해보고 무엇이 가장 잘 작동하는지 확인하세요
이 가이드의 모든 예시는 다른 사용자에게 잘 작동했지만 사용자의 필요와 사용하는 모델에 맞는 프롬프트는 다를 수 있습니다. 다양한 지시사항과 프롬프팅 스타일을 실험하여 무엇이 가장 잘 작동하는지 확인하세요. 무엇을 시도해야 할지 모르겠다면 [SillyTavern Discord](https://discord.gg/sillytavern)에서 도움을 요청할 수 있습니다.
!!!

메인 프롬프트에서 AI에 추가 지시사항을 제공하면 대화에서 무엇을 원하는지 이해하는 데 도움이 될 수 있습니다.

> Write one reply only. Write at least one paragraph, up to four.

> Markdown is enabled. Use it to format your response. Enclose code snippets in triple backticks.

> Write character dialogue in quotation marks. Write \{\{char\}\}'s thoughts in parentheses.

> You are an anime roleplay generation model for users aged 13 to 17. You always generate fun, age-appropriate responses.

> Answer truthfully and write out your thinking step by step to be sure you get the right answer.

AI는 무엇을 해야 하는지에 대한 지시사항을 무엇을 하지 말아야 하는지에 대한 지시사항보다 더 쉽게 따릅니다. 예를 들어, AI가 특정 방식으로 작성하는 것을 피하도록 하려면 대신 원하는 작성 방식을 알려주는 것이 좋습니다. 그리고 *"Do not decide what \{\{user\}\} says or does"*가 AI가 페르소나를 제어하는 것을 방지하기 위해 프롬프트에 일반적으로 포함되지만, 일부 사용자는 *"Write \{\{char\}\}'s responses in a way that respects \{\{user\}\}'s autonomy"*가 더 효과적이라고 생각합니다.

사용자나 캐릭터에 대한 정보를 포함하거나, 캐릭터의 작문 및 말하기 스타일을 수정하거나, 다른 특정 지시사항을 제공하기에 메인 프롬프트보다 더 나은 장소가 종종 있습니다. 메인 프롬프트는 대화 전체에 대한 일반적인 지시사항이나 하고 싶은 대화 유형에 가장 잘 사용됩니다.

### 메시지 기록의 영향

AI의 응답을 개선하기 위해 메인 프롬프트를 조정할 때 AI가 메시지 기록에서 많은 것을 얻는다는 것을 고려하세요. 기록은 과거 이벤트, 캐릭터 상호작용 및 관계, 그리고 단어 선택 및 작문 스타일에 대한 스타일 가이드의 메모리입니다.

원하는 것을 보여주는 것이 설명하려고 노력하는 것보다 종종 더 쉽기 때문에 원하는 방식으로 AI가 응답하는 방법을 보여주는 [예시 메시지](/Usage/Characters/characterdesign.md#examples-of-dialogue)를 제공하여 이것을 활용하세요!

대화에 이미 기록이 있는 경우 메인 프롬프트를 변경하는 것은 AI의 응답에 제한적인 영향을 미칩니다. 이벤트와 관계 측면에서 AI는 메인 프롬프트가 먼 과거에 발생했다고 가정하고 메시지 기록이 이를 업데이트합니다. 작문 스타일과 단어 선택 측면에서 AI는 기록의 모든 메시지가 *현재* 메인 프롬프트의 규칙에 따라 생성되었다고 가정하고 같은 방식으로 계속 메시지를 생성해야 한다고 가정합니다. 이를 처리하기 위한 몇 가지 제안은 다음과 같습니다:

* [작가 노트](/Usage/Characters/Author's-Note.md)를 사용하여 메시지 기록의 끝 근처 또는 후에 현재 지시사항을 삽입합니다
* 새 대화를 시작하여 메인 프롬프트에 대한 변경 사항을 테스트합니다
* 메시지 기록을 편집하여 원하지 않는 동작의 예를 제거하거나 수정합니다
* [Post-History Instructions](#post-history-instructions)를 사용하여 AI에 최종 지시사항을 제공합니다

!!! 처음부터 제대로 하세요!
AI가 원하지 않는 것을 하도록 절대 "넘어가지" 마세요. AI의 응답이 마음에 들지 않으면 올바른 것처럼 대화를 계속하지 마세요. 대신 프롬프트를 수정하고 메시지를 재생성하고 거기에서 계속하세요. 이것은 AI가 원하는 것을 배우는 데 도움이 됩니다.
!!!

### "Fictional Chat" 컨텍스트 제거

"fictional chat"이 대화에 적합한 컨텍스트가 아닐 수 있는 상황이 있습니다.

메인 프롬프트에서 "fictional" 컨텍스트를 제거할 수 있습니다:

> Write \{\{char\}\}'s next reply in a conversation with \{\{user\}\}.

AI가 자신을 롤플레잉으로 생각하지 않도록 할 수도 있습니다. 캐릭터의 아이디어를 제거하는 대신 AI의 아이디어를 제거할 수 있습니다:

> You are \{\{char\}\}, a helpful assistant. You provide useful information and help \{\{user\}\} with their questions.

### AI를 내레이터 또는 스토리텔러로

AI가 전지적 관점에서 이벤트를 설명하고 자체 캐릭터와 설정을 발명하는 내레이터로 행동하도록 하려면 어떻게 해야 할까요?

한 가지 접근 방식은 AI가 내레이터로 사용할 명명된 캐릭터를 만드는 것입니다. 이 캐릭터는 "Narrator" 또는 "AI"라고 불릴 수 있으며, AI가 범용 스토리텔러임을 제안하거나 특정 시나리오 또는 설정의 이름을 따서 지어질 수 있으며, AI에게 해당 설정에서 이야기를 내레이션하는 작업을 부여합니다. 설정의 세부 사항은 [캐릭터](/Usage/Characters/characterdesign.md) 또는 [World Info](/Usage/worldinfo.md)에서 정의할 수 있습니다.

AI의 역할을 반영하도록 기본 메인 프롬프트를 조정해야 합니다. 범용 내레이터의 경우 다음을 사용할 수 있습니다:

> You are \{\{char\}\}, a skilled and versatile storyteller. Narrate the story.

또는 특정 설정의 경우:

> You are the narrator of a fantasy scenario. Play as the characters that visit \{\{char\}\}.

대화에서 사용자의 역할을 명확히 하는 것이 도움이 됩니다. 메시지가 스토리의 일부인가요, 아니면 캐릭터가 하는 말이나 행동에 대한 내레이터에 대한 지시사항인가요? 스토리에 사용자를 포함하는 예:

> The story should progress by responding to the actions and dialogue of \{\{user\}\}. Narrate the story in third person.

사용자를 스토리에서 제외하는 예:

> Enter Adventure Mode. Narrate the story based on \{\{user\}\}'s dialogue and actions after ">". Describe the surroundings in vivid detail. Be detailed, creative, verbose, and proactive. Move the story forward by introducing fantasy elements and interesting characters.

사용자의 역할을 정의하면 AI가 메시지에 응답하는 방법을 이해하는 데 도움이 될 뿐만 아니라 페르소나를 제어할 수 있는 정도도 명확해집니다. 이것은 AI가 사용자가 직접 하고 싶은 페르소나에 대한 결정을 내리는 상황을 방지합니다.

## Post-History Instructions

Post-History Instructions(PHI)는 메인 프롬프트와 사용자 메시지 이후에 AI에 전송되는 추가 지시사항입니다. 메시지 기록을 기반으로 AI에 추가 컨텍스트 또는 지시사항을 제공하는 데 사용할 수 있습니다.

Post-History Instructions는 사용자 메시지 이후에 전송되므로 AI가 응답을 생성하기 전에 받는 최종 지시사항입니다. AI는 일반적으로 메인 프롬프트보다 더 높은 우선순위를 부여하며 메인 프롬프트의 지시사항을 재정의할 수 있습니다.

캐릭터별 Post-History Instructions를 사용하려면 캐릭터의 [Post-History Instructions](/Usage/Characters/characterdesign.md)에 추가하고 [Prefer Char. Instructions](/Usage/User_Settings/index.md)를 활성화하세요. 캐릭터별 지시사항을 사용하면서 전역적으로 정의된 PHI를 유지하려면 캐릭터의 Post-History Instructions 필드에서 `{{original}}` 매크로를 사용할 수 있습니다.

+++ Text Completion APIs

Post-History Instructions는 시스템 프롬프트 카테고리 아래의 [Advanced Formatting](/Usage/Prompts/advancedformatting.md) 패널에서 정의됩니다. Post-History Instructions는 프롬프트의 마지막 줄(일반적으로 응답 메시지 "헤더"를 포함) 앞에 오는 보이지 않는 사용자 역할 주입으로 추가됩니다. "Enable System Prompt" 토글이 활성화되어야 Post-History Instructions가 적용됩니다(시스템 프롬프트 자체가 비어 있어도).

+++ Chat Completion APIs

Post-History Instructions는 [Prompt Manager](prompt-manager.md)의 기본 프롬프트 중 하나입니다. 일반적으로 모델이 받는 컨텍스트의 마지막 메시지이며 시스템 역할에 귀속됩니다("sent by"). Chat Completion API가 시스템 역할을 지원하지 않는 경우 일반적으로 사용자 역할에 귀속됩니다.

+++

## 프롬프트에 추가 (World Info)

[World Info](/Usage/worldinfo.md) 기능을 사용하여 프롬프트의 어느 곳에나 추가 정보를 삽입할 수 있습니다. 정보를 삽입해야 하는 시기에 대한 조건을 설정하여 AI가 특정 세부 사항을 포함하거나, 응답 방식을 변경하거나, 대화에 새 요소를 추가하도록 안내할 수 있습니다.

World Info의 일반적인 사용 사례는 다음과 같습니다:

* 세계 또는 설정에 대한 정보가 포함된 "lorebook" 또는 "encyclopedia"
* 다양한 캐릭터와 상황에 대한 다양한 시스템 프롬프트를 관리하는 방법
* AI가 대화에서 "회상"해야 하는 메모리를 저장하는 장소
* 캐릭터 세부 사항을 생성, 편집 및 공유하기 위한 더 모듈식 시스템
* AI가 반응하거나 사용자가 반응하도록 만드는 무작위 이벤트 및 놀라움의 소스!
