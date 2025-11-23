---
route: /extensions/translation/
templating: false
---

# 채팅 번역

## 개요

채팅 번역 확장 기능은 다양한 번역 제공업체를 사용하여 다양한 언어 간 채팅 메시지의 실시간 번역을 가능하게 합니다. 수동 및 자동 번역 모드를 모두 지원합니다.

![Character message translated from English to Chinese using 'Translate Message/翻譯訊息' message action button](../static/extensions/translation/sensei.png)

+++ English
!["Translate Chat", "Translate Input"](../static/extensions/translation/wand-menu-en.png)
+++ 简体中文
!["翻译聊天", "翻译输入"](../static/extensions/translation/wand-menu-zh-cn.png)
+++ 繁體中文
!["翻譯聊天內容", "翻譯輸入內容"](../static/extensions/translation/wand-menu-zh-tw.png)
+++ 한국어
!["채팅 번역하기", "입력 번역하기"](../static/extensions/translation/wand-menu-ko.png)
+++ Русский
!["Перевести чат", "Перевести моё сообщение"](../static/extensions/translation/wand-menu-ru.png)
+++

## 사용법

채팅 메시지를 번역하는 모든 방법:

**<i class="fa-solid fa-magic-wand-sparkles"></i> Extensions** 메뉴의 **<i class="fa-solid fa-language"></i> Translate Chat** 버튼

- 전체 채팅 기록을 한 번에 번역합니다

**<i class="fa-solid fa-magic-wand-sparkles"></i> Extensions** 메뉴의 **<i class="fa-solid fa-keyboard"></i> Translate Input** 버튼

- 현재 입력 텍스트만 번역합니다
- 메시지를 보내기 전에 유용합니다

모든 메시지의 **<i class="fa-solid fa-ellipsis"></i> Message Actions** 툴바의 **<i class="fa-solid fa-language"></i> Translate Message** 아이콘

- 클릭하여 해당 메시지만 번역합니다
- 다시 클릭하면 원본 텍스트로 되돌아갑니다

**<i class="fa-solid fa-cubes"></i> Extensions** 패널의 **Chat Translation** 드로어의 **Auto-mode** 구성

- 사용자 입력, AI 응답 또는 둘 다를 자동으로 번역합니다

**/translate** slash 명령

- `/translate [target=language_code] text`를 사용하여 텍스트를 번역합니다

## 구성

**<i class="fa-solid fa-cubes"></i> Extensions** 패널의 **Chat Translation** 드로어에서 구성 옵션을 사용할 수 있습니다.

#### Provider

- 선호하는 [번역 서비스](#translation-providers)를 선택합니다
- 나타나면 **<i class="fa-solid fa-key"></i> API Key** 아이콘을 클릭하여 API 키를 입력합니다
- 나타나면 **<i class="fa-solid fa-link"></i> Custom URL** 아이콘을 클릭하여 사용자 정의 API URL을 입력합니다

#### Target Language

메시지를 작성하거나 AI 응답을 읽을 언어를 선택합니다.

#### Auto-mode

자동 번역 동작을 구성합니다.

- **None**: 자동 번역 없음
- **Translate responses**: AI 응답을 대상 언어로 자동 번역
- **Translate inputs**: 사용자 입력을 영어로 자동 번역
- **Translate both**: 사용자 입력과 AI 응답 모두 번역

#### Clear Translations

**<i class="fa-solid fa-trash-can"></i> Clear Translations** 버튼은 현재 채팅의 메시지에서 모든 번역을 제거합니다. 원본 메시지는 보존됩니다.

### 구성 예제: 중국어에서 영어로 채팅

중국어를 사용하는 사용자가 영어로 작동하는 AI와 중국어로 채팅할 수 있는 워크플로를 설정하려면:

1. Auto-mode를 "Translate both"로 설정합니다
2. Target Language를 "Chinese (Simplified)" 또는 "Chinese (Traditional)"로 설정합니다
3. 언어 자동 감지 기능이 우수한 번역 제공업체를 선택합니다(예: Google 또는 DeepL)

이 설정은 다음을 수행합니다:

- 사용자의 중국어 입력을 AI를 위해 영어로 번역합니다
- AI의 영어 응답을 사용자를 위해 중국어로 다시 번역합니다

이 설정은 입력에 대한 자동 언어 감지에 의존합니다. 더 정확한 제어를 위해 향후 업데이트에는 명시적인 소스 언어 선택이 포함될 수 있습니다.

## 번역 제공업체

**:icon-cloud:** 클라우드 기반
**<i class="fa-solid fa-link"></i>** 로컬, 사용자 정의 URL
**<i class="fa-solid fa-key"></i>** API 키 필요

| Provider                                                            | 위치                                                                      | 기능                                                                                               |
|---------------------------------------------------------------------|-------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| [Libre Translate](https://libretranslate.com/)                      | :icon-cloud: <i class="fa-solid fa-key"></i> <i class="fa-solid fa-link"></i> | 독점 번역 서비스의 자체 호스팅(AGPL-3.0) 대안, 클라우드 호스팅 Pro 계층 포함     |
| [Google Translate](https://cloud.google.com/translate)              | :icon-cloud:                                                                  | 널리 사용됨, 많은 언어 지원, 좋은 정확도                                                    |
| [Lingva Translate](https://lingva.ml/)                              | <i class="fa-solid fa-link"></i>                                              | Google Translate의 대체 프런트엔드, 오픈 소스(AGPL-3.0), 개인 정보 보호 중심                    |
| [DeepL](https://www.deepl.com/)                                     | :icon-cloud: <i class="fa-solid fa-key"></i>                                  | 고품질 번역, 특히 유럽 언어에 적합                                           |
| [DeepLX](https://github.com/OwO-Network/DeepLX)                     | <i class="fa-solid fa-link"></i>                                              | 자체 호스팅 DeepL 프록시, 오픈 소스(MIT), 무료이지만 DeepL Pro 프록시에는 DeepL API 키 필요         |
| [Bing Translator](https://www.bing.com/translator)                  | :icon-cloud:                                                                  | Microsoft의 번역 서비스, Azure 서비스와 통합                                        |
| [OneRing Translator](https://github.com/janvarev/OneRingTranslator) | <i class="fa-solid fa-link"></i>                                              | Google Translate 및 기타 제공업체의 자체 호스팅 프런트엔드, 개인 정보 보호 중심, 오픈 소스(AGPL-3.0) |
| [Yandex Translate](https://translate.yandex.com/)                   | :icon-cloud:                                                                  | 러시아어 및 동유럽 언어에 적합                                                        |

### DeepL별 구성

- 독일어, 프랑스어, 이탈리아어, 스페인어, 네덜란드어, 일본어 및 러시아어에 대해 격식 수준 사용 가능
- [config.yaml](/Administration/config-yaml.md#deepl-configuration)의 `deepl.formality`를 통해 구성

## Slash 명령

빠른 번역을 위해 `/translate` 명령을 사용합니다. 구문: `/translate [target=language_code] text`. 대상 언어가 제공되지 않으면 확장 기능 설정의 값이 사용됩니다.

### 기본 사용법

현재 대상 언어로 텍스트를 번역하고 팝업으로 표시:

```
/translate Welcome to the Tavern | /echo
```

![Popup in Chinese (Simplified), '欢迎来到酒馆/Welcome to the Tavern'](../static/extensions/translation/welcome-tavern.png)

스페인어로 텍스트를 번역하고 채팅에 추가:

```
/translate target=es Hello world | /send
```

![User message in Spanish, 'Hola Mundo/Hello world'](/static/extensions/translation/hola-mundo.png)

### 테스트, 파이프라인 번역, 로컬라이제이션

사용자에게 메시지와 언어를 묻고 메시지를 해당 언어로 번역한 다음 구성된 대상 언어로 다시 번역하고 두 번역을 모두 팝업으로 표시합니다. 이 예제는 `/input` 및 `/buttons` 명령을 사용하여 사용자 입력을 수집합니다:

```shell
/input default="Hello, world!" <span data-i18n="Test Message">Sample text</span> |
/let key=input ||
/buttons labels=["zh-CN", "zh-TW", "es", "hu", "en"] <span data-i18n="UI Language">Language</span> |
/let key=lang ||
/translate target={{var::lang}} {{var::input}} | /let key=tx_target |
/translate | /let key=tx_orig ||
/echo escapeHtml=false cssClass=wider_dialogue_popup
<b data-i18n="Test Message">Test message</b>: {{var::input}} <br/>
<b data-i18n="Output">Output</b> ({{var::lang}}): {{var::tx_target}} <br/>
<b data-i18n="Output">Output</b> (<span data-i18n="ext_translate_target_lang">target language</span>): {{var::tx_orig}} <br/>
```

이것은 중요한 곳에 작성하기 전에 말하지 않는 언어로의 번역 품질을 확인하는 데 유용합니다.

![Popup, 'Welcome to the Tavern/欢迎来到酒馆/welcome to the pub', en, zh-CN, en](../static/extensions/translation/welcome-tavern-en-cn.png)
![Popup, 'My hovercraft is full of eels/我的氣墊船裡裝滿了鰻魚/My hovercraft is filled with eels', en, zh-TW, en](../static/extensions/translation/eels-out-zh-tw.png)

UI 컨트롤은 구성된 대상 언어와 독립적으로 현재 로케일로 표시됩니다.

| `/input`                                                                                        | `/buttons`                                                                          |
|-------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|
| ![Input dialog, '发送测试消息/Send Test Message'](../static/extensions/translation/eels-input-zh.png) | ![Buttons dialog, '语言/Language'](../static/extensions/translation/eels-lang-zh.png) |

![Popup, '我的氣墊船裡裝滿了鰻魚/My hovercraft is full of eels', zh-TW -> en -> zh-TW](../static/extensions/translation/eels-out-tw-en.png)

다음 예제에서 입력 언어 감지가 비교적 효과적입니다:

![Popup, '(My hovercraft is full of eels)/A légpárnás hajóm tele van angolnával/我的氣墊船裡裝滿了鰻魚', zh-TW -> hu -> zh-TW](../static/extensions/translation/eels-out-tw-hu.png)
![Popup, '我的氣墊船裡裝滿了鰻魚/Mi aerodeslizador está lleno de anguilas/My hovercraft is full of eels', zh-TW -> es -> en](../static/extensions/translation/eels-out-tw-es-en.png)
![Popup, 'Il mio hovercraft è pieno di anguille/我的气垫船里装满了鳗鱼/My hovercraft is filled with eels', it -> zh-CN -> en](../static/extensions/translation/eels-out-it-zhCN-en.png)

## 기술 참고 사항

- UTF-8 인코딩, 특수 문자 및 이모지 지원
- 필요할 때 청크로 분할하여 큰 메시지 처리
- 메시지의 서식 및 포함된 이미지 보존
- 중복 API 호출을 피하기 위해 번역 캐시

### AI 입력 언어

`internal_language`는 사용자 메시지가 AI로 보내지기 전에 자동 번역되는 언어를 제어합니다. 기본 설정에서 'en'으로 하드코딩되어 있으며 UI를 통해 변경할 수 없습니다. 따라서 *AI에 대한* 메시지의 번역 대상 언어는 항상 영어입니다. 이전 테스트에서는 영어 메시지를 받을 때 AI 성능이 더 좋았지만 더 많은 LLM이 더 다양한 언어 데이터로 훈련됨에 따라 이것이 변경될 수 있습니다. `settings.json`에서 `internal_language`를 변경하여 확인할 수 있을 것 같습니다.

### 중국어 변형 처리

확장 기능은 간체 중국어와 번체 중국어를 모두 지원하지만 모든 번역 제공업체가 지원하는 것은 아닙니다. UI는 이들을 각각 언어 코드 'zh-CN' 및 'zh-TW'와 함께 'Chinese (Simplified)' 및 'Chinese (Traditional)'로 표시합니다. 번역 제공업체에 대해 다음 언어 코드에 매핑됩니다:

* Libre Translate: 'zh-CN'을 'zh'로, 'zh-TW'를 'zt'로.
* DeepL 및 DeepLX: 두 변형 모두 'ZH'로.
* Bing: 'zh-CN'을 'zh-Hans'로, 'zh-TW'는 그대로.
* 다른 제공업체는 제공된 대로 'zh-CN' 및 'zh-TW'를 사용합니다.

### 텍스트 길이 제한

일부 제공업체는 요청당 문자 제한이 있습니다:

- Yandex: 5000자
- DeepLX: 1500자
- Bing: 1000자
- Google: 5000자

더 긴 텍스트는 번역을 위해 자동으로 청크로 분할됩니다.
