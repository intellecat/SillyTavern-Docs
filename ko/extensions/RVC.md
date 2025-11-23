---
route: /extensions/rvc/
---

# Retrieval-based Voice Conversion (RVC)

이 가이드는 한 오디오 클립에서 다른 오디오 클립으로 음성 특성을 전송하여 다양한 톤과 스타일로 음성을 말할 수 있게 하는 기술인 RVC 사용 방법을 안내합니다.

유명한 "Presidents Play X" 동영상을 본 적이 있나요? 이들은 RVC를 사용하여 만들어졌습니다. RVC 확장 기능을 사용하면 SillyTavern 캐릭터가 애니메이션, 영화 또는 심지어 자신만의 고유한 음성 등 원하는 모든 음성으로 말하도록 할 수 있습니다.

RVC는 TTS가 아닙니다: 음성 간 변환에 가깝습니다. 입력으로 오디오 클립을 받습니다. 백그라운드에서 RVC가 하는 일은 SillyTavern의 TTS 확장 기능과 함께 작동하는 것입니다: TTS가 오디오 파일을 생성할 때까지 기다린 다음(RVC 사용 여부와 관계없이 TTS가 했을 작업), RVC가 TTS 오디오 파일을 가져와 RVC 구성에서 복제된 음성으로 변환하는 두 번째 처리를 수행합니다.

## RVC 설정

SillyTavern의 RVC는 오디오 변환을 수행하는 여러 API 소스를 지원합니다:

* [rvc-python](https://github.com/daswer123/rvc-python)
* [SillyTavern Extras](https://github.com/SillyTavern/SillyTavern-Extras) (더 이상 사용되지 않음)

### 공통 사전 요구 사항

시작하기 전에 다음 사전 요구 사항을 충족했는지 확인하세요.

#### ffmpeg

PATH 환경 변수에 `ffmpeg` 바이너리가 있는지 확인하세요. 이 도구는 들어오는 오디오를 변환하는 데 사용됩니다.

**Windows**:

* SillyTavern Launcher 스크립트의 Toolbox를 사용하여 ffmpeg를 자동으로 설치합니다: <https://github.com/SillyTavern/SillyTavern-Launcher>
* 또는 여기에서 빌드를 다운로드합니다: <https://www.gyan.dev/ffmpeg/builds/>
* PATH 변수를 수정하는 방법: <https://www.architectryan.com/2018/03/17/add-to-the-path-on-windows-10/>
* 제대로 했는지 테스트하려면 명령 프롬프트를 열고 ```ffmpeg```를 실행합니다. ffmpeg 버전과 정보가 출력되어야 합니다.

**Linux**:

패키지 관리자를 사용하여 ffmpeg를 설치합니다.

```shell
# Debian/Ubuntu
sudo apt install ffmpeg
# Arch Linux
sudo pacman -S ffmpeg
# Fedora
sudo dnf install ffmpeg
```

**macOS**:

[Homebrew](https://brew.sh/)를 사용하여 ffmpeg를 설치합니다:

```shell
brew install ffmpeg
```

#### TTS가 활성화되어 작동하는지 확인

RVC는 TTS에 의존하므로 TTS 확장 기능을 활성화해야 합니다. RVC를 추가하기 전에 TTS가 이미 제대로 작동하고 채팅을 내레이션하고 있어야 합니다!

다음 사항에 유의하세요:

* 시스템 TTS 엔진은 음성 변환을 전혀 지원하지 않습니다.
* 스트리밍 TTS는 변환 전에 오디오 스트림이 끝날 때까지 기다립니다.

#### 확장 기능 설치

Extensions 패널(쌓인 블록 아이콘)의 "Download Extensions & Assets" 메뉴에서 "RVC" 확장 기능을 설치합니다.

#### SillyTavern에서 RVC 활성화*

SillyTavern에서 **Extensions** > **RVC**로 이동하여 활성화합니다.

#### 소스 선택

확장 기능 설정에서 사용할 RVC 소스를 선택합니다. 그런 다음 소스별 설치 지침을 진행합니다.

### rvc-python 설정

#### 1. 패키지 설치

GitHub 페이지의 설치 지침을 따르세요: [rvc-python Installation](https://github.com/daswer123/rvc-python?tab=readme-ov-file#installation). Nvidia GPU가 있는 경우 CUDA 설치 지침을 따르는 것이 좋습니다.

Windows에 설치할 때 문제가 발생하는 경우(예: fairseq 빌드 단계 실패) PC에 다음 소프트웨어가 설치되어 있는지 확인하세요:

* [Windows 10 SDK](https://developer.microsoft.com/en-us/windows/downloads/windows-sdk/)
* [Visual Studio Build Tools 2022](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2022)

#### 2. 모델 준비

RVC 모델을 저장할 디렉토리를 만듭니다. 기본적으로 `rvc_models`라는 이름이 지정되며 서버를 시작할 때 현재 디렉토리에서 선택됩니다. 모든 모델은 UI에 표시되는 이름의 하위 폴더이며 `.pth`(필수) 및 `.index`(선택 사항) 파일을 포함해야 합니다.

자세한 내용: [rvc-python Model Management](https://github.com/daswer123/rvc-python?tab=readme-ov-file#model-management)

#### 3. API 서버 시작

다음 명령을 실행하여 API 서버를 시작합니다:

```shell
python -m rvc_python api -p 5050 -l -md models_path
```

**인수:**

* `5050` - 서버의 수신 포트를 설정합니다. 다른 포트에서 호스팅하려면 변경하세요.
* `models_path` - 모델의 경로를 설정합니다. 기본 `rvc_models` 디렉토리를 사용하려면 제거하세요.
* `-l` - 서버가 모든 네트워크 인터페이스에서 수신하도록 설정합니다. localhost에서만 수신하려면 제거하세요.

#### 4. 서버에 연결

* RVC 확장 기능 설정에서 적절한 **rvc-python API URL**을 설정합니다. 기본적으로 `http://localhost:5050`입니다.
* CUDA 가속을 지원하도록 rvc-python을 설치한 경우 **Use CUDA** 체크박스를 선택합니다.
* "Refresh"를 눌러 사용 가능한 음성 목록을 로드합니다.

#### 5. 음성 맵 구성

**음성 맵은 모든 캐릭터 또는 사용자 페르소나에 대한 음성 변환 설정을 정의합니다.**

* 음성 맵을 설정하려면 "Character" 드롭다운에서 캐릭터 또는 페르소나 이름을 선택한 다음 RVC "Voice"를 선택하고 Apply를 클릭합니다.
* 선택적으로 피치 보정 또는 필터링과 같은 다른 관련 설정을 구성할 수도 있습니다.
* 모든 것을 올바르게 수행했다면 Voice Map 디버그 영역에 'Betty:MyVoice(rvpme)'와 같은 내용이 표시됩니다.

### SillyTavern Extras 설정

#### 1. RVC 모델 파일 준비

* 파일 브라우저에서 `\SillyTavern-extras\data\models\rvc`로 이동합니다.
* 'Betty'와 같은 하위 폴더를 만들고 `.pth` 및 `.index` 파일을 넣습니다. (힌트: https://voice-models.com에서 음성 파일을 다운로드할 수 있습니다. 음성 이름이 RVPME라고 표시되어 있는지 확인하세요.)

#### 2. 요구 사항 설치

다음 명령을 사용하여 필요한 요구 사항을 설치합니다:

```shell
pip install -r requirements-rvc.txt`
```

#### 3. RVC를 활성화한 상태로 SillyTavern-extras 실행

RVC 모듈을 활성화한 상태로 SillyTavern-extras를 시작합니다. 이 예제 호출은 SillyTavern-extras와 함께 사전 설치된 Edge TTS를 사용했다고 가정합니다:

```shell
python server.py --enable-modules=rvc,edge-tts
```

선택적으로, 가능한 GPU가 있는 경우 시작 명령에 ```--cuda```를 추가하여 GPU에서 RVC를 실행할 수 있습니다. 빠른 테스트에 따르면 VRAM 사용량은 50 토큰(~36 단어) 내레이션의 경우 3.4GB, 200 토큰(~150 단어)의 경우 7.6GB였습니다.

#### 4. 음성 매핑 설정

RVC용 음성 맵을 만듭니다. Character를 원하는 SillyTavern 캐릭터 이름으로 설정하고 Voice를 1단계에서 만든 RVC 폴더로 설정한 다음 Apply를 클릭합니다. 제대로 했다면 Voice Map에 'Betty:MyVoice(rvpme)'와 같은 내용이 표시됩니다.

#### 5. 피치 추출 선택

* 피치 추출 방법으로 "rmvpe"를 선택합니다.
* "rmvpe"에 문제가 있는 경우 다른 방법을 시도하세요(예: "harvest" 또는 "torchcrepe").

#### 6. (선택 사항) 생성 파일을 저장하도록 RVC 구성

테스트 또는 문제 해결 목적으로 생성된 RVC 오디오를 저장하려면 시작 명령에 ```--rvc-save-file```을 추가합니다. 그러면 마지막 생성이 `SillyTavern-extras/data/tmp/rvc_output.wav` 아래에 저장됩니다:

```shell
python server.py --enable-modules=rvc,edge-tts --rvc-save-file
```

#### 표정 기반 동적 음성

##### 1. RVC 모델 구성

RVC 모델 폴더에 분류된 각 표정(예: anger, fear, joy, love, sadness, surprise)에 대한 별도의 `.pth` 및 `.index` 파일을 포함합니다.

##### 2. 모듈 활성화

RVC 및 classify 모듈을 모두 활성화합니다:

```shell
python server.py --enable-modules=rvc,classify
```

##### 3. RVC 모듈 사용

나머지 설정은 RVC 모듈만 사용하는 것과 유사합니다(위에서 설명한 대로).

## 자신만의 RVC 모델 훈련

### RVC Easy Menu by Deffcolony 사용 (Windows만 해당)

Mangio-RVC를 자동으로 설치하고 시작합니다: https://github.com/deffcolony/rvc-easy-menu

#### 1. 저장소 복제

원하는 위치에 저장소를 복제합니다:

```shell
git clone https://github.com/deffcolony/rvc-easy-menu.git
```

#### 2. RVC-Launcher.bat 시작

* `RVC-Launcher.bat` 파일을 엽니다.
* RVC를 설치하려면 옵션 1을 선택합니다.

#### 3. 설치 완료

메시지가 표시되면 필요한 패키지와 종속성을 설치합니다.

#### 4. 음성 훈련을 위한 WebUI 열기

설치 후 옵션 2를 선택하여 음성 훈련을 위한 WebUI를 엽니다.

### Mangio-RVC: 음성 모델 훈련

***데이터셋 준비***:

**1. 오디오 준비**:

* 훈련하려는 오디오를 `datasets` 폴더에 넣습니다.
* 오디오에 배경 소음이 없는지 확인하세요 – 순수한 음성만 필요합니다.
* 오디오가 길수록 출력 품질이 좋아집니다.

***WebUI 훈련***:

**1. Training 탭 액세스**:

* WebUI에서 training 탭을 클릭합니다.

**2. 실험 구성**:

* 실험 이름을 입력합니다(예: `my-epic-voice-model`).
* 버전을 v2로 설정합니다.

**3. 데이터 처리 및 특성 추출**:

* "Process data" 및 "Feature extraction"을 클릭합니다.
* "Save frequency"를 50으로 설정합니다.

**4. 훈련 매개변수**:

* "Total training epochs"를 300으로 설정합니다.
* "Train feature index" 및 "Train model"을 클릭합니다.
