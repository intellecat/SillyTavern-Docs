---
order: 10
route: /ko/usage/api-connections/openrouter/
label: OpenRouter
title: OpenRouter
---

# OpenRouter

!!!info
OpenRouter는 Text Completion 및 Chat Completion 소스로 모두 사용할 수 있습니다. 모든 모델은 두 API를 통해 사용할 수 있지만 선택한 API 유형에 따라 기능이 다를 수 있습니다. 예를 들어 이미지 인라인 및 도구 호출은 Chat Completion API에서만 사용할 수 있습니다.
!!!

수십 개의 API 서비스에 가입하고 싶지 않지만 최신 모델에 모두 액세스하고 싶으신가요? OpenRouter를 사용하세요.

OpenRouter는 단일 엔드포인트를 사용하여 DeepSeek, Claude 및 Gemini와 같은 모델에 액세스할 수 있도록 하여 공유 크레딧 풀과 함께 하나의 서비스에서 모두 사용할 수 있습니다.

무료 평가판(약 $1)과 그 이후 유료 액세스가 있습니다. 구독이나 월별 청구서가 없습니다. 실제로 사용한 만큼만 지불합니다. 일부 모델은 제한된 일일 요청 수로 무료로 액세스할 수 있습니다.

!!!tip
관대한 일일 한도로 무료 모델에 영구 액세스하려면 **한 번** 최소 $10의 크레딧을 구매해야 합니다.

자세한 내용은 [OpenRouter FAQ 페이지](https://openrouter.ai/docs/faq)를 참조하세요.
!!!

- OpenRouter 계정 만들기: [openrouter.ai](https://openrouter.ai/)
- [OpenRouter 모델 목록](https://openrouter.ai/models?order=pricing-low-to-high)

![OpenRouter-ConnectionPanel](/static/openrouter-connection.png)

위에서 아래로(위 이미지 참조):

1. 'Chat Completion' API를 선택합니다.
2. OpenRouter를 소스로 선택합니다.
3. "Authorize"를 클릭하여 OAuth 흐름을 사용하여 키를 받습니다. 또는 [여기](https://openrouter.ai/keys)에서 API 키를 생성하고 상자에 붙여넣습니다.
4. "Connect"를 클릭하고 모델을 선택합니다.
5. (선택 사항) "Test Message" 버튼을 사용하여 연결을 확인합니다.
