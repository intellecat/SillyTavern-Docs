---
order: 90
route: /usage/core-concepts/macros/
---

# 매크로 (대체 태그)

!!! 참고
이 목록은 불완전하거나 오래되었을 수 있습니다. 인스턴스에서 작동하는 매크로 목록을 가져오려면 SillyTavern 채팅에서 `/help macros` 슬래시 명령을 사용하세요.
!!!

매크로는 캐릭터 설명, 작가 노트, world info 및 기타 많은 곳에서 사용할 수 있으며 응답을 생성할 때 해당 값으로 대체됩니다. 프롬프트에 동적 콘텐츠를 삽입하는 데 사용할 수 있습니다. 예를 들어 사용자 이름, 캐릭터 설명 또는 현재 시간 등입니다. 매크로는 이중 중괄호로 묶여 있습니다. 예: `{{user}}`이며 일반적으로 대소문자를 구분하지 않습니다. **매크로 중첩은 현재 지원되지 않습니다.**

참고: 일부 확장 프로그램은 특정 영역에서만 작동하는 특수 컨텍스트별 매크로를 추가할 수도 있습니다(즉, 확장 프롬프트에 대한 특수 플레이스홀더). 매크로가 특정 기능에 바인딩되지 않은 경우 여기에 문서화되지 않습니다.

## 일반 매크로

| 매크로 | 설명 |
|-------|-------------|
| `{{pipe}}` | 슬래시 명령 배치에만 해당. 이전 명령의 반환된 결과로 대체됩니다. |
| `{{newline}}` | 줄바꿈을 삽입합니다. |
| `{{trim}}` | 이 매크로를 둘러싼 줄바꿈을 제거합니다. |
| `{{noop}}` | 아무 작업도 하지 않으며 빈 문자열입니다. |
| `{{user}}` 또는 `<USER>` | 사용자 이름. |
| `{{charPrompt}}` | 캐릭터의 메인 프롬프트 재정의. |
| `{{charJailbreak}}` | 캐릭터의 Post-History Instructions 프롬프트 재정의. |
| `{{group}}` 또는 `{{charIfNotGroup}}` | 쉼표로 구분된 그룹 멤버 이름 목록 또는 단독 채팅의 캐릭터 이름. |
| `{{groupNotMuted}}` | `{{group}}`과 동일하지만 음소거된 멤버는 제외합니다. |
| `{{notChar}}` | 현재 화자(`{{char}}`)를 제외한 모든 채팅 참가자의 쉼표로 구분된 목록. 그룹 채팅에서는 여전히 음소거된 캐릭터를 포함하며, 메시지가 생성되지 않을 때는 명단의 모든 캐릭터를 나열합니다. |
| `{{char}}` 또는 `<BOT>` | 캐릭터 이름. |
| `{{description}}` | 캐릭터 설명. |
| `{{scenario}}` | 캐릭터의 시나리오 또는 채팅 시나리오 재정의(설정된 경우). |
| `{{personality}}` | 캐릭터 성격. |
| `{{persona}}` | 사용자 페르소나 설명. |
| `{{mesExamples}}` | 캐릭터의 대화 예시(instruct 형식). |
| `{{mesExamplesRaw}}`  | 캐릭터의 대화 예시(변경되지 않고 분할되지 않음). |
| `{{charVersion}}` | 캐릭터 버전 번호. |
| `{{charDepthPrompt}}` | 캐릭터의 at-depth 프롬프트. |
| `{{model}}` | 현재 선택된 API의 텍스트 생성 모델 이름. **부정확할 수 있습니다!** |
| `{{lastMessageId}}` | 마지막 채팅 메시지 ID. |
| `{{lastMessage}}` | 마지막 채팅 메시지 텍스트. |
| `{{firstIncludedMessageId}}` | 컨텍스트에 포함된 첫 번째 메시지의 ID. 현재 세션에서 생성이 한 번 이상 실행되어야 합니다. |
| `{{lastCharMessage}}` | 캐릭터가 보낸 마지막 채팅 메시지. |
| `{{lastUserMessage}}` | 사용자가 보낸 마지막 채팅 메시지. |
| `{{currentSwipeId}}` | 현재 표시된 마지막 메시지 스와이프의 1기반 ID. |
| `{{lastSwipeId}}` | 마지막 채팅 메시지의 스와이프 수. |
| `{{lastGenerationType}}` | 마지막으로 대기열에 추가된 생성 요청의 유형. 값: "normal", "impersonate", "regenerate", "quiet", "swipe", "continue". |
| `{{original}}` | 프롬프트 재정의 필드에서 시스템 설정의 기본 프롬프트를 포함하는 데 사용할 수 있습니다. Chat Completion API 및 Instruct 모드에만 적용됩니다. |
| `{{time}}` | 현재 시스템 시간. |
| `{{time_UTC±X}}` | 지정된 UTC 오프셋(시간대)의 현재 시간. 예: UTC+02:00의 경우 `{{time_UTC+2}}`를 사용합니다. |
| `{{timeDiff::(time1)::(time2)}}` | time1과 time2 간의 시간 차이. 시간 및 날짜 매크로를 허용합니다. |
| `{{date}}` | 현재 시스템 날짜. |
| `{{input}}` | 사용자 입력 바의 내용. |
| `{{weekday}}` | 현재 요일. |
| `{{isotime}}` | 현재 ISO 시간(24시간 형식). |
| `{{isodate}}` | 현재 ISO 날짜(YYYY-MM-DD). |
| `{{datetimeformat ...}}` | 지정된 형식의 현재 날짜/시간 (예: `{{datetimeformat DD.MM.YYYY HH:mm}}`). |
| `{{idle_duration}}` | 마지막 사용자 메시지가 전송된 이후의 시간 범위를 사람이 읽을 수 있는 문자열로 삽입합니다(예: 4 hours, 1 day). |
| `{{random:(args)}}` | 목록에서 무작위 항목을 반환합니다 (예: `{{random:1,2,3,4}}`는 4개의 숫자 중 1개를 무작위로 반환합니다). |
| `{{random::arg1::arg2}}` | 인수에 쉼표를 지원하는 random의 대체 구문. |
| `{{pick::(args)}}` | random의 대안이지만, 소스 문자열이 변경되지 않으면 현재 채팅에서 후속 평가 시 선택된 인수가 안정적입니다. |
| `{{roll:(formula)}}` | D&D 주사위 구문을 사용하여 무작위 값을 생성합니다: XdY+Z (예: `{{roll:d6}}`는 1-6 값을 생성합니다). |
| `{{bias "text here"}}` | 다음 사용자 입력까지 AI에 대한 행동 편향을 설정합니다. 텍스트 주위에 따옴표가 필요합니다. |
| `{{// (note)}}` | 빈 콘텐츠로 대체될 노트를 남길 수 있습니다. AI에게 보이지 않습니다. |
| `{{banned "text here"}}` | Text Generation WebUI 백엔드의 금지 단어 시퀀스에 인용된 텍스트를 동적으로 추가합니다. 다른 백엔드에서는 아무 작업도 하지 않습니다. 따옴표가 필요합니다. |
| `{{reverse:(content)}}` | 매크로의 콘텐츠를 반전합니다. |
| `{{outlet::(name)}}` | 명명된 [World Info outlet](/Usage/worldinfo.md#outlet-name)의 콘텐츠로 대체되며, 줄바꿈으로 구분된 활성화된 항목을 포함합니다. |

## Instruct Mode 및 Context Template 매크로

(고급 형식 설정에서 활성화됨)

| 매크로 | 설명 |
|-------|-------------|
| `{{exampleSeparator}}` | 컨텍스트 템플릿 예시 대화 구분자. |
| `{{chatStart}}` | 컨텍스트 템플릿 채팅 시작 줄. |
| `{{instructSystemPrompt}}` | Instruct 시스템 프롬프트. |
| `{{instructSystemPromptPrefix}}` | 시스템 프롬프트 접두사 시퀀스. |
| `{{instructSystemPromptSuffix}}` | 시스템 프롬프트 접미사 시퀀스. |
| `{{instructUserPrefix}}` | 사용자 메시지 접두사 시퀀스. |
| `{{instructAssistantPrefix}}` | 어시스턴트 메시지 접두사 시퀀스. |
| `{{instructSystemPrefix}}` | 시스템 메시지 접두사 시퀀스. |
| `{{instructUserSuffix}}` | 사용자 메시지 접미사 시퀀스. |
| `{{instructAssistantSuffix}}` | 어시스턴트 메시지 접미사 시퀀스. |
| `{{instructSystemSuffix}}` | 시스템 메시지 접미사 시퀀스. |
| `{{instructFirstAssistantPrefix}}` | 어시스턴트 첫 출력 시퀀스. |
| `{{instructLastAssistantPrefix}}` | 어시스턴트 마지막 출력 시퀀스. |
| `{{instructFirstUserPrefix}}` | Instruct 사용자 첫 입력 시퀀스. |
| `{{instructLastUserPrefix}}` | Instruct 사용자 마지막 입력 시퀀스. |
| `{{instructSystemInstructionPrefix}}` | 시스템 지시 접두사 시퀀스. |
| `{{instructUserFiller}}` | 사용자 필러 메시지 텍스트. |
| `{{instructStop}}` | Instruct 중지 시퀀스. |
| `{{maxPrompt}}` | 토큰 단위의 프롬프트 최대 크기(응답 길이만큼 감소된 컨텍스트 길이). |
| `{{systemPrompt}}` | 허용되고 사용 가능한 경우 캐릭터 프롬프트 재정의를 포함한 시스템 프롬프트 콘텐츠. |
| `{{defaultSystemPrompt}}` | 시스템 프롬프트 콘텐츠(캐릭터 프롬프트 재정의 제외). |

## 채팅 변수 매크로

- 로컬 변수 = 현재 채팅에 고유
- 글로벌 변수 = 모든 캐릭터의 모든 채팅에서 작동

| 매크로 | 설명 |
|-------|-------------|
| `{{getvar::name}}` | 로컬 변수 "name"의 값으로 대체됩니다. |
| `{{setvar::name::value}}` | 빈 문자열로 대체되며, 로컬 변수 "name"을 "value"로 설정합니다. 빈 값을 허용합니다. |
| `{{addvar::name::increment}}` | 빈 문자열로 대체되며, 로컬 변수 "name"에 "increment"의 숫자 값을 추가합니다. |
| `{{incvar::name}}` | 변수 "name"의 값을 1씩 증가시킨 결과로 대체됩니다. |
| `{{decvar::name}}` | 변수 "name"의 값을 1씩 감소시킨 결과로 대체됩니다. |
| `{{getglobalvar::name}}` | 글로벌 변수 "name"의 값으로 대체됩니다. |
| `{{setglobalvar::name::value}}` | 빈 문자열로 대체되며, 글로벌 변수 "name"을 "value"로 설정합니다. 빈 값을 허용합니다. |
| `{{addglobalvar::name::value}}` | 빈 문자열로 대체되며, 글로벌 변수 "name"에 "increment"의 숫자 값을 추가합니다. |
| `{{incglobalvar::name}}` | 글로벌 변수 "name"의 값을 1씩 증가시킨 결과로 대체됩니다. |
| `{{decglobalvar::name}}` | 글로벌 변수 "name"의 값을 1씩 감소시킨 결과로 대체됩니다. |
| `{{var::name}}` | 스코프 변수 "name"의 값으로 대체됩니다(STscript만 해당). |
| `{{var::name::index}}` | 스코프 변수 "name"의 인덱스에 있는 값으로 대체됩니다(STscript의 배열/객체용). |

## 확장 프로그램별 매크로

확장 프로그램에 의해 추가되며 특정 조건에서만 작동합니다.

| 매크로 | 설명 |
|-------|-------------|
| `{{summary}}` | 현재 채팅 세션의 요약으로 대체됩니다(사용 가능한 경우). |
| `{{authorsNote}}` | 작가 노트의 내용으로 대체됩니다. |
| `{{charAuthorsNote}}` | 캐릭터 작가 노트의 내용으로 대체됩니다. |
| `{{defaultAuthorsNote}}` | 기본 작가 노트의 내용으로 대체됩니다. |
| `{{charPrefix}}` | 캐릭터별 이미지 생성 긍정 프롬프트 접두사로 대체됩니다(사용 가능한 경우). |
| `{{charNegativePrefix}}` | 캐릭터별 이미지 생성 부정 프롬프트 접두사로 대체됩니다(사용 가능한 경우). |
