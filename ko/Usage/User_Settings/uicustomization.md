---
order: 20
route: /usage/core-concepts/uicustomization/
---

# UI 사용자 정의

## UI 테마

### 테마 관리

테마 파일을 사용하면 UI 사용자 정의를 저장, 공유 및 재사용할 수 있습니다. 다양한 분위기나 목적을 위해 여러 테마를 유지 관리하고 즉시 전환할 수 있습니다.

* 테마 파일 가져오기/내보내기
* 기존 테마 삭제
* 현재 테마에 변경 사항 저장
* 새 테마로 저장

이 섹션의 모든 설정은 현재 테마에 저장됩니다. 테마를 전환하면 설정이 새 테마의 설정으로 대체됩니다.

### 디스플레이 설정

이러한 디스플레이 옵션은 채팅 인터페이스에서 캐릭터 및 메시지가 표시되는 방법에 영향을 미칩니다.

#### Avatar Style

Circle, Square, Rectangle 또는 Rounded Square 중에서 선택합니다. 이 설정은 사용자 및 AI 아바타 모두에 적용됩니다.

#### Chat Style

| Style        | 설명                                                                                                                                                    | [슬래시 명령](/For_Contributors/st-script.md#ui-styling) |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| **Flat**     | 깨끗하고 연속적인 "채팅 로그" 스타일, AI 상호작용이 생생하게 펼쳐지는 평평한 캔버스입니다.                                                                 | `/flat`<br>`/default`                                      |
| **Bubbles**  | 각 메시지에 대한 뚜렷한 버블, 즐거운 둥근 모서리 및 미묘한 3D 효과가 있는 "인스턴트 메신저" 스타일입니다.                                          | `/bubble`<br>`/bubbles`                                    |
| **Document** | 텍스트 중심 레이아웃의 간결한 문서형 모습입니다. 과거 메시지에 대한 아바타, 타임스탬프 및 메시지 컨트롤 버튼을 숨깁니다. | `/single`<br>`/story`                                      |

### 알림

알림 팝업(토스트 메시지)이 화면에 나타날 위치를 설정합니다.

* Top Left
* Top Center (기본값)
* Top Right
* Bottom Left
* Bottom Center
* Bottom Right

### 테마 색상

모든 UI 요소의 색 구성표를 사용자 정의하여 완벽한 테마를 만듭니다. 색상은 색상 선택기를 사용하여 선택할 수 있으며, 해당하는 경우 투명도 옵션을 포함합니다.

* Main Text
* Italics Text
* Underlined Text
* Quote Text
* Text Shadow
* Chat Background
* UI Background
* UI Border
* User Message
* AI Message

### 레이아웃 및 비주얼 설정

이러한 슬라이더로 인터페이스의 시각적 프레젠테이션을 미세 조정합니다.

* **Chat Width**: 채팅 창 너비 조정 (화면의 25-100%)
* **Font Scale**: 텍스트 크기 사용자 정의 (0.5-1.5배)
* **Blur Strength**: UI 패널 블러 제어 (0-30)
* **Shadow Width**: 텍스트 그림자 강도 조정 (0-5)

### 테마 토글

이러한 스위치는 다양한 UI 기능 및 동작을 제어합니다. 일부 옵션은 저사양 장치의 성능을 향상시킬 수 있으며, 다른 옵션은 채팅 인터페이스에 유용한 정보 또는 기능을 추가합니다.

* **Reduced Motion**: 애니메이션 및 전환 비활성화
* **No Blur Effect**: 더 나은 성능을 위해 배경 블러 제거
* **No Text Shadows**: 텍스트 그림자 효과 비활성화
* **[Visual Novel mode](Visual-Novel.md)**: 배경 스프라이트가 있는 간결한 채팅
* **Expand Message Actions**: 항상 전체 메시지 컨텍스트 메뉴 표시
* **Zen Sliders**: 간소화된 매개변수 컨트롤
* **Mad Lab Mode**: 제한 없는 매개변수 범위
* **Message Timer**: AI 응답 생성 시간 표시
* **Chat Timestamps**: 메시지 타임스탬프 표시
* **Model Icons**: 메시지에 대한 AI 모델 아이콘 표시
* **Message IDs**: 순차적인 메시지 번호 표시
* **Hide Chat Avatars**: 채팅에서 아바타 제거
* **Message Token Count**: 메시지당 토큰 수 표시
* **Compact Input Area**: 단일 행 입력 (모바일만 해당)
* **Swipe # for All Messages**: 모든 메시지에 스와이프 번호 표시
* **Characters Hotswap**: 즐겨찾는 캐릭터를 위한 빠른 선택 버튼
* **Avatar Hover Magnification**: 아바타 호버 시 줌 효과
* **Tags as Folders**: 태그를 폴더로 사용하여 캐릭터 정리
* **Click to Edit**: 메시지를 클릭하여 메시지 편집기를 빠르게 엽니다

### Custom CSS

채팅 인터페이스의 모양을 더욱 사용자 정의하기 위해 사용자 정의 CSS 스타일을 적용할 수 있습니다.

<i class="fa-fw fa-solid fa-maximize" title="Expand icon"></i> **Expand**를 사용하여 더 나은 가시성과 편집을 위해 편집기 창을 확장합니다.

테마를 전환하면 사용자 정의 CSS가 새 테마의 사용자 정의 CSS로 대체됩니다. 테마를 전환할 때 유지하려면 사용자 정의 CSS를 테마에 저장하세요.

많은 사용자 정의 CSS를 사용하거나 여러 테마에서 동일한 사용자 정의 CSS를 사용하려는 경우 비공식 [CSS Snippets extension](https://github.com/LenAnderson/SillyTavern-CssSnippets)을 사용하여 사용자 정의 CSS를 관리하고 구성하는 데 도움이 될 수 있습니다.

---

## Message Sound

봇에서 새 메시지를 받을 때 자신의 사용자 정의 소리를 재생하려면 SillyTavern 폴더의 다음 MP3 파일을 교체하세요:

`public/sounds/message.mp3`

80% 볼륨으로 재생됩니다.

"[Background Sound Only](index.md#miscellaneous)" 옵션이 활성화된 경우 SillyTavern 창이 **포커스되지 않은** 경우에만 소리가 재생됩니다.

## 수식 렌더링

수식 렌더링을 활성화하려면 [LaTeX extension](https://github.com/SillyTavern/Extension-LaTeX)을 사용하세요. 확장 프로그램을 얻으려면 SillyTavern의 "Download Extensions & Assets" 메뉴를 통해 설치해야 합니다.

수식을 LaTeX 및 AsciiMath에 대해 각각 `latex` 또는 `asciimath` 언어 식별자가 있는 코드 블록에 입력하세요. 확장 프로그램은 렌더링을 위해 [KaTeX](https://katex.org/)를 사용합니다.

<pre><code>```latex
\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}
```

```asciimath
int_{-oo}^{oo} e^{-x^2} dx = sqrt{pi}
```</code></pre>

!!!info 사용 중단 알림
레거시 `$` 및 `$$` 래퍼 구문은 더 이상 지원되지 않습니다. 이전 구문을 폴리필하려면 다음 regex 스크립트를 사용하세요:

* [$$ - LaTeX](https://github.com/SillyTavern/Extension-LaTeX/raw/refs/heads/main/assets/$$_-_latex.json)
* [$ - AsciiMath](https://github.com/SillyTavern/Extension-LaTeX/raw/refs/heads/main/assets/$_-_asciimath.json)
!!!
