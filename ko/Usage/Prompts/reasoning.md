---
order: 40
tags: ['>=1.12.12']
route: /ko/usage/prompts/reasoning/
---

# Reasoning

언어 모델에서 reasoning(모델 사고라고도 함)은 단계별 분석을 통해 인간의 문제 해결을 반영하는 chain-of-thought(CoT) 기술을 의미합니다. SillyTavern은 지원되는 백엔드 전체에서 reasoning 모델의 사용을 더 효율적이고 일관되게 만드는 여러 기능을 제공합니다.

## 일반적인 문제

1. reasoning 모델을 사용할 때 모델의 내부 reasoning 프로세스는 최종 출력에 표시되지 않더라도(예: o3-mini 또는 Gemini Thinking) 응답 토큰 허용량의 일부를 소비합니다. 응답이 불완전하거나 비어 있는 경우 **<i class="fa-solid fa-sliders"></i> AI Response Configuration** 패널에 있는 Max Response Length 설정을 조정해 보세요. reasoning 모델의 경우 표준 대화 모델에 비해 훨씬 더 높은 토큰 제한(1024~4096 토큰 사이)을 사용하는 것이 일반적입니다.

## 구성

!!!
대부분의 reasoning 관련 설정은 **<i class="fa-solid fa-font"></i> Advanced Formatting** 패널의 "Reasoning" 섹션에서 구성할 수 있습니다.
!!!

Reasoning 블록은 접을 수 있는 메시지 섹션으로 채팅에 나타납니다. 수동으로, 백엔드에 의해 자동으로 또는 응답 파싱을 통해 추가할 수 있습니다(아래 참조).

기본적으로 reasoning 블록은 공간을 절약하기 위해 접혀 있습니다. 블록을 클릭하여 확장하고 내용을 봅니다. reasoning 설정에서 **Auto-Expand**를 활성화하여 블록이 자동으로 확장되도록 설정할 수 있습니다.

reasoning 블록이 확장되면 **<i class="fa-solid fa-copy"></i> Copy** 및 **<i class="fa-solid fa-pencil"></i> Edit** 버튼을 사용하여 내용을 복사하거나 편집할 수 있습니다.

일부 모델은 reasoning을 지원하지만 생각을 다시 보내지 않습니다. **Show Hidden** 설정을 토글하여 reasoning 시간이 있는 reasoning 블록을 표시할 수 있습니다.

## Reasoning 추가

### 수동으로

**<i class="fa-solid fa-pencil"></i> Message Edit** 메뉴를 통해 모든 메시지에 reasoning 블록을 추가합니다. 편집하는 동안 **<i class="fa-solid fa-lightbulb"></i>**를 클릭하여 reasoning 섹션을 추가합니다. 타사 확장 프로그램은 채팅에 추가하기 전에 메시지 객체의 `extra.reasoning` 필드에 작성하여 reasoning을 추가할 수도 있습니다.

### 명령으로

`/reasoning-set` STscript 명령을 사용하여 메시지에 reasoning을 추가합니다. 명령은 `at`(메시지 ID, 기본값은 마지막 메시지) 및 reasoning 텍스트를 인수로 사용합니다.

```stscript
/reasoning-set at=0 This is the reasoning for the first message.
```

### 백엔드로

선택한 LLM 백엔드 및 모델이 reasoning 출력을 지원하는 경우 **<i class="fa-solid fa-sliders"></i> AI Response Configuration** 패널에서 "Request model reasoning"을 활성화하면 모델의 사고 프로세스를 포함하는 reasoning 블록이 추가됩니다.

지원되는 소스:

- Claude
- DeepSeek
- Google AI Studio
- Google Vertex AI
- OpenRouter
- xAI (Grok)
- AI/ML API
- Z.AI
- Pollinations
- MistralAI
- Electron Hub
- Chutes
- NanoGPT
- Moonshot

!!!
**대부분의** 소스에서 "Request model reasoning"은 비활성화할 수 없기 때문에 모델이 reasoning을 수행하는지 여부를 결정하지 않습니다. 백엔드와 모델이 reasoning을 명시적으로 비활성화하도록 요청하는 것을 지원하는 경우, 이 설정이 그렇게 합니다. 그렇지 않으면 모델은 항상 reasoning을 수행합니다.
!!!

제공자별 참고 사항:

- Claude 및 Google(2.5 Flash)은 thinking 모드를 토글할 수 있습니다. [Reasoning Effort](#reasoning-effort)를 참조하세요.
- [Z.AI (GLM)](https://docs.z.ai/api-reference/llm/chat-completion#body-one-of-0-thinking) 및 [Moonshot (Kimi)](https://platform.moonshot.ai/docs/guide/use-kimi-k2-thinking-model)는 reasoning을 비활성화할 수 있습니다. 이 설정은 `thinking.type` 매개변수로 매핑됩니다. 이들은 "Reasoning Effort"를 지원하지 않습니다.
- OpenRouter의 경우 "Request model reasoning" 토글이 비활성화되고 최소 reasoning effort가 설정되어 있으면, 이를 지원하는 모델에 대해 thinking이 비활성화로 설정됩니다. 이 동작은 모델에 따라 다르며, 일부 제공자는 이러한 요청을 거부할 수 있습니다.

### 파싱으로

**<i class="fa-solid fa-font"></i> Advanced Formatting** 패널에서 "Auto-Parse"를 활성화하여 모델의 출력에서 reasoning을 자동으로 파싱합니다.

응답에는 구성된 Prefix 및 Suffix 시퀀스로 감싸인 reasoning 섹션이 포함되어야 합니다. 기본적으로 제공되는 시퀀스는 DeepSeek R1 reasoning 형식에 해당합니다. 이 설정은 MiniMax 또는 Perplexity와 같이 파싱되지 않은 reasoning을 반환하는 일부 API 소스에서 활성화해야 합니다.

prefix `<think>` 및 suffix `</think>`를 사용한 예:

```txt
<think>
This is the reasoning.
</think>

This is the main content.
```

## Reasoning으로 프롬프팅

기본적으로 인식된 reasoning 블록 내용은 모델로 다시 전송되지 않습니다. 프롬프트에 reasoning을 포함하려면 **<i class="fa-solid fa-font"></i> Advanced Formatting** 패널에서 "Add to Prompts"를 활성화하세요. Reasoning 콘텐츠는 구성된 Prefix 및 Suffix 시퀀스로 감싸지고 Separator로 메인 컨텍스트와 분리됩니다. Max Additions 숫자 설정은 프롬프트 끝에서 계산하여 포함될 수 있는 reasoning 블록 수를 제어합니다.

!!!
대부분의 모델 제공자는 다중 턴 대화에서 CoT를 모델로 다시 보내는 것을 권장하지 않습니다.
!!!

### Reasoning에서 계속

"Add to Prompts" 토글이 활성화되지 않고도 reasoning을 모델로 다시 보낼 수 있는 특수한 경우는 생성이 계속될 때(예: **<i class="fa-solid fa-bars"></i> Options** 메뉴에서 "Continue"를 누를 때)이지만 계속되는 메시지에 실제 콘텐츠 없이 reasoning만 포함된 경우입니다. 이것은 모델이 불완전한 reasoning을 완료하고 메인 콘텐츠 생성을 시작할 기회를 제공합니다. 프롬프트는 다음과 같이 전송됩니다:

```txt
<think>
Incomplete reasoning...
```

## Regex Scripts

[Regex extension](/extensions/Regex.md)의 정규 표현식 스크립트는 reasoning 블록의 내용에 적용될 수 있습니다. 스크립트 편집기의 "Affects" 섹션에서 "Reasoning"을 선택하여 reasoning 블록을 특별히 대상으로 지정합니다.

다양한 ephemerality 옵션은 다음과 같이 reasoning 블록에 영향을 미칩니다:

1. No ephemerality: reasoning 콘텐츠가 영구적으로 변경됩니다.
2. Run on edit: regex 스크립트는 reasoning 블록이 편집될 때 다시 평가됩니다.
3. Alter chat display: regex는 기본 콘텐츠가 아닌 reasoning 블록의 디스플레이 텍스트에 적용됩니다.
4. Alter outgoing prompts: regex는 모델로 전송되기 전에 reasoning 블록에만 적용됩니다.

## Reasoning Effort

Reasoning Effort는 reasoning에 잠재적으로 사용될 수 있는 토큰 수에 영향을 미치는 **<i class="fa-solid fa-sliders"></i> AI Response Configuration** 패널의 Chat Completion 설정입니다. 각 옵션의 효과는 연결된 소스에 따라 다릅니다. 아래 소스의 경우 Auto는 단순히 관련 매개변수가 요청에 포함되지 않음을 의미합니다.

| Option  | Claude (스트리밍 없으면 ≤ 21333) | OpenAI (keyword)     | OpenRouter (keyword)             | xAI (Grok) (keyword) | Perplexity (keyword) | NanoGPT (keyword) |
| ------- | -------------------------------- | -------------------- | -------------------------------- | -------------------- | -------------------- | ----------------- |
| Models  | Opus 4, Sonnet 4/3.7             | o4-mini, o3\*, o1\*  | 해당 모델                | grok-3-mini          | sonar-deep-research  | 해당 모델 |
| Auto    | 지정되지 않음, **thinking 없음**   | 지정되지 않음        | 지정되지 않음, 효과는 의존    | 지정되지 않음        | 지정되지 않음        | 지정되지 않음     |
| Minimum | 1024 토큰 예산            | "low"                | "low", 또는 max response의 20%    | "low"                | "low"                | "none"            |
| Low     | max response의 15%, 최소 1024    | "low"                | "low", 또는 max response의 20%    | "low"                | "low"                | "minimal"         |
| Medium  | max response의 25%, 최소 1024    | "medium"             | "medium", 또는 max response의 50% | "low"                | "medium"             | "low"             |
| High    | max response의 50%, 최소 1024    | "high"               | "high", 또는 max response의 80%   | "high"               | "high"               | "medium"          |
| Maximum | max response의 95%, 최소 1024    | "high"               | "high", 또는 max response의 80%   | "high"               | "high"               | "high"            |

- adaptive thinking을 지원하지 않는 이전 Claude 모델의 경우 스트리밍이 비활성화되면 예산이 21333으로 제한됩니다. 계산된 예산이 1024보다 적으면 max response가 2048로 변경됩니다.
- Claude는 또한 Opus 4.6+ 모델에 대해 adaptive thinking을 지원하며, [config.yaml](/Administration/config-yaml.md)의 `claude.enableAdaptiveThinking`을 통해 활성화할 수 있습니다(Opus 4.7+에서는 항상 켜져 있음). 활성화되면 Reasoning Effort 설정이 토큰 예산 대신 adaptive thinking 레벨로 매핑됩니다. 이 설정은 해당하는 모델에서 "Verbosity" 설정보다 우선합니다.
- OpenRouter, Pollinations, Perplexity, xAI, Chutes, DeepSeek, AI/ML API, xAI, Electron Hub의 경우 OpenAI 스타일 키워드만 전송됩니다.
- OpenAI의 GPT-5.4 및 GPT-5.5 모델의 경우 "Minimal" reasoning effort는 "none"에 해당하며, 이는 reasoning을 비활성화합니다.
- Chat Completion Custom API 소스로 실행되는 KoboldCpp의 경우 reasoning effort는 "minimal", "low", "medium", "high", "xhigh" 값을 가진 `reasoning_effort` 매개변수로 전송됩니다.
- 다른 Custom(OpenAI 호환) 소스의 경우 공식 OpenAI 소스에서 모델이 이를 지원하는 경우에만 reasoning effort가 전송됩니다.

Google AI Studio 및 Vertex AI는 다음과 같습니다:

| Model          | Auto (dynamic thinking) | Minimum            | Low                          | Medium     | High       | Maximum               |
| -------------- | ----------------------- | ------------------ | ---------------------------- | ---------- | ---------- | --------------------- |
| 2.5 Pro        | thinkingBudget = -1     | 128                | max response의 15%, 최소 128 | max의 25% | max의 50% | max 또는 32768 중 낮은 값 |
| 2.5 Flash      | thinkingBudget = -1     | 0, **thinking 없음** | max response의 15%          | max의 25% | max의 50% | max 또는 24576 중 낮은 값 |
| 2.5 Flash Lite | thinkingBudget = -1     | 0, **thinking 없음** | max response의 15%, 최소 512 | max의 25% | max의 50% | max 또는 24576 중 낮은 값 |
| 3.0/3.1 Pro    | thinkingLevel = null    | "low"              | "low"                        | "low"      | "high"     | "high"                |
| 3.0/3.1 Flash  | thinkingLevel = null    | "minimal"          | "low"                        | "medium"   | "high"     | "high"                |

- Gemini 2.5 Pro 및 2.5 Flash/Lite의 경우 스트리밍 설정에 관계없이 예산이 각각 32768 또는 24576 토큰으로 제한됩니다.
