---
order: 140
icon: codescan
route: /ko/usage/core-concepts/macros/
templating: false
label: 매크로
title: 매크로
---

# Macros

!!!tip Experimental Macro Engine
Experimental Macro Engine은 중첩, 안정적인 치환 순서 및 기타 개선 사항을 지원합니다. 새로운 설치에서는 기본적으로 활성화되어 있습니다. 기존 설치에서는 **User Settings** > **Chat/Message Handling** > **Experimental Macro Engine**에서 활성화할 수 있습니다.
!!!

매크로는 텍스트가 처리될 때 실제 값으로 대체되는 동적 자리 표시자입니다. 프롬프트, 캐릭터 카드, lorebook, Quick Replies 등 SillyTavern 전반에서 사용됩니다.

## 사용 가능한 매크로 찾기

SillyTavern은 사용 가능한 모든 매크로에 대한 내장 문서를 제공합니다:

- **슬래시 명령**: 채팅 입력에 `/? macros`를 입력하면 등록된 모든 매크로 목록과 설명이 표시됩니다.
- **자동 완성**: 입력하는 동안 제안을 받는 방법은 아래 [매크로 자동 완성](#macro-autocomplete)을 참조하세요.

### 매크로 자동 완성

매크로 자동 완성은 입력하는 동안 사용 가능한 매크로에 대한 제안을 제공합니다. 매크로를 지원하는 SillyTavern 전체의 모든 텍스트 필드에서 작동합니다.

`{{`를 입력하면 매크로에 대한 자동 완성이 시작되어 사용 가능한 매크로와 해당 인수, 가능한 [매크로 플래그](#macro-flags), [변수 축약형](#variable-shorthands) 등을 보여줍니다.

**자동 완성이 기본적으로 나타나는 위치:**

- 채팅 사용자 입력 상자
- 확장된 편집기(텍스트 필드 옆의 'Expand' 버튼을 통해 열리는 전체 화면 텍스트 편집)
- Prompt manager 편집기

**다른 필드에서 자동 완성 트리거하기:**

- 매크로를 지원하는 텍스트 필드에서 **Ctrl+Space**를 눌러 자동 완성 팝업을 엽니다
- **Settings → AutoComplete Settings → Show in all macro fields**를 활성화하면 매크로를 지원하는 모든 필드에서 자동 완성이 자동으로 나타납니다

## 기본 구문

매크로는 이중 중괄호로 둘러싸여 있습니다:

```txt
{{macroName}}
```

매크로 이름은 **대소문자를 구분하지 않습니다**. `{{User}}`, `{{USER}}`, `{{user}}`는 모두 동일한 매크로로 해석됩니다.

예시:

```txt
{{user}}        // Returns the current user/persona name
{{char}}        // Returns the current character name
{{time}}        // Returns the current time
{{date}}        // Returns the current date
```

## 인수

많은 매크로가 동작을 사용자 정의하기 위해 인수를 받습니다.

### 공백 구분자

인수가 하나인 매크로의 경우 공백으로 매크로 이름과 인수를 구분할 수 있습니다:

```txt
{{macroName argument}}
```

예시:

```txt
{{getvar myVariable}}
{{roll 1d20}}
{{reverse Hello World}}
```

### 이중 콜론 구분자

여러 인수를 구분하려면 `::`를 사용하세요:

```txt
{{macroName::arg1::arg2::arg3}}
```

예시:

```txt
{{setvar::myVariable::Hello World}}
{{random::red::green::blue}}
{{roll::2d6+3}}
```

공백과 `::` 모두 매크로 인수에 권장되는 구문입니다.

### 단일 콜론 구분자 (레거시)

단일 `:`도 인수를 도입할 수 있지만, 이 구문은 레거시로 간주되며 새로운 콘텐츠에는 권장되지 않습니다:

```txt
{{macroName:argument}}
```

예시:

```txt
{{roll:1d20}}
```

## 매크로 정의의 공백

매크로 이름, 구분자, 인수 사이의 공백은 무시됩니다. 이를 통해 더 읽기 쉬운 형식을 사용할 수 있습니다:

```txt
{{ macroName :: arg1 :: arg2 }}
{{ setvar :: myVariable :: some value }}
{{ if :: condition }}
```

위의 모든 예시는 추가 공백이 없는 압축된 형식과 동일합니다.

## 중첩된 매크로

매크로는 다른 매크로 안에 중첩될 수 있습니다. 내부 매크로가 먼저 해석됩니다:

```txt
{{getvar::{{char}}_mood}}
```

이것은 먼저 `{{char}}`를 해석한 다음(예: "Alice"로), `{{getvar::Alice_mood}}`를 해석합니다.

더 많은 예시:

```txt
{{setvar::greeting::Hello, {{user}}!}}
```

사용자 이름이 포함된 콘텐츠로 변수를 설정합니다.

```txt
{{if {{getvar::showDetails}}}}Details here{{/if}}
```

조건 자체가 변수 값을 검색하는 매크로입니다.

## 범위 지정 매크로

인수를 하나 이상 받는 모든 매크로는 범위 지정 구문을 지원합니다. 여는 태그와 닫는 태그 사이의 콘텐츠는 매크로의 **마지막 인수**가 됩니다.

### 범위 지정 구문

마지막 인수를 인라인으로 작성하는 대신 여는 태그와 닫는 태그 사이에 배치할 수 있습니다:

```txt
{{macroName argument}}
  scoped content here
{{/macroName}}
```

닫는 태그는 매크로 이름 앞에 `/` 플래그를 사용합니다: `{{/macroName}}`.

이것은 다음을 작성하는 것과 동일합니다:

```txt
{{macroName::argument::scoped content here}}
```

### 예시

여러 줄 콘텐츠로 변수 설정하기:

```txt
{{ setvar backstory }}
  This character was born in a small village
  and grew up to become a renowned scholar.
{{ /setvar }}
```

범위가 지정된 콘텐츠와 함께 `reverse` 사용하기:

```txt
{{ reverse }}
  Hello World
{{ /reverse }}
```

이것은 "dlroW olleH"를 반환합니다.

### 콘텐츠 트리밍

기본적으로 범위가 지정된 콘텐츠는 자동으로 트리밍됩니다:

- 선행 및 후행 공백이 제거됩니다
- 일관된 들여쓰기가 제거됩니다(첫 번째 비어 있지 않은 줄의 들여쓰기가 모든 줄에서 제거됨)

이를 통해 깔끔한 형식을 사용할 수 있습니다:

```txt
{{ if condition }}
    # Heading
    Some content here
{{ /if }}
```

`# Heading\nSome content here`를 생성합니다(선행 공백 없이).

선행/후행 줄 바꿈을 포함한 모든 공백을 보존하려면 `#` 플래그를 사용하세요. 자세한 내용은 [매크로 플래그](#macro-flags)를 참조하세요.

## 조건부 매크로

`{{if}}` 매크로는 값이 truthy인지 falsy인지에 따라 조건부로 콘텐츠를 렌더링합니다.

### 단순 조건

```txt
{{ if description }}
  # Character Description
  {{ description }}
{{ /if }}
```

이것은 `description`이 비어 있지 않은 값을 반환하는 경우에만 제목과 설명을 표시합니다.

조건은 다음이 될 수 있습니다:

- 매크로 이름(인수가 필요하지 않은 경우 자동으로 해석됨)
- `{{getvar::flag}}`와 같은 중첩 매크로의 모든 값
- `.myFlag` 또는 `$globalFlag`와 같은 변수 축약형(자세한 내용은 [변수 축약형](#variable-shorthands) 참조)
- 원하는 모든 텍스트(콘텐츠에 따라 암묵적으로 truthy 또는 falsy로 해석됨)

Falsy 값: 빈 문자열, `false`, `0`, `off`, `no`.

### 조건에서 변수 축약형 사용하기

변수 축약형은 조건에서 변수 값을 확인하는 간결한 방법을 제공합니다:

```txt
{{ if .isEnabled }}
  Feature is enabled.
{{ /if }}

{{ if !$globalDisabled }}
  Not globally disabled.
{{ /if }}
```

축약 표기법에 대한 자세한 내용은 [변수 축약형](#variable-shorthands)을 참조하세요.

### 반전된 조건

조건 앞에 `!`를 붙이면 반전됩니다:

```txt
{{ if !personality }}
  No personality defined for this character.
{{ /if }}
```

### If/Else 분기

`{{if}}` 블록 안에서 `{{else}}`를 사용하여 대체 분기를 정의합니다:

```txt
{{ if personality }}
  {{ personality }}
{{ else }}
  No personality defined.
{{ /if }}
```

또 다른 예시:

```txt
{{ if {{getvar::details-block}} }}
  # Details Block
  {{ getvar::details-block }}
{{ else }}
  No details currently exist.
{{ /if }}
```

## 매크로 플래그

플래그는 매크로 동작을 수정하기 위해 여는 중괄호와 매크로 이름 사이에 배치되는 특수 기호 문자입니다.

### 구문

```txt
{{!macroName}}
{{#macroName}}
```

플래그는 결합할 수 있습니다:

```txt
{{!?macroName}}
```

플래그와 매크로 이름 사이에 공백이 허용됩니다:

```txt
{{ / macroName }}
{{ # macroName }}
```

### 구현된 플래그

| 플래그 | 이름 | 설명 |
|------|------|-------------|
| `/` | 닫는 블록 | 범위 지정 매크로의 닫는 태그를 표시합니다. 예: `{{/if}}` |
| `#` | 공백 보존 | 범위가 지정된 콘텐츠의 자동 트리밍을 방지합니다. |

### 계획된 플래그 (아직 구현되지 않음)

| 플래그 | 이름 | 설명 |
|------|------|-------------|
| `!` | 즉시 | 같은 텍스트에서 다른 매크로보다 먼저 이 매크로를 해석합니다. |
| `?` | 지연 | 같은 텍스트에서 다른 매크로보다 나중에 이 매크로를 해석합니다. |
| `~` | 재평가 | 이 매크로를 재평가하도록 표시합니다. |
| `>` | 필터 | 이 매크로에 대해 파이프 기반 출력 필터를 활성화합니다. |

### 플래그와 유사한 접두사 연산자

변수 축약형 구문은 플래그와 비슷하게 동작하지만 플래그 자체는 아닌 접두사 연산자(`.` 및 `$`)를 사용합니다.
자세한 내용은 [변수 축약형](#variable-shorthands) 섹션을 참조하세요.

### 공백 보존 플래그

선행/후행 줄 바꿈 및 들여쓰기를 포함하여 범위가 지정된 콘텐츠의 모든 공백을 보존해야 하는 경우 `#` 플래그를 사용하세요:

```txt
{{ # setvar code }}
    function hello() {
        return "world";
    }
{{ /setvar }}
```

`#`이 없으면 콘텐츠가 트리밍되고 들여쓰기가 제거됩니다. `#`을 사용하면 여는 태그 뒤와 닫는 태그 앞의 줄 바꿈을 포함하여 모든 공백이 작성된 그대로 정확히 보존됩니다.

## 주석

출력에 나타나지 않는 메모를 추가하려면 comment 매크로를 사용하세요:

```txt
{{// This is a comment and will be removed}}
```

여러 줄 주석의 경우 범위 지정 구문을 사용하세요:

```txt
{{ // }}
  This entire block is a comment.
  It can span multiple lines.
{{ /// }}
```

## 매크로 이스케이프

매크로 해석 없이 리터럴 중괄호를 표시하려면 백슬래시로 이스케이프하세요:

```txt
\{\{notAMacro\}\}
```

이것은 `{{notAMacro}}`를 일반 텍스트로 출력합니다.

## 변수 축약형

변수 축약형은 일반적인 변수 작업을 위한 간결한 구문을 제공합니다. 로컬 변수에는 `.`을, 전역 변수에는 `$`를 사용하세요.

### 변수 축약형 접두사

| 접두사 | 이름            | 설명                                                     |
| ------ | --------------- | --------------------------------------------------------- |
| `.`    | 로컬 변수  | 로컬 변수 작업을 위한 축약형. 예: `{{.myvar}}`  |
| `$`    | 전역 변수 | 전역 변수 작업을 위한 축약형. 예: `{{$myvar}}` |

이 접두사 연산자는 선택적으로 나타나는 [매크로 플래그](#macro-flags) 뒤, 변수 이름 **바로 앞**에 배치되어야 합니다. 이들은 매크로 플래그로 간주되지 않으며, 오히려 이름으로 지정된 매크로 대신 변수 축약형이 삽입되고 있음을 나타내는 표시입니다. 접두사 연산자는 변수 이름 자체의 일부가 아니라 변수에 액세스하는 방식을 변경하는 수정자입니다.

### 변수 이름

변수 이름은 매크로 식별자와 동일한 규칙을 따릅니다: 문자로 시작하고 문자, 숫자, 밑줄 또는 하이픈이 뒤따릅니다. 마지막 문자는 밑줄이나 하이픈일 수 없습니다.

```txt
{{.my-var}}       // Valid
{{.my_var}}       // Valid
{{.myVar123}}     // Valid
```

변수의 식별자가 표준 규칙과 일치하지 않으면 전체 변수 매크로 구문을 사용해야 하며(예: `{{getvar::my§var----}}`), 또는 변수 값을 이름을 바꾸거나 옮겨야 합니다.

### 값 내 중첩 매크로

변수 값에는 중첩된 매크로가 포함될 수 있습니다:

```txt
{{.greeting = Hello, {{user}}!}}
```

내부에 `Hello, User!`를 저장하는 변수로 해석됩니다. (`{{user}}`가 "User"라는 이름인 경우)

### 공백 처리

연산자 주위의 공백이 허용됩니다:

```txt
{{ .myvar = spaced value }}
{{ .counter ++ }}
```

### 변수 축약형 연산자

다음 연산자를 변수 축약형과 함께 사용할 수 있습니다. 각 연산자는 `{{.varName operator value}}` 또는 `{{$varName operator value}}` 패턴을 따릅니다.

| 연산자 | 이름                      | 예시                | 설명                                            |
| -------- | ------------------------- | ---------------------- | ------------------------------------------------------ |
| *(없음)* | [Get](#get-variable)      | `{{.myvar}}`           | 변수 값을 반환합니다                             |
| `=`      | [Set](#set-variable)      | `{{.myvar = value}}`   | 변수를 값으로 설정하고, 아무것도 반환하지 않습니다         |
| `++`     | [Increment](#increment)   | `{{.counter++}}`       | 1씩 증가시키고, 새 값을 반환합니다                     |
| `--`     | [Decrement](#decrement)   | `{{.counter--}}`       | 1씩 감소시키고, 새 값을 반환합니다                     |
| `+=`     | [Add](#add)               | `{{.score += 10}}`     | 변수에 더함(숫자 또는 문자열 연결), 아무것도 반환하지 않습니다 |
| `-=`     | [Subtract](#subtract)     | `{{.health -= 5}}`     | 변수에서 뺌(숫자만 해당), 아무것도 반환하지 않습니다 |
| `||`   | [Logical Or](#logical-or) | `{{.name || Guest}}` | 변수가 falsy이면 대체 값을 반환합니다                  |
| `??`     | [Nullish Coalescing](#nullish-coalescing) | `{{.name ?? Guest}}` | 변수가 정의되지 않은 경우에만 대체 값을 반환합니다 |
| `||=`  | [Logical Or Assign](#logical-or-assign) | `{{.name ||= Guest}}` | 변수가 falsy이면 값을 설정하고, 새 값을 반환합니다 |
| `??=`    | [Nullish Coalescing Assign](#nullish-coalescing-assign) | `{{.name ??= Guest}}` | 변수가 정의되지 않은 경우에만 값을 설정하고, 새 값을 반환합니다 |
| `==`     | [Equals](#equals)         | `{{.status == active}}`| 값을 비교하고, `"true"` 또는 `"false"`를 반환합니다         |
| `!=`     | [Not Equals](#not-equals) | `{{.status != active}}`| 값을 비교하고, 같지 않으면 `"true"`를 반환합니다         |
| `>`      | [Greater Than](#greater-than) | `{{.score > 50}}` | 변수가 값보다 크면 `"true"`를 반환합니다     |
| `>=`     | [Greater Than or Equal](#greater-than-or-equal) | `{{.level >= 10}}` | 변수가 값보다 크거나 같으면 `"true"`를 반환합니다 |
| `<`      | [Less Than](#less-than)   | `{{.health < 20}}`     | 변수가 값보다 작으면 `"true"`를 반환합니다        |
| `<=`     | [Less Than or Equal](#less-than-or-equal) | `{{.health <= 0}}` | 변수가 값보다 작거나 같으면 `"true"`를 반환합니다 |

#### Get Variable

간단한 접두사로 변수 값을 검색합니다:

```txt
{{.myvar}}       // Get local variable "myvar"
{{$myvar}}       // Get global variable "myvar"
```

`{{getvar::myvar}}` 및 `{{getglobalvar::myvar}}`와 동일합니다.

#### Set Variable

`=` 연산자를 사용하여 변수 값을 설정합니다:

```txt
{{ .myvar = Hello World }}     // Set local variable
{{ $myvar = Some value }}      // Set global variable
```

`{{setvar::myvar::Hello World}}` 및 `{{setglobalvar::myvar::Hello World}}`와 동일합니다. 빈 문자열을 반환합니다.

#### Increment

`++`를 사용하여 숫자 변수를 1씩 증가시킵니다:

```txt
{{.counter++}}    // Increment local variable, returns new value
{{$counter++}}    // Increment global variable, returns new value
```

`{{incvar counter}}` 및 `{{incglobalvar counter}}`와 동일합니다. 증가 후 새 값을 반환합니다.

#### Decrement

`--`를 사용하여 숫자 변수를 1씩 감소시킵니다:

```txt
{{.counter--}}    // Decrement local variable, returns new value
{{$counter--}}    // Decrement global variable, returns new value
```

`{{decvar counter}}` 및 `{{decglobalvar counter}}`와 동일합니다. 감소 후 새 값을 반환합니다.

#### Add

`+=`를 사용하여 변수에 숫자 값을 더합니다:

```txt
{{.score += 10}}     // Add 10 to local variable
{{$total += 5}}      // Add 5 to global variable
```

`{{addvar::score::10}}` 및 `{{addglobalvar::total::5}}`와 동일합니다. 빈 문자열을 반환합니다.

add 연산자는 두 값 모두 숫자가 아닌 경우 기존 문자열 변수에 문자열을 추가하는 것도 지원합니다:

```txt
{{.myvar += {{noop}} | Second block}}   // Resolves to "Content | Second block" when the variable before was "Content".
                                        // Use `{{noop}}` to be able to add whitespaces, that otherwise would be trimmed automatically.
```

#### Subtract

`-=`를 사용하여 변수에서 숫자 값을 뺍니다:

```txt
{{.health -= 10}}    // Subtract 10 from local variable
{{$points -= 5}}     // Subtract 5 from global variable
```

`{{addvar::score::10}}` 및 `{{addglobalvar::total::5}}`와 동일하지만, 음수/반전된 숫자를 사용합니다. 빈 문자열을 반환합니다.
값이 유효한 숫자가 아니면 경고가 기록되고 변수는 변경되지 않습니다.

#### Logical Or

`||`를 사용하여 변수가 falsy(빈 문자열, `0`, `false`)일 때 대체 값을 제공합니다:

```txt
{{.name || Anonymous}}     // Returns "Anonymous" if .name is empty or falsy
{{$setting || default}}    // Returns "default" if $setting is falsy
```

truthy이면 변수 값을 반환하고, 그렇지 않으면 대체 값을 반환합니다. 대체 값은 **필요한 경우에만 평가**됩니다(지연 평가).

#### Nullish Coalescing

`??`를 사용하여 변수가 존재하지 않을 때만 대체 값을 제공합니다:

```txt
{{.name ?? Guest}}         // Returns "Guest" only if .name is not defined
{{$config ?? default}}     // Returns "default" only if $config doesn't exist
```

`||`와 달리, 이것은 변수가 존재하는 한 falsy(빈 문자열, `0`, `false`)이더라도 변수 값을 반환합니다. 대체 값은 **필요한 경우에만 평가**됩니다(지연 평가).

#### Logical Or Assign

`||=`를 사용하여 변수가 현재 falsy인 경우에만 값을 설정합니다:

```txt
{{.name ||= Anonymous}}    // Sets and returns "Anonymous" if .name is falsy
{{$count ||= 0}}           // Sets and returns "0" if $count is falsy
```

변수가 이미 truthy이면 수정 없이 현재 값을 반환합니다. 최종 값(기존 값 또는 새로 설정된 값)을 반환합니다.

#### Nullish Coalescing Assign

`??=`를 사용하여 변수가 존재하지 않는 경우에만 값을 설정합니다:

```txt
{{.name ??= Guest}}        // Sets and returns "Guest" only if .name is undefined
{{$config ??= default}}    // Sets and returns "default" only if $config doesn't exist
```

`||=`와 달리, 변수가 이미 존재하면 falsy 값(빈 문자열, `0`, `false`)을 그대로 유지합니다. 최종 값(기존 값 또는 새로 설정된 값)을 반환합니다.

#### Equals

`==`를 사용하여 변수 값을 다른 값과 비교합니다:

```txt
{{.status == active}}      // Returns "true" if .status equals "active", otherwise "false"
{{$mode == dark}}          // Returns "true" if $mode equals "dark", otherwise "false"
```

문자열 비교를 수행하고 리터럴 문자열 `"true"` 또는 `"false"`를 반환합니다.
존재하지 않는 변수, null 변수 및 빈 변수를 동일하게 취급합니다.

`{{if}}` 조건에서 유용합니다:

```txt
{{if {{.status == active}} }}Active mode{{/if}}
```

#### Not Equals

`!=`를 사용하여 변수 값을 다른 값과 비교하여 불일치를 확인합니다:

```txt
{{.status != inactive}}    // Returns "true" if .status is NOT "inactive", otherwise "false"
{{$mode != light}}         // Returns "true" if $mode is NOT "light", otherwise "false"
```

문자열 비교를 수행하며, 값이 다르면 `"true"`를, 같으면 `"false"`를 반환합니다.
존재하지 않는 변수, null 변수 및 빈 변수를 동일하게 취급합니다.

`{{if}}` 조건에서 유용합니다:

```txt
{{if {{.status != disabled}} }}Feature enabled{{/if}}
```

#### Greater Than

`>`를 사용하여 변수의 숫자 값이 다른 값보다 큰지 확인합니다:

```txt
{{.score > 50}}        // Returns "true" if .score is greater than 50
{{$level > 5}}         // Returns "true" if $level is greater than 5
```

숫자 비교를 수행하고 리터럴 문자열 `"true"` 또는 `"false"`를 반환합니다.

`{{if}}` 조건에서 유용합니다:

```txt
{{if {{.score > 100}} }}High score!{{/if}}
```

#### Greater Than or Equal

`>=`를 사용하여 변수의 숫자 값이 다른 값보다 크거나 같은지 확인합니다:

```txt
{{.level >= 10}}       // Returns "true" if .level is at least 10
{{$points >= 100}}     // Returns "true" if $points is 100 or more
```

숫자 비교를 수행하고 리터럴 문자열 `"true"` 또는 `"false"`를 반환합니다.

`{{if}}` 조건에서 유용합니다:

```txt
{{if {{$level >= 10}} }}Unlocked advanced features{{/if}}
```

#### Less Than

`<`를 사용하여 변수의 숫자 값이 다른 값보다 작은지 확인합니다:

```txt
{{.health < 20}}       // Returns "true" if .health is below 20
{{$timer < 0}}         // Returns "true" if $timer is negative
```

숫자 비교를 수행하고 리터럴 문자열 `"true"` 또는 `"false"`를 반환합니다.

`{{if}}` 조건에서 유용합니다:

```txt
{{if {{.health < 20}} }}Low health warning!{{/if}}
```

#### Less Than or Equal

`<=`를 사용하여 변수의 숫자 값이 다른 값보다 작거나 같은지 확인합니다:

```txt
{{.health <= 0}}       // Returns "true" if .health is 0 or below
{{$attempts <= 3}}     // Returns "true" if $attempts is 3 or fewer
```

숫자 비교를 수행하고 리터럴 문자열 `"true"` 또는 `"false"`를 반환합니다.

`{{if}}` 조건에서 유용합니다:

```txt
{{if {{.health <= 0}} }}Game over{{/if}}
```

## 레거시 구문

이전 버전과의 호환성을 위해 꺾쇠 괄호 마커가 계속 지원됩니다:

| 레거시 | 동등한 매크로 |
|--------|------------------|
| `<USER>` | `{{user}}` |
| `<BOT>` | `{{char}}` |
| `<CHAR>` | `{{char}}` |
| `<GROUP>` | `{{group}}` |
| `<CHARIFNOTGROUP>` | `{{charIfNotGroup}}` |

이들은 처리 중에 자동으로 해당 매크로로 변환됩니다.

> **참고:** 레거시 구문은 권장되지 않습니다. 새로운 콘텐츠에는 대신 동등한 `{{macro}}` 구문을 사용하세요.

## 카테고리별 일반 매크로

!!!tip
사용 가능한 매크로의 전체 목록과 자세한 설명을 보려면 `/? macros`를 사용하세요.
!!!

### 이름 및 참가자

| 매크로 | 설명 |
|-------|-------------|
| `{{user}}` | 현재 사용자/페르소나 이름 |
| `{{char}}` | 현재 캐릭터 이름 |
| `{{group}}` | 그룹 멤버 이름의 쉼표로 구분된 목록(음소거된 멤버 포함) 또는 솔로 채팅의 캐릭터 이름 |
| `{{groupNotMuted}}` | 음소거된 멤버를 제외한 그룹 멤버 이름의 쉼표로 구분된 목록 |
| `{{charIfNotGroup}}` | 캐릭터 이름(그룹에서는 비어 있음) |
| `{{notChar}}` | 현재 발화자를 제외한 모든 참가자의 쉼표로 구분된 목록 |

### 캐릭터 카드 및 페르소나 필드

| 매크로 | 설명 |
|-------|-------------|
| `{{description}}` | 캐릭터 설명 |
| `{{personality}}` | 캐릭터 성격 |
| `{{scenario}}` | 캐릭터 시나리오 |
| `{{persona}}` | 사용자 페르소나 설명 |
| `{{charPrompt}}` | 캐릭터의 Main Prompt 재정의 |
| `{{charInstruction}}` | 캐릭터의 Post-History Instructions 재정의 |
| `{{charDepthPrompt}}` | 캐릭터의 @ Depth Note |
| `{{charCreatorNotes}}` | 캐릭터 카드의 제작자 노트 |
| `{{charVersion}}` | 캐릭터의 버전 번호 |
| `{{mesExamples}}` | Instruct mode용으로 형식이 지정된 캐릭터의 대화 예시 |
| `{{mesExamplesRaw}}` | 캐릭터 카드의 형식이 지정되지 않은 대화 예시 |
| `{{charFirstMessage}}` | 캐릭터의 첫 번째 메시지(인사말). 대체 인사말을 위한 선택적 인덱스를 받습니다. 예: `{{charFirstMessage::1}}` |
| `{{original}}` | 캐릭터 프롬프트 재정의에서 치환할 원본 메시지 콘텐츠 |

### 채팅 기록 및 메시지

| 매크로 | 설명 |
|-------|-------------|
| `{{lastMessage}}` | 채팅의 마지막 메시지 |
| `{{lastMessageId}}` | 채팅에서 마지막 메시지의 인덱스 |
| `{{lastUserMessage}}` | 채팅의 마지막 사용자 메시지 |
| `{{lastCharMessage}}` | 채팅의 마지막 캐릭터/봇 메시지 |
| `{{firstIncludedMessageId}}` | 현재 컨텍스트에 포함된 첫 번째 메시지의 인덱스 |
| `{{firstDisplayedMessageId}}` | 채팅에서 표시된 첫 번째 메시지의 인덱스 |
| `{{lastSwipeId}}` | 마지막 메시지의 마지막 스와이프의 1부터 시작하는 인덱스 |
| `{{currentSwipeId}}` | 현재 스와이프의 1부터 시작하는 인덱스 |
| `{{allChatRange}}` | 전체 채팅의 범위를 제공합니다(예: `0-{{lastMessageId}}`), 메시지 범위를 받는 명령에 유용합니다 |
| `{{summary}}` | "Summarize" 확장 프로그램의 최신 채팅 요약(사용 가능한 경우) |

### 시간 및 날짜

| 매크로 | 설명 |
|-------|-------------|
| `{{time}}` | 현재 로컬 시간 |
| `{{time::UTC±(offset)}}` | UTC 오프셋이 있는 시간 |
| `{{date}}` | 짧은 형식의 현재 로컬 날짜 |
| `{{weekday}}` | 현재 요일 |
| `{{isotime}}` | HH:mm 형식의 현재 시간 |
| `{{isodate}}` | YYYY-MM-DD 형식의 현재 날짜 |
| `{{datetimeformat::format}}` | 사용자 정의 형식의 날짜/시간(예: `YYYY-MM-DD HH:mm:ss`) |
| `{{idleDuration}}` | 마지막 사용자 메시지 이후 경과 시간(사람이 읽을 수 있는 형식) |
| `{{timeDiff::left::right}}` | 두 시간 사이의 차이(사람이 읽을 수 있는 형식) |

### 변수

| 매크로 | 설명 |
|-------|-------------|
| `{{getvar::name}}` | 로컬 변수 값 가져오기 |
| `{{setvar::name::value}}` | 로컬 변수 설정 |
| `{{addvar::name::value}}` | 로컬 변수에 값 추가(숫자 또는 문자열 추가) |
| `{{incvar::name}}` | 로컬 변수를 1씩 증가시키고 새 값 반환 |
| `{{decvar::name}}` | 로컬 변수를 1씩 감소시키고 새 값 반환 |
| `{{hasvar::name}}` | 로컬 변수가 존재하는지 확인(`"true"` 또는 `"false"` 반환) |
| `{{deletevar::name}}` | 로컬 변수 삭제 |
| `{{getglobalvar::name}}` | 전역 변수 값 가져오기 |
| `{{setglobalvar::name::value}}` | 전역 변수 설정 |
| `{{addglobalvar::name::value}}` | 전역 변수에 값 추가(숫자 또는 문자열 추가) |
| `{{incglobalvar::name}}` | 전역 변수를 1씩 증가시키고 새 값 반환 |
| `{{decglobalvar::name}}` | 전역 변수를 1씩 감소시키고 새 값 반환 |
| `{{hasglobalvar::name}}` | 전역 변수가 존재하는지 확인(`"true"` 또는 `"false"` 반환) |
| `{{deleteglobalvar::name}}` | 전역 변수 삭제 |

### 랜덤화

| 매크로 | 설명 |
|-------|-------------|
| `{{random::a::b::c}}` | 무작위 선택(매번 다시 롤링됨) |
| `{{pick::a::b::c}}` | 안정적인 무작위 선택(채팅 및 위치별로 일관됨). `/reroll-pick` 명령으로 다시 롤링할 수 있습니다 |
| `{{roll::1d20}}` | droll 구문을 사용한 주사위 굴림 |

### 런타임 상태

| 매크로 | 설명 |
|-------|-------------|
| `{{maxPrompt}}` | 최대 프롬프트 컨텍스트 크기(프롬프트 토큰 = 컨텍스트 토큰 - 응답 토큰) |
| `{{maxContextTokens}}` | 현재 생성 설정에 대한 최대 컨텍스트 토큰 수 |
| `{{maxResponseTokens}}` | 현재 생성 설정에 대한 최대 응답 토큰 수 |
| `{{model}}` | 현재 선택된 API의 모델 이름 |
| `{{isMobile}}` | 모바일 환경에서 실행 중이면 "true", 그렇지 않으면 "false" |
| `{{lastGenerationType}}` | 마지막으로 대기열에 추가된 생성 요청의 유형(예: "normal", "impersonate", "regenerate", "quiet", "swipe", "continue") |
| `{{hasExtension::name}}` | 확장 프로그램이 활성화되어 있는지 확인(`"true"` 또는 `"false"` 반환). 확장 프로그램 이름으로 일치하며 대소문자를 구분하지 않습니다 |

### 프롬프트 템플릿

| 매크로 | 설명 |
|-------|-------------|
| `{{systemPrompt}}` | 활성 시스템 프롬프트 텍스트(선택적으로 캐릭터에 의해 재정의됨) |
| `{{defaultSystemPrompt}}` | 기본 시스템 프롬프트 |
| `{{authorsNote}}` | Author's Note의 내용 |
| `{{charAuthorsNote}}` | Character Author's Note의 내용 |
| `{{defaultAuthorsNote}}` | Default Author's Note의 내용 |
| `{{instructStoryStringPrefix}}` | Instruct story string 접두사 |
| `{{instructStoryStringSuffix}}` | Instruct story string 접미사 |
| `{{instructUserPrefix}}` | Instruct 입력/사용자 접두사 시퀀스 |
| `{{instructUserSuffix}}` | Instruct 입력/사용자 접미사 시퀀스 |
| `{{instructAssistantPrefix}}` | Instruct 출력/어시스턴트 접두사 시퀀스 |
| `{{instructAssistantSuffix}}` | Instruct 출력/어시스턴트 접미사 시퀀스 |
| `{{instructSeparator}}` | Instruct 구분자 시퀀스 |
| `{{instructSystemPrefix}}` | Instruct 시스템 접두사 시퀀스 |
| `{{instructSystemSuffix}}` | Instruct 시스템 접미사 시퀀스 |
| `{{instructFirstAssistantPrefix}}` | Instruct 첫 번째 어시스턴트/출력 접두사 시퀀스 |
| `{{instructLastAssistantPrefix}}` | Instruct 마지막 어시스턴트/출력 접두사 시퀀스 |
| `{{instructFirstUserPrefix}}` | Instruct 첫 번째 사용자/입력 접두사 시퀀스 |
| `{{instructLastUserPrefix}}` | Instruct 마지막 사용자/입력 접두사 시퀀스 |
| `{{instructStop}}` | Instruct 중지 시퀀스 |
| `{{instructUserFiller}}` | Instruct 사용자 정렬 필러 |
| `{{instructSystemInstructionPrefix}}` | Instruct 시스템 지침 접두사 시퀀스 |
| `{{chatSeparator}}` | 텍스트 완성 프롬프트에서 예시 채팅 블록 사이의 구분자 |
| `{{chatStart}}` | 텍스트 완성 프롬프트의 채팅 시작 마커 |
| `{{reasoningPrefix}}` | reasoning 블록 앞에 사용되는 접두사 문자열 |
| `{{reasoningSuffix}}` | reasoning 블록 뒤에 사용되는 접미사 문자열 |
| `{{reasoningSeparator}}` | 콘텐츠와 응답 사이의 구분자 |
| `{{charPrefix}}` | 캐릭터의 긍정적인 Image Generation 프롬프트 접두사 |
| `{{charNegativePrefix}}` | 캐릭터의 부정적인 Image Generation 프롬프트 접두사 |

### 유틸리티

| 매크로 | 설명 |
|-------|-------------|
| `{{newline}}` | 줄 바꿈 문자 삽입 |
| `{{newline::count}}` | 여러 줄 바꿈 삽입 |
| `{{space}}` | 공백 문자 삽입 |
| `{{space::count}}` | 여러 공백 삽입 |
| `{{noop}}` | 아무것도 하지 않으며 빈 문자열을 생성합니다 |
| `{{trim}}` | 주변의 줄 바꿈 제거 |
| `{{reverse::text}}` | 문자열 반전 |
| `{{input}}` | 현재 채팅 입력 필드의 내용 |
| `{{banned::word}}` | Text Completion 백엔드에서 단어 금지 |
| `{{outlet::key}}` | 주어진 outlet 키에 대한 world info outlet 프롬프트 반환 |
