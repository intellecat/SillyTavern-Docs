---
order: -10
icon: code-review
route: /ko/for-contributors/function-calling/
label: 함수 호출
---

# 함수 호출

함수 호출을 사용하면 LLM이 사용하는 구조화된 데이터를 통해 익스텐션의 동적 기능을 추가할 수 있으며, 이를 사용하여 익스텐션의 특정 기능을 트리거할 수 있습니다.

## 사용 사례 예시

1. 추가 정보를 위해 외부 API 쿼리(뉴스, 날씨, 웹 검색 등).
2. 사용자 입력을 기반으로 계산 또는 변환 수행.
3. RAG 및 데이터베이스 쿼리를 포함한 중요한 기억이나 사실 저장 및 회상.
4. 대화에 진정한 무작위성 도입(주사위 굴리기, 동전 던지기 등).

## 함수 호출을 사용하는 공식적으로 지원되는 익스텐션

1. [이미지 생성](/extensions/Stable-Diffusion.md) (내장) - 사용자 프롬프트를 기반으로 이미지 생성.
2. [웹 검색](/extensions/WebSearch.md) - 쿼리에 대한 웹 검색 트리거.
3. [RSS](https://github.com/SillyTavern/Extension-RSS/) - RSS 피드에서 최신 뉴스 가져오기.
4. [Weather](https://github.com/SillyTavern/Extension-Weather) - 날씨 API에서 날씨 정보 가져오기.
5. [D&D 주사위](https://github.com/SillyTavern/Extension-Dice) - D&D 게임용 주사위 굴리기.

## 전제 조건 및 제한 사항

1. 이 기능은 Chat Completion API에서만 사용할 수 있으며, 다음 소스가 이를 지원합니다: Custom(OpenAI 호환), AI/ML API, AI21, Azure OpenAI, Chutes, Claude, Cloudflare Workers AI, Cohere, DeepSeek, Electron Hub, Fireworks, Google AI Studio, Google Vertex AI, Groq, MiniMax, MistralAI, Moonshot (Kimi), NanoGPT, OpenAI, OpenRouter, Pollinations, SiliconFlow, xAI (Grok), Z.AI (GLM).
2. Text Completion API는 함수 호출을 지원하지 않지만, Ollama 및 TabbyAPI와 같은 일부 로컬 호스팅 백엔드는 Chat Completion 아래의 Custom OpenAI 호환 모드에서 실행될 수 있습니다.
3. 함수 호출에 대한 지원은 먼저 사용자가 명시적으로 허용해야 합니다. 이는 AI 응답 구성 패널에서 "함수 호출 활성화" 옵션을 활성화하여 수행됩니다.
4. LLM이 함수 호출을 수행한다는 보장은 없습니다. 대부분은 프롬프트를 통한 명시적인 "활성화"가 필요합니다(예: 사용자가 "주사위 굴리기", "날씨 가져오기" 등을 요청).
5. 모든 프롬프트가 도구 호출을 트리거할 수 있는 것은 아닙니다. 계속, 가장, 백그라운드('quiet') 프롬프트는 도구 호출을 트리거할 수 없습니다. 여전히 응답에서 과거의 성공적인 도구 호출을 사용할 수 있습니다.
6. API 소스가 함수 호출을 지원하더라도 특정 모델은 함수 호출을 지원하지 않을 수 있습니다. 어떤 모델이 함수 호출을 지원하는지 자세한 내용은 API 제공업체의 문서를 참조하십시오.

## 도구 호출 재귀 제한

도구 호출의 무한 루프를 방지하기 위해 재귀 제한이 있습니다(기본값: 5회). LLM이 도구를 계속 반복해서 호출하면 이 제한에 도달한 후 실행이 중단됩니다. "함수 호출 활성화" 옵션 옆의 AI 응답 구성 설정에서 이 제한을 조정할 수 있습니다.

## 인터리브 사고(Interleaved Thinking)

[추론 모델](../Usage/Prompts/reasoning.md)로 도구 호출을 사용할 때, 도구 호출 요청과 함께 반환된 추론을 활용하여 인터리브 사고 컨텍스트를 유지할 수 있습니다. 일부 모델의 경우 도구 호출 간의 일관성을 보장하고 중요한 세부 정보를 유지하기 위해 이것이 필요합니다.

AI 응답 구성에서 다음 설정 중 하나를 선택하십시오:

- 비활성화됨: 도구 호출 요청에 추론 컨텍스트가 포함되지 않습니다. 호환성이 가장 높으며 기본 설정입니다.
- 마지막 사용자 메시지 이후: 최신 사용자 메시지 이후에 발생한 모든 도구 턴에 대한 추론을 포함합니다.
- 활성 도구 체인: 현재 해결되지 않은 도구 루프 내에 있는 동안에만 추론을 포함합니다. 일반적인 어시스턴트 응답이 생성되면 이전 도구 체인 추론은 더 이상 다시 전송되지 않습니다.

## 함수 도구를 만드는 방법

### 기능이 지원되는지 확인

`SillyTavern.getContext()`의 `isToolCallingSupported()`를 사용하여 현재 API가 함수 도구 호출을 지원하고 설정에서 활성화되어 있는지 확인합니다:

```js
const { isToolCallingSupported } = SillyTavern.getContext();

if (isToolCallingSupported()) {
    console.log('Function tool calling is supported');
}
```

특정 생성 유형에 대해 도구 호출을 수행할 수 있는지도 확인할 수 있습니다. 계속, 가장, 백그라운드('quiet') 프롬프트는 도구 호출을 트리거할 수 없습니다:

```js
const { canPerformToolCalls } = SillyTavern.getContext();

if (canPerformToolCalls('normal')) {
    console.log('Can perform tool calls for this generation');
}
```

### 함수 도구 등록

`SillyTavern.getContext()`의 `registerFunctionTool()`을 사용하여 도구를 등록합니다. 도구 정의는 매개변수에 대해 [JSON Schema](https://json-schema.org/) 형식을 따릅니다:

```js
const { registerFunctionTool } = SillyTavern.getContext();

registerFunctionTool({
    name: 'get_weather',
    displayName: 'Get Weather',
    description: 'Get the current weather for a given location',
    parameters: {
        $schema: 'http://json-schema.org/draft-04/schema#',
        type: 'object',
        properties: {
            location: {
                type: 'string',
                description: 'The city name, e.g. "London"',
            },
            unit: {
                type: 'string',
                enum: ['celsius', 'fahrenheit'],
                description: 'Temperature unit',
            },
        },
        required: ['location'],
    },
    action: async ({ location, unit }) => {
        // Perform your logic here (API calls, computations, etc.)
        const data = await fetchWeatherData(location, unit);
        return JSON.stringify(data);
    },
    formatMessage: ({ location }) => `Checking weather for ${location}...`,
    shouldRegister: () => isWeatherFeatureEnabled(),
    stealth: false,
});
```

### 등록 필드

| 필드 | 필수 여부 | 설명 |
|-------|----------|-------------|
| `name` | 예 | 도구의 고유 식별자 |
| `displayName` | 아니오 | UI에 표시되는 사용자 친화적인 표시 이름 |
| `description` | 예 | 도구가 하는 일과 사용 시기를 설명하기 위해 LLM으로 전송되는 설명 |
| `parameters` | 예 | 도구의 입력 매개변수를 정의하는 JSON Schema |
| `action` | 예 | LLM이 도구를 호출할 때 호출되는 함수. 파싱된 매개변수를 객체로 받습니다. 비동기일 수 있습니다. 문자열 결과를 반환해야 합니다(문자열이 아닌 값은 JSON 문자열화됩니다). |
| `formatMessage` | 아니오 | 도구가 실행되는 동안 토스트로 표시되는 문자열을 반환하는 함수. 빈 문자열을 반환하면 토스트가 표시되지 않습니다. |
| `shouldRegister` | 아니오 | 부울 값을 반환하는 함수. `false`이면 도구가 현재 요청에서 제외됩니다. 제공되지 않으면 도구는 모든 프롬프트에 대해 등록됩니다. |
| `stealth` | 아니오 | `true`이면 도구 호출 결과가 보이는 채팅 기록에 기록되지 않으며 후속 생성이 트리거되지 않습니다. |

### 함수 도구 등록 해제

함수 도구를 비활성화하려면 도구의 이름과 함께 `unregisterFunctionTool()`을 호출하십시오:

```js
const { unregisterFunctionTool } = SillyTavern.getContext();

unregisterFunctionTool('get_weather');
```

## 팁과 요령

1. 성공적인 도구 호출은 보이는 채팅 기록의 일부로 저장되며 채팅 UI에 표시되므로 실제 매개변수와 결과를 검사할 수 있습니다. 이것이 바람직하지 않은 경우 도구를 등록할 때 `stealth: true`를 설정하세요.
2. 사용자 정의 CSS로 도구 호출 메시지를 스타일링하거나 숨기려면 `.mes` 요소의 `toolCall` 클래스를 대상으로 지정합니다. 예: `.mes.toolCall { display: none; }` 또는 `.mes.toolCall { opacity: 0.5; }`.
3. 도구에 대한 명확하고 구체적인 설명을 작성하십시오. LLM은 이를 사용하여 언제, 어떻게 도구를 호출할지 결정합니다. 도구를 사용해야 하는 시기에 대한 안내를 포함하십시오.
4. 매개변수 스키마를 간단하고 잘 문서화된 상태로 유지하십시오. 각 속성의 `description`은 LLM이 올바른 값을 입력하는 데 도움이 됩니다.
5. 도구 호출에는 재귀 제한이 있습니다(기본값: 5회). LLM이 도구를 계속 반복해서 호출하면 이 제한에 도달한 후 실행이 중단됩니다.
