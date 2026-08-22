---
route: /ko/extensions/stable-diffusion/
templating: false
---

# 이미지 생성

로컬 또는 클라우드 기반 Stable Diffusion, FLUX 또는 DALL-E API를 사용하여 이미지를 생성합니다.

완전한 몰입감을 위해 메시지에 대한 응답으로 이미지를 자동으로 생성하거나,
wand 메뉴 또는 slash 명령에서 채팅 기록 및 캐릭터 정보로부터 생성하거나,
채팅 입력 창에서 `/sd (anything_here)` 명령을 사용하여 자신만의 프롬프트로 이미지를 만듭니다.

가장 일반적인 Stable Diffusion 생성 설정은 SillyTavern UI 내에서 사용자 정의할 수 있습니다.

- 로컬 및 클라우드 기반 [여러 이미지 생성 소스](#supported-sources) 지원
- 캐릭터, 장면 및 사용자 정의 프롬프트를 위한 다양한 [생성 모드](#generation-modes)
- 채팅 내에서 쉬운 이미지 생성을 위한 [Slash 명령](#how-to-generate-an-image)
- 자연어 요청에 따라 이미지 생성을 트리거하는 [대화형 모드](#use-interactive-mode)
- 일관된 스타일과 품질을 위한 사용자 정의 가능한 프롬프트 템플릿 및 [접두사](#common-prompt-prefix)
- 맞춤형 캐릭터 이미지를 위한 [캐릭터별 프롬프트 접두사](#character-specific-prompt-prefix)
- 다양한 이미지 생성 설정 간에 빠르게 전환하는 [스타일 프리셋](#styles)
- 채팅에서 생성된 이미지를 위한 유연한 [가시성 옵션](#chat-message-visibility)
- 고도로 사용자 정의 가능한 워크플로를 위한 고급 [ComfyUI 통합](#comfyui-configuration)
- 캐릭터 갤러리에서 [생성된 모든 이미지 보기](#view-all-generated-images) 기능
- 같은 프롬프트를 유지하면서 이미지를 재생성하는 [이미지 스와이프](#image-swipes) 기능
- [생성 전 프롬프트 편집](#edit-prompts-before-generation) 및 [free-mode 프롬프트 확장](#extend-free-mode-prompts) 옵션
- 자동 이미지 생성 감지를 위한 AI [function calling](#use-function-tool)과의 통합

## 지원되는 소스

| 소스                                                                                            | 비고                                                                                         |
|:--------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------|
| [AI/ML API](https://aimlapi.com/)                                                                 | 클라우드, 유료                                                                                     |
| [Black Forest Labs](https://bfl.ai/)                                                              | 클라우드, 유료                                                                                     |
| [Cloudflare Workers AI](https://www.cloudflare.com/developer-platform/products/workers-ai/)       | 클라우드, 유료, 비전 기능이 있는 다양한 모델                                            |
| [Chutes](https://chutes.ai/)                                                                      | 클라우드                                                                                           |
| [ComfyUI](https://github.com/comfyanonymous/ComfyUI)                                              | 로컬, 오픈 소스 (GPL3), 무료, [ComfyUI Configuration](#comfyui-configuration) 참조. |
| [Draw Things](https://drawthings.ai/)                                                             | 로컬, Mac/iOS, 무료                                                                  |
| [Electron Hub](https://electronhub.ai/)                                                           | 클라우드, 유료                                                                                     |
| [FAL.AI](https://fal.ai/)                                                                         | 클라우드, 유료                                                                                     |
| [Google AI Studio](https://aistudio.google.com/) / [Google Vertex AI](https://cloud.google.com/vertex-ai) | 클라우드, 유료. Imagen 모델 시리즈. AI Studio는 더 적은 모델을 지원합니다.                       |
| [HuggingFace Serverless](https://huggingface.co/docs/api-inference/index)                         | 클라우드, 무료                                                                           |
| [NanoGPT](https://nano-gpt.com/)                                                                  | 클라우드, 유료                                                                                     |
| [NovelAI Diffusion](https://novelai.net/)                                                         | 클라우드, 활성 구독 필요                                                          |
| [OpenAI](https://platform.openai.com/)                                                            | 클라우드, 유료                                                                                     |
| [OpenRouter](https://openrouter.ai/)                                                              | 클라우드                                                                                           |
| [Pollinations](https://pollinations.ai/)                                                          | 클라우드, 오픈 소스 (MIT), 유료                                                        |
| [SD.Next / vladmandic](https://github.com/vladmandic/automatic)                                   | 로컬, 오픈 소스 (AGPL3), 무료                                                      |
| [SillyTavern Extras](https://github.com/SillyTavern/SillyTavern-Extras)                           | 더 이상 사용되지 않음, 권장하지 않음                                                                     |
| [stable-diffusion.cpp](https://github.com/leejet/stable-diffusion.cpp)                            | 로컬, 오픈 소스 (MIT), 무료                                                        |
| [Stability AI](https://platform.stability.ai/)                                                    | 클라우드, 유료                                                                                     |
| [Stable Diffusion WebUI / AUTOMATIC1111](https://github.com/AUTOMATIC1111/stable-diffusion-webui) | 로컬, 오픈 소스 (AGPL3), 무료                                                      |
| [Stable Horde](https://stablehorde.net/)                                                          | 클라우드, 오픈 소스 (AGPL3), 무료                                                      |
| [TogetherAI](https://docs.together.ai/docs/serverless-models#image-models)                        | 클라우드                                                                                           |
| [x.AI](https://x.ai/)                                                                             | 클라우드, 유료                                                                                     |
| [Z.AI](https://z.ai/)                                                                             | 클라우드, 유료                                                                                     |

## 생성 모드

| 완드 메뉴 항목     | Slash 명령 인수 | 설명                                    | 비고                               |
|:-------------------|:-----------------------|:-----------------------------------------------|:--------------------------------------|
| "Yourself"         | `you`                  | 현재 캐릭터의 전신 초상화. | -                                     |
| "Your Face"        | `face`                 | 현재 캐릭터의 클로즈업 초상화.  | 초상화 종횡비 강제 적용.       |
| "Me"               | `me`                   | 사용자 페르소나의 초상화.                | -                                     |
| "The Whole Story"  | `scene`                | 채팅 이벤트의 시각적 요약.             | -                                     |
| "The Last Message" | `last`                 | 마지막 채팅 메시지의 시각적 요약.       | -                                     |
| "Raw Last Message" | `raw_last`             | 마지막 메시지를 프롬프트로 그대로 사용.        | -                                     |
| "Background"       | `background`           | 스토리 컨텍스트를 기반으로 한 채팅 배경.      | 넓은 가로 종횡비 강제 적용. |

## 이미지 생성 방법

1. extensions context 메뉴(wand)의 "Image Generation" 항목을 사용합니다.
2. Generation modes 테이블의 인수와 함께 `/sd (argument)` slash 명령을 입력합니다. 다른 것은 "free mode"를 트리거하여 SD가 프롬프트한 내용을 생성하도록 합니다. 예: `/sd apple tree`는 사과 나무 그림을 생성합니다.
3. 채팅 메시지의 context actions에서 페인트브러시 아이콘을 찾습니다. 이렇게 하면 선택한 메시지에 대해 "Raw Message" 모드가 강제 적용됩니다.

raw message 및 free mode를 제외한 모든 생성 모드는 현재 선택한 main generation API를 사용하여 채팅 컨텍스트를 SD 프롬프트로 변환하는 프롬프트 생성을 트리거합니다.
extensions 패널의 "SD Prompt Templates" 설정 드로어를 사용하여 모든 생성 모드에 대한 프롬프트 생성을 위한 instruction 템플릿을 구성할 수 있습니다.

### `/sd` 명령 사용을 위한 팁과 트릭

#### 생성된 모든 이미지 보기

캐릭터에 대해 저장된 모든 이미지를 보려면(다른 채팅 포함) 캐릭터 정보 패널의 "More..." 드롭다운 메뉴에서 갤러리를 열거나 `/show-gallery` slash 명령을 사용합니다.

#### negative 프롬프트 지정

프롬프트 앞에 `negative` 명명된 인수를 사용하여 이 생성에 대한 특정 negative 프롬프트를 강제합니다.

```stscript
/sd negative="fries" cute tater farmer holding a tayto in a spud-field
```

#### 캐릭터별 접두사 포함

free-prompt 모드에서 특수 `{{charPrefix}}` 매크로를 사용하여 현재 캐릭터에 대해 정의된 경우 positive 및 negative 프롬프트 접두사를 포함합니다.

```stscript
/sd {{charPrefix}}, riding a bike
```

#### 채팅 메시지 억제

`quiet=true` 명명된 인수를 전달하여 생성된 이미지가 채팅에 게시되는 것을 방지할 수 있습니다. 이미지는 여전히 캐릭터 갤러리에 추가되며 명령은 다른 명령에서 사용할 수 있는 이미지에 대한 상대 URL을 생성합니다.

아래 예제는 생성된 이미지를 Markdown을 사용하여 사용자 페르소나로 보냅니다.

```stscript
/sd quiet=true me | /send Here's a picture of me: ![my portrait]({{pipe}})
```

### 이미지 스와이프

이미지 스와이프를 사용하면 같은 프롬프트를 유지하면서 이미지 생성을 다시 시도할 수 있습니다. 고정 시드가 설정된 경우 다음 생성을 위해 무작위화됩니다. `/sd` slash 명령을 통해 재정의된 이미지 크기는 스와이프된 이미지에도 유지됩니다.

이미지를 순환하려면 생성된 이미지 위에 마우스 커서를 올려놓고(모바일에서는 탭) 화살표 버튼과 스와이프 카운터를 표시합니다. 최신 이미지에서 오른쪽 화살표를 탭하면 새 이미지가 생성됩니다.

*여기서 'Swipes'는 이름일 뿐이며 실제 스와이프 제스처를 시도하지 마세요. 이렇게 하면 첨부된 이미지가 아니라 메시지 자체가 재생성됩니다.*

## 옵션

### 생성 전 프롬프트 편집

이 옵션을 사용하면 Image Generation API로 전송되기 전에 자동으로 생성된 프롬프트를 편집할 수 있습니다. 또한 저장된 negative 프롬프트를 편집하거나 삭제할 수 있으며, 원래 `/sd` 명령으로 생성된 이미지를 재생성할 때 해상도를 재정의할 수 있습니다.

### function tool 사용

[function calling](/extensions/Stable-Diffusion.md)을 사용하여 이미지 생성 의도를 자동으로 감지합니다.

**요구 사항:**

1. 지원되는 소스로 이미지 생성을 구성해야 합니다.
2. 지원되는 Chat Completion API 모델을 사용하고 AI Response 설정에서 function tool calling을 활성화해야 합니다.
3. Image Generation 설정에서 "Use function tool" 옵션을 활성화해야 합니다.
4. 사용자는 채팅 메시지에서 이미지 생성 의도를 표현해야 합니다. 예: "Send me a picture of a cat".

!!!warning
function tool이 활성화되면 대화형 모드가 트리거되지 않습니다.
!!!

### 대화형 모드 사용

다음 특수 패턴을 따르는 사용자 메시지에 대한 응답으로 텍스트 대신 이미지 생성을 트리거할 수 있습니다:

1. 다음 동사 중 하나를 포함: send, mail, imagine, generate, make, create, draw, paint, render
2. 다음 명사 중 하나가 뒤따름(10자 이내): pic, picture, image, drawing, painting, photo, photograph
3. 이미지 생성의 대상 주제가 뒤따르며 선택적으로 "of a" 또는 "of this"와 같은 구문이 앞에 올 수 있습니다.

유효한 요청 및 캡처된 주제의 예:

* `Can you please send me a picture of a cat` => `cat`
* `Generate a picture of the Eiffel tower` => `Eiffel tower`
* `Let's draw a painting of Mona Lisa` => `Mona Lisa`

일부 특수 주제는 사전 정의된 생성 모드를 트리거합니다:

* 'you, 'yourself' => "Yourself"
* 'your face', 'your portrait', 'your selfie' => "Your Face"
* 'me', 'myself' => "Me"
* 'story', 'scenario', 'whole story' => "The Whole Story"
* 'last message' => "The Last Message"
* 'background', 'scene background', 'scene', 'scenery', 'surroundings', 'environment' => "Background"

### free-mode 프롬프트 확장

대화형 모드 또는 slash 명령을 사용할 때 main API를 프롬프트하여 free-mode 생성 주제 설명을 자동으로 확장합니다.

### 최소 프롬프트 처리

활성화되면 이미지 생성을 위해 LLM이 반환한 프롬프트에 적용되는 처리를 줄입니다. 정규화 및 공백 축소만 수행되며 기본적으로 수행되는 적극적인 살균(sanitization) 과정은 건너뜁니다. JSON과 같은 구조화된 프롬프트 형식을 허용하는 고급 워크플로(예: ComfyUI)로 작업할 때 유용합니다.

### 자동 조정된 해상도 스냅

강제 종횡비(초상화, 배경)가 있는 이미지 생성 요청을 절대 픽셀 수를 유지하면서 가장 가까운 알려진 해상도로 스냅합니다. 가능한 옵션 목록은 "Resolution" 드롭다운을 참조하세요.

**SDXL 모델에 권장**.

## 공통 프롬프트 접두사

!!!tip Pro Tip
`{prompt}` 매크로를 사용하여 생성된 프롬프트가 정확히 어디에 삽입될지 지정합니다.
!!!

생성된 모든 프롬프트 또는 free-mode 프롬프트 앞에 추가됩니다. 일반적으로 그림의 전체 스타일을 설정하는 데 사용됩니다.

예: `best quality, anime lineart`.

## 네거티브 프롬프트

출력에 표시하지 않으려는 이미지의 특성.

예: `bad quality, watermark`.

## 캐릭터별 프롬프트 접두사

!!!tip Pro Tip
생성 소스에서 지원하는 경우 여기에서 LoRA/embeddings를 사용할 수도 있습니다. 예: `<lora:DonaldDuck:1>`.
!!!

현재 선택한 캐릭터를 설명하는 모든 특성. 공통 접두사 뒤에 추가됩니다.

예: `female, green eyes, brown hair, pink shirt`.

원하지 않는 콘텐츠에 대한 negative 프롬프트 접두사를 지정할 수도 있습니다. 일반 negative 프롬프트와 결합됩니다.

제한 사항:
1. 1대1 채팅에서만 작동합니다. 그룹에서는 사용되지 않습니다.
2. 배경 및 free mode 생성에는 사용되지 않습니다.

!!! Note
free mode 프롬프트에 캐릭터 접두사를 강제로 포함하려면 프롬프트 어디든 `{{charPrefix}}` 매크로를 사용하세요.
!!!

접두사를 다른 사람과 공유하려면 "Shareable" 체크박스를 선택하세요. 그러면 로컬 설정이 아닌 캐릭터 데이터와 함께 저장됩니다.

## 스타일

이것을 사용하여 좋아하는 스타일/품질 프리셋을 빠르게 저장하고 복원하여 나중에 사용하거나 모델 간에 전환할 때 사용합니다. 다음이 Style 프리셋에 포함됩니다:

1. Common Prompt Prefix
2. Negative Prompt

`/imagine-style` 명령(또는 `/sd-style` 또는 `/img-style`)을 사용하여 스타일 간에 전환할 수도 있습니다.

## 채팅 메시지 가시성

채팅에 삽입된 생성된 이미지는 기본적으로 main API 프롬프트에서 숨겨져 있지만 생성 시작자별로 개별적으로 재정의할 수 있습니다("Magic wand" 아이콘, slash 명령, 대화형 모드). 이것은 캐릭터가 이미지를 "인식"하도록 하여 경험을 더 몰입감 있게 만드는 데 사용할 수 있습니다. Chat Completions API의 multimodal 모델은 "Send inline images"가 활성화된 경우 이미지를 '볼' 수도 있습니다.

Image Prompt Templates 아래의 "Chat Message Template"을 변경하여 텍스트 메시지를 사용자 정의할 수 있습니다. 이 템플릿에서 모든 일반 매크로를 사용할 수 있으며 이미지 프롬프트가 추가될 위치를 지정하는 특수 `{{prompt}}` 매크로도 사용할 수 있습니다.

## ComfyUI 구성

[ComfyUI](https://github.com/comfyanonymous/ComfyUI)는 이미지 생성을 위한 빠르고 매우 유연한 옵션입니다.

ComfyUI에 익숙하다면 간단히 말해: ComfyUI에서 워크플로를 만들고 **API 형식으로** 다운로드한 다음 SillyTavern ComfyUI Workflow Editor에 붙여넣습니다. ST는 워크플로를 ComfyUI의 API에 제출하고 채팅에서 이미지를 받게 됩니다. 그러나 큰 힘에는 큰 책임이 따르며 주요 책임은 SillyTavern에서 설정을 변경할 수 있도록 워크플로 JSON에 자리 표시자를 삽입하는 것입니다.

ComfyUI에 익숙하지 않다면 기본 워크플로를 사용하여 SillyTavern에서 이미지를 생성하는 데 여전히 사용할 수 있습니다. 나중에 큰 힘을 원할 때 ComfyUI 사용 방법을 배울 수 있습니다...

### 컨트롤

이 패널을 사용하면 SillyTavern과 ComfyUI 통합을 구성하고 관리할 수 있습니다.

#### 서버 유형

* Standard Server는 로컬 머신이든 다른 곳에서 호스팅되든 ComfyUI를 직접 호출하는 경우입니다.
* RunPod Serverless Endpoint는 [RunPod의 serverless API](https://www.runpod.io/product/serverless)를 통해 ComfyUI를 실행하기 위한 것입니다. Serverless는 standard server와 동일한 수준의 워크플로 제어를 유지하면서 더 강력한 호스팅 GPU를 활용하고 실제로 이미지를 생성할 때만 비용이 청구되므로 원격 생성에 좋은 옵션이 될 수 있습니다. 대부분의 사용법은 동일합니다. standard server 설정 및 동작과의 차이점은 [아래](#comfyui-runpod-setup)에 설명되어 있습니다.

#### Standard Server 설정

**ComfyUI URL** 입력 필드에 ComfyUI 서버의 URL을 입력합니다. 기본값은 `http://127.0.0.1:8188`입니다.
[SwarmUI](https://github.com/mcmonkeyprojects/SwarmUI)를 사용하는 경우
[managed ComfyUI server](https://github.com/mcmonkeyprojects/SwarmUI/blob/master/src/BuiltinExtensions/ComfyUIBackend/README.md)의 기본 포트는 `7821`이며
SwarmUI의 기본 포트보다 20 포트 높습니다.

URL을 입력한 후 <i class="fa-solid fa-check"></i> **Connect**를 선택하여 연결을 확인하고 설정합니다. ComfyUI 서버는 SillyTavern 호스트 머신에서 액세스할 수 있어야 합니다.

#### ComfyUI RunPod 설정

* RunPod 계정이 필요하며 어느 정도 금액을 충전해야 합니다. RTX 4090에서 Qwen 이미지 생성 기준으로 이미지당 약 2센트를 예상할 수 있지만 결과는 다를 수 있습니다(YMMV). $5의 크레딧이면 상당 기간 사용할 수 있습니다.
* <https://console.runpod.io/hub/runpod-workers/worker-comfyui>는 자신만의 serverless endpoint를 만드는 데 사용할 수 있는 flux1 dev 구성입니다.
  * 다른 모델을 사용하거나 LoRA를 추가하려는 경우 자신만의 구성을 만드는 방법에 대한 정보가 있습니다.
* serverless endpoint에 액세스하기 위한 API 키를 생성합니다: <https://console.runpod.io/user/settings>

* ST에서 **Source**로 **ComfyUI**를, **Server Type**으로 **RunPod Serverless Endpoint**를 선택합니다.
* **ComfyUI RunPod URL**을 엔드포인트의 URL로 설정합니다.
* API 키를 설정합니다.
* **Connect**를 클릭합니다. API 키와 URL이 올바르면 성공을 나타내는 토스트가 표시됩니다.
* ComfyUI 워크플로 구성 과정은 로컬과 동일합니다.
  * "Export (API)" 옵션을 사용하세요.
  * 로컬 설정에 따라 RunPod에서 사용할 모델의 변형을 선택해야 할 수도 있습니다. 예를 들어 로컬에서 양자화된 GGUF를 사용하지만 RunPod에서는 fp16 버전을 사용하고 싶은 경우입니다. ST에서 사용하는 JSON 워크플로에 이 변경 사항이 반영되어야 합니다.
  * 모델, 샘플러, VAE 등은 동적으로 결정할 수 없으므로 워크플로에 이러한 값이 하드코딩되어 있어야 합니다(`%model%` 치환 없음).
  * 다른 치환은 로컬과 동일하게 작동해야 합니다.

!!!info 참고
serverless 구성은 현재 워크플로를 출력 이미지에 임베드하지 않습니다. 즉, 시드나 프롬프트를 확인하기 위해 이미지를 로컬 ComfyUI로 드래그 앤 드롭할 수 없습니다. 이것은 RunPod 핸들러의 한계일 뿐이며 해당 측에서 추가될 수 있는 기능입니다.
!!!

### 워크플로 관리

드롭다운 메뉴에서 ComfyUI 워크플로를 선택합니다. 두 가지 기본 워크플로가 제공됩니다:

- Default_Comfy_Workflow.json: 가장 일반적인 이미지 생성 설정을 지원하는 기본 텍스트 to 이미지 워크플로.
- Char_Avatar_Comfy_Workflow.json: 캐릭터 아바타와 프롬프트를 사용하여 이미지를 생성하는 샘플 이미지 to 이미지 워크플로.

다음 버튼을 사용하여 워크플로를 관리합니다:

- <i class="fa-solid fa-pen-to-square"></i> **Open workflow editor**를 사용하여 선택한 워크플로를 보고 수정합니다.
- <i class="fa-solid fa-plus"></i> **Create new workflow**를 사용하여 사용자 정의 이름으로 새 워크플로를 만듭니다.
- <i class="fa-solid fa-trash-can"></i> **Delete workflow**를 사용하여 선택한 워크플로를 제거합니다.

### 워크플로 편집기

ComfyUI Workflow Editor를 사용하면 SillyTavern과 함께 사용할 ComfyUI 워크플로를 보고 수정할 수 있습니다.

편집기의 주요 구성 요소는 JSON 형식으로 ComfyUI 워크플로를 삽입하거나 편집할 수 있는 큰 텍스트 영역입니다.

편집기에 ComfyUI 워크플로를 추가하려면 다음 단계를 따르세요:

1. ComfyUI 설정에서 'Dev Mode'를 활성화합니다.
2. ComfyUI에서 'Save (API Format)' 옵션을 사용하여 JSON 데이터를 다운로드합니다.
3. SillyTavern에서 새 워크플로를 만들고 편집기를 엽니다.
4. 다운로드한 JSON 데이터를 텍스트 영역에 붙여넣습니다.
5. 사용 사례에 필요한 경우 특정 값을 자리 표시자로 교체합니다.

!!!tip Tips
API 형식 JSON 파일을 SillyTavern 설치의 `data/default-user/user/workflows` 디렉토리에 직접 추가할 수 있습니다. 이렇게 하면 3단계와 4단계를 건너뛸 수 있습니다.

원본 JSON 파일을 유지하세요. 변경하기 위해 ComfyUI에서 워크플로를 다시 열어야 하는 경우 모든 자리 표시자가 있는 파일보다 원본 파일을 편집하는 것이 훨씬 편리합니다.
!!!

### 자리 표시자

편집기는 워크플로 JSON에서 사용할 수 있는 사전 정의된 자리 표시자 목록을 제공합니다. 이러한 자리 표시자는 SillyTavern에서 워크플로가 실행될 때 동적 값으로 교체됩니다.

✅로 표시된 자리 표시자는 워크플로 JSON에 있습니다. ❌로 표시된 자리 표시자는 워크플로 JSON에 없습니다. 필요에 따라 이러한 자리 표시자를 워크플로 JSON에 추가할 수 있습니다. 워크플로에서 사용하고 동적으로 교체하려는 자리 표시자만 추가하면 됩니다.

#### 프롬프트

`%prompt%` 및 `%negative_prompt%` 자리 표시자는 이미지 생성 프롬프트를 워크플로에 삽입하는 데 사용됩니다. 여기에는 선택한 `/sd` 모드에 대해 생성된 프롬프트, 공통 프롬프트 접두사, negative 프롬프트 및 캐릭터별 프롬프트 접두사를 포함하여 SillyTavern에서 생성된 최종 프롬프트가 포함됩니다.

예를 들어 ComfyUI에서 "forest elf"와 같은 프롬프트로 워크플로를 테스트했을 수 있습니다. SillyTavern에서 이 워크플로를 사용하려면 "forest elf" 프롬프트를 `%prompt%` 자리 표시자로 교체할 수 있습니다:

+++ 자리 표시자가 있는 JSON
```json
{
    "class_type": "CLIPTextEncode",
    "inputs": {
        "clip": ["4", 1],
        "text": "%prompt%"
    }
}
```
+++ 원본 JSON
```json
{
    "class_type": "CLIPTextEncode",
    "inputs": {
        "clip": ["4", 1],
        "text": "forest elf"
    }
}
```
+++

자리 표시자가 큰따옴표로 묶여 있는지 확인하세요. 이것은 JSON 형식에 중요하며 SillyTavern의 자리 표시자 교체 시스템에 필요합니다. 숫자의 경우에도 템플릿 JSON에서 큰따옴표를 사용해야 합니다.

때때로 프롬프트(또는 다른 값)가 예상한 위치에 나타나지 않습니다. ComfyUI는 API 모드에서 워크플로가 작동하는 데 필요하지 않은 경우 워크플로의 API 버전에서 노드를 제거합니다.

예를 들어 이 워크플로는 UI 모드에서 워크플로가 더 명확하도록 프롬프트 primitive와 함께 [LoRA tag loader node](https://github.com/badjeff/comfyui_lora_tag_loader)를 사용합니다:

![Prompt primitive and LoRA loader](/static/extensions/sd-comfy-prompt-primitive.png)

프롬프트 primitive 노드는 워크플로의 API 버전에서 제거되므로 LoraTagLoader 노드에 자리 표시자를 삽입합니다. 워크플로에서 텍스트 "apple tree"를 찾아 `%prompt%` 자리 표시자로 교체합니다:

+++ 자리 표시자가 있는 JSON
```json
{
    "inputs": {
      "text": "%prompt%",
      "model": ["112", 0],
      "clip": ["112", 1]
    },
    "class_type": "LoraTagLoader",
    "_meta": {"title": "Load LoRA Tag"}
}
```
+++ 원본 JSON
```json
{
    "inputs": {
      "text": "apple tree",
      "model": ["112", 0],
      "clip": ["112", 1]
    },
    "class_type": "LoraTagLoader",
    "_meta": {"title": "Load LoRA Tag"}
}
```
+++

경우에 따라 프롬프트가 UI에 한 번만 나타나더라도 워크플로 JSON에서 여러 번 교체해야 할 수 있습니다.

#### 모델

`%model%` 자리 표시자는 이미지 생성 설정에서 선택한 모델의 값을 삽입합니다.

기본 텍스트 to 이미지 워크플로의 예:

+++ 자리 표시자가 있는 JSON
```json
{
    "class_type": "CheckpointLoaderSimple",
    "inputs": {
        "ckpt_name": "%model%"
    }
}
```
+++ 원본 JSON
```json
{
    "class_type": "CheckpointLoaderSimple",
    "inputs": {
        "ckpt_name": "sd15.safetensors"
    }
}
```
+++

GGUF로 양자화된 UNet을 로드하려면 워크플로에서 [UNet Loader (GGUF)](https://github.com/city96/ComfyUI-GGUF) 노드를 사용하고
SillyTavern 모델 드롭다운에서 `GGUF` 모델을 선택한 다음 다음과 같이 노드 설정에서 `%model%` 자리 표시자를 사용합니다:

+++ 자리 표시자가 있는 JSON
```json
{
    "inputs": {
      "unet_name": "%model%"
    },
    "class_type": "UnetLoaderGGUF",
    "_meta": {
      "title": "Unet Loader (GGUF)"
    }
}
```
+++ 원본 JSON
```json
{
    "inputs": {
      "unet_name": "flux1-dev-Q4_0.gguf"
    },
    "class_type": "UnetLoaderGGUF",
    "_meta": {
      "title": "Unet Loader (GGUF)"
    }
}
```
+++

!!!info ComfyUI에 일반적인 SD 체크포인트 이외의 모델 유형이 있는 경우
Stable Diffusion 체크포인트, SD UNet 및 GGUF로 양자화된 UNet은 모두 Model 드롭다운에 나타납니다.
한 유형의 모델은 다른 유형을 기대하는 워크플로/로더 노드와 작동하지 않습니다.
ST에서 호환되지 않는 모델 유형을 선택하면 ComfyUI가 로더 노드에 문제가 있다고 보고합니다.
!!!

#### 아바타 이미지

`%user_avatar%` 및 `%char_avatar%` 자리 표시자를 사용하여 워크플로에 사용자 및 캐릭터 아바타를 포함합니다. 이러한 자리 표시자는 워크플로가 실행될 때 아바타의 PNG 데이터로 교체됩니다. 이미지 데이터는 base64 형식으로 인코딩되므로 워크플로에서 디코딩해야 합니다. 이 작업에 널리 사용되는 선택은 [Load image (Base64)](https://github.com/Acly/comfyui-tooling-nodes) 노드입니다.

이 예에서 캐릭터 아바타는 `Load Image (Base64)` 노드로 로드됩니다. 또한 Image Resize 노드를 사용하여 이미지 생성 설정에서 지정된 크기로 이미지 크기를 조정합니다:

![Load image from base64 string and resize](/static/extensions/sd-comfy-load-b64.png)

Load Image (Base64) 및 Image Resize 노드의 JSON에 `%char_avatar%`, `%width%` 및 `%height%` 자리 표시자를 삽입합니다:

```json
{
    "97": {
        "inputs": {
            "image": "%char_avatar%"
        },
        "class_type": "ETN_LoadImageBase64",
        "_meta": {"title": "Load Image (Base64)"}
    },
    "98": {
        "inputs": {
            "mode": "resize",
            "resize_width": "%width%",
            "resize_height": "%height%",
            "image": ["97", 0]
        },
        "class_type": "Image Resize",
        "_meta": {"title": "Resize image"}
    }
}
```

ComfyUI에서 워크플로를 테스트하기 위한 base64로 인코딩된 이미지 문자열을 얻으려면 이미지를 base64 문자열로 변환하는 온라인 도구를 사용하세요.
초기 테스트에 사용할 수 있는 예제 문자열은 다음과 같습니다: [sd-comfy-base64-test-string.txt](/static/extensions/sd-comfy-base64-test-string.txt).

#### 기타 자리 표시자

대부분의 다른 자리 표시자는 이미지 생성 설정의 해당 컨트롤 값 또는 `/sd` 명령으로 지정한 값을 사용합니다:

- `%vae%`, 하지만 대부분의 SD 모델에는 VAE가 포함되어 있으므로 기본 워크플로에서는 이 자리 표시자를 사용하지 않습니다. 사용자 정의 워크플로와 함께 사용하여 UNet과 함께 VAE를 로드하거나 기본 VAE를 재정의하는 등의 작업을 수행합니다.
- `%sampler%`
- `%scheduler%`
- `%steps%`
- `%scale%`
- `%width%`
- `%height%`
- `%denoise%`: 샘플 이미지 to 이미지 워크플로의 경우 denoise 양을 약 0.5(소스 이미지에 거의 눈에 띄지 않는 변경)에서 1.0(소스 이미지가 사용되지 않은 것처럼 완전히 다른 이미지) 사이로 변경합니다. 텍스트 to 이미지의 경우 1.0 이외의 값을 사용할 의미가 없으므로 기본 텍스트 to 이미지 워크플로에서는 사용되지 않습니다.
- `%clip_skip%`: 기본 워크플로에서는 사용되지 않지만 사용자 정의 워크플로에 사용할 수 있습니다.

`%seed%` 자리 표시자는 지정한 경우 컨트롤에서 시드 값을 삽입합니다. 시드를 `-1`로 설정하면 SillyTavern은 `%seed%`의 각 이미지에 대해 새로운 무작위 시드를 생성합니다.

#### 사용자 정의 자리 표시자

워크플로에 사용자 정의 자리 표시자를 추가할 수 있습니다:

1. 사전 정의된 자리 표시자 아래의 "Custom" 섹션을 찾습니다.
2. "+" 버튼을 클릭하여 새 사용자 정의 자리 표시자를 추가합니다.
3. `find` 필드에 자리 표시자의 이름을 입력합니다.
4. `replace` 필드에 자리 표시자를 교체할 값을 입력합니다.

사용자 정의 자리 표시자는 사전 정의된 자리 표시자 아래의 별도 목록에 나타납니다.

예를 들어 기본 워크플로에서 저장된 이미지 파일 이름의 "SillyTavern" 접두사를 사용자 정의 자리 표시자로 교체할 수 있습니다. `find`를 `filename_prefix`로 설정하고 `replace`를 `ServiceTensor`로 설정한 새 사용자 정의 자리 표시자를 추가합니다. 워크플로 JSON에 새 `%filename_prefix%` 자리 표시자를 삽입합니다. 이제 사용자 정의 자리 표시자의 값을 변경하여 파일 이름 접두사를 SillyTavern에서 ServiceTensor로 변경할 수 있습니다.

+++ 자리 표시자가 있는 JSON
```json
{
    "class_type": "SaveImage",
    "inputs": {
        "filename_prefix": "%filename_prefix%",
        "images": ["8", 0]
    }
}
```
+++ 원본 JSON
```json
{
    "class_type": "SaveImage",
    "inputs": {
        "filename_prefix": "SillyTavern",
        "images": ["8", 0]
    }
}
```
+++

### Comfy 트릭

이 페이지의 모든 일반 정보를 읽어 이미지 생성 옵션에 익숙해지세요. 전환 가능한 스타일 및 공통 프롬프트 접두사와 같은 옵션을 ComfyUI 워크플로의 완전한 유연성과 결합하면 다양한 이미지 생성 설정을 만들 수 있습니다.

#### LoRA 로드

LoRA tag loader 노드([Load LoRA Tag](https://github.com/badjeff/comfyui_lora_tag_loader)와 같은)를 사용하여 프롬프트에 지정된 LoRA를 로드합니다.
이제 `<lora:CroissantStyle:0.8>`와 같은 태그로 프롬프트에 원하는 만큼 LoRA를 추가할 수 있으며 워크플로에 로드됩니다.
이렇게 하면 [캐릭터별 프롬프트 접두사](#character-specific-prompt-prefix)에서 LoRA를 사용하는 "pro-tip"도 ComfyUI에서 작동합니다.

#### 스타일 또는 slash 명령에서 워크플로 값 설정

사용자 정의 자리 표시자 값에서 매크로를 사용할 수 있습니다. 실용적인 예로
때때로 배경 없이 이미지를 생성하고 싶고 이것을 slash 명령 또는 이미지 스타일로 전환할 수 있기를 원한다고 가정해 보겠습니다. 다음과 같이 할 수 있습니다:

1. 입력 값에 따라 이미지 배경을 제거하거나 제거하지 않는 ComfyUI 워크플로를 만듭니다
2. 사용자 정의 자리 표시자를 사용하여 해당 입력의 값을 설정하지만 `{{getvar::remove_background}}`를 replace 값으로 사용합니다
3. 이제 이미지를 생성하기 전에 `/setvar key=remove_background true` 또는 `/setvar key=remove_background false`로 `remove_background` 값을 설정할 수 있습니다
4. 워크플로는 설정한 값을 사용하여 배경을 제거할지 여부를 결정합니다
5. 공통 프롬프트 접두사 `{{setvar::remove_background::true}}`로 이미지 스타일 "No background"를 만듭니다
6. 이미지를 생성하기 전에 스타일 컨트롤 또는 `/imagine-style No background`를 사용하여 `remove_background` 값을 `true`로 설정합니다
