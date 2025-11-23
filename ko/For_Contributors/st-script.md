---
order: 0
icon: file-symlink-file
route: /usage/st-script/
templating: false
label: STscript 언어 레퍼런스
---

# STscript 언어 레퍼런스

## STscript란 무엇인가요?

심각한 코딩 없이 SillyTavern의 기능을 확장하는 데 사용할 수 있는 간단하면서도 강력한 스크립팅 언어로, 다음을 수행할 수 있습니다:

- 미니 게임 또는 스피드런 챌린지 생성
- AI 기반 채팅 인사이트 구축
- 창의성을 발휘하고 다른 사람들과 공유

STscript는 슬래시 명령 엔진을 사용하여 구축되었으며, 명령 배치, 데이터 파이핑, 매크로 및 변수를 활용합니다.
이러한 개념은 다음 문서에서 설명됩니다.

### 보안 주의사항

큰 힘에는 큰 책임이 따릅니다. 항상 스크립트를 실행하기 전에 검사하고 주의하세요.

## Hello, World!

첫 번째 스크립트를 실행하려면 SillyTavern 채팅을 열고 채팅 입력창에 다음을 입력하세요:

```stscript
/pass Hello, World! | /echo
```

| ![Hello World](/static/scripts/hello-world.png) |
|-------------------------------------------------|

화면 상단의 토스트에 메시지가 표시되어야 합니다. 이제 하나씩 분석해 봅시다.

스크립트는 명령의 배치이며, 각 명령은 슬래시로 시작하고 명명된 인수와 명명되지 않은 인수를 사용하거나 사용하지 않으며 명령 구분 문자인 `|`로 종료됩니다.

명령은 순차적으로 하나씩 실행되며 서로 간에 데이터를 전송합니다.

1. `/pass` 명령은 "Hello, World!"의 상수 값을 명명되지 않은 인수로 받아들이고 파이프에 씁니다.
2. `/echo` 명령은 이전 명령의 파이프를 통해 값을 받고 토스트 알림으로 표시합니다.

!!!tip
**힌트:** 사용 가능한 모든 명령 목록을 보려면 채팅에 `/help slash`를 입력하세요.
!!!

상수 명명되지 않은 인수와 파이프는 서로 바꿔 사용할 수 있으므로 이 스크립트를 다음과 같이 간단히 다시 작성할 수 있습니다:

```stscript
/echo Hello, World!
```

## 사용자 입력

이제 스크립트에 약간의 상호작용을 추가해 봅시다. 사용자로부터 입력 값을 받고 알림에 표시하겠습니다.

```stscript
/input Enter your name |
/echo Hello, my name is {{pipe}}
```

1. `/input` 명령은 명명되지 않은 인수에 지정된 프롬프트와 함께 입력 상자를 표시한 다음 출력을 파이프에 씁니다.
2. `/echo`는 이미 출력 템플릿을 설정하는 명명되지 않은 인수가 있으므로 `{{pipe}}` 매크로를 사용하여 파이프 값이 렌더링될 위치를 지정합니다.

| ![Slim Shady Input](/static/scripts/slim-input.png) | ![Slim Shady Output](/static/scripts/slim-output.png) |
|-----------------------------------------------------|-------------------------------------------------------|

### 기타 입력/출력 명령

- `/popup (text)` — 차단 팝업을 표시합니다. 간단한 HTML 형식을 지원합니다. 예: `/popup <font color=red>I'm red!</font>`.
- `/setinput (text)` — 사용자 입력창의 내용을 제공된 텍스트로 대체합니다.
- `/speak voice="name" (text)` — 선택한 TTS 엔진과 음성 맵의 캐릭터 이름을 사용하여 텍스트를 낭독합니다. 예: `/speak name="Donald Duck" Quack!`.
- `/buttons labels=["a","b"] (text)` — 지정된 텍스트와 버튼 레이블이 있는 차단 팝업을 표시합니다. `labels`는 JSON 직렬화된 문자열 배열 또는 그러한 배열을 포함하는 변수 이름이어야 합니다. 클릭한 버튼 레이블을 파이프로 반환하거나 취소된 경우 빈 문자열을 반환합니다. 텍스트는 간단한 HTML 형식을 지원합니다.

#### `/popup` 및 `/input`의 인수

`/popup` 및 `/input`은 다음과 같은 추가 명명된 인수를 지원합니다:
- `large=on/off` - 팝업의 세로 크기를 늘립니다. 기본값: `off`.
- `wide=on/off` - 팝업의 가로 크기를 늘립니다. 기본값: `off`.
- `okButton=string` - "Ok" 버튼의 텍스트를 사용자 정의할 수 있는 기능을 추가합니다. 기본값: `Ok`.
- `rows=number` - (`/input`만 해당) 입력 컨트롤의 크기를 늘립니다. 기본값: 1.

예제:
```stscript
/popup large=on wide=on okButton="Accept" Please accept our terms and conditions....
```

#### `/echo`의 인수

`/echo`는 표시된 메시지의 스타일을 설정하는 추가 `severity` 인수에 대해 다음 값을 지원합니다.
  - `warning`
  - `error`
  - `info` (기본값)
  - `success`

예제:

```stscript
/echo severity=error Something really bad happened.
```

## 변수

변수는 명령 또는 매크로를 사용하여 스크립트에서 데이터를 저장하고 조작하는 데 사용됩니다. 변수는 다음 타입 중 하나일 수 있습니다:

- 로컬 변수 — 현재 채팅의 메타데이터에 저장되며 고유합니다.
- 글로벌 변수 — settings.json에 저장되며 앱 전체에서 존재합니다.

1. `/getvar name` 또는 `{{getvar::name}}` — 로컬 변수의 값을 가져옵니다.
2. `/setvar key=name value` 또는 `{{setvar::name::value}}` — 로컬 변수의 값을 설정합니다.
3. `/addvar key=name increment` 또는 `{{addvar::name::increment}}` — 로컬 변수의 값에 `increment`를 추가합니다.
4. `/incvar name` 또는 `{{incvar::name}}` — 로컬 변수의 값을 1 증가시킵니다.
5. `/decvar name` 또는 `{{decvar::name}}` — 로컬 변수의 값을 1 감소시킵니다.
6. `/getglobalvar name` 또는 `{{getglobalvar::name}}` — 글로벌 변수의 값을 가져옵니다.
7. `/setglobalvar key=name` 또는 `{{setglobalvar::name::value}}` — 글로벌 변수의 값을 설정합니다.
8. `/addglobalvar key=name` 또는 `{{addglobalvar::name:increment}}` — 글로벌 변수의 값에 `increment`를 추가합니다.
9. `/incglobalvar name` 또는 `{{incglobalvar::name}}` — 글로벌 변수의 값을 1 증가시킵니다.
10. `/decglobalvar name` 또는 `{{decglobalvar::name}}` — 글로벌 변수의 값을 1 감소시킵니다.
11. `/flushvar name` — 로컬 변수의 값을 삭제합니다.
12. `/flushglobalvar name` — 글로벌 변수의 값을 삭제합니다.

- 이전에 정의되지 않은 변수의 기본값은 빈 문자열이거나 `/addvar`, `/incvar`, `/decvar` 명령에서 처음 사용되는 경우 0입니다.
- `/addvar` 명령의 증분은 증분과 변수 값을 모두 숫자로 변환할 수 있는 경우 값의 더하기 또는 빼기를 수행하고, 그렇지 않으면 문자열 연결을 수행합니다.
- 명령 인수가 변수 이름을 허용하고 같은 이름의 로컬 변수와 글로벌 변수가 모두 존재하는 경우 *로컬 변수*가 우선합니다.
- 변수 조작을 위한 모든 *슬래시 명령*은 결과 값을 다음 명령이 사용할 파이프에 씁니다.
- *매크로*의 경우 "get", "inc", "dec" 타입 매크로만 값을 반환하고, "add" 및 "set"은 대신 빈 문자열로 대체됩니다.

이제 다음 예제를 고려해 봅시다:

```stscript
/input What do you want to generate? |
/setvar key=SDinput |
/echo Requesting an image of {{getvar::SDinput}} |
/getvar SDinput |
/imagine
```

1. 사용자 입력의 값이 `SDinput`이라는 로컬 변수에 저장됩니다.
2. `getvar` 매크로는 `/echo` 명령에 값을 표시하는 데 사용됩니다.
3. `getvar` 명령은 변수의 값을 검색하고 파이프를 통해 전달하는 데 사용됩니다.
4. 값은 입력 프롬프트로 사용되도록 `/imagine` 명령(이미지 생성 플러그인에서 제공)에 전달됩니다.

변수는 스크립트 실행 사이에 저장되고 플러시되지 않으므로 다른 스크립트에서 그리고 매크로를 통해 변수를 참조할 수 있으며 예제 스크립트 실행 중과 동일한 값으로 해석됩니다. 값이 삭제될 것을 보장하려면 스크립트에 `/flushvar` 명령을 추가하세요.

### 배열 및 객체

변수 값은 JSON 직렬화된 배열 또는 키-값 쌍(객체)을 포함할 수 있습니다.

예제:
- 배열: `["apple","banana","orange"]`
- 객체: `{"fruits":["apple","banana","orange"]}`

이러한 변수와 함께 작동하도록 명령에 다음 수정을 적용할 수 있습니다:

- `/len` 명령은 배열의 항목 수를 가져옵니다.
- `index=number/string` 명명된 인수는 `/getvar` 또는 `/setvar` 및 글로벌 대응물에 추가하여 배열의 0 기반 인덱스 또는 객체의 문자열 키로 하위 값을 가져오거나 설정할 수 있습니다.
  - 존재하지 않는 변수에 숫자 인덱스를 사용하면 변수가 빈 배열 `[]`로 생성됩니다.
  - 존재하지 않는 변수에 문자열 인덱스를 사용하면 변수가 빈 객체 `{}`로 생성됩니다.
- `/addvar` 및 `/addglobalvar` 명령은 배열 타입 변수에 새 값을 푸시하는 것을 지원합니다.

## 흐름 제어 - 조건문

`/if` 명령을 사용하여 정의된 규칙에 따라 실행을 분기하는 조건식을 만들 수 있습니다.

```stscript
/if left=valueA right=valueB rule=comparison else={: /echo (command on false) :} {: /echo (command on true) :}
```

다음 사항에 유의하세요.

```stscript
/if left=valueA right=valueB rule=comparison else="(command on false)" "(command on true)"
```

구문도 지원되지만 `{: closures :}`는 더 깔끔한 스크립트를 작성하는 데 도움이 됩니다.

다음 예제를 검토해 봅시다:

```stscript
/input What's your favorite drink? |
/if left={{pipe}} right="black tea" rule=eq else={: /echo You shall not pass | /abort :} {: /echo Welcome to the club, {{user}} :}
```

이 스크립트는 사용자 입력을 필수 값과 비교하고 입력 값에 따라 다른 메시지를 표시합니다.

### `/if`의 인수

1. `left`는 첫 번째 피연산자입니다. A라고 부르겠습니다.
2. `right`는 두 번째 피연산자입니다. B라고 부르겠습니다.
3. `rule`은 피연산자에 적용할 연산입니다.
4. `else`는 부울 비교 결과가 false인 경우 실행할 하위 명령의 선택적 문자열입니다.
5. 명명되지 않은 인수는 부울 비교 결과가 true인 경우 실행할 하위 명령입니다.

피연산자 값은 다음 순서로 평가됩니다:

1. 숫자 리터럴
2. 로컬 변수 이름
3. 글로벌 변수 이름
4. 문자열 리터럴

명명된 인수의 문자열 값은 여러 단어 문자열을 허용하기 위해 따옴표로 이스케이프할 수 있습니다. 그러면 따옴표가 삭제됩니다.

### 부울 연산

부울 비교를 위해 지원되는 규칙은 다음과 같습니다. 피연산자에 적용된 연산은 true 또는 false 값을 생성합니다.

1. `eq` (같음) => A = B
2. `neq` (같지 않음) => A != B
3. `lt` (미만) => A < B
4. `gt` (초과) => A > B
5. `lte` (이하) => A <= B
6. `gte` (이상) => A >= B
7. `not` (단항 부정) => !A
8. `in` (하위 문자열 포함) => A가 B를 포함, 대소문자 구분 안 함
9. `nin` (하위 문자열 포함 안 함) => A가 B를 포함하지 않음, 대소문자 구분 안 함

### 하위 명령

하위 명령은 실행할 슬래시 명령 목록을 포함하는 문자열입니다.

1. 하위 명령에서 명령 배치를 사용하려면 명령 구분 문자를 이스케이프해야 합니다(아래 참조).
2. 매크로 값은 조건문이 입력될 때 실행되고 하위 명령이 실행될 때가 아니므로 매크로를 추가로 이스케이프하여 하위 명령 실행 시간으로 평가를 지연시킬 수 있습니다.
3. 하위 명령 실행의 결과는 `/if` 이후의 명령으로 파이프됩니다.
4. `/abort` 명령은 만났을 때 스크립트 실행을 중단합니다.

`/if` 명령은 삼항 연산자로 사용할 수 있습니다.
다음 예제는 변수 `a`가 5와 같으면 다음 명령에 "true" 문자열을 전달하고 그렇지 않으면 "false" 문자열을 전달합니다.

```stscript
/if left=a right=5 rule=eq else={: /pass false:} {: /pass true :} |
/echo
```

## 이스케이프 시퀀스

### 매크로

매크로의 이스케이프는 이전과 동일하게 작동합니다. 그러나 클로저를 사용하면 이전보다 매크로를 이스케이프해야 하는 경우가 훨씬 적습니다. 여는 중괄호 두 개 또는 여는 쌍과 닫는 쌍을 모두 이스케이프합니다.

```stscript
/echo \{\{char}} |
/echo \{\{char\}\}
```

### 파이프

파이프는 클로저에서 (명령 구분자로 사용될 때) 이스케이프할 필요가 없습니다. 명령 구분자 대신 리터럴 파이프 문자를 사용하려는 모든 곳에서 이스케이프해야 합니다.

```stscript
/echo title="a\|b" c\|d |
/echo title=a\|b c\|d |
```

파서 플래그 `STRICT_ESCAPING`을 사용하면 따옴표로 묶인 값에서 파이프를 이스케이프할 필요가 없습니다.

```stscript
/parser-flag STRICT_ESCAPING |
/echo title="a|b" c\|d |
/echo title=a\|b c\|d |
```

### 따옴표

따옴표로 묶인 값 내에서 리터럴 따옴표 문자를 사용하려면 문자를 이스케이프해야 합니다.

```stscript
/echo title="a \"b\" c" d "e" f
```

### 공백

명명된 인수의 값에 공백을 사용하려면 값을 따옴표로 묶거나 공백 문자를 이스케이프해야 합니다.

```stscript
/echo title="a b" c d |
/echo title=a\ b c d
```

### 클로저 구분자

클로저의 시작 또는 끝을 표시하는 데 사용되는 문자 조합을 사용하려면 단일 백슬래시로 시퀀스를 이스케이프해야 합니다.

```stscript
/echo \{: |
/echo \:}
```

## 파이프 브레이커

```stscript
||
```

이전 명령의 출력이 다음 명령의 명명되지 않은 인수로 자동 주입되는 것을 방지하려면 두 명령 사이에 이중 파이프를 넣습니다.

```stscript
/echo we don't want to pass this on ||
/world
```

## 클로저

```stscript
{: ... :}
```

클로저(블록 문, 람다, 익명 함수, 원하는 대로 부르세요)는 코드의 해당 부분이 실행될 때만 평가되는 `{:`와 `:}` 사이에 래핑된 일련의 명령입니다.

### 하위 명령

클로저는 하위 명령을 사용하는 것을 훨씬 쉽게 만들고 파이프와 매크로를 이스케이프할 필요가 없어집니다.

```stscript
// 클로저 없는 if |
/if left=1 rule=eq right=1
    else="
        /echo not equal \|
        /return 0
    "
    /echo equal \|
    /return \{\{pipe}}
```

```stscript
// 클로저가 있는 if |
/if left=1 rule=eq right=1
    else={:
        /echo not equal |
        /return 0
    :}
    {:
        /echo equal |
        /return {{pipe}}
    :}
```

### 스코프

클로저는 자체 스코프를 가지며 스코프 변수를 지원합니다. 스코프 변수는 `/let`으로 선언되고 `/var`로 값을 설정하고 검색합니다. 스코프 변수를 가져오는 또 다른 방법은 `{{var::}}` 매크로입니다.

```stscript
/let x |
/let y 2 |
/var x 1 |
/var y |
/echo x is {{var::x}} and y is {{pipe}}.
```

클로저 내에서는 동일한 클로저 또는 조상 중 하나에서 선언된 모든 변수에 액세스할 수 있습니다. 클로저의 자손에서 선언된 변수에는 액세스할 수 없습니다.
클로저의 조상 중 하나에서 선언된 변수와 동일한 이름으로 변수가 선언된 경우 이 클로저와 그 자손에서 조상 변수에 액세스할 수 없습니다.

```stscript
/let x this is root x |
/let y this is root y |
/return {:
    /echo called from level-1: x is "{{var::x}}" and y is "{{var::y}}" |
    /delay 500 |
    /let x this is level-1 x |
    /echo called from level-1: x is "{{var::x}}" and y is "{{var::y}}" |
    /delay 500 |
    /return {:
        /echo called from level-2: x is "{{var::x}}" and y is "{{var::y}}" |
        /let x this is level-2 x |
        /echo called from level-2: x is "{{var::x}}" and y is "{{var::y}}" |
        /delay 500
    :}()
:}() |
/echo called from root: x is "{{var::x}}" and y is "{{var::y}}"
```

### 명명된 클로저

```stscript
/let x {: ... :} | /:x
```

클로저는 나중에 호출하거나 하위 명령으로 사용하기 위해 변수(스코프 변수만)에 할당할 수 있습니다.

```stscript
/let myClosure {:
    /echo this is my closure
:} |
/:myClosure
```

```stscript
/let myClosure {:
    /echo this is my closure |
    /delay 500
:} |
/times 3 {{var::myClosure}}
```

`/:`는 Quick Reply를 실행하는 데도 사용할 수 있습니다. `/run`의 약칭일 뿐이기 때문입니다.

```stscript
/:QrSetName.QrButtonLabel |
/run QrSetName.QrButtonLabel
```

### 클로저 인수

명명된 클로저는 슬래시 명령과 마찬가지로 명명된 인수를 사용할 수 있습니다. 인수는 기본값을 가질 수 있습니다.

```stscript
/let myClosure {: a=1 b=
    /echo a is {{var::a}} and b is {{var::b}}
:} |
/:myClosure b=10
```

### 클로저와 파이프된 인수

부모 클로저의 파이프된 값은 자식 클로저의 첫 번째 명령에 자동으로 주입되지 않습니다.
`{{pipe}}`로 부모의 파이프된 값을 명시적으로 참조할 수 있지만, 클로저 내부의 첫 번째 명령의 명명되지 않은 인수를 비워 두면 값이 자동으로 주입되지 *않습니다*.

```stscript
/* 이전에는 루프 밖의 /echo에서 "foo" 값이
   루프 내부의 /model 명령에 주입되어
   모델을 "foo"로 변경하려고 시도했습니다.
   이제는 단순히 변경하려고 시도하지 않고
   현재 모델을 에코합니다.
*|
/echo foo |
/times 2 {:
	/model |
	/echo |
:} |
```
```stscript
/* {{pipe}} 매크로를 명시적으로 사용하여
   이전 동작을 여전히 재현할 수 있습니다.
*|
/echo foo |
/times 2 {:
	/model {{pipe}} |
	/echo |
:} |
```

### 즉시 실행되는 클로저

```stscript
{: ... :}()
```

클로저는 즉시 실행될 수 있으며, 이는 반환 값으로 대체됨을 의미합니다. 이는 클로저에 대한 명시적인 지원이 없는 곳에서 유용하며 많은 중간 변수가 필요한 일부 명령을 단축하는 데 유용합니다.

```stscript
// 클로저 없이 두 문자열의 간단한 길이 비교 |
/len foo |
/var lenOfFoo {{pipe}} |
/len bar |
/var lenOfBar {{pipe}} |
/if left={{var::lenOfFoo}} rule=eq right={{var:lenOfBar}} /echo yay!
```

```stscript
// 즉시 실행되는 클로저와 동일한 비교 |
/if left={:/len foo:}() rule=eq right={:/len bar:}() /echo yay!
```

스코프 변수 내에 저장된 명명된 클로저를 실행하는 것 외에도 `/run` 명령을 사용하여 클로저를 즉시 실행할 수 있습니다.

```stscript
/run {:
	/add 1 2 3 4 |
:} |
/echo |
```

## 주석

```stscript
// ... | /# ...
```

주석은 스크립트 코드의 사람이 읽을 수 있는 설명 또는 주석입니다. 주석은 파이프를 끊지 않습니다.

```stscript
// 이것은 주석입니다 |
/echo foo |
/# 이것도 주석입니다
```

### 블록 주석

블록 주석을 사용하여 한 번에 여러 명령을 빠르게 주석 처리할 수 있습니다. 파이프에서 종료되지 않습니다.

```stscript
/echo foo |
/*
/echo bar |
/echo foobar |
*|
/echo foo again |
```


## 흐름 제어

### 루프: `/while` 및 `/times`

특정 조건이 충족될 때까지 루프에서 일부 명령을 실행해야 하는 경우 `/while` 명령을 사용합니다.

```stscript
/while left=valueA right=valueB rule=operation guard=on "commands"
```

루프의 각 단계에서 변수 A의 값을 변수 B의 값과 비교하고 조건이 true를 산출하면 따옴표로 묶인 유효한 슬래시 명령을 실행하고 그렇지 않으면 루프를 종료합니다. 이 명령은 출력 파이프에 아무것도 쓰지 않습니다.

#### `/while`의 인수

**사용 가능한 부울 비교, 변수 처리, 리터럴 값 및 하위 명령 세트는 `/if` 명령과 동일합니다.**

선택적 `guard` 명명된 인수(기본적으로 `on`)는 무한 루프로부터 보호하는 데 사용되며 반복 횟수를 100으로 제한합니다.
무한 루프를 허용하려면 `guard=off`로 설정하세요.

이 예제는 `i`의 값이 10에 도달할 때까지 1을 추가한 다음 결과 값(이 경우 10)을 출력합니다.

```stscript
/setvar key=i 0 |
/while left=i right=10 rule=lt "/addvar key=i 1" |
/echo {{getvar::i}} |
/flushvar i
```

#### `/times`의 인수

하위 명령을 지정된 횟수만큼 실행합니다.

`/times (repeats) "(command)"` – 따옴표로 묶인 유효한 슬래시 명령이 여러 번 반복됩니다. 예: `/setvar key=i 1 | /times 5 "/addvar key=i 1"`은 "i"의 값에 1을 5번 추가합니다.
- {{timesIndex}}는 반복 번호(0 기반)로 대체됩니다. 예: `/times 4 {:/echo {{timesIndex}}:}`는 0부터 4까지의 숫자를 에코합니다.
- 루프는 기본적으로 100번의 반복으로 제한됩니다. 비활성화하려면 `guard=off`를 전달하세요.

### 루프 및 클로저에서 빠져나오기

```stscript
/break |
```

`/break` 명령을 사용하여 루프(`/while` 또는 `/times`) 또는 클로저를 조기에 빠져나올 수 있습니다. `/break`의 명명되지 않은 인수를 사용하여 현재 파이프와 다른 값을 전달할 수 있습니다.
`/break`는 현재 다음 명령에서 구현됩니다:
- `/while` - 루프를 조기에 종료합니다
- `/times` - 루프를 조기에 종료합니다
- `/run` (클로저 또는 변수를 통한 클로저와 함께) - 클로저를 조기에 종료합니다
- `/:` (클로저와 함께) - 클로저를 조기에 종료합니다

```stscript
/times 10 {:
	/echo {{timesIndex}}
	/delay 500 |
	/if left={{timesIndex}} rule=gt right=3 {:
		/break
	:} |
:} |
```

```stscript
/let x {: iterations=2
	/if left={{var::iterations}} rule=gt right=10 {:
		/break too many iterations! |
	:} |
	/times {{var::iterations}} {:
		/delay 500 |
		/echo {{timesIndex}} |
	:} |
:} |
/:x iterations=30 |
/echo the final result is: {{pipe}}
```

```stscript
/run {:
	/break 1 |
	/pass 2 |
:} |
/echo pipe will be one: {{pipe}} |
```

```stscript
/let x {:
	/break 1 |
	/pass 2 |
:} |
/:x |
/echo pipe will be one: {{pipe}} |
```

## 수학 연산

- 다음 연산은 모두 일련의 숫자 또는 변수 이름을 허용하고 결과를 파이프에 출력합니다.
- 잘못된 연산(예: 0으로 나누기)과 NaN 값 또는 무한대를 생성하는 연산은 0을 반환합니다.
- 곱셈, 덧셈, 최소값 및 최대값은 공백으로 구분된 무제한 수의 인수를 허용합니다.
- 뺄셈, 나눗셈, 거듭제곱 및 모듈로는 공백으로 구분된 두 개의 인수를 허용합니다.
- 사인, 코사인, 자연 로그, 제곱근, 절대값 및 반올림은 하나의 인수를 허용합니다.

**연산 목록:**

1. `/add (a b c d)` – 값 집합의 덧셈을 수행합니다. 예: `/add 10 i 30 j`
2. `/mul (a b c d)` – 값 집합의 곱셈을 수행합니다. 예: `/mul 10 i 30 j`
3. `/max (a b c d)` – 값 집합에서 최대값을 반환합니다. 예: `/max 1 0 4 k`
4. `/min (a b c d)` – 값 집합에서 최소값을 반환합니다. 예: `/min 5 4 i 2`
5. `/sub (a b)` – 두 값의 뺄셈을 수행합니다. 예: `/sub i 5`
6. `/div (a b)` – 두 값의 나눗셈을 수행합니다. 예: `/div 10 i`
7. `/mod (a b)` – 두 값의 모듈로 연산을 수행합니다. 예: `/mod i 2`
8. `/pow (a b)` – 두 값의 거듭제곱 연산을 수행합니다. 예: `/pow i 2`
9. `/sin (a)` – 값의 사인 연산을 수행합니다. 예: `/sin i`
10. `/cos (a)` – 값의 코사인 연산을 수행합니다. 예: `/cos i`
11. `/log (a)` – 값의 자연 로그 연산을 수행합니다. 예: `/log i`
12. `/abs (a)` – 값의 절대값 연산을 수행합니다. 예: `/abs -10`
13. `/sqrt (a)`– 값의 제곱근 연산을 수행합니다. 예: `/sqrt 9`
14. `/round (a)` – 값의 가장 가까운 정수로 반올림 연산을 수행합니다. 예: `/round 3.14`
15. `/rand (round=round|ceil|floor from=number=0 to=number=1)` – from과 to 사이의 난수를 반환합니다. 예: `/rand` 또는 `/rand 10` 또는 `/rand from=5 to=10`. 범위는 포함됩니다. 반환된 값에는 소수 부분이 포함됩니다. 정수 값을 얻으려면 `round` 명명된 인수를 사용하세요. 예: `/rand round=ceil`은 올림, `round=floor`는 내림, `round=round`는 가장 가까운 값으로 반올림합니다.

### 예제 1: 반지름이 50인 원의 면적 구하기.

```stscript
/setglobalvar key=PI 3.1415 |
/setvar key=r 50 |
/mul r r PI |
/round |
/echo Circle area: {{pipe}}
```

### 예제 2: 5의 계승 계산.

```stscript
/setvar key=input 5 |
/setvar key=i 1 |
/setvar key=product 1 |
/while left=i right=input rule=lte "/mul product i \| /setvar key=product \| /addvar key=i 1" |
/getvar product |
/echo Factorial of {{getvar::input}}: {{pipe}} |
/flushvar input |
/flushvar i |
/flushvar product
```

## LLM 사용

스크립트는 다음 명령을 사용하여 현재 연결된 LLM API에 요청할 수 있습니다:

- `/gen (prompt)` — 선택한 캐릭터에 대해 제공된 프롬프트를 사용하고 채팅 메시지를 포함하여 텍스트를 생성합니다.
- `/genraw (prompt)` — 현재 캐릭터와 채팅을 무시하고 제공된 프롬프트만 사용하여 텍스트를 생성합니다.
- `/trigger` — 일반 생성을 트리거합니다("전송" 버튼 클릭과 동일). 그룹 채팅인 경우 선택적으로 1 기반 그룹 멤버 인덱스 또는 캐릭터 이름을 제공하여 응답하도록 할 수 있습니다. 그렇지 않으면 그룹 설정에 따라 그룹 라운드를 트리거합니다.

### `/gen` 및 `/genraw`의 인수

```stscript
/genraw lock=on/off stop=[] instruct=on/off (prompt)
```

- `lock` — `on` 또는 `off`일 수 있습니다. 생성이 진행 중일 때 사용자 입력을 차단해야 하는지 여부를 지정합니다. 기본값: `off`.
- `stop` — JSON 직렬화된 문자열 배열. 이 생성에만 (API가 지원하는 경우) 사용자 정의 중지 문자열을 추가합니다. 기본값: 없음.
- `instruct` (`/genraw`만 해당) — `on` 또는 `off`일 수 있습니다. 입력 프롬프트에 대해 지시 형식을 사용할 수 있습니다(지시 모드가 활성화되고 API가 지원하는 경우). 순수 프롬프트를 강제하려면 `off`로 설정하세요. 기본값: `on`.
- `as` (Text Completion API용) — `system` (기본값) 또는 `char`일 수 있습니다. 마지막 프롬프트 라인의 형식을 정의합니다. `char`는 캐릭터 이름을 사용하고 `system`은 이름 없음 또는 중립 이름을 사용합니다.

생성된 텍스트는 파이프를 통해 다음 명령으로 전달되며 변수에 저장하거나 I/O 기능을 사용하여 대체할 수 있습니다:

```stscript
/genraw Write a funny message from Cthulhu about taking over the world. Use emojis. |
/popup <h3>Cthulhu says:</h3><div>{{pipe}}</div>
```

| ![Cthulhu Says](/static/scripts/cthulhu-says.png) |
|---------------------------------------------------|

또는 생성된 메시지를 캐릭터의 응답으로 삽입하려면:

```stscript
/genraw You have been memory wiped, your name is now Lisa and you're tearing me apart. You're tearing me apart Lisa! |
/sendas name={{char}} {{pipe}}
```

## 임시 캐릭터

그룹 채팅이 아닌 경우 스크립트는 현재 연결된 LLM에 다른 캐릭터로 일시적으로 요청할 수 있습니다.

- `/ask (prompt)` — 지정된 캐릭터에 대해 제공된 프롬프트를 사용하고 채팅 메시지를 포함하여 텍스트를 생성합니다. 이 캐릭터의 응답 스와이프는 현재 캐릭터로 되돌아갑니다.

```stscript
/ask name=... (prompt)
```
### `/ask`의 인수

- `name` — **필수**. 질문할 캐릭터의 이름(또는 아바타 키와 같은 고유한 캐릭터 식별자). 이것은 명명된 인수로 제공되어야 합니다.
- `return` — 반환 값을 제공하는 방법을 지정합니다. 기본값은 `pipe`(명령 파이프를 통한 출력)입니다. API에서 지원하는 경우 다른 옵션을 지정할 수 있습니다.


```stscript
/ask name=Alice What is your favorite color?
```

## 프롬프트 주입

스크립트는 사용자 정의 LLM 프롬프트 주입을 추가하여 본질적으로 무제한의 작성자 노트와 동등하게 만들 수 있습니다.

- `/inject (text)` — 현재 채팅을 위해 일반 LLM 프롬프트에 텍스트를 삽입하며 고유 식별자가 필요합니다. 채팅 메타데이터에 저장됩니다.
- `/listinjects` — 현재 채팅에 대해 스크립트로 추가된 모든 프롬프트 주입 목록을 시스템 메시지로 표시합니다.
- `/flushinjects` — 현재 채팅에 대해 스크립트로 추가된 모든 프롬프트 주입을 삭제합니다.
- `/note (text)` — 현재 채팅에 대한 작성자 노트 값을 설정합니다. 채팅 메타데이터에 저장됩니다.
- `/interval` — 현재 채팅에 대한 작성자 노트 삽입 간격을 설정합니다.
- `/depth` — 채팅 내 위치에 대한 작성자 노트 삽입 깊이를 설정합니다.
- `/position`  — 현재 채팅에 대한 작성자 노트 위치를 설정합니다.

### `/inject`의 인수

```stscript
/inject id=IdGoesHere position=chat depth=4 My prompt injection
```

- `id` — 식별자 문자열 또는 변수에 대한 참조. 동일한 ID로 `/inject`를 연속 호출하면 이전 텍스트 주입을 덮어씁니다. **필수 인수.**
- `position` — 주입의 위치를 설정합니다. 기본값: `after`. 가능한 값:
  - `after`: 메인 프롬프트 이후.
  - `before`: 메인 프롬프트 이전.
  - `chat`: 채팅 내.
- `depth` — 채팅 내 위치에 대한 주입 깊이를 설정합니다. 0은 마지막 메시지 이후 삽입을 의미하고, 1은 마지막 메시지 이전 등을 의미합니다. 기본값: 4.
- 명명되지 않은 인수는 주입될 텍스트입니다. 빈 문자열은 제공된 식별자에 대한 이전 값을 제거합니다.

## 채팅 메시지 액세스

### 메시지 읽기

`/messages` 명령을 사용하여 현재 선택한 채팅의 메시지에 액세스할 수 있습니다.

```stscript
/messages names=on/off start-finish
```

- `names` 인수는 캐릭터 이름을 포함할지 여부를 지정하는 데 사용됩니다. 기본값: `on`.
- 명명되지 않은 인수에서 메시지 인덱스 또는 `start-finish` 형식의 범위를 허용합니다. 범위는 포함됩니다!
- 범위를 만족할 수 없는 경우, 즉 잘못된 인덱스이거나 존재하는 것보다 더 많은 메시지가 요청된 경우 빈 문자열이 반환됩니다.
- 프롬프트에서 숨겨진 메시지(고스트 아이콘으로 표시됨)는 출력에서 제외됩니다.
- 최신 메시지의 인덱스를 알고 싶다면 `{{lastMessageId}}` 매크로를 사용하고 `{{lastMessage}}`는 메시지 자체를 가져옵니다.

범위의 시작 인덱스를 계산하려면, 예를 들어 마지막 N개의 메시지를 가져와야 하는 경우 변수 뺄셈을 사용하세요.
이 예제는 채팅의 마지막 3개의 메시지를 가져옵니다:

```stscript
/setvar key=start {{lastMessageId}} |
/addvar key=start -2 |
/messages names=off {{getvar::start}}-{{lastMessageId}} |
/setinput
```

### 메시지 전송

스크립트는 사용자, 캐릭터, 페르소나, 중립 내레이터로 메시지를 전송하거나 주석을 추가할 수 있습니다.

1. `/send (text)` — 현재 선택한 페르소나로 메시지를 추가합니다.
2. `/sendas name=charname (text)` — 이름으로 일치하는 모든 캐릭터로 메시지를 추가합니다. `name` 인수가 필요합니다. `{{char}}` 매크로를 사용하여 현재 캐릭터로 전송하세요.
3. `/sys (text)` — 사용자 또는 캐릭터에 속하지 않는 중립 내레이터로부터 메시지를 추가합니다. 표시된 이름은 순전히 장식적이며 `/sysname` 명령으로 사용자 정의할 수 있습니다.
4. `/comment (text)` — 채팅에 표시되지만 프롬프트에는 표시되지 않는 숨겨진 주석을 추가합니다.
5. `/addswipe (text)` — 마지막 캐릭터 메시지에 스와이프를 추가합니다. 사용자 또는 숨겨진 메시지에는 스와이프를 추가할 수 없습니다.
6. `/hide (message id or range)` — 제공된 메시지 인덱스 또는 `start-finish` 형식의 포함 범위를 기반으로 프롬프트에서 하나 또는 여러 메시지를 숨깁니다.
7. `/unhide (message id or range)` — 제공된 메시지 인덱스 또는 `start-finish` 형식의 포함 범위를 기반으로 하나 또는 여러 메시지를 프롬프트로 반환합니다.

`/send`, `/sendas`, `/sys` 및 `/comment` 명령은 선택적으로 메시지 삽입의 정확한 위치를 지정하는 0 기반 숫자 값(또는 그러한 값을 포함하는 변수 이름)이 있는 명명된 인수 `at`를 허용합니다. 기본적으로 새 메시지는 채팅 로그의 끝에 삽입됩니다.

이것은 대화 기록의 시작 부분에 사용자 메시지를 삽입합니다:

```stscript
/send at=0 Hi, I use Linux.
```

### 메시지 삭제

**이러한 명령은 잠재적으로 파괴적이며 "실행 취소" 기능이 없습니다. 실수로 중요한 것을 삭제한 경우 /backups/ 폴더를 확인하세요.**

1. `/cut (message id or range)` — 제공된 메시지 인덱스 또는 `start-finish` 형식의 포함 범위를 기반으로 채팅에서 하나 또는 여러 메시지를 자릅니다.
2. `/del (number)` — 채팅에서 마지막 N개의 메시지를 삭제합니다.
3. `/delswipe (1-based swipe id)` — 제공된 1 기반 스와이프 ID를 기반으로 마지막 캐릭터 메시지에서 스와이프를 삭제합니다.
4. `/delname (character name)` — 지정된 이름의 캐릭터에 속하는 현재 채팅의 모든 메시지를 삭제합니다.
5. `/delchat` — 현재 채팅을 삭제합니다.

## World Info 명령

World Info(Lorebook이라고도 함)는 프롬프트에 데이터를 동적으로 삽입하는 매우 유용한 도구입니다. 자세한 설명은 전용 페이지를 참조하세요: [World Info](/Usage/worldinfo.md).

1. `/getchatbook` – 채팅 바운드 World Info 파일의 이름을 가져오거나 바인딩되지 않은 경우 새 파일을 생성하고 파이프로 전달합니다.
2. `/findentry file=bookName field=fieldName [text]` – 제공된 텍스트와 필드 값의 퍼지 매칭을 사용하여 지정된 파일(또는 파일 이름을 가리키는 변수)에서 레코드의 UID를 찾습니다(기본 필드: `key`). UID를 파이프로 전달합니다. 예: `/findentry file=chatLore field=key Shadowfang`.
3. `/getentryfield file=bookName field=field [UID]` – 지정된 World Info 파일(또는 파일 이름을 가리키는 변수)에서 UID가 있는 레코드의 필드 값(기본 필드: `content`)을 가져와 값을 파이프로 전달합니다. 예: `/getentryfield file=chatLore field=content 123`.
4. `/setentryfield file=bookName uid=UID field=field [text]` – 지정된 World Info 파일(또는 파일 이름을 가리키는 변수)에서 UID(또는 UID를 가리키는 변수)가 있는 레코드의 필드 값(기본 필드: `content`)을 설정합니다. 키 필드에 여러 값을 설정하려면 쉼표로 구분된 목록을 텍스트 값으로 사용하세요. 예: `/setentryfield file=chatLore uid=123 field=key Shadowfang,sword,weapon`.
5. `/createentry file=bookName key=keyValue [content text]` – 키와 콘텐츠(이 두 인수는 모두 *선택 사항*)를 사용하여 지정된 파일(또는 파일 이름을 가리키는 변수)에 새 레코드를 생성하고 UID를 파이프로 전달합니다. 예: `/createentry file=chatLore key=Shadowfang The sword of the king`.

### 유효한 항목 필드

| 필드              | UI 요소        | 값 타입      |
|:-------------------|:------------------|:----------------|
| `content`          | Content           | String          |
| `comment`          | Title / Memo      | String          |
| `key`              | Primary Keywords  | List of strings |
| `keysecondary`     | Optional Filter   | List of strings |
| `constant`         | Constant Status   | Boolean (1/0)   |
| `disable`          | Disabled Status   | Boolean (1/0)   |
| `order`            | Order             | Number          |
| `selectiveLogic`   | Logic             | (아래 참조)     |
| `excludeRecursion` | Non-recursable    | Boolean (1/0)   |
| `probability`      | Trigger%          | Number (0-100)  |
| `depth`            | Depth             | Number (0-999)  |
| `position`         | Position          | (아래 참조)     |
| `role`             | Depth Role        | (아래 참조)     |
| `scanDepth`        | Scan Depth        | Number (0-100)  |
| `caseSensitive`    | Case-Sensitive    | Boolean (1/0)   |
| `matchWholeWords`  | Match Whole Words | Boolean (1/0)   |
| `vectorized`       | Vectorized Status | Boolean (1/0)   |
| `automationId`     | Automation ID     | String          |
| `group`            | Inclusion Group   | String          |
| `groupOverride`    | Inclusion Group Prioritize | Boolean (1/0) |
| `groupWeight`      | Inclusion Group Weight | Number (0-100) |
| `useGroupScoring`  | Group Scoring     | Boolean (1/0)   |
| `characterFilterExclude` | Character Filter Exclude Mode | List of strings |
| `characterFilterNames` | Character Filter Names | List of strings |
| `characterFilterTags` | Character Filter Tags | List of strings |
| `matchCharacterDepthPrompt` | Match Character Depth Prompt | Boolean (1/0) |
| `matchCharacterDescription` | Match Character Description | Boolean (1/0) |
| `matchCharacterPersonality` | Match Character Personality | Boolean (1/0) |
| `matchCreatorNotes` | Match Creator Notes | Boolean (1/0) |
| `matchPersonaDescription` | Match Persona Description | Boolean (1/0) |
| `matchScenario` | Match Scenario | Boolean (1/0) |

**Logic 값**

- 0 = AND ANY
- 1 = NOT ALL
- 2 = NOT ANY
- 3 = AND ALL

**Position 값**

- 0 = before main prompt
- 1 = after main prompt
- 2 = top of Author's Note
- 3 = bottom of Author's Note
- 4 = in-chat at depth
- 5 = top of example messages
- 6 = bottom of example messages

**Role 값** (Position = 4만 해당)
- 0 = System
- 1 = User
- 2 = Assistant

### 예제 1: 키로 채팅 로어북에서 콘텐츠 읽기

```stscript
/getchatbook | /setvar key=chatLore |
/findentry file={{getvar::chatLore}} field=key Shadowfang |
/getentryfield file={{getvar::chatLore}} field=key |
/echo
```

### 예제 2: 키와 콘텐츠로 채팅 로어북 항목 만들기

```stscript
/getchatbook | /setvar key=chatLore |
/createentry file={{getvar::chatLore}} key="Milla" Milla Basset is a friend of Lilac and Carol. She is a hush basset puppy who possesses the power of alchemy. |
/echo
```

### 예제 3: 채팅의 새 정보로 기존 로어북 항목 확장

```stscript
/getchatbook | /setvar key=chatLore |
/findentry file={{getvar::chatLore}} field=key Milla |
/setvar key=millaUid |
/getentryfield file={{getvar::chatLore}} field=content |
/setvar key=millaContent |
/gen lock=on Tell me more about Milla Basset based on the provided conversation history. Incorporate existing information into your reply: {{getvar::millaContent}} |
/setvar key=millaContent |
/echo New content: {{pipe}} |
/setentryfield file={{getvar::chatLore}} uid=millaUid field=content {{getvar::millaContent}}
```

## 텍스트 조작

다양한 스크립트 시나리오에서 사용할 유용한 텍스트 조작 유틸리티 명령이 많이 있습니다.

1. `/trimtokens` — 입력을 시작 또는 끝에서 지정된 수의 텍스트 토큰으로 자르고 결과를 파이프에 출력합니다.
2. `/trimstart` — 입력을 첫 번째 완전한 문장의 시작으로 자르고 결과를 파이프에 출력합니다.
3. `/trimend` — 입력을 마지막 완전한 문장의 끝으로 자르고 결과를 파이프에 출력합니다.
4. `/fuzzy` — 입력 텍스트를 문자열 목록에 대해 퍼지 매칭을 수행하여 최상의 문자열 일치를 파이프에 출력합니다.
5. `/regex name=scriptName [text]` — 지정된 텍스트에 대해 Regex 익스텐션의 regex 스크립트를 실행합니다. 스크립트는 활성화되어야 합니다.

### `/trimtokens`의 인수

```stscript
/trimtokens limit=number direction=start/end (input)
```

1. `direction`은 자르기 방향을 설정하며 `start` 또는 `end`일 수 있습니다. 기본값: `end`.
2. `limit`은 출력에 남을 토큰의 양을 설정합니다. 숫자를 포함하는 변수 이름을 지정할 수도 있습니다. **필수 인수.**
3. 명명되지 않은 인수는 자를 입력 텍스트입니다.

### `/fuzzy`의 인수

```stscript
/fuzzy list=["candidate1","candidate2"] (input)
```

1. `list`는 후보를 포함하는 JSON 직렬화된 문자열 배열입니다. 목록을 포함하는 변수 이름을 지정할 수도 있습니다. **필수 인수.**
2. 명명되지 않은 인수는 일치시킬 입력 텍스트입니다. 출력은 입력과 가장 밀접하게 일치하는 후보 중 하나입니다.

## 자동 완성

- 자동 완성은 채팅 입력과 큰 Quick Reply 편집기 모두에서 활성화됩니다.
- 자동 완성은 입력의 어디에서나 작동합니다. 여러 파이프된 명령과 중첩된 클로저가 있어도 마찬가지입니다.
- 자동 완성은 일치하는 명령을 찾는 세 가지 방법을 지원합니다(*사용자 설정* -> *STscript 매칭*).

1. **Starts with** "이전" 방법. 입력된 값으로 정확히 시작하는 명령만 표시됩니다.
2. **Includes**  입력된 값을 *포함*하는 모든 명령이 표시됩니다. 예: `/delete`를 입력하면 자동 완성 목록에 `/qr-delete` 및 `/qr-set-delete` 명령이 표시됩니다(/qr-**delete**, /qr-set-**delete**).
3. **Fuzzy**  입력된 값에 대해 퍼지 매칭할 수 있는 모든 명령이 표시됩니다. 예: `/seas`를 입력하면 자동 완성 목록에 `/sendas` 명령이 표시됩니다(/**se**nd**as**).

- 명령 인수도 자동 완성에서 지원됩니다. 필수 인수의 경우 목록이 자동으로 표시됩니다. 선택적 인수의 경우 *Ctrl*+*Space*를 눌러 사용 가능한 옵션 목록을 엽니다.
- `/:`를 입력하여 클로저 또는 QR을 실행하면 자동 완성이 스코프 변수 및 QR 목록을 표시합니다.
- 자동 완성은 (슬래시 명령에서) 매크로에 대한 제한적인 지원이 있습니다. `{{`를 입력하여 사용 가능한 매크로 목록을 가져옵니다.
- *위* 및 *아래* *화살표 키*를 사용하여 자동 완성 옵션 목록에서 옵션을 선택합니다.
- *Enter* 또는 *Tab*을 누르거나 옵션을 *클릭*하여 커서 위치에 옵션을 배치합니다.
- *Escape*를 눌러 자동 완성 목록을 닫습니다.
- *Ctrl*+*Space*를 눌러 자동 완성 목록을 열거나 선택한 옵션의 세부 정보를 전환합니다.

## 파서 플래그

```stscript
/parser-flag
```

파서는 동작을 수정하기 위해 플래그를 허용합니다. 이러한 플래그는 스크립트의 어느 시점에서나 켜고 끌 수 있으며 모든 후속 입력은 그에 따라 평가됩니다.
사용자 설정에서 기본 플래그를 설정할 수 있습니다.

### Strict Escaping

```stscript
/parser-flag STRICT_ESCAPING on |
```

`STRICT_ESCAPING`이 활성화된 변경 사항은 다음과 같습니다.

#### 파이프

파이프는 따옴표로 묶인 값에서 이스케이프할 필요가 없습니다.

```stscript
/echo title="a|b" c\|d
```

#### 백슬래시

기호 앞의 백슬래시를 이스케이프하여 기능 기호 뒤에 리터럴 백슬래시를 제공할 수 있습니다.

```stscript
// 이것은 "foo \"를 에코한 다음 "bar"를 에코합니다 |
/echo foo \\|
/echo bar
```

```stscript
/echo \\|
/echo \\\|
```

### Replace Variable Macros

```stscript
/parser-flag REPLACE_GETVAR on |
```

이 플래그는 변수 값에 매크로로 해석될 수 있는 텍스트가 포함된 경우 이중 대체를 피하는 데 도움이 됩니다. `{{var::}}` 매크로는 마지막에 대체되며 결과 텍스트/변수 값에 대한 추가 대체가 발생하지 않습니다.

모든 `{{getvar::}}` 및 `{{getglobalvar::}}` 매크로를 `{{var::}}`로 대체합니다.
내부적으로 파서는 대체된 매크로가 있는 명령 앞에 일련의 명령 실행기를 삽입합니다:

- `/let`을 호출하여 현재 `{{pipe}}`를 스코프 변수에 저장합니다
- `/getvar` 또는 `/getglobalvar`를 호출하여 매크로에 사용된 변수를 가져옵니다
- `/let`을 호출하여 검색된 변수를 스코프 변수에 저장합니다
- 저장된 `{{pipe}}` 값으로 `/return`을 호출하여 다음 명령의 올바른 파이프된 값을 복원합니다

```stscript
// 다음은 마지막 메시지의 id / number를 에코합니다 |
/setvar key=x \{\{lastMessageId}} |
/echo {{getvar::x}}
```

```stscript
// 이것은 리터럴 텍스트 {{lastMessageId}}를 에코합니다 |
/parser-flag REPLACE_GETVAR |
/setvar key=x \{\{lastMessageId}} |
/echo {{getvar::x}}
```

## Quick Reply: 스크립트 라이브러리 및 자동 실행

Quick Reply는 스크립트를 저장하고 실행하는 쉬운 방법을 제공하는 내장 SillyTavern 익스텐션입니다.

### Quick Reply 구성

시작하려면 익스텐션 패널(쌓인 블록 아이콘)을 열고 Quick Reply 메뉴를 확장합니다.

<div style="display:flex;justify-content:center">

![Quick Reply](/static/scripts/quick-reply.png)

</div>

**Quick Reply는 기본적으로 비활성화되어 있으므로 먼저 활성화해야 합니다.** 그러면 채팅 입력창 위에 바가 나타납니다.

표시된 버튼 텍스트 레이블(간결함을 위해 이모지 사용을 권장합니다)과 버튼을 클릭할 때 실행될 스크립트를 설정할 수 있습니다.

버튼 수는 **슬롯 수** 설정(최대 = 100)으로 제어되며 필요에 따라 조정하고 완료되면 "적용"을 클릭합니다.

**사용자 입력 자동 주입**은 STscript를 사용할 때 비활성화하는 것이 좋습니다. 그렇지 않으면 입력을 방해할 수 있습니다. 대신 `{{input}}` 매크로를 사용하여 스크립트에서 입력창의 현재 값을 가져옵니다.

**Quick Reply 프리셋**을 사용하면 사전 정의된 Quick Reply의 여러 세트를 가질 수 있으며 `/qrset (name of set)` 명령을 사용하여 수동으로 또는 전환할 수 있습니다.
현재 사용 중인 프리셋에 변경 사항을 쓰려면 다른 세트로 전환하기 전에 "업데이트"를 클릭하는 것을 잊지 마세요!

### 수동 실행

이제 라이브러리에 첫 번째 스크립트를 추가할 수 있습니다. 빈 슬롯을 선택(또는 생성)하고 왼쪽 상자에 "Click me"를 입력하여 레이블을 설정한 다음 오른쪽 상자에 다음을 붙여넣습니다:

```stscript
/addvar key=clicks 1 |
/if left=clicks right=5 rule=eq else="/echo Keep going..." "/echo You did it!  \| /flushvar clicks"
```

그런 다음 채팅창 위에 나타난 버튼을 5번 클릭합니다.
클릭할 때마다 변수 `clicks`가 1씩 증가하고 값이 5와 같으면 다른 메시지를 표시하고 변수를 재설정합니다.

### 자동 실행

생성된 명령의 `⋮` 버튼을 클릭하여 모달 메뉴를 엽니다.

| ![Automatic execution](/static/scripts/autoexecute.png) |
|---------------------------------------------------------|

이 메뉴에서 다음을 수행할 수 있습니다:

- 편리한 전체 화면 편집기에서 스크립트를 편집합니다
- 채팅창에서 버튼을 숨겨 자동 실행에만 액세스할 수 있도록 합니다.
- 다음 조건 중 하나 이상에서 자동 실행을 활성화합니다:
  * 앱 시작
  * 채팅에 사용자 메시지 전송
  * 채팅에서 AI 메시지 수신
  * 캐릭터 또는 그룹 채팅 열기
  * 그룹 멤버로부터 응답 트리거
  * 동일한 Automation ID를 사용하여 World Info 항목 활성화
- Quick Reply에 대한 사용자 정의 도구 팁 제공(UI에서 Quick Reply 위에 마우스를 올릴 때 표시되는 텍스트)
- 테스트 목적으로 스크립트 실행

Quick Reply 익스텐션이 활성화된 경우에만 명령이 자동으로 실행됩니다.

예를 들어 다음 스크립트를 추가하고 사용자 메시지에서 자동 실행되도록 설정하여 5개의 사용자 메시지를 보낸 후 메시지를 표시할 수 있습니다.

```stscript
/addvar key=usercounter 1 |
/echo You've sent {{pipe}} messages. |
/if left=usercounter right=5 rule=gte "/echo Game over! \| /flushvar usercounter"
```

### 디버거

확장된 Quick Reply 편집기 내에 기본 디버거가 있습니다. 스크립트의 어디에나 `/breakpoint |`로 중단점을 설정합니다. QR 편집기에서 스크립트를 실행하면 해당 지점에서 실행이 중단되어 현재 사용 가능한 변수, 파이프, 명령 인수 등을 검사하고 나머지 코드를 하나씩 단계별로 진행할 수 있습니다.

```stscript
/let x {: n=1
	/echo n is {{var::n}} |
	/mul n n |
:} |
/breakpoint |
/:x n=3 |
/echo result is {{pipe}} |
```

| ![QR Editor Debugger](/static/scripts/st-debugger.png) |
|--------------------------------------------------------|

### 프로시저 호출

`/run` 명령은 Quick Reply에서 레이블로 정의된 스크립트를 호출하여 기본적으로 프로시저를 정의하고 결과를 반환하는 기능을 제공할 수 있습니다. 이를 통해 다른 스크립트가 참조할 수 있는 재사용 가능한 스크립트 블록을 가질 수 있습니다. 프로시저 파이프의 마지막 결과는 그 이후의 다음 명령으로 전달됩니다.

```stscript
/run ScriptLabel
```

두 개의 Quick Reply를 만들어 봅시다:

***
**레이블:**

`GetRandom`

**명령:**

```stscript
/pass {{roll:d100}}
```
***
**레이블:**

`GetMessage`

**명령:**
```stscript
/run GetRandom | /echo Your lucky number is: {{pipe}}
```
***

`GetMessage` 버튼을 클릭하면 `GetRandom` 프로시저를 호출하여 `{{roll}}` 매크로를 해석하고 숫자를 호출자에게 전달하여 사용자에게 표시합니다.

- 프로시저는 명명된 인수 또는 명명되지 않은 인수를 허용하지 않지만 호출자와 동일한 변수를 참조할 수 있습니다.
- 부주의하게 처리하면 "호출 스택 초과" 오류가 발생할 수 있으므로 프로시저를 호출할 때 재귀를 피하세요.

#### 다른 Quick Reply 프리셋에서 프로시저 호출

`a.b` 구문을 사용하여 다른 Quick Reply 프리셋에서 프로시저를 호출할 수 있습니다. 여기서 a = QR 프리셋 이름, b = QR 레이블 이름입니다.

```stscript
/run QRpreset1.QRlabel1
```

기본적으로 시스템은 먼저 Quick Reply 레이블 `a.b`를 찾으므로 레이블 중 하나가 문자 그대로 "QRpreset1.QRlabel1"인 경우 이를 실행하려고 시도합니다. 그러한 레이블이 없으면 "QRlabel1"로 레이블이 지정된 QR이 있는 QR 프리셋 이름 "QRpreset1"을 검색합니다.

### Quick Reply 관리 명령

#### Quick Reply 만들기

* `/qr-create (arguments, [message])` – 새 Quick Reply를 만듭니다. 예: `/qr-create set=MyPreset label=MyButton /echo 123`

인수:
- `label`    - string - 버튼의 텍스트, 예: `label=MyButton`
- `set`      - string - QR 세트의 이름, 예: `set=PresetName1`
- `hidden`   - bool   - 버튼을 숨길지 여부, 예: `hidden=true`
- `startup`  - bool   - 앱 시작 시 자동 실행, 예: `startup=true`
- `user`     - bool   - 사용자 메시지에서 자동 실행, 예: `user=true`
- `bot`      - bool   - AI 메시지에서 자동 실행, 예: `bot=true`
- `load`     - bool   - 채팅 로드 시 자동 실행, 예: `load=true`
- `title`    - bool   - 버튼에 표시될 제목/툴팁, 예: `title="My Fancy Button"`

#### Quick Reply 삭제

* `/qr-delete (set=string [label])` – Quick Reply를 삭제합니다

#### Quick Reply 업데이트

* `/qr-update (arguments, [message])` – Quick Reply를 업데이트합니다. 예: `/qr-update set=MyPreset label=MyButton newlabel=MyRenamedButton /echo 123`

인수:
- `newlabel` - string - 버튼의 새 텍스트, 예: `newlabel=MyRenamedButton`
- `label`    - string - 버튼의 텍스트, 예: `label=MyButton`
- `set`      - string - QR 세트의 이름, 예: `set=PresetName1`
- `hidden`   - bool   - 버튼을 숨길지 여부, 예: `hidden=true`
- `startup`  - bool   - 앱 시작 시 자동 실행, 예: `startup=true`
- `user`     - bool   - 사용자 메시지에서 자동 실행, 예: `user=true`
- `bot`      - bool   - AI 메시지에서 자동 실행, 예: `bot=true`
- `load`     - bool   - 채팅 로드 시 자동 실행, 예: `load=true`
- `title`    - bool   - 버튼에 표시될 제목/툴팁, 예: `title="My Fancy Button"`

####

* `qr-get` - Quick Reply의 모든 속성을 검색합니다. 예: `/qr-get set=myQrSet id=42`

#### QR 프리셋 만들기 또는 업데이트

* `/qr-presetupdate (arguments [label])` 또는 `/qr-presetadd (arguments [label])`

인수:
- `enabled` - bool - 프리셋 활성화 또는 비활성화
- `nosend`  - bool - 전송 비활성화 / 사용자 입력에 삽입(슬래시 명령에 대해 유효하지 않음)
- `before`  - bool - 사용자 입력 전에 QR 배치
- `slots`   - int  - 슬롯 수
- `inject`  - bool - 사용자 입력 자동 주입(비활성화된 경우 `{{input}}` 사용)

새 프리셋 만들기(기존 프리셋 재정의). 예: `/qr-presetadd slots=3 MyNewPreset`

#### QR 컨텍스트 메뉴 추가

* `/qr-contextadd (set=string label=string chain=bool [preset name])` – QR에 컨텍스트 메뉴 프리셋 추가. 예: `/qr-contextadd set=MyPreset label=MyButton chain=true MyOtherPreset`

#### 모든 컨텍스트 메뉴 제거

* `/qr-contextclear (set=string [label])` – QR에서 모든 컨텍스트 메뉴 프리셋 제거. 예: `/qr-contextclear set=MyPreset MyButton`

#### 하나의 컨텍스트 메뉴 제거

* `/qr-contextdel (set=string label=string [preset name])` – QR에서 컨텍스트 메뉴 프리셋 제거. 예: `/qr-contextdel set=MyPreset label=MyButton MyOtherPreset`

### Quick Reply 값 이스케이프

`|{}`는 QR 메시지/명령에서 백슬래시로 이스케이프할 수 있습니다.

예를 들어, `/qr-create label=MyButton /getvar myvar \| /echo \{\{pipe\}\}`를 사용하여 `/getvar myvar | /echo {{pipe}}`를 호출하는 QR을 만듭니다.

## 익스텐션 명령

SillyTavern 익스텐션(내장, 다운로드 가능 및 서드파티 모두)은 자체 슬래시 명령을 추가할 수 있습니다. 다음은 공식 익스텐션의 기능 예시일 뿐입니다. 목록이 불완전할 수 있으므로 사용 가능한 명령의 가장 완전한 목록은 `/help slash`를 확인하세요.

1. `/websearch (query)` — 지정된 쿼리에 대해 웹 페이지 스니펫을 온라인으로 검색하고 결과를 파이프로 반환합니다. Web Search 익스텐션에서 제공합니다.
2. `/imagine (prompt)` — 제공된 프롬프트를 사용하여 이미지를 생성합니다. Image Generation 익스텐션에서 제공합니다.
3. `/emote (sprite)` — 이름의 퍼지 매칭으로 활성 캐릭터의 스프라이트를 설정합니다. Character Expressions 익스텐션에서 제공합니다.
4. `/costume (subfolder)` — 활성 캐릭터의 스프라이트 세트 재정의를 설정합니다. Character Expressions 익스텐션에서 제공합니다.
5. `/music (name)` — 이름으로 재생되는 배경 음악 파일을 강제로 변경합니다. Dynamic Audio 익스텐션에서 제공합니다.
6. `/ambient (name)` — 이름으로 재생되는 앰비언트 사운드 파일을 강제로 변경합니다. Dynamic Audio 익스텐션에서 제공합니다.
7. `/roll (dice formula)` — 주사위 굴림 결과와 함께 채팅에 숨겨진 메시지를 추가합니다. D&D Dice 익스텐션에서 제공합니다.

## UI 상호 작용

스크립트는 SillyTavern의 UI와도 상호 작용할 수 있습니다: 채팅을 탐색하거나 스타일 매개변수를 변경합니다.

### 캐릭터 탐색

1. `/random` — 무작위 캐릭터와의 채팅을 엽니다.
2. `/go (name)` — 지정된 이름의 캐릭터와의 채팅을 엽니다. 먼저 정확한 이름 일치를 검색한 다음 접두사로, 그 다음 하위 문자열로 검색합니다.

### UI 스타일링

1. `/bubble` — 메시지 스타일을 "bubble chat" 스타일로 설정합니다.
2. `/flat` — 메시지 스타일을 "flat chat" 스타일로 설정합니다.
3. `/single` — 메시지 스타일을 "single document" 스타일로 설정합니다.
4. `/movingui (name)` — 이름으로 MovingUI 프리셋을 활성화합니다.
5. `/resetui` — MovingUI 패널 상태를 원래 위치로 재설정합니다.
6. `/panels` — UI 패널 가시성을 전환합니다: 상단 바, 왼쪽 및 오른쪽 서랍.
7. `/bg (name)` — 퍼지 이름 매칭을 사용하여 배경을 찾아 설정합니다. 채팅 배경 잠금 상태를 존중합니다.
8. `/lockbg` — 현재 채팅의 배경 이미지를 잠급니다.
9. `/unlockbg` — 현재 채팅의 배경 이미지 잠금을 해제합니다.

## 더 많은 예제

### 채팅 요약 생성 (by @IkariDevGIT)

```stscript
/setglobalvar key=summaryPrompt Summarize the most important facts and events that have happened in the chat given to you in the Input header. Limit the summary to 100 words or less. Your response should include nothing but the summary. |
/setvar key=tmp |
/messages 0-{{lastMessageId}} |
/trimtokens limit=3000 direction=end |
/setvar key=s1 |
/echo Generating, please wait... |
/genraw lock=on instruct=off {{instructInput}}{{newline}}{{getglobalvar::summaryPrompt}}{{newline}}{{newline}}{{instructInput}}{{newline}}{{getvar::s1}}{{newline}}{{newline}}{{instructOutput}}{{newline}}The chat summary:{{newline}} |
/setvar key=tmp |
/echo Done! |
/setinput {{getvar::tmp}} |
/flushvar tmp |
/flushvar s1
```

### 버튼 팝업 사용법

```stscript
/setglobalvar key=genders ["boy", "girl", "other"] |
/buttons labels=genders Who are you? |
/echo You picked: {{pipe}}
```

### N번째 피보나치 수 구하기 (Binet의 공식 사용)

!!!tip
**힌트**: `fib_no`의 값을 원하는 숫자로 설정하세요
!!!

```stscript
/setvar key=fib_no 5 |
/pow 5 0.5 | /setglobalvar key=SQRT5 |
/setglobalvar key=PHI 1.618033 |
/pow PHI fib_no | /div {{pipe}} SQRT5 |
/round |
/echo {{getvar::fib_no}}th Fibonacci's number is: {{pipe}}
```

### 재귀 계승 (클로저 사용)

```stscript
/let fact {: n=
    /if left={{var::n}} rule=gt right=1
        else={:
            /return 1
        :}
        {:
            /sub {{var::n}} 1 |
            /:fact n={{pipe}} |
            /mul {{var::n}} {{pipe}}
        :}
:} |

/input Calculate factorial of: |
/let n {{pipe}} |
/:fact n={{var::n}} |
/echo factorial of {{var::n}} is {{pipe}}
```
