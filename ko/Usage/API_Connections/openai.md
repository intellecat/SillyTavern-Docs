---
order: 20
route: /ko/usage/api-connections/openai/
label: Chat Completions
title: Chat Completions
---

# Chat Completions

## 소스별 지침

!!!warning **중요!**
대부분의 API 플랫폼에서는 생성 시 한 번만 생성된 API 키를 볼 수 있습니다. 잃어버리면 새 키를 생성해야 합니다. 안전하게 보관하세요!
!!!

### OpenAI

OpenAI의 개발자 플랫폼을 사용하여 gpt-4o, gpt-4.1, o3 등 다양한 OpenAI 모델에 액세스하세요.

**API 키를 받는 방법:**

1. [OpenAI](https://platform.openai.com/)로 이동하여 로그인합니다.
2. "[API 키 보기](https://platform.openai.com/account/api-keys)" 옵션을 사용하여 새 API 키를 만듭니다.

### Claude

Claude는 Anthropic이 개발한 AI 모델 제품군입니다. Anthropic 콘솔을 통해 Claude 모델에 액세스할 수 있습니다.

**API 키를 받는 방법:**

1. [Anthropic Console](https://console.anthropic.com/)로 이동하여 로그인합니다.
2. "[API 키 받기](https://console.anthropic.com/settings/keys)" 섹션을 사용하여 새 API 키를 만듭니다.

### Mistral AI

Mistral AI는 높은 과학적 기준과 개방성에 중점을 둔 오픈 및 독점 모델을 모두 개발하는 팀입니다. API 서비스인 La Plateforme을 통해 로컬로 또는 모델을 실행할 수 있습니다.

**API 키를 받는 방법:**

1. 첫 번째 단계는 [La Plateforme](https://console.mistral.ai/)에서 계정을 만드는 것입니다.
2. 완료되면 [플랜](https://console.mistral.ai/billing/plans)을 선택하고 결제 정보를 설정하거나 무료 계층을 선택할 수 있습니다.
3. 다음으로 [API 키](https://console.mistral.ai/api-keys)를 만들 수 있습니다. 키가 유효해지기까지 몇 분 정도 기다려야 할 수 있습니다!

### DeepSeek

DeepSeek Platform은 API를 통해 최신 DeepSeek 모델에 대한 액세스를 제공합니다. DeepSeek V3 및 DeepSeek R1을 포함한 다양한 모델을 제공합니다.

**API 키를 받는 방법:**

1. [DeepSeek Platform](https://platform.deepseek.com/)에 가입합니다.
2. 가입하고 계정을 충전한 후 "[API keys](https://platform.deepseek.com/api_keys)" 섹션에서 API 키를 만들 수 있습니다.

### AI21

AI21 Labs는 주력 Jamba 시리즈를 포함한 다양한 AI 모델을 제공합니다. AI21 Studio API를 통해 모델에 액세스할 수 있습니다.

**API 키를 받는 방법:**

1. [AI21 Studio](https://studio.ai21.com/)로 이동하여 로그인합니다.
2. "Settings => API Keys" 섹션으로 이동하여 새 API 키를 만듭니다.

### Cohere

Cohere는 텍스트 생성 및 임베딩을 포함한 다양한 작업을 위한 AI 모델 제품군을 제공합니다. Cohere API를 통해 모델에 액세스할 수 있습니다.

**API 키를 받는 방법:**

1. [Cohere](https://cohere.com/)로 이동하여 로그인합니다.
2. 계정 설정의 "[API Keys](https://dashboard.cohere.com/api-keys)" 섹션으로 이동하여 새 API 키를 만듭니다.

### Perplexity

Perplexity AI는 실시간 연구 및 정보 검색을 위한 API를 통해 온라인 지원 Sonar 모델에 대한 액세스를 제공합니다.

공식 시작 가이드: [Perplexity Quickstart](https://docs.perplexity.ai/getting-started/quickstart)

**API 키를 받는 방법:**

1. [Perplexity](https://perplexity.ai/)로 이동하여 로그인합니다.
2. "[API billing](https://www.perplexity.ai/account/api/billing)" 섹션으로 이동하여 API 사용을 위한 크레딧을 구매합니다.
3. 설정의 "[API keys](https://www.perplexity.ai/account/api/keys)" 섹션으로 이동하여 새 API 키를 만듭니다.

### Fireworks AI

Fireworks AI는 최첨단 오픈 소스 언어 모델에 대한 빠르고 비용 효율적인 액세스를 제공하는 고성능 플랫폼입니다. 이 플랫폼은 OpenAI 호환 API와 함께 서버리스 배포를 제공하며 최대 256,000 토큰의 컨텍스트 창을 지원합니다.

**API 키를 받는 방법:**

1. [Fireworks AI](https://fireworks.ai/)로 이동하여 계정을 만들거나 로그인합니다.
2. 계정 설정의 [API Keys 페이지](https://app.fireworks.ai/settings/users/api-keys)로 이동합니다.
3. "Create API key"를 클릭하고 설명적인 이름을 제공합니다(예: "SillyTavern").

## Electron Hub

Electron Hub는 단일 API 키를 통해 여러 공급업체의 모델에 대한 액세스를 제공하는 통합 OpenAI 호환 플랫폼입니다.

**API 키를 받는 방법:**

1. [Electron Hub](https://playground.electronhub.ai/console)에서 계정을 만듭니다.
2. **Console → API Keys** 페이지에서 API 키를 생성합니다.

## 사용자 정의 OpenAI 호환 엔드포인트

!!!warning
발생할 수 있는 문제에 대한 지원을 제공하지 않습니다!
모든 가능한 API 엔드포인트와의 호환성을 보장하지 않습니다!
!!!

!!!
이 기능을 사용하여 TabbyAPI, Oobabooga, Aphrodite 또는 이와 유사한 로컬 엔드포인트를 사용하려는 경우 대신 [내장 호환성](/Usage/API_Connections/index.md)을 확인하는 것이 좋습니다. 사용자 정의 엔드포인트 기능은 주로 OpenAI 호환 API Chat Completion 엔드포인트를 노출하는 다른 서비스 및 프로그램과 함께 사용하기 위한 것입니다.

대부분의 Text Completion API는 OpenAI의 표준이 허용하는 것보다 훨씬 더 큰 사용자 정의 옵션을 지원합니다. Min-P 샘플러와 같은 이러한 더 큰 사용자 정의 옵션은 SillyTavern 사용자가 확인하기에 가치가 있을 수 있으며 생성 품질을 크게 향상시킬 수 있습니다.
!!!

Chat Completions 백엔드에 대한 대체 엔드포인트를 구성할 수 있습니다. 이 사용자 정의 엔드포인트는 일반 OpenAI API 스키마를 지원하는 모든 서버에 연결할 수 있습니다.

호환 가능한 백엔드의 예:

* [LM Studio](https://lmstudio.ai/)
* [LiteLLM](https://www.litellm.ai/)
* [LocalAI](https://localai.io/)

### 연결

이 기능에 액세스하려면:

1. 'Chat Completion' API 유형으로 전환
2. 'Chat Completion Source'에 대해 'Custom (OpenAI-compatible)' 선택

필요한 경우 사용자 정의 엔드포인트 URL 및 API 키를 입력합니다. 예를 들어 TabbyAPI는 인증을 위해 API 키가 필요합니다.

!!!tip
**힌트:** 연결 문제가 발생하면 엔드포인트 URL 끝에 `/v1`을 추가해 보세요. `/chat/completions` 접미사를 추가하지 마세요.
!!!

### 모델 선택

사용자 정의 API가 사용 가능한 모델 목록을 제공하기 위해 `/v1/models` 엔드포인트를 구현하는 경우 드롭다운 목록에서 선택할 수 있습니다. 그렇지 않으면 텍스트 필드를 사용하여 모델 ID를 수동으로 입력합니다.

'Bypass API status check'를 선택하면 SillyTavern이 작동하지 않는 API 엔드포인트에 대해 경고하지 않습니다. API 엔드포인트가 제대로 작동하지만 SillyTavern이 계속 경고를 표시하는 경우 이 옵션을 활성화하세요.

"Test Message"를 클릭하여 모델에 간단한 프롬프트를 전송하여 연결을 확인합니다.

## 프롬프트 후처리

!!!warning
**참고:** "no tools"가 있는 후처리 옵션을 사용할 때 Tool Calling은 지원되지 않습니다!
!!!

일부 엔드포인트는 하나의 시스템 메시지만 필요하거나 엄격하게 교대로 역할이 필요한 것과 같이 들어오는 프롬프트의 형식에 대한 특정 제한을 부과할 수 있습니다.

SillyTavern은 이러한 요구 사항을 충족하는 데 도움이 되는 내장 프롬프트 변환기를 제공합니다(가장 제한적이지 않은 것부터 가장 제한적인 것까지):

1. None - API에서 엄격하게 요구하지 않는 한 명시적인 처리가 적용되지 않습니다
2. Merge consecutive messages from the same role
3. Semi-strict - 역할 병합 및 하나의 선택적 시스템 메시지만 허용
4. Strict - 역할 병합, 하나의 선택적 시스템 메시지만 허용하고 사용자 메시지가 먼저 필요합니다
5. Single user message - 모든 역할의 모든 메시지를 단일 사용자 메시지로 병합

Merge, semi-strict 및 strict는 "with tools" 변형이 선택되지 않는 한 프롬프트에서 모든 도구 호출을 추가로 제거합니다. 이는 도구 호출을 지원하지 않는 API에 유용하며 기존 프롬프트에 도구 호출이 포함되어 있습니다.

덜 제한적인 옵션은 "Custom OpenAI-compatible"이 아닌 SillyTavern에 구현된 더 제한적인 엔드포인트에 영향을 미치지 않습니다. Custom은 유효하지 않은 요청 시 오류가 발생할 수 있습니다.

strict 모드에서 첫 번째 어시스턴트 메시지 앞에 사용자 메시지가 없으면 `config.yaml`의 `promptPlaceholder`가 삽입됩니다. 기본값은 "\[Start a new chat]"입니다.
