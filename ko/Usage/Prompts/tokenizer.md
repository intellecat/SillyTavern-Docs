---
order: 70
route: /ko/usage/prompts/tokenizer/
---

# Tokenizer

tokenizer는 텍스트 조각을 토큰이라는 더 작은 단위로 분해하는 도구입니다. 이러한 토큰은 개별 단어 또는 접두사, 접미사 또는 구두점과 같은 단어의 일부일 수도 있습니다. 경험상 하나의 토큰은 일반적으로 텍스트의 3~4자에 해당합니다.

SillyTavern은 사용되는 API 제공자에 따라 다음 규칙을 사용하여 tokenizer를 일치시키려고 시도하는 "Best match" 옵션을 제공합니다.

Text Completion APIs **(재정의 가능)**:

1. NovelAI Clio: NerdStash tokenizer.
2. NovelAI Kayra: NerdStash v2 tokenizer.
3. Text Completion: API tokenizer(지원되는 경우) 또는 Llama tokenizer.
4. KoboldAI Classic / AI Horde: Llama tokenizer.
5. KoboldCpp: 모델 API tokenizer.

부정확한 결과를 얻거나 실험하고 싶다면 AI 백엔드에 요청을 형성하는 동안 SillyTavern이 사용할 _override tokenizer_를 설정할 수 있습니다:

1. None. 각 토큰은 약 3.3자로 추정되며 가장 가까운 정수로 반올림됩니다. **높은 컨텍스트 길이에서 프롬프트가 잘리면 이것을 시도하세요.** 이 접근 방식은 KoboldAI Lite에서 사용됩니다.
2. Llama tokenizer. Llama 1/2 모델 제품군에서 사용됩니다: Vicuna, Hermes, Airoboros 등. **Llama 1/2 모델을 사용하는 경우 선택하세요.**
3. Llama 3 tokenizer. Llama 3/3.1 모델에서 사용됩니다. **Llama 3/3.1 모델을 사용하는 경우 선택하세요.**
4. NerdStash tokenizer. NovelAI의 Clio 모델에서 사용됩니다. **Clio 모델을 사용하는 경우 선택하세요.**
5. NerdStash v2 tokenizer. NovelAI의 Kayra 모델에서 사용됩니다. **Kayra 모델을 사용하는 경우 선택하세요.**
6. Mistral V1 tokenizer. 이전 Mistral 모델 제품군 및 파인튠에서 사용됩니다. **이전 Mistral 모델을 사용하는 경우 선택하세요.**
7. Mistral Nemo tokenizer. Mistral Nemo 모델 제품군 및 파인튠에서 사용됩니다. **Mistral Nemo/Pixtral 모델을 사용하는 경우 선택하세요.**
8. Yi tokenizer. Yi 모델에서 사용됩니다. **Yi 모델을 사용하는 경우 선택하세요.**
9. Gemma tokenizer. Gemini/Gemma 모델에서 사용됩니다. **Gemma 모델을 사용하는 경우 선택하세요.**
10. DeepSeek tokenizer. DeepSeek 모델(R1과 같은)에서 사용됩니다. **DeepSeek 모델을 사용하는 경우 선택하세요.**
11. API tokenizer. 생성 API를 쿼리하여 모델에서 직접 토큰 수를 가져옵니다. 지원하는 것으로 알려진 백엔드: Text Generation WebUI(ooba), koboldcpp, TabbyAPI, Aphrodite API. **지원되는 백엔드를 사용하는 경우 선택하세요.**

Chat Completion APIs **(재정의 불가능)**:

1. OpenAI: [tiktoken](https://github.com/openai/tiktoken)을 통한 모델 종속 tokenizer.
2. Claude: [WebTokenizers](https://github.com/mlc-ai/tokenizers-cpp)를 통한 모델 종속 tokenizer.
3. OpenRouter: 각 모델에 대한 Llama, Mistral, Gemma, Yi tokenizers.
4. Google AI Studio: Gemma tokenizer.
5. AI21 API: Jamba tokenizer(일회성 다운로드 필요).
6. Cohere API: Command-R 또는 Command-A tokenizer(일회성 다운로드 필요).
7. MistralAI API: Mistral V1 또는 V3 tokenizer(일회성 다운로드 필요).
8. DeepSeek API: DeepSeek tokenizer(일회성 다운로드 필요).
9. Fallback tokenizer: GPT-3.5 turbo tokenizer.

#### 추가 Tokenizers

이러한 tokenizers는 크기 때문에 기본 설치에 포함되지 않습니다. 처음 사용할 때 일회성 다운로드가 필요합니다.

1. Qwen2 tokenizer.
2. Command-R / Command-A tokenizers. Chat Completion의 Cohere 소스에서 사용됩니다.
3. Mistral V3 (Nemo) tokenizer. Chat Completion의 MistralAI 소스(Nemo 및 Pixtral 모델)에서 사용됩니다.
4. DeepSeek (deepseek-chat) tokenizer. Chat Completion의 DeepSeek 소스에서 사용됩니다.

인터넷 다운로드를 사용하지 않으려면 config.yaml에서 옵트아웃 옵션이 있습니다: `enableDownloadableTokenizers`. `false`로 설정하여 다운로드를 비활성화합니다.

[SillyTavern-Tokenizers](https://github.com/SillyTavern/SillyTavern-Tokenizers) 저장소에서 tokenizers를 수동으로 다운로드할 수도 있습니다. JSON 파일을 다운로드하고 데이터 루트의 `_cache` 하위 디렉토리에 넣으세요. 기본적으로 경로는 `./data/_cache`입니다. `_cache` 디렉토리가 없으면 만드세요. 그런 다음 SillyTavern 서버를 다시 시작하여 tokenizers를 다시 초기화합니다.

필요한 tokenizer 모델이 캐시되지 않고 다운로드가 비활성화된 경우 카운팅을 위해 대체 tokenizer(Llama 3)가 사용됩니다.

### Token Padding

!!! 적용 대상: Text Completion APIs
SillyTavern은 항상 Chat Completion 모델에 일치하는 tokenizer를 사용하므로 토큰 패딩이 필요하지 않습니다.
!!!

SillyTavern이 모델을 실행하는 원격 백엔드 API에서 제공하는 tokenizer를 사용하지 않는 한, 프롬프트 생성 중에 가정된 모든 토큰 수는 선택한 [tokenizer](#tokenizer) 유형을 기반으로 추정됩니다.

토큰화 결과가 모델 정의 최대값에 가까운 컨텍스트 크기에서 부정확할 수 있으므로 프롬프트의 일부가 잘리거나 삭제될 수 있으며, 이는 캐릭터 정의의 일관성에 부정적인 영향을 미칠 수 있습니다.

이를 방지하기 위해 SillyTavern은 컨텍스트 크기의 일부를 패딩으로 할당하여 모델이 수용할 수 있는 것보다 더 많은 채팅 항목을 추가하지 않도록 합니다. 가장 일치하는 tokenizer가 선택되어 있어도 프롬프트의 일부가 잘린다면 설명이 잘리지 않도록 패딩을 조정하세요.

역 패딩을 위해 음수 값을 입력할 수 있으며, 이를 통해 설정된 최대 토큰 양보다 더 많이 할당할 수 있습니다.
