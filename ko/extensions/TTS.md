---
order: tts
route: /extensions/tts/
---

# TTS

SillyTavern은 채팅의 일부를 음성으로 낭독하는 데 사용되는 다양한 TTS(text-to-speech) 옵션을 제공합니다. 이 페이지에서는 설정 및 사용법을 설명합니다.

## TTS 구성

### TTS 공급자 선택

사용하려는 TTS 서비스를 선택하는 데 사용됩니다. 일부 옵션은 무료이고, 일부는 유료 구독이 필요하며, 일부는 PC에서 로컬로 실행됩니다.

사용 가능한 옵션(시간이 지남에 따라 목록이 변경될 수 있음):

- **AllTalk** - 무료, 오픈 소스 로컬 설치, 다양한 TTS 엔진 제공. 설정 지침은 [AllTalk](./AllTalk.md) 페이지를 참조하세요.
- **Azure TTS** - Microsoft Edge와 동일한 음성. Azure 계정 및 유료 구독이 필요합니다.
- **Coqui-TTS** (더 이상 사용되지 않음) - 무료, 실행하려면 Extras API가 필요합니다. 고성능 Text2Speech 모델(Tacotron, Tacotron2, Glow-TTS, SpeedySpeech) 및 Bark.
- **Edge** - 무료, Azure를 통해 실행. 공급자로 "Plugin"을 선택한 경우 [이 서버 플러그인](https://github.com/SillyTavern/SillyTavern-EdgeTTS-Plugin)도 설치해야 합니다. 다른 옵션은 Extras API(더 이상 사용되지 않음)를 실행해야 합니다.
- **Electron Hub** - [Electron Hub](https://electronhub.ai/) API 키를 재사용하여 모델별 제어로 클라우드 음성(GPT-4o Mini TTS, Microsoft neural voices 등)에 액세스합니다.
- **ElevenLabs** - 유료 구독 필요. [ElevenLabs](https://elevenlabs.io/)에서 API 키를 받으세요.
- **Google Translate** - Google에서 제공하는 무료 음성, 언어당 하나, 품질은 크게 다를 수 있습니다.
- **Google Gemini TTS** - [Vertex AI](/Usage/API_Connections/google.md#google-vertex-ai) 또는 [AI Studio](/Usage/API_Connections/google.md#google-ai-studio)에서 API 키가 필요하며, [Gemini TTS](https://cloud.google.com/text-to-speech/docs/gemini-tts) 모델을 사용합니다.
- **Kokoro** - 무료, [kokoro.js](https://www.npmjs.com/package/kokoro-js)를 사용하여 브라우저에서 로컬로 모델을 실행합니다. 그러나 [일부 브라우저](https://caniuse.com/webgpu)는 장치 옵션에 대해 WebGPU를 지원하지 않을 수 있습니다.
- **MiniMax** - [MiniMax](https://www.minimax.io/)의 API 키가 필요합니다. 설정 지침은 [MiniMax TTS](./MiniMaxTTS.md) 페이지를 참조하세요.
- **Novel** - 유료 NovelAI 구독 필요, NovelAI의 TTS 엔진에서 생성
- **OpenAI** - 유료 API 키 필요, OpenAI의 TTS 모델을 사용합니다.
- **Pollinations** - OpenAI TTS 모델에 무료로 액세스할 수 있지만 속도 제한이 있습니다. [웹사이트](https://pollinations.ai/).
- **Silero** - 무료, PC에서 실행, 품질은 크게 다를 수 있습니다. [전용 API 서버](https://github.com/ouoertheo/silero-api-server) 설치 또는 Extras API(더 이상 사용되지 않음)가 필요합니다.
- **System** - OS TTS 엔진을 사용합니다(있는 경우). 품질은 OS에 따라 크게 다를 수 있습니다.
- **XTTS** - 무료, 전용 API 서버 설치가 필요합니다. 설정 지침은 [XTTS](./XTTS.md) 페이지를 참조하세요.

### 체크박스

- **Enabled** - TTS 재생 켜기/끄기
- **Auto Generation** - 새 메시지가 채팅에 들어올 때 TTS가 자동으로 재생되도록 합니다
- **Only narrate "quotes"** - TTS 재생을 `"따옴표"` 내의 텍스트만 포함하도록 제한합니다. 이것은 `*별표 줄 내의 "따옴표"도 포함*`합니다 (내부 변수 이름 = `narrate_quoted_only`)
- **Ignore \*text, even "quotes", inside asterisks\*** - TTS는 `*별표*` 내의 텍스트를 재생하지 않으며, "따옴표"도 포함하지 않습니다 (내부 변수 이름 = `narrate_dialogues_only`)
- *"only narrate quotes"와 "ignore asterisks" 체크박스를 모두 체크하면 TTS는 별표 안에 없는 "따옴표"만 읽고 다른 모든 것은 무시합니다.*
- **Narrate only the translated text** - 번역된 텍스트만 낭독합니다.

예제 텍스트: `*Cohee approaches you with a faint "nya"* "Good evening, senpai", she says.`
다음은 **Ignore \*text, even "quotes", inside asterisks\***와 **Only narrate "quotes"**의 부울 상태에 따라 텍스트가 어떻게 수정되는지 보여주는 표입니다:

| **Ignore \*text, even "quotes", inside asterisks\*** 	 | **Only narrate "quotes"**	 | **출력**                                                                |
|:-------------------------------------------------------|:---------------------------|:--------------------------------------------------------------------------|
| Disabled                                               | 	Disabled	                 | Cohee approaches you with a faint "nya" "Good evening, senpai", she says. |
| Disabled                                               | Enabled	                   | "nya"... "Good evening, senpai"                                           |
| Enabled	                                               | Disabled	                  | "Good evening, senpai", she says.                                         |
| Enabled	                                               | Enabled	                   | "Good evening, senpai"                                                    |

### 슬라이더

선택한 API에 따라 변경됩니다.

### 버튼

- **Apply** - TTS API를 설정한 후 및 음성 맵을 편집한 후 클릭해야 합니다.
- **Refresh** - 선택한 TTS API에서 음성 목록을 다시 로드합니다.
- **Available voices** - 선택한 API에 사용 가능한 모든 음성이 포함된 팝업을 로드하고 샘플 대화로 미리 들을 수 있습니다.

## TTS 사용

1. "Enable" 체크박스를 클릭하세요. 그렇지 않으면 아무 일도 일어나지 않습니다.
2. 새 메시지가 채팅에 도착할 때마다 TTS가 자동으로 시작되도록 하려면 "Auto-generation" 체크박스를 클릭하세요.
3. 선택적으로 메시지 오른쪽 상단에 있는 메가폰 아이콘을 클릭하여 요청 시 재생하세요.
4. 재생을 중지하려면 오른쪽 하단의 "Stop" 버튼(지팡이 메뉴 내부)을 클릭하세요.

### 음성 맵

TTS가 사용할 음성 맵을 제공해야 하며, 그렇지 않으면 각 캐릭터에 어떤 음성을 사용해야 하는지 알 수 없습니다. 음성 맵을 설정하려면 먼저 음성을 할당하려는 캐릭터와 채팅을 열고 및/또는 음성을 할당하려는 사용자 페르소나를 선택한 다음 드롭다운에서 TTS 공급자가 나열한 음성을 선택하세요. 음성 및/또는 캐릭터 목록이 표시되지 않으면 TTS 공급자가 올바르게 구성되었는지 확인하고 "Refresh"를 클릭하세요. 일부 공급자(OpenAI 호환 또는 NovelAI 등)는 음성 목록을 수동으로 채워야 합니다.
