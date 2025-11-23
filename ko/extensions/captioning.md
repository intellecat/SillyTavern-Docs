---
route: /extensions/captioning/
templating: false
---

# Image Captioning

Image Captioning을 사용하면 SillyTavern이 채팅에 사용된 이미지에 대한 텍스트 설명을 자동으로 생성할 수 있습니다.

AI 캐릭터가 대화에서 시각적 콘텐츠를 "보고" 응답하도록 하려면 Image Captioning을 사용하세요.

- 메시지에 업로드하거나 붙여넣은 이미지에 대한 캡션 생성
- 채팅 기록의 기존 이미지에 컨텍스트 추가
- 로컬 모델, 클라우드 API 및 크라우드소싱 네트워크를 포함한 다양한 소스를 생성에 사용

설정이 필요 없고, 비용이 들지 않고, GPU가 필요 없는 옵션이 있습니다. 이러한 것들 중 일부 또는 전부가 필요한 옵션도 있습니다. 필요와 리소스에 맞는 것을 선택하세요.

이미지 캡션 확장 기능은 SillyTavern에 내장되어 있으며 별도로 설치할 필요가 없습니다.

## 빠른 시작

1. 설정:
    - **<i class="fa-solid fa-cubes"></i> Extensions** 패널에서 **Image Captioning** 패널을 여세요
    - 캡션 소스를 선택하세요(대부분 "Local" 또는 "Multimodal")
    - "Multimodal"의 경우 **<i class="fa-solid fa-plug"></i> API Connections** 탭에서 연결을 설정했는지 확인하세요
2. 캡션 생성:
    - **<i class="fa-solid fa-magic-wand-sparkles"></i> Extensions** 팝업 메뉴에서 "**Generate Caption**"을 선택하세요
    - 메시지가 표시되면 이미지 파일을 선택하세요
    - 캡션이 생성될 때까지 기다리세요
3. 검토 및 전송:
    - 캡션이 있는 이미지가 메시지에 삽입됩니다
    - 이미지 툴팁을 사용하여 캡션을 확인하세요
    - **<i class="fa-solid fa-paper-plane"></i> Send**를 클릭하여 캐릭터가 이미지에 대해 어떻게 생각하는지 확인하세요!

## 패널 컨트롤

### 소스 선택

이미지 캡션 소스를 선택하세요. 지원되는 옵션:

| 소스                           | 설명                                                                                                                                                                                  |
|----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Multimodal](#multimodal-source) | **클라우드**: OpenAI, Anthropic, Google, MistralAI 등. <br>**로컬**: Ollama, llama.cpp, KoboldCpp, Text Generation WebUI 및 vLLM. <br>이미지에 질문할 수 있도록 사용자 지정 프롬프트를 지원합니다. |
| [Local](#local-source)           | SillyTavern 서버 내에서 로컬로 실행되는 [transformers.js](https://huggingface.co/docs/transformers.js/en/index)를 사용합니다. 설정이 전혀 필요 없습니다!                                                                     |
| Horde                            | [AI Horde](https://aihorde.net/) 네트워크, 이미지 생성 모델의 크라우드소싱 분산 네트워크를 사용합니다. 다운로드, 구성 또는 비용이 필요 없습니다. 가변 응답 시간.                       |
| Extras                           | Extras 프로젝트는 2024년 4월에 중단되었으며 유지 관리 또는 지원되지 않습니다.                                                                                                                        |

### 캡션 구성
- **Caption Prompt**: 캡션 작성을 위한 사용자 지정 프롬프트를 입력하세요. 기본 프롬프트는 "What's in this image?"입니다.
- **Ask every time**: 각 이미지 캡션에 대한 사용자 지정 프롬프트를 요청하려면 토글하세요

### 메시지 템플릿
- **Message Template**: 캡션 메시지 템플릿을 사용자 지정하세요. 생성된 캡션을 삽입하려면 `{{caption}}` 매크로를 사용하세요. 기본 템플릿은 `[{{user}} sends {{char}} a picture that contains: {{caption}}]`입니다.

### 자동 캡션
- **Automatically caption images**: 메시지에 붙여넣거나 첨부된 이미지의 자동 캡션을 활성화하려면 토글하세요
- **Edit captions before saving**: 캡션을 저장하기 전에 편집할 수 있도록 하려면 토글하세요

## 이미지 캡션 작성

SillyTavern에서 이미지에 캡션을 작성하는 모든 방법:

* **<i class="fa-solid fa-magic-wand-sparkles"></i> Extensions** 팝업 메뉴에서 "**Generate Caption**"을 선택하고 메시지가 표시되면 이미지 파일을 선택하세요
* 메시지에 이미 있는 이미지 상단의 <i class="fa-solid fa-envelope-open-text"></i> **Caption** 아이콘을 클릭하세요
* [자동 캡션](#auto-captioning)이 활성화된 상태에서 채팅 입력에 직접 이미지를 붙여넣으세요
* 메시지 작업의 <i class="fa-solid fa-paperclip"></i> **Embed File or Image** 버튼을 사용하여 메시지에 이미지 파일을 첨부하세요.
* 포함된 이미지가 있는 메시지를 보내세요
* `/caption` [슬래시 명령어](#slash-command-caption)를 사용하세요

## 자동 캡션
자동 캡션 기능을 사용하면 매번 수동으로 캡션 프로세스를 트리거하지 않고도 채팅에 추가되는 이미지에 대한 캡션을 자동으로 생성할 수 있습니다.

활성화하려면 Image Captioning 패널에서 "Automatically caption images" 체크박스를 선택하세요. "Edit captions before saving" 상자를 선택하여 저장하기 전에 캡션을 편집할 수도 있습니다.

활성화되면 다음 시나리오에서 자동 캡션이 트리거됩니다:

- 이미지가 채팅 입력에 직접 붙여넣어질 때.
- 이미지 파일이 메시지에 첨부될 때.
- 포함된 이미지가 있는 메시지가 전송될 때.

시스템은 선택한 캡션 소스(Local, Extras, Horde 또는 Multimodal) 및 구성된 설정을 사용하여 이미지에 대한 캡션을 생성합니다.

### 저장하기 전에 캡션 편집(Refine Mode)

"Edit captions before saving" 옵션을 활성화한 경우:
1. 이미지가 추가되면 생성된 캡션과 함께 팝업이 나타납니다.
2. 필요에 따라 캡션을 검토하고 편집할 수 있습니다.
3. "OK"를 클릭하여 캡션을 적용하거나 "Cancel"을 클릭하여 저장하지 않고 캡션을 삭제하세요.

### 캡션 전송
생성된(및 선택적으로 편집된) 캡션은 구성한 Message Template을 사용하여 프롬프트에 자동으로 삽입됩니다. 기본적으로 다음 형식으로 전송됩니다:

```
[BaronVonUser sends Seraphina a picture that contains: ...]
```


## 슬래시 명령어: /caption
확장 기능은 채팅 상자 또는 스크립트에서 사용할 `/caption` 슬래시 명령어를 제공합니다.

### 사용법

```
/caption [quiet=true|false]? [mesId=number]? [prompt]
```

- `prompt` (선택 사항): 캡션 모델에 대한 사용자 지정 프롬프트. 멀티모달 소스만 지원합니다.
- `quiet=true|false`: true로 설정하면 채팅에 캡션이 있는 메시지를 보내지 않습니다. 기본값은 false입니다.
- `mesId=number`: 새 이미지를 업로드하는 대신 기존 메시지의 이미지에 캡션을 작성하도록 메시지 ID를 지정합니다.

`mesId`가 제공되지 않으면 명령어에서 이미지를 업로드하라는 메시지가 표시됩니다. `quiet`이 false(기본값)인 경우 캡션이 있는 이미지가 포함된 새 메시지가 채팅으로 전송됩니다. 생성된 캡션은 다른 명령어의 입력으로 사용할 수 있습니다.

### 예제
기본 설정으로 새 이미지에 캡션 작성:

```
/caption
```

사용자 지정 프롬프트로 새 이미지에 캡션 작성:

```
/caption Describe the main colours and shapes in this image
```

새 메시지를 보내지 않고 메시지 #5의 이미지에 캡션 작성:

```
/caption mesId=5 quiet=true
```

사용자 지정 프롬프트로 메시지 #10의 이미지에 캡션을 작성한 다음 캡션을 기반으로 [새 이미지 생성](/extensions/Stable-Diffusion.md):

```
/caption mesId=10 Describe this image using comma-separated keywords | /imagine
```

## Local 소스

[config.yaml](/Administration/config-yaml.md#extensions-configuration)에서 모델을 변경할 수 있습니다. 키는 `extensions.models.captioning`이라고 합니다. 사용하려는 Hugging Face 모델 ID를 입력하세요. 기본값은 `Xenova/vit-gpt2-image-captioning`입니다.

이미지 캡션을 지원하는 모든 모델(`VisionEncoderDecoderModel` 또는 "image-to-text" 파이프라인)을 사용할 수 있습니다. 모델은 transformers.js 라이브러리와 호환되어야 합니다. 즉, ONNX 가중치가 필요합니다. `ONNX` 및 `image-to-text` 태그가 있는 모델을 찾거나 `.onnx` 파일로 가득 찬 `onnx`라는 폴더가 있는 모델을 찾으세요.

## Multimodal 소스

### 일반 구성

- **Model**: 이미지 캡션 모델을 선택하세요. 옵션은 선택한 API에 따라 다릅니다.
- **Allow reverse proxy**: 정의되고 유효한 경우 리버스 프록시 사용을 허용하려면 토글하세요(OpenAI, Anthropic, Google, Mistral, xAI)

캡션 소스의 API 키 및 엔드포인트 URL은 [API Connections](/Usage/API_Connections/index.md) 패널에서 관리됩니다. 먼저 API Connections에서 연결을 설정한 다음 Captioning에서 캡션 소스로 선택하세요.

!!!warning 먼저 API Connections 패널에서 설정하세요
마지막으로 한 번 더: **<i class="fa-solid fa-plug"></i> API Connections**에서 API 키/주소/포트를 구성하고 Captioning에서 연결을 사용하세요.

여전히 채팅에는 Claude를 사용하고 이미지 캡션에는 Google AI Studio를 사용할 수 있습니다. 먼저 'API Connections' 탭에서 *둘 다*를 설정하세요. 그런 다음 Chat Completion 소스를 Claude로, Captioning 소스를 Google AI Studio로 전환하세요.
!!!

대부분의 로컬 백엔드의 경우 SillyTavern이 아닌 모델 백엔드에서 일부 옵션을 설정해야 합니다. 백엔드가 한 번에 하나의 모델만 실행할 수 있고 자동 전환을 지원하지 않는 경우 채팅과 캡션에 다른 모델을 사용하기 위한 몇 가지 옵션이 있습니다:

1. **보조 엔드포인트:** 보조 엔드포인트 기능을 사용하여([보조 엔드포인트](#secondary-endpoints) 섹션 참조) 캡션에 대해 다른 API 서버에 연결
2. **여러 연결 유형:** API Connections에서 Text Completion 및 Chat Completion 모드를 모두 사용하여 백엔드에 연결 - 이렇게 하면 동일한 백엔드 유형에 대한 두 개의 별도 연결을 제공합니다

### 소스

이러한 캡션 소스 중 하나를 사용하려면 Source 드롭다운에서 Multimodal을 선택하세요.

* "최고의 캡션을 원하고 비용을 지불해도 괜찮습니다": Anthropic
* "비용을 지불하거나 실행하고 싶지 않습니다": Google AI Studio 무료 등급
* "로컬로 이미지 캡션을 작성하고 작동하기를 원합니다": Ollama
* "로컬 AI의 꿈을 계속 유지하고 싶습니다": [KoboldCpp](#koboldcpp)
* "작동하지 않을 때 불평하고 싶습니다": ~~Extras~~

| API 공급자                      | 설명                                                                                                                                                                   |
|-----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AI/ML API                         | 클라우드, 유료, 비전 기능이 있는 다양한 GPT, Claude 및 Gemini 모델                                                                                                  |
| Claude                            | 클라우드, 유료, 비전 기능이 있는 모든 Claude 모델                                                                                                                       |
| Cohere                            | 클라우드, 유료, Aya Vision 8B / 32B                                                                                                                                              |
| Custom (OpenAI-compatible)        | 사용자 지정 OpenAI 호환 API의 경우 API Connections 탭에서 현재 구성된 모델을 사용                                                                                     |
| Electron Hub                      | 클라우드, 유료, 비전 기능이 있는 다양한 모델.                                                                                                                         |
| Google AI Studio                  | 클라우드, 무료 등급 후 유료, Gemini Flash/Pro                                                                                                                                  |
| Google Vertex AI                  | 클라우드, 무료 등급, Gemini Flash/Pro                                                                                                                                            |
| Groq                              | 클라우드, llama-4 scout/maverick                                                                                                                                 |
| KoboldCpp                         | 로컬, KoboldCpp에서 모델을 구성해야 함                                                                                                                                      |
| llama.cpp                         | 로컬, llama.cpp에서 모델을 구성해야 함                                                                                                                                      |
| MistralAI                         | 클라우드, 유료, pixtral-large, pixtral-12B, magistral, mistral-large 등.                                                                                                       |
| Moonshot AI                       | 클라우드, 유료, moonshot-vision                                                                                                                                                  |
| NanoGPT                           | 클라우드, 유료, 비전 기능이 있는 다양한 GPT/Claude/Google 모델                                                                                                        |
| Ollama                            | 로컬, API Connections에서 구성한 후 Captioning 내에서 사용 가능한 모델 간에 전환하고 [추가 비전 모델](https://ollama.com/search?c=vision)을 다운로드할 수 있음 |
| OpenAI                            | 클라우드, 유료, GPT-4 Vision, 4-turbo, 4o, 4o-mini                                                                                                                               |
| OpenRouter                        | 클라우드, 유료(무료 옵션 가능), 많은 모델, API connections에서 구성한 후 Captioning 내에서 사용 가능한 것 중에서 선택                                              |
| Pollinations                      | 클라우드, 무료                                                                                                                                                                   |
| Text Generation WebUI (oobabooga) | 로컬, ooba에서 모델을 구성해야 함                                                                                                                                           |
| vLLM                              | 로컬                                                                                                                                                                         |
| xAI (Grok)                        | 클라우드, 유료, grok-vision                                                                                                                                                      |

### 보조 엔드포인트

기본적으로 Multimodal 소스는 API Connections 탭에서 구성된 기본 엔드포인트를 사용합니다.
멀티모달 캡션 전용 보조 엔드포인트를 설정할 수도 있습니다.

- **<i class="fa-solid fa-cubes"></i> Extensions** 패널에서 **Image Captioning** 패널을 여세요.
- 캡션 소스로 "Multimodal"을 선택하고 선호하는 API 공급자를 선택하세요.
- "Secondary captioning endpoint URL" 필드에 보조 엔드포인트에 대한 유효한 URL을 입력하세요.
- 보조 엔드포인트를 활성화하려면 "Use secondary URL" 상자를 선택하세요.

!!!tip
URL 끝에 `/v1` 또는 `/chat/completions`를 추가하지 마세요. 확장 기능이 자동으로 처리합니다.
!!!

다음 API만 지원됩니다:

- KoboldCpp
- llama.cpp
- Ollama
- Text Generation WebUI (oobabooga)
- vLLM

### 소스별 가이드

#### KoboldCpp

[KoboldCpp](https://github.com/LostRuins/koboldcpp) 설치 및 사용에 대한 일반 정보는 [KoboldCpp 문서](https://github.com/LostRuins/koboldcpp/wiki)를 참조하세요.

멀티모달 캡션에 KoboldCpp를 사용하려면:

* 텍스트와 이미지 프롬프트를 동시에 처리하도록 훈련된 멀티모달 가능 모델을 얻으세요.
* 모델에 대한 멀티모달 프로젝션도 얻으세요. 이러한 가중치를 사용하면 모델이 입력의 텍스트와 이미지 부분이 서로 어떻게 관련되는지 이해할 수 있습니다.
* KoboldCpp 실행 GUI 또는 명령줄 인터페이스에서 모델과 프로젝션을 로드하세요.

원본이자 클래식한 로컬 멀티모달 모델은 LLaVA입니다. 모델 및 프로젝션에 대한 GGUF 형식 파일은 [Mozilla/llava-v1.5-7b-llamafile](https://huggingface.co/Mozilla/llava-v1.5-7b-llamafile)에서 사용할 수 있습니다. 명령줄에서 로드하려면 `--model` 및 `--mmproj` 플래그로 모델과 프로젝션을 설정하세요. 예를 들어:

```shell
./koboldcpp \
--model="models/llava-v1.5-7b-Q4_K.gguf" \
--mmproj="models/ llava-v1.5-7b-mmproj-Q4_0.gguf" \
... other flags ...
```

시도해 볼 수 있는 일부 LLaVA 파인튠: [xtuner/llava-llama-3-8b-v1_1-gguf](https://huggingface.co/xtuner/llava-llama-3-8b-v1_1-gguf), [xtuner/llava-phi-3-mini-gguf](https://huggingface.co/xtuner/llava-phi-3-mini-gguf).

특정 파인튠이 구축된 기본 모델에 대한 멀티모달 프로젝션을 사용할 수 있습니다. 일부 일반적인 기본 모델에 대한 프로젝션은 [koboldcpp/mmproj](https://huggingface.co/koboldcpp/mmproj/tree/main)에서 사용할 수 있습니다.
