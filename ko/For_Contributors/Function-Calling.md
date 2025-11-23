---
order: -10
icon: code-review
route: /for-contributors/function-calling/
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
4. [AccuWeather](https://github.com/SillyTavern/Extension-AccuWeather) - AccuWeather에서 날씨 정보 가져오기.
5. [D&D 주사위](https://github.com/SillyTavern/Extension-Dice) - D&D 게임용 주사위 굴리기.

## 전제 조건 및 제한 사항

1. 이 기능은 특정 Chat Completion 소스에서만 사용할 수 있습니다: OpenAI, Claude, MistralAI, Groq, Cohere, OpenRouter, AI21, Google AI Studio, Google Vertex AI, DeepSeek, AI/ML API 및 Custom API 소스.
2. Text Completion API는 함수 호출을 지원하지 않지만, Ollama 및 TabbyAPI와 같은 일부 로컬 호스팅 백엔드는 Chat Completion 아래의 Custom OpenAI 호환 모드에서 실행될 수 있습니다.
3. 함수 호출에 대한 지원은 먼저 사용자가 명시적으로 허용해야 합니다. 이는 AI 응답 구성 패널에서 "함수 호출 활성화" 옵션을 활성화하여 수행됩니다.
4. LLM이 함수 호출을 수행한다는 보장은 없습니다. 대부분은 프롬프트를 통한 명시적인 "활성화"가 필요합니다(예: 사용자가 "주사위 굴리기", "날씨 가져오기" 등을 요청).
5. 모든 프롬프트가 도구 호출을 트리거할 수 있는 것은 아닙니다. 계속, 가장, 백그라운드('quiet') 프롬프트는 도구 호출을 트리거할 수 없습니다. 여전히 응답에서 과거의 성공적인 도구 호출을 사용할 수 있습니다.

## 함수 도구를 만드는 방법

### 기능이 지원되는지 확인

함수 도구 호출 기능이 지원되는지 확인하려면 `SillyTavern.getContext()` 객체에서 `isToolCallingSupported`를 호출할 수 있습니다. 이것은 현재 API가 함수 도구 호출을 지원하고 설정에서 활성화되어 있는지 확인합니다. 다음은 기능이 지원되는지 확인하는 방법의 예입니다:

```ts
if (SillyTavern.getContext().isToolCallingSupported()) {
    console.log("Function tool calling is supported");
} else {
    console.log("Function tool calling is not supported");
}
```

### 함수 등록

함수 도구를 등록하려면 `SillyTavern.getContext()` 객체에서 `registerFunctionTool` 함수를 호출하고 필요한 매개변수를 전달해야 합니다. 다음은 함수 도구를 등록하는 방법의 예입니다:

```ts
SillyTavern.getContext().registerFunctionTool({
    // 함수 도구의 내부 이름. 고유해야 합니다.
    name: "myFunction",
    // 함수 도구의 표시 이름. UI에 표시됩니다. (선택 사항)
    displayName: "My Function",
    // 함수 도구의 설명. 함수가 수행하는 작업과 사용 시기를 설명해야 합니다.
    description: "My function description. Use when you need to do something.",
    // 함수 도구의 매개변수에 대한 JSON 스키마. 참조: https://json-schema.org/
    parameters: {
        $schema: 'http://json-schema.org/draft-04/schema#',
        type: 'object',
        properties: {
            param1: {
                type: 'string',
                description: 'Parameter 1 description',
            },
            param2: {
                type: 'string',
                description: 'Parameter 2 description',
            },
        },
        required: [
            'param1', 'param2',
        ],
    },
    // 도구가 트리거될 때 호출할 함수. 비동기 가능.
    // 결과가 문자열이 아니면 JSON 문자열화됩니다.
    action: async ({ param1, param2 }) => {
        // 여기에 함수 코드 작성
        console.log(`Function called with parameters: ${param1}, ${param2}`);
        return "Function result";
    },
    // 함수가 호출될 때 표시되는 토스트 메시지를 형식화하는 선택적 함수.
    // 빈 문자열이 반환되면 토스트 메시지가 표시되지 않습니다.
    formatMessage: ({ param1, param2 }) => {
        return `Function is called with: ${param1} and ${param2}`;
    },
    // 현재 프롬프트에 대해 도구를 등록해야 하는지 여부를 나타내는 부울 값을 반환하는 선택적 함수.
    // shouldRegister 함수가 제공되지 않으면 모든 프롬프트에 대해 도구가 등록됩니다.
    shouldRegister: () => {
        return true;
    },
    // 선택적 플래그. true로 설정하면 함수 호출이 수행되지만 결과는 보이는 채팅 기록에 기록되지 않습니다.
    stealth: false,
});
```

### 함수 등록 해제

함수 도구를 비활성화하려면 `SillyTavern.getContext()` 객체에서 `unregisterFunctionTool` 함수를 호출하고 비활성화할 함수 도구의 이름을 전달해야 합니다. 다음은 함수 도구를 등록 해제하는 방법의 예입니다:

```ts
SillyTavern.getContext().unregisterFunctionTool("myFunction");
```

## 팁과 요령

1. 성공적인 도구 호출은 보이는 기록의 일부로 저장되며 채팅 UI에 표시되므로 실제 매개변수와 결과를 검사할 수 있습니다. 이것이 바람직하지 않은 경우 함수 도구를 등록할 때 `stealth: true` 플래그를 설정하세요.
2. 채팅 기록에서 도구 호출을 보고 싶지 않다면. 사용자 정의 CSS로 스타일을 지정하거나 숨기려면 `.mes` 요소의 `toolCall` 클래스를 대상으로 지정합니다. 즉, `.mes.toolCall { display: none; }` 또는 `.mes.toolCall { color: #999; }`.
