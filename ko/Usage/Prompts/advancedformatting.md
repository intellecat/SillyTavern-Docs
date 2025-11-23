---
order: 100
route: /usage/core-concepts/advancedformatting/
---

# Advanced Formatting

이 섹션에서 제공하는 설정은 주로 Text Completion API에 대한 [프롬프트 구축](index.md) 전략에 대한 더 많은 제어를 허용합니다.

이 패널의 대부분의 설정은 프롬프트 매니저 시스템에 의해 대신 제어되므로 Chat Completions API에는 적용되지 않습니다.

+++ Text Completion APIs
* [System Prompt](#system-prompt)
* [Context Template](#context-template)
* [Tokenizer](#tokenizer)
* [Custom Stopping Strings](#custom-stopping-strings)
+++ Chat Completion APIs
* System Prompt: 해당 없음, [Prompt Manager](prompt-manager.md) 사용
* Context Template: 해당 없음, [Prompt Manager](prompt-manager.md) 사용
* [Tokenizer](#tokenizer)
* [Custom Stopping Strings](#custom-stopping-strings)
+++

## 템플릿 재설정

기본 템플릿을 원래 상태로 복원할 수 있습니다. UI를 통해 또는 관련 데이터 파일을 수동으로 삭제하여 수행할 수 있습니다.

### UI 재설정

1. **<i class="fa-solid fa-font"></i> Advanced Formatting** 메뉴를 엽니다.
2. 재설정하려는 템플릿을 선택합니다.
3. **<i class="fa-solid fa-recycle"></i> Restore current template** 버튼을 클릭합니다.
4. 메시지가 표시되면 작업을 확인합니다.

### 수동 재설정

!!!
[config.yaml](/Administration/config-yaml.md#data-configuration)에서 `skipContentCheck` 설정이 `false`로 설정되어 있는지 확인하세요. 그렇지 않으면 콘텐츠 확인이 트리거되지 않습니다.
!!!

1. 사용자 데이터 디렉토리로 이동합니다(자세한 내용은 [Data paths](/Installation/index.md#data-paths) 참조).
2. 사용자 데이터 디렉토리의 루트에서 `content.log` 파일을 삭제합니다. 이 파일은 사용자를 위해 복사된 기본 파일을 추적합니다.
3. 관련 하위 디렉토리(`context`, `instruct`, `sysprompt` 등)에서 템플릿 JSON 파일을 삭제합니다.
4. SillyTavern 서버를 다시 시작합니다. 애플리케이션이 기본 콘텐츠를 다시 채워 삭제된 기본 템플릿을 복원합니다.

## 백엔드 정의 템플릿

!!! 적용 대상: Text Completion APIs
Chat Completion API에는 다른 프롬프트 빌더를 사용하므로 적용되지 않습니다.
!!!

일부 Text Completion 소스는 모델 작성자가 권장하는 템플릿을 자동으로 선택하는 기능을 제공합니다. 이것은 모델의 `tokenizer_config.json` 파일에 정의된 채팅 템플릿의 해시를 기본 SillyTavern 템플릿 중 하나와 비교하여 작동합니다.

1. **<i class="fa-solid fa-font"></i> Advanced Formatting** 메뉴에서 **<i class="fa-solid fa-bolt"></i> Derive templates** 옵션이 활성화되어야 합니다. 이것은 Context, Instruct 또는 둘 다에 적용될 수 있습니다.
2. 지원되는 백엔드를 Text Completion 소스로 선택해야 합니다. 현재 llama.cpp 및 KoboldCpp만 템플릿 파생을 지원합니다.
3. API에 대한 연결이 설정될 때 모델이 메타데이터를 올바르게 보고해야 합니다. 이것이 작동하지 않으면 백엔드를 최신 버전으로 업데이트해 보세요.
4. 보고된 채팅 템플릿 해시는 [알려진 SillyTavern 템플릿](https://github.com/SillyTavern/SillyTavern/blob/release/public/scripts/chat-templates.js) 중 하나와 일치해야 합니다. 이것은 Llama 3, Gemma 2, Mistral V7 등과 같은 기본 템플릿만 포함합니다.
5. 해시가 일치하면 템플릿 목록에 존재하는 경우(즉, 이름이 변경되거나 삭제되지 않은 경우) 템플릿이 자동으로 선택됩니다.

## System Prompt

!!! 적용 대상: Text Completion APIs
Chat Completion API의 동등한 설정은 [Prompt Manager](prompt-manager.md)를 사용하세요. **Main Prompt**는 Chat Completion API의 System Prompt에 해당합니다.
!!!

시스템 프롬프트는 모델이 따라야 할 일반 지시사항을 정의합니다. 대화의 톤과 컨텍스트를 설정합니다. 예를 들어, AI 어시스턴트, 작문 파트너 또는 가상 캐릭터로 행동하도록 모델에 지시합니다.

시스템 프롬프트는 [Story String](context-template.md#story-string)의 일부이며 일반적으로 모델이 받는 프롬프트의 첫 번째 부분입니다.

시스템 프롬프트에 대해 자세히 알아보려면 [프롬프팅 가이드](index.md#main-prompt-system-prompt)를 참조하세요.

## Context Template

!!! 적용 대상: Text Completion APIs
Chat Completion API의 동등한 설정은 [Prompt Manager](prompt-manager.md)를 사용하세요.
!!!

일반적으로 AI 모델은 특정 방식으로 캐릭터 데이터를 제공하도록 요구합니다. SillyTavern에는 다양한 모델에 대한 사전 제작된 변환 규칙 목록이 포함되어 있지만 원하는 대로 사용자 정의할 수 있습니다.

이 섹션의 옵션은 [Context Template](context-template.md)에서 설명합니다.

## Tokenizer

tokenizer는 텍스트 조각을 토큰이라는 더 작은 단위로 분해하는 도구입니다. 이러한 토큰은 개별 단어 또는 접두사, 접미사 또는 구두점과 같은 단어의 일부일 수도 있습니다. 경험상 하나의 토큰은 일반적으로 텍스트의 3~4자에 해당합니다.

이 섹션의 옵션은 [Tokenizer](tokenizer.md)에서 설명합니다.

## Custom Stopping Strings

중지 문자열의 JSON 직렬화된 배열을 허용합니다. 예: `["\n", "\nUser:", "\nChar:"]`. 형식에 대해 확실하지 않은 경우 [온라인 JSON 검증기](https://jsonlint.com/)를 사용하세요. 모델 출력이 중지 문자열 중 하나로 **끝나면** 출력에서 제거됩니다.

지원되는 API:

1. KoboldAI Classic(버전 1.2.2 이상) 또는 KoboldCpp
2. AI Horde
3. Text Completion APIs: Text Generation WebUI(ooba), Tabby, Aphrodite, Mancer, TogetherAI, Ollama 등.
4. NovelAI
5. OpenAI(최대 4개 문자열) 및 호환 API
6. OpenRouter(Text 및 Chat Completion 둘 다)
7. Claude
8. Google AI Studio
9. MistralAI

## Start Reply With

!!! 참고
기본적으로 Start Reply With 접두사는 결과 메시지에 표시되지 않습니다. "Show reply prefix in chat"을 활성화하여 표시합니다.
!!!

### Text Completion APIs

프롬프트의 마지막 줄을 미리 채워 모델이 해당 지점에서 계속하도록 강제합니다. 이것은 정의된 접두사로 [Model Reasoning](/Usage/Prompts/reasoning.md)을 향해 넛지하는 것과 같은 콘텐츠를 강제하는 데 유용합니다:

```txt
<think>
Sure!
```

### Chat Completion APIs

프롬프트 끝에 assistant 역할 메시지를 추가합니다. 일부 백엔드 모델의 경우 이것은 모델 응답을 미리 채우는 것과 동등하지만 일부는 전혀 지원하지 않으며 검증 오류로 실패합니다. 확실하지 않으면 이 필드를 비워 두세요.
