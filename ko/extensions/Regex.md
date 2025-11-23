---
route: /extensions/regex/
---

# Regex

## 이게 뭔가요?

Regex 확장 기능을 사용하면 사용자가 텍스트 문자열에서 특정 패턴('시퀀스'라고 함)을 자동으로 감지하고 조작(교체)을 적용할 수 있습니다. [Quick Replies 또는 STscript](/For_Contributors/st-script.md)와 같은 다른 SillyTavern 기능과 함께 사용하거나 단순히 채팅에서 특정 단어를 제거하는 데 사용할 수 있는 강력한 도구입니다.

## 유용한 링크

**이 문서는 RegEx 시퀀스 작성 프로세스를 자세히 설명하지 않습니다. 이를 돕기 위한 많은 온라인 리소스가 있습니다.**

- [https://regexr.com](https://regexr.com)

- [https://regex101.com](https://regex101.com)

- [https://extendsclass.com/regex-tester.html](https://extendsclass.com/regex-tester.html)

- [https://en.wikipedia.org/wiki/Regular_expression](https://en.wikipedia.org/wiki/Regular_expression)

## 사전 요구 사항

Regex는 SillyTavern의 내장 확장 기능이므로 추가 설정이 필요하지 않습니다.

**<i class="fa-solid fa-cubes"></i> Extensions** 패널에서 설정을 찾을 수 있습니다.

## 일반적인 사용 사례

RegEx는 채팅의 특정 단어에 찾기-바꾸기 기능을 적용하거나, 특정 단어 또는 문장 유형에 마크다운 스타일을 추가하거나, STscript에 부울 값을 반환하는 데 자주 사용됩니다.

## 스크립트 목록

![RegEx Extension Script List](/static/extensions/regex-listview.png)

- 상단의 버튼은 새 스크립트를 만드는 데 사용됩니다.
  - 'Global' 스크립트는 모든 캐릭터에 적용되며 `settings.json`에 저장됩니다.
  - 'Scoped' 스크립트는 현재 활성 캐릭터에만 적용되며 캐릭터 카드의 데이터에 저장됩니다.
- 'Import'를 사용하면 다른 SillyTavern 인스턴스에서 내보낸 RegEx 스크립트를 가져올 수 있습니다.

아래에는 일부 작업 버튼이 있는 스크립트 목록이 있습니다.

- 드래그 핸들(스크립트 이름 왼쪽의 가로 막대 3개)을 사용하여 스크립트를 원하는 순서로 드래그/드롭할 수 있습니다.
- 기본 on/off 스위치를 빠르게 토글하여 다른 것을 변경하지 않고 스크립트를 활성화 또는 비활성화할 수 있습니다. 비활성화된 스크립트는 ~~취소선~~ 스타일로 표시됩니다. **여기서 스크립트가 비활성화되면 Quick Reply 또는 STscript로 트리거할 수 없습니다.**
- 'Edit'(연필) 버튼을 클릭하면 RegEx 스크립트 편집기가 열립니다.
- 'Move to scoped'(아래 화살표)는 전역 스크립트를 범위 지정 스크립트로 변환하고 현재 캐릭터에 적용합니다. 반대로(위 화살표) 범위 지정 스크립트를 전역으로 변환합니다.
- 'Export'는 브라우저가 스크립트의 내보낸 `.json` 파일을 다운로드하도록 하며, 이를 공유하고 다른 SillyTavern 인스턴스로 가져올 수 있습니다.
- 'Delete'(휴지통)는 스크립트를 삭제합니다.

## RegEx 편집기

![RegEx Editor](/static/extensions/regex-editor.png)

- **Test Mode** : 편집기 상단에 비교 뷰가 열립니다. 'Input' 상자에 일부 텍스트를 입력하면 Output 상자에 RegEx 스크립트의 결과가 표시됩니다. 스크립트 설정을 변경할 때 Output 상자가 실시간으로 업데이트되므로 유용한 디버그 도구입니다.

- **Name** : 확장의 스크립트 목록에 표시되는 스크립트의 레이블입니다. **슬래시 명령 또는 STscript를 통해 스크립트를 트리거할 때 스크립트를 대상으로 지정하는 데도 사용됩니다.**

- **Find Regex** : 대상 텍스트 패턴을 감지하는 데 사용되는 정규 표현식입니다. 일반적으로 RegEx 스크립트의 가장 복잡한 부분이며 실수하기 가장 쉬운 곳입니다. RegEx 시퀀스 작성 방법에 대한 정보는 페이지 상단의 링크를 참조하세요. 이 상자는 'Macros in Find Regex'가 그렇게 설정된 경우(아래 참조) [일반적인 SillyTavern 매크로](/Usage/Characters/macros.md)(예: \{\{user\}\}, \{\{char\}\} 등)의 값을 해결할 수 있습니다.

- **Replace With**: 일치하는 시퀀스를 대체할 항목입니다. 매우 간단한 예로 'Find Regex'가 `apple`이고 'Replace With'가 `orange`인 경우 스크립트가 적용되는 텍스트에서 'apple'의 첫 번째 발생이 자동으로 'orange'로 변경됩니다.

  - 이 상자에 확장별 매크로 \{\{match\}\}를 추가하면 일치하는 전체 텍스트 시퀀스가 삽입됩니다. 이것은 일반적으로 특정 단어에 스타일을 적용하는 데 사용됩니다. 위의 예로 돌아가서 'Replace With' 상자에 \*\*\{\{match\}\}\*\*를 넣으면 'apple'이라는 단어의 모든 발생이 `**apple**`로 대체되어 굵은 마크다운 스타일이 적용됩니다.

  - $1, $2, $3 등과 같은 변수를 사용하여 'Capture Groups'라고 하는 것을 삽입할 수 있습니다. 이것은 'Find Regex' 시퀀스와 일치하는 텍스트 시퀀스에 있는 부분 문자열입니다. **이러한 변수를 사용하려면 일치하는 표현식에 일치하는 문자열의 어느 부분이 캡처된 그룹으로 간주되는지 정의하기 위해 괄호 세트가 포함되어야 합니다.** Capture Groups 설정 방법에 대한 참조는 상단의 링크를 참조하세요.

- **Trim Out** : 이 상자에 넣은 텍스트는 'Replace With' 프로세스가 적용되기 전에 일치하는 텍스트 시퀀스에서 제거됩니다. 예를 들어 일치가 'apple'이고 Trim Out 상자에 'le'가 포함되어 있으면 'Replace With' 프로세스가 적용되기 전에 문자 'le'가 먼저 제거됩니다. 'Replace With' 상자에 \*\*\{\{match\}\}\*\*가 포함되어 있으므로 'apple'의 대체로 `**app**`가 삽입됩니다(먼저 'le'가 제거되고 나머지 일치하는 텍스트에 굵은 마크다운 스타일이 지정됩니다). 제거하려는 각 문자열 사이에 줄 바꿈을 추가하여 여러 트림을 적용할 수 있습니다.

- **Affects** : 이 체크박스 목록은 RegEx 스크립트가 적용될 텍스트 소스를 정의합니다.
  - 'User Input': 사용자가 Send를 누른 후 사용자의 입력한 내용에 대해 스크립트가 실행됩니다.
  - 'AI Response': AI의 응답을 받은 후 AI의 응답 내용에 대해 스크립트가 실행됩니다.
  - 'Slash Commands': 슬래시 명령으로 프롬프트/채팅에 삽입된 값에 대해 스크립트가 실행됩니다.
  - 'World Info': World Info 항목이 프롬프트에 주입될 때 World Info 항목의 내용에 대해 스크립트가 실행됩니다. **'Alter Outgoing Prompt'를 선택해야 합니다(또는 두 ephemerality 상자를 모두 선택 해제).**
  - 'Reasoning': Gemini 또는 Deepseek와 같은 Chat Completion API에서 반환하는 'reasoning' 객체의 내용에 대해 스크립트가 실행됩니다. Ephemerality에서 'Alter Outgoing Prompt'를 선택하면 후속 채팅 턴에서 프롬프트에 추가되는 reasoning 블록에도 스크립트가 적용됩니다.
  - **여기서 모든 것이 선택 해제되면 스크립트는 일반 채팅 중에 활성화되지 않지만 슬래시 명령 또는 STscript를 통해 활성화할 수 있습니다.**

- **Other Options** :
  - 'Disabled'는 스크립트가 실행되지 않도록 합니다. 스크립트의 설정을 변경하고 싶지 않고 스크립트 목록의 스위치를 통해 완전히 비활성화하고 싶지 않을 때 재정의로 사용됩니다(그렇게 하면 슬래시 명령이 트리거되지 않기 때문).
  - 'Run on Edit'는 채팅 메시지가 편집된 후에도 스크립트가 실행되도록 합니다. 이것이 선택 해제되면 편집된 채팅 메시지의 내용이 스크립트를 트리거하지 않습니다.

- **Macros in Find Regex** : Find Regex 상자의 시퀀스에 있는 매크로(예: \{\{user\}\}, \{\{char\}\} 등)를 교체할지 여부를 선택합니다.
  - 'Don't Substitute'는 SillyTavern 매크로가 무시되도록 하여 RegEx 스크립트가 검색할 때 문자 그대로 처리하도록 합니다.
  - 'Raw'는 매크로의 값을 그대로 보냅니다. 매크로의 값에 특정 특수 문자가 포함되어 있으면 RegEx 스크립트가 텍스트를 검색하는 방식이 변경될 수 있습니다.
  - 'Escaped'는 각 문자 앞에 RegEx 이스케이프 슬래시 `\`를 추가하여 실수로 전체 RegEx 시퀀스를 변경하지 않도록 합니다. 매크로의 값에 특정 특수 문자가 있는 경우 유용할 수 있습니다.

### Depth 설정

Min/Max Depth 설정은 regex 패턴이 채팅 기록의 어떤 메시지에 영향을 미칠지 정확하게 제어합니다:

- **Min Depth**: 채팅 기록에서 최소 N 레벨 깊이에 있는 메시지에만 영향을 미칩니다
  - 0 = 마지막 메시지
  - 1 = 마지막에서 두 번째 메시지
  - 등등.
  - 비어 있으면('Unlimited'로 설정) 또는 -1이면 Continue 작업에서 계속할 메시지에도 영향을 미칩니다

- **Max Depth**: 채팅 기록에서 N 레벨보다 깊지 않은 메시지에만 영향을 미칩니다
  - regex가 적용되려면 Min Depth보다 커야 합니다
  - 시스템 프롬프트 및 유틸리티 프롬프트는 이러한 설정의 영향을 받지 않습니다

예를 들어 Min Depth를 0으로, Max Depth를 2로 설정하면 채팅의 최근 3개 메시지에만 regex가 적용됩니다.

### 플래그

기본적으로 Find Regex 패턴은 대소문자를 구분하며 첫 번째 일치에만 적용됩니다. 이 동작과 기타 RegEx 플래그를 조정하려면 다음과 같이 추가할 수 있습니다:

```txt
/yourpattern/flags
```

예: `/yourpattern/gi`는 대소문자에 관계없이 텍스트에서 'yourpattern'의 모든 인스턴스와 일치합니다.

가장 일반적인 플래그 중 일부는 다음과 같습니다:

- `i` : 대소문자 구분 안 함
- `g` : 전역(첫 번째뿐만 아니라 모든 일치에 적용)
- `s` : dotAll(입력을 단일 줄로 처리하므로 `.`가 줄 바꿈과 일치)
- `m` : 여러 줄(입력을 여러 줄로 처리하므로 `^`와 `$`가 전체 문자열이 아닌 각 줄의 시작/끝과 일치)
- `u` : 유니코드(입력을 유니코드로 처리하므로 `\d`, `\w` 등이 유니코드 문자와 일치)

RegEx 플래그에 대한 자세한 내용은 다음 MDN 페이지를 참조하세요: [Advanced searching with flags](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions#advanced_searching_with_flags)

### Ephemerality

기본적으로(여기서 어느 상자도 선택하지 않은 경우) RegEx 스크립트는 채팅의 JSONL 파일에 저장된 텍스트 값을 직접 편집합니다. 이렇게 하면 나가는 프롬프트와 채팅 표시가 항상 동일한 값을 포함하도록 합니다. 그러나 채팅 파일에 대한 이러한 변경 사항은 되돌릴 수 없습니다.

이런 일이 발생하지 않도록 하려면 여기서 체크박스 중 하나를 활성화하여 RegEx 스크립트의 영향을 표시 또는 나가는 프롬프트로만 제한할 수 있습니다.

상자 중 하나만 선택하면 채팅 파일에 변경 사항이 없지만 **선택한 항목만** 변경됩니다. 즉, 한 가지를 보지만 LLM은 다른 것을 보게 됩니다. 이것을 신중하게 사용하세요.

둘 다 선택하면 스크립트는 채팅 파일에 변경 사항을 쓰지 않는 것을 제외하고 모든 면에서 정상적으로 작동합니다.

## 고급 사용

RegEx는 일반적으로 간단한 찾기/바꾸기 도구로 사용되지만 더 복잡한 방식으로도 사용할 수 있습니다.

예를 들어 'Replace With' 상자에 CSS 규칙과 HTML 세트를 포함하여 특정 단어가 발견될 때마다 특정 스타일의 HTML 요소를 채팅에 추가할 수 있습니다. 이를 위해서는 User Settings 패널에서 `Show <tags> in responses` 상자를 선택 해제해야 합니다.

스크립트는 일반 사용 중에 트리거되지 않도록 설정할 수도 있지만 대신 STscript 내의 논리 검사의 일부로 슬래시 명령을 통해 트리거될 수 있습니다. 'Replace With' 상자에는 논리 검사가 참인지 거짓인지 나타내기 위해 스크립트가 인식하는 고유한 값이 포함됩니다. 이렇게 하면 RegEx의 유용성이 모든 슬래시 명령의 전체 기능으로 확장되어 채팅 내용을 기반으로 진정으로 무제한 수준의 제어 및 자동화가 가능합니다.
