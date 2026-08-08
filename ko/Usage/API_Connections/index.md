---
order: 150
icon: repo-forked
expanded: false
route: /ko/usage/api-connections/
label: API 연결
title: API 연결
---

# API 연결

SillyTavern은 다양한 LLM API에 연결할 수 있습니다.
아래는 각각의 장단점 및 사용 사례에 대한 설명입니다.

## ELI5: Chat Completions vs Text Completions

ST의 "API 연결" 페이지로 처음 이동하면 "Chat Completion" 및 "Text Completion"과 같은 명명법을 사용하는 옵션 중에서 선택하는 드롭다운 옵션이 표시됩니다. 이것이 무엇을 의미하는지 이해하는 것이 도움이 됩니다.

그것이 아닌 것: "Text Completion"을 로컬 모델로, "Chat Completion"을 클라우드 기반 LLM으로 생각하기 쉽지만 그렇지 않습니다. 예를 들어 "NovelAI" 또는 "Kobold"가 실제로 별도의 모델 유형인 것도 아닙니다. ST의 API 드롭다운에서 별도의 옵션이지만 말입니다. 적절한 백엔드로 모델을 다른 API 구조로 강제할 수 있지만 이 섹션의 요점은 아닙니다.

ST를 사용하여 메시지를 보내면 채팅, 캐릭터 설명 및 lorebooks나 작성자 노트와 같은 기타 프롬프트가 AI로 보낼 단일 "프롬프트"로 구성됩니다. 사용 중인 모델의 API "유형"은 이 프롬프트가 정확히 어떻게 구성될지 결정합니다(ST가 백그라운드에서 자동으로 처리하는 것 - ST 터미널을 열고 AI로 전송되는 프롬프트가 정확히 어떻게 보이는지 확인할 수 있습니다).

### Chat Completions

Chat Completion 모델은 이름에서 알 수 있듯이 프롬프트를 사용자(귀하)와 어시스턴트(AI) 또는 시스템(중립) 간의 일련의 메시지로 구조화합니다. Chat Completion용으로 훈련된 모델은 마지막 메시지에 "응답"하는 AI와 함께 "채팅" 느낌을 만드는 데 도움이 됩니다. ChatGPT 웹사이트를 사용할 때 백그라운드에서 Chat Completions API를 다루고 있습니다.

### Text Completions (일명 "Completions")

반면에 Text Completion은 프롬프트를 하나의 긴 문자열로 변환하고 모델은 이를 계속하려고 시도합니다(말 그대로 모든 텍스트, 수백 개의 메시지, 모든 형식, 줄 바꿈 등을 하나의 매우 긴 문장으로 짜낸 것을 상상하세요).

ST의 메시지가 YourPersona:와 Character: 사이의 일련의 메시지로 형식화되면 Text Completion 모델은 이 패턴을 계속하려고 시도하고 ST는 이를 새 채팅 메시지로 렌더링하지만 실제로 모델은 텍스트를 계속하려고 시도할 뿐입니다. "The Sun rises in the"라는 입력을 제공하면 text completion 모델은 "East"로 메시지를 완성할 가능성이 높습니다.

대부분의 Text Completion 모델에는 Chat Completion 모델처럼 메시지와 지시에 "응답"하는 데 도움이 되는 권장 "Instruct Template"가 있습니다(일반적으로 모델의 문서 또는 다운로드 페이지에 언급됨). ST는 일반적으로 "고급 포맷팅" 페이지에서 선택할 수 있는 대부분의(전부는 아니더라도) Instruct Templates를 가지고 있습니다.

## 로컬 API

- 이러한 LLM API는 PC에서 실행할 수 있습니다.
- 무료로 사용할 수 있으며 콘텐츠 필터가 없습니다.
- 설치 프로세스가 복잡할 수 있습니다(**SillyTavern 개발 팀은 이에 대한 지원을 제공하지 않습니다**).
- [HuggingFace](https://huggingface.co/models?other=LLM)에서 LLM 모델을 별도로 다운로드해야 하며 각각 5-50GB가 될 수 있습니다.
- 대부분의 모델은 클라우드 LLM API만큼 강력하지 않습니다.

### KoboldCpp

- CPU 오프로딩(낮은 VRAM 사용자에게 유용) 및 스트리밍이 있는 사용하기 쉬운 API
- Windows, Mac 및 Linux에서 단일 바이너리 파일에서 실행
- GGUF 모델 지원
- AutoGPTQ 및 Exllama/v2와 같은 GPU 전용 로더보다 느립니다
- [GitHub](https://github.com/LostRuins/koboldcpp), [설정 지침](/Usage/API_Connections/koboldcpp.md)

### llama.cpp

- KoboldCpp와 Ollama가 포크된 원본 소스
- 미리 컴파일된 바이너리와 소스에서 컴파일하는 옵션을 제공합니다
- GGUF 모델 지원
- llama-server용 경량 CLI 인터페이스
- [GitHub](https://github.com/ggml-org/llama.cpp)

### Ollama

- 모든 llama.cpp 기반 API 중 가장 설정 및 사용이 쉽습니다
- 원클릭 다운로드가 가능한 멋진 [카탈로그](https://ollama.com/library) 모델
- Ollama 자체 형식으로 래핑된 GGUF 모델 지원
- [GitHub](https://github.com/ollama/ollama), [웹사이트](https://ollama.com/)

### Oobabooga TextGeneration WebUI

- 스트리밍이 있는 올인원 Gradio UI
- 양자화된(AWQ, Exl2, GGML, GGUF, GPTQ) 및 FP16 모델에 대한 가장 광범위한 지원
- 원클릭 설치 프로그램 사용 가능
- 정기적인 업데이트는 때때로 SillyTavern과의 호환성을 손상시킬 수 있습니다
- [GitHub](https://github.com/oobabooga/text-generation-webui#one-click-installers)

**SillyTavern을 Ooba의 새 OpenAI API에 연결하는 올바른 방법:**

1. Oobabooga의 TextGen의 최신 업데이트를 사용하고 있는지 확인하세요(2023년 11월 14일 기준).
2. CMD_FLAGS.txt 파일을 편집하고 거기에 `--api` 플래그를 추가합니다. 그런 다음 Ooba의 서버를 다시 시작합니다.
3. 'Legacy API' 상자를 선택하지 않고 ST를 `http://localhost:5000/`(기본값)에 연결합니다. Ooba의 콘솔이 제공하는 URL에서 `/v1` 접미사를 제거할 수 있습니다.

*`--api-port 5001` 플래그로 API 호스팅 포트를 변경할 수 있습니다. 여기서 5001은 사용자 정의 포트입니다.*

### TabbyAPI

- 스트리밍이 있는 경량 [Exllamav2](https://github.com/turboderp/exllamav2) 기반 API
- Exl2, GPTQ 및 FP16 모델 지원
- [공식 확장 프로그램](https://github.com/theroyallab/ST-tabbyAPI-loader)을 사용하면 SillyTavern에서 직접 모델 로드/언로드 가능
- 낮은 VRAM 사용자에게는 권장하지 않습니다(CPU 오프로딩 없음)
- [GitHub](https://github.com/theroyallab/tabbyAPI), [설정 지침](/Usage/API_Connections/tabbyapi.md)

### KoboldAI Classic (더 이상 사용되지 않음, 중단됨)

- PC에서 실행, 100% 비공개, 다양한 모델 사용 가능
- AI의 생성 설정을 가장 직접적으로 제어
- GPU에 많은 양의 VRAM 필요(모델에 따라 6-24GB)
- 2k 컨텍스트로 제한된 모델
- 스트리밍 없음
- 인기 있는 KoboldAI 버전:
  - [Henky's United](https://github.com/henk717/KoboldAI)
  - [0cc4m's 4bit-supporting United](https://github.com/0cc4m/KoboldAI)

## 클라우드 LLM API

- 이러한 LLM API는 클라우드 서비스로 실행되며 PC에 리소스가 필요하지 않습니다
- 대부분의 로컬 LLM보다 더 강력하거나 똑똑합니다
- 그러나 모두 다양한 정도의 콘텐츠 필터링이 있으며 대부분 지불이 필요합니다

### AI Horde

- SillyTavern은 추가 설정 없이 즉시 이 API에 액세스할 수 있습니다
- 개별 자원봉사자의 GPU(Horde Workers)를 사용하여 채팅 입력에 대한 응답을 처리합니다
- 생성 대기 시간, AI 설정 및 사용 가능한 모델 측면에서 Worker의 재량에 따릅니다
- [웹사이트](https://aihorde.net/), [설정 지침](./horde.md)

### OpenAI (ChatGPT)

- 설정 및 API 키 획득이 쉽습니다
- 크레딧에 대한 선불이 필요하며 프롬프트당 요금이 부과됩니다
- 매우 논리적입니다. 창의적인 스타일은 반복적이고 예측 가능할 수 있습니다
- 대부분의 최신 모델(gpt-4-turbo, gpt-4o)은 다중 모드를 지원합니다
- [웹사이트](https://platform.openai.com/), [설정 지침](/Usage/API_Connections/openai.md#openai)

### Claude (Anthropic)

- AI 채팅이 창의적이고 독특한 글쓰기 스타일을 갖기를 원하는 사용자에게 권장됩니다
- 크레딧에 대한 선불이 필요하며 프롬프트당 요금이 부과됩니다
- 최신 모델(Claude 3)은 다중 모드를 지원합니다
- 응답 조정을 위한 특정 프롬프트 스타일 및 [prefills](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/prefill-claudes-response) 활용 필요
- [웹사이트](https://console.anthropic.com/), [설정 지침](/Usage/API_Connections/openai.md#claude)

### Google AI Studio 및 Vertex AI

- 속도 제한이 있는 무료 계층(Gemini Flash), 청구 정보가 필요할 수 있습니다
- [AI Studio](https://aistudio.google.com/)는 일반적으로 최신 모델과 기능을 제공합니다
- [Vertex AI](https://console.cloud.google.com/vertex-ai/studio)는 설정하기가 더 까다롭지만 더 안정적입니다
- [설정 지침](/Usage/API_Connections/google.md)

### Mistral (Mistral AI)

- 다양한 크기와 사용 사례의 효율적인 모델. [플랫폼](https://console.mistral.ai/api-keys/)에서 계정과 API 키를 만들 수 있습니다.
- 일반 사용의 경우 32k에서 128k 컨텍스트 크기, 코딩의 경우 32k에서 256k 컨텍스트 크기.
- 속도 제한이 있는 무료 계층.
- 합리적인 조정, Mistral의 주요 원칙은 중립적이고 사용자에게 권한을 부여하는 것입니다. 자세한 정보는 [여기](https://mistral.ai/terms/)를 참조하세요.
- [웹사이트](https://console.mistral.ai/), [설정 지침](/Usage/API_Connections/openai.md#mistral-ai)

### OpenRouter

- 시장의 모든 주요 LLM에 액세스할 수 있는 통합 API를 제공합니다
- 토큰당 지불 크레딧 시스템과 일일 요청이 제한된 무료 모델
- LLM 공급업체에서 요구하지 않는 한 강제 조정 없음
- [웹사이트](https://openrouter.ai), [설정 지침](/Usage/API_Connections/OpenRouter.md)

### DeepSeek

- 매우 인기 있는 DeepSeek V3 (`deepseek-chat`) 및 DeepSeek R1 (`deepseek-reasoner`) 모델의 최신 버전에 대한 액세스를 제공합니다
- 크레딧에 대한 지불이 필요합니다(최소 $2). 그러나 모델은 품질 대비 상당히 저렴합니다
- API에 대한 조정 없음. 그러나 모델이 특정 프롬프트를 거부할 수 있습니다
- [웹사이트](https://platform.deepseek.com/), [설정 지침](/Usage/API_Connections/openai.md#deepseek)

### AI21

- Jamba Family 오픈 모델에 대한 액세스를 제공합니다
- 무료 평가판($10 3개월), 그 다음 토큰당 월 요금 지불 필요
- [웹사이트](https://ai21.com/), [설정 지침](/Usage/API_Connections/openai.md#ai21)

### Cohere

- Cohere의 최신 모델(command-r, command-a, c4ai-aya 등)에 대한 액세스를 제공합니다
- 일상적인 사용에 충분한 속도 제한이 있는 무료 계층(Trial Keys)
- [웹사이트](https://cohere.com/), [설정 지침](/Usage/API_Connections/openai.md#cohere)

### Perplexity

- API를 통해 고유한 Perplexity Sonar 온라인 지원 모델에 대한 액세스를 제공합니다
- 청구가 구성되어 있고 크레딧을 구매해야 합니다
- [웹사이트](https://perplexity.ai/), [설정 지침](/Usage/API_Connections/openai.md#perplexity)

### Mancer AI

- 다양한 패밀리의 제약이 없는 모델을 호스팅하는 서비스
- 다양한 모델에서 토큰에 대해 지불하기 위해 '크레딧'을 사용합니다
- 기본적으로 프롬프트를 로그하지 않지만 활성화하여 토큰에 대한 크레딧 할인을 받을 수 있습니다.
- `Oobabooga TextGeneration WebUI`와 유사한 API를 사용합니다. 자세한 내용은 [Mancer 문서](https://mancer.tech/docs/clients/#sampling-parameters)를 참조하세요.
- [웹사이트](https://mancer.tech/), [설정 지침](/Usage/API_Connections/mancer.md)

### DreamGen

- 조종 가능한 창의적 글쓰기를 위해 조정된 무검열 모델
- 무료 월간 크레딧 및 유료 구독
- 7B에서 70B까지의 모델
- [설정 지침](DreamGen.md)

### Pollinations

- 설정이 필요 없으며 즉시 사용 가능
- 다양한 모델에 무료로 액세스 제공
- 출력에는 때때로 타사 서비스에 대한 링크가 있는 광고가 포함될 수 있습니다

### NovelAI

- 콘텐츠 필터 없음, 최신 모델은 Llama 3 기반
- 유료 구독 필요, 계층에 따라 최대 컨텍스트 길이가 결정됩니다
- [웹사이트](https://novelai.net/), [설정 지침](/Usage/API_Connections/novelai.md)

### Electron Hub

- 텍스트 및 이미지 생성을 위해 여러 공급업체(OpenAI, Anthropic, DeepSeek 등)의 모델을 잠금 해제하는 하나의 API 키
- 매일 $0.25의 무료 크레딧, 유료 플랜 이용 가능
- [웹사이트](https://www.electronhub.ai/), [설정 지침](/Usage/API_Connections/openai.md#electron-hub)

### AI/ML API

- Claude, GPT-4o, Gemini, LLaMA 3, Mistral 등을 포함한 300개 이상의 모델을 위한 통합 API
- 속도 제한, 구독 계획 및 종량제 옵션이 있는 무료 계층
- [웹사이트](https://aimlapi.com), [문서](https://docs.aimlapi.com), [모델](https://aimlapi.com/models)
