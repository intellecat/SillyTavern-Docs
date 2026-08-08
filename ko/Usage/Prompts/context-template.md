---
order: 90
templating: false
route: /ko/usage/prompts/context-template/
---

# Context Template

!!! 적용 대상: Text Completion APIs
Chat Completion API의 동등한 설정은 [Prompt Manager](prompt-manager.md)를 사용하세요.
!!!

일반적으로 AI 모델은 특정 방식으로 캐릭터 데이터를 제공하도록 요구합니다. SillyTavern에는 다양한 모델에 대한 사전 제작된 변환 규칙 목록이 포함되어 있지만 원하는 대로 사용자 정의할 수 있습니다.

"[Advanced Formatting](advancedformatting.md)" 패널에서 이러한 설정을 편집합니다.

## Story String

이 필드는 프롬프트 서문(내부적으로 story string으로 알려짐)에 대한 템플릿입니다. 이것은 text completion 및 instruct 모델에 대한 [캐릭터 카드](/Usage/Characters/index.md)에서 정의된 정보를 추가하는 주요 방법입니다.

템플릿은 Handlebars 구문, 사용자 정의 텍스트 주입 또는 형식화 및 기타 [매크로](/usage/macros.md)를 지원합니다. 언어 참조는 여기를 참조하세요: <https://handlebarsjs.com/guide/>

Handlebars 평가기에 다음 매개변수를 제공합니다(이중 중괄호로 감싸짐):

1. `{{anchorBefore}}`: "Before Story String" 위치를 사용하도록 설정된 프롬프트.
2. `{{anchorAfter}}`: "After Story String" 위치를 사용하도록 설정된 프롬프트.
3. `{{description}}`: 캐릭터의 [설명](/Usage/Characters/characterdesign.md#character-description).
4. `{{scenario}}`: 캐릭터의 [시나리오](/Usage/Characters/characterdesign.md#scenario).
5. `{{personality}}`: 캐릭터의 [성격](/Usage/Characters/characterdesign.md#personality-summary).
6. `{{system}}`: [시스템 프롬프트](advancedformatting.md#system-prompt) 또는 캐릭터의 [메인 프롬프트](/Usage/Characters/characterdesign.md#prompt-overrides) 재정의(존재하고 User Settings에서 "Prefer Char. Prompt"가 활성화된 경우).
7. `{{persona}}`: 선택한 [페르소나의 설명](/Usage/personas.md#persona-description).
8. `{{char}}`: 캐릭터의 이름.
9. `{{user}}`: 선택한 페르소나의 이름.
10. `{{wiBefore}}` 또는 `{{loreBefore}}`: Position이 "Before Char Defs"로 설정된 결합된 활성화된 [World Info](/Usage/worldinfo.md) 항목.
11. `{{wiAfter}}` 또는 `{{loreAfter}}`: Position이 "After Char Defs"로 설정된 결합된 활성화된 [World Info](/Usage/worldinfo.md) 항목.
12. `{{mesExamples}}`: (선택 사항) 구분자로 instruct 형식화된 캐릭터의 [예시 대화](/Usage/Characters/characterdesign.md#examples-of-dialogue).
13. `{{mesExamplesRaw}}`: 형식화 없이 원시 형식의 캐릭터의 [예시 대화](/Usage/Characters/characterdesign.md#examples-of-dialogue).

!!!tip **중요**
Story String에서 `{{mesExamples}}`를 사용할 때 **<i class="fa-solid fa-user-cog"></i> User Settings** 패널의 **"Example Messages Behavior"**를 **"Never include examples"**로 설정하여 프롬프트에서 예시 메시지를 중복하지 않도록 하세요.
!!!

특수 `{{trim}}` 매크로는 그것을 둘러싼 줄바꿈을 제거하도록 지원됩니다. 텍스트의 일부가 이전 줄과 줄바꿈으로 구분되지 않도록 하려면 사용하세요(_공백은 **트림되지 않습니다**_).

**경고**: 위의 매개변수 중 하나라도 story string 템플릿에서 누락되면 프롬프트에 전혀 전송되지 않습니다.

### Prompt Anchors

`{{anchorBefore}}` 및 `{{anchorAfter}}`는 선택한 정적 위치에서 다양한 확장 프로그램 및 기타 기능에 의해 추가된 프롬프트에 대한 일반적인 플레이스홀더입니다. 예:

* [작가 노트](/Usage/Characters/Author's-Note.md)
* [요약](/extensions/Summarize.md)
* [Chat Vectorization](/extensions/Chat-vectorization.md) / [Data Bank](/Usage/Characters/data-bank.md)
* [STscript injections](/For_Contributors/st-script.md#prompt-injections)
* [Web Search](/extensions/WebSearch.md)

### Story String position

기본적으로 렌더링된 story string(모든 플레이스홀더가 대체됨)은 프롬프트의 맨 처음에 배치되고 예시 메시지와 가시적인 채팅 기록이 뒤따릅니다.

또는 "In-chat @ Depth" 옵션을 선택하여 동적 위치로 이동할 수 있으며, 이는 채팅 컨텍스트의 특정 깊이에 story string을 배치합니다.

!!!warning **주의**
템플릿에 story string을 감싸기 위한 정적 프롬프트 요소(모델별 접두사 또는 접미사)가 포함된 경우, "In-Chat @ Depth" 위치를 사용하면 중복 시퀀스로 잘못 이중 감싸져 예기치 않은 결과가 발생할 수 있습니다.

이 경우 다음 방법 중 하나로 문제를 해결할 수 있습니다:

1. **내장 템플릿**: [Advanced Formatting](/Usage/Prompts/advancedformatting.md#resetting-templates)에 설명된 단계를 사용하여 템플릿을 기본값으로 재설정합니다.
2. **사용자 정의 템플릿**: story string 템플릿에서 정적 요소를 [Story String Sequences](/Usage/Prompts/instructmode.md#sequences-story-string-wrapping)로 이동합니다.
!!!

### Story String wrapping

!!!
다음 섹션은 **Instruct Mode**가 ON일 때만 적용됩니다.
!!!

* **Default** 위치: 렌더링된 Story String은 [Story String Sequences](/Usage/Prompts/instructmode.md#sequences-story-string-wrapping)에 정의된 시퀀스를 사용하여 감싸집니다.
* **In-chat @ Depth** 위치: 렌더링된 Story String은 선택한 역할(기본값: System)에 대한 [Chat Messages Sequences](/Usage/Prompts/instructmode.md#sequences-chat-messages-wrapping)에 정의된 시퀀스를 사용하여 감싸집니다.

## Example Separator

블록 헤더 및 예시 대화 블록 간의 구분자로 사용됩니다. 예시 대화에서 `<START>` 태그의 모든 인스턴스는 이 필드의 내용으로 대체됩니다.

## Chat Start

렌더링된 story string 이후 및 예시 대화 블록 이후에 구분자로 삽입되지만 컨텍스트의 첫 번째 메시지 앞에 삽입됩니다.

## Separators as Stop Strings

중지 문자열 목록에 "Example Separator" 및 "Chat Start"를 추가합니다.

모델이 구분자가 앞에 오는 예시 대화의 전체 블록을 환각하거나 누출하는 경향이 있는 경우 유용합니다.

## Names as Stop Strings

중지 문자열 목록에 캐릭터 및 User Persona 이름을 추가합니다.

모델 가장을 방지하기 위해 켜 두는 것이 좋습니다.

## Always add character's name to prompt

!!!info
이 설정은 Instruct Mode가 ON일 때 효과가 없습니다. 대신 이름 동작은 선택한 [Include Names](/Usage/Prompts/instructmode.md#include-names) 옵션에 의해 정의됩니다.
!!!

모델이 캐릭터로 메시지를 완성하도록 강제하기 위해 프롬프트에 캐릭터의 이름을 추가합니다:

```txt
** OTHER CONTEXT HERE **
Character:
```
