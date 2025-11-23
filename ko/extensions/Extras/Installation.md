---
icon: gear
label: 로컬 설치
route: /extensions/extras/installation/
---

# Extras 설치

이 페이지에는 로컬 장치에 SillyTavern Extras를 설치하는 지침이 포함되어 있습니다.

!!! Discontinued
Extras 프로젝트는 2024년 4월에 중단되었으며 새로운 업데이트나 모듈을 받지 않습니다. 대부분의 모듈은 기본 SillyTavern 애플리케이션에서 기본적으로 사용할 수 있습니다. 여전히 설치하고 사용할 수 있지만 문제가 발생하면 즉각적인 지원을 받을 것으로 기대하지 마세요.
!!!

Extras의 로컬 설치는 OS(특히 Termux)에서 어렵거나 불가능할 수 있습니다.

## [공식 Extras Colab](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb) 사용

* 설정이 간단합니다
* 무료 사용
* Colab GPU 크레딧 불필요(`use_cpu` 옵션 사용)
* 자세한 내용은 [Colab Guide Page](/extensions/Extras/Installation.md#running-extras-in-colab)를 참조하세요.

### Colab에서 Extras 실행

* [공식 Extras Colab](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb) 열기
* 원하는 "Extra" 옵션 선택
* GPU 크레딧 없이 Extras를 실행하려면 `use_cpu` 선택
  * 이렇게 하면 Stable Diffusion이 느려지지만 다른 모든 것은 정상적으로 실행됩니다
* 필수는 아니지만 권장: 공유 인스턴스를 보호하기 위해 API 키를 생성하려면 `secure` 옵션을 선택합니다.
* 왼쪽의 Start 버튼을 클릭합니다(삼각형 '재생' 버튼처럼 보임)
* 모든 것이 로드될 때까지 기다립니다
* 출력 하단에서 `trycloudflare.com` 링크를 찾습니다. localhost 링크는 무시하세요, 작동하지 않습니다(시도해 봤습니다!).
* `Running on` 텍스트로 시작합니다
* 해당 줄 아래에 나열된 API URL 링크를 복사합니다. (**localhost URL을 복사하지 마세요, 다른 것을 사용하세요**)
* extensions 지원으로 SillyTavern 시작: (필요한 경우 `config.yaml`에서 `enableExtensions`를 `true`로 설정)
* SillyTavern의 Extensions 메뉴로 이동합니다(페이지 상단의 'stacked blocks' 아이콘 클릭).
* 상단의 상자에 API URL을 붙여넣습니다. (**API Key 상자가 아님**)
* `secure` 옵션을 활성화하지 않은 경우 공식 colab을 사용할 때 API Key 상자가 완전히 비어 있는지 확인하세요.
* `secure` 옵션을 활성화한 경우 생성된 API 키를 API Key 상자에 붙여넣습니다.
* API 키는 colab의 콘솔 출력에 나타납니다. 예: `Your API key is fee2f3f559`
* "Connect" 클릭

---

## 로컬 설치 방법

### MiniConda (권장)

이 방법은 Conda가 Extras 요구 사항 패키지가 살 수 있는 '가상 환경'을 만들어 시스템 전체 Python 설정에 영향을 주지 않기 때문에 권장됩니다.

1. [Miniconda](https://docs.conda.io/en/latest/miniconda.html) 설치

    _(중요!) [Conda 사용 방법](https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html) 읽기_

2. [git](https://git-scm.com/downloads) 설치

    _(처음에 git으로 SillyTavern을 설치한 Chads는 이 단계를 건너뛸 수 있습니다!)_

    둘 다 설치한 후...

    아래 명령을 `CONDA COMMAND PROMPT WINDOW`에서 `하나씩` 입력/붙여넣고 각 명령 후에 `Enter`를 누르세요.

3. 새 Conda 환경 만들기(`extras`라고 부르겠습니다):

    `conda create -n extras`

4. 새 환경 활성화

    `conda activate extras` (명령 프롬프트의 왼쪽에 `(extras)`가 나타나야 합니다)

5. 필요한 시스템 패키지 설치(시간이 걸립니다)

    `conda install python=3.11 git`

6. Extras GitHub 저장소 복제

    `git clone https://github.com/SillyTavern/SillyTavern-extras`

7. 복제된 Extras 저장소로 이동

    `cd SillyTavern-extras`

8. 다음 명령 중 **하나**를 사용하여 Extras의 요구 사항 설치(다시 시간이 걸립니다):

   * `pip install -r requirements.txt` - 기본 기능용
   * `pip install -r requirements-rvc.txt` - 실시간 음성 복제용
   * `pip install -r requirements-coqui.txt` - Coqui TTS용(권장하지 않음)

    이 단계에서 오류가 발생하면 [Common Problems](/extensions/Extras/Installation.md#extras-install-common-problems) 페이지를 참조하세요!

9. 아래 'Running Extras After Install' 참조

---

### 시스템 전체 설치

이것은 더 쉽지만 시스템 전체 Python 설치에 영향을 줍니다.

요구 사항이 다른 많은 Python 프로그램으로 작업하는 경우 충돌이 발생할 수 있습니다.

Python 관련 작업을 처음 접하는 경우라면 문제가 되지 않을 것입니다.

1. Python 3.11 설치: <https://www.python.org/downloads/release/python-3115/>
2. git 설치: <https://git-scm.com/downloads>
3. 명령 프롬프트 창을 열고 완전한 액세스 권한이 있는 폴더로 이동합니다.
4. 저장소 복제: `git clone https://github.com/SillyTavern/SillyTavern-extras`, Enter를 누릅니다.
5. 복제가 완료되면 `cd SillyTavern-extras`를 입력하고 Enter를 누릅니다.
6. `python -m pip install -r requirements.txt`를 입력합니다
7. 아래 'Running Extras After Install' 참조

---

## 설치 후 Extras 실행

### 확장 기능이 활성화되어 있는지 확인

1. ST의 기본 설치 폴더에 있는 `config.yaml`이라는 파일을 텍스트 편집기에서 엽니다.
2. `enableExtensions`라고 읽는 줄을 찾습니다.
3. 해당 줄에 `false`가 아닌 `true`가 있는지 확인하세요.

### 사용할 모듈 결정

(한 번만 수행하면 됩니다)

* Extras는 항상 Python 명령줄로 시작됩니다.
* `python server.py`는 최소한이지만 유용한 모듈을 활성화하지 않습니다.
* 모듈을 활성화하려면 쉼표로 구분된 모듈 이름 목록과 함께 `--enable-modules=` 수정자를 사용해야 합니다

예: `python server.py --enable-modules=caption,summarize,classify`

이렇게 하면 Image Captioning, Chat Summary 및 live updating Character Expressions가 활성화됩니다.

아래는 각 모듈을 설명하는 표입니다.

| 이름         | 설명                                                         |
|--------------|---------------------------------------------------------------------|
| `caption`    | 이미지 캡션                                                    |
| `summarize`  | 텍스트 요약                                                  |
| `classify`   | 텍스트 감정 분류                                       |
| `sd`         | Stable Diffusion 이미지 생성                                   |
| `silero-tts` | [Silero TTS server](https://github.com/ouoertheo/silero-api-server) |
| `edge-tts`   | [Microsoft Edge TTS client](https://github.com/rany2/edge-tts)      |
| `chromadb`   | Vector storage server                                               |
| `coqui-tts`  | Coqui TTS                                                           |
| `rvc`        | 실시간 음성 복제                                             |

* Python 명령줄에 추가할 모듈을 결정합니다.
* 다음 단계에서 사용됩니다.

**참고: Python 명령의 모듈 목록에 공백이 전혀 없어야 합니다!`**

### Extras Server 시작

Extras 설치 폴더 내부의 명령 프롬프트 창에서 계속...

1. conda 환경이 활성화되어 있는지 확인합니다(Conda 설치 방법을 사용한 경우)
2. 환경이 활성화되지 않은 경우 `activate extras`를 입력합니다.
3. `python server.py --enable-modules=YOUR,SELECTED,MODULE,LIST,HERE`를 입력합니다
4. extras 서버가 로드됩니다.
5. 잠시 후 끝에 URL이 표시됩니다. 로컬 설치의 경우 기본적으로 `http://localhost:5100`입니다.
6. API URL을 복사합니다.

### ST를 Extras 서버에 연결

1. SillyTavern 서버를 시작하고 브라우저에서 SillyTavern 인터페이스를 봅니다.
2. Extensions 패널을 엽니다(페이지 상단의 'Stacked Blocks' 아이콘을 통해)
3. API URL을 입력 상자에 붙여넣습니다.
4. `Connect`를 클릭합니다.

Extras를 다시 실행하려면 환경을 활성화하고 명령 프롬프트에서 다음 명령을 실행하기만 하면 됩니다.

`conda activate extras`, Enter를 누릅니다.
`python server.py`, Enter를 누릅니다.

설정에 필요한 server.py의 추가 옵션을 포함해야 합니다(아래 참조).

## 쉬운 시작을 위한 .bat 파일 만들기

이것은 선택 사항이며 Windows에만 적용되지만 MacOS에서도 비슷한 것이 가능합니다.

1. Windows 데스크톱 보기
2. 마우스 오른쪽 버튼을 클릭하고 `New`를 선택한 다음 `Text Document`를 클릭합니다
3. 데스크톱에 새 파일이 나타나며 이름을 묻습니다.
4. 파일 이름을 `STExtras.txt`로 지정합니다
5. 텍스트 편집기에서 새로 만든 파일을 엽니다.
6. 다음 코드를 붙여넣습니다:

    ```
    cd C:\_your_\_full_\_Extras_\_folder_\_path_\
    call conda activate extras
    python server.py --enable-modules=YOUR,SELECTED,MODULE,LIST,HERE,WITH,NO,SPACES
    call conda deactivate
    pause
    ```

7. 자리 표시자 폴더 경로를 실제 Extras 설치 폴더 경로로 교체합니다.
8. python 명령줄을 실제 명령줄로 교체합니다
9. 파일을 새 이름 `STExtras.bat`로 저장합니다(대부분의 텍스트 편집기에서 `File` >> `Save As` 사용)

이제 이 .bat 파일을 두 번 클릭하기만 하면 Extras를 쉽게 시작할 수 있습니다.

모듈 목록(또는 extras 서버의 다른 명령줄 수정자)을 변경하려면 .bat 파일 내부의 python 명령을 편집하기만 하면 됩니다.

## Extras 설치 일반적인 문제

이 섹션에는 SillyTavern Extras를 설치하는 동안 발생하는 일반적인 질문과 문제가 나열되어 있습니다.

### 오류: Linux에서 'talkinghead' 모듈을 가져올 수 없음

Colab과의 비호환성으로 인해 자동으로 설치되지 않으므로 추가 패키지 설치가 필요합니다. 다른 요구 사항을 설치한 후 실행:

`pip install wxpython`

### Extras 서버가 AUTOMATIC1111의 Stable Diffusion Web UI에 연결할 수 없음

> Could not connect to remote SD backend at <http://127.0.0.1:7860>! Disabling SD module...

**Stable Diffusion을 시작하는 webui-user.bat의 COMMANDLINE_ARGS 변수에 --api 명령줄 옵션이 포함되어 있는지 확인하세요.**

"webui-user.bat"에서 해당 줄을 찾아 교체: `set COMMANDLINE_ARGS=--api`

![How it should look](/static/extensions/sd-user.png)

SD Web UI에 대해 API 모드가 비활성화되면 Extras 서버가 연결할 수 없으며 이미지를 생성할 수 없습니다!

#### 여전히 작동하지 않나요?

모든 것을 적절한 순서로 시작하고 다음 단계로 진행하기 전에 모든 프로그램이 로드를 완료할 때까지 기다리는지 확인하세요:

1. Stable Diffusion Web UI
2. SillyTavern Extras
3. SillyTavern

extras 서버는 나중에 로드된 경우 Stable Diffusion API에 다시 연결할 수 없습니다.

### ChromaDB를 설치할 때 hnswlib wheel 빌드 오류

> ERROR: Could not build wheels for hnswlib, which is required to install pyproject.toml-based projects

ChromaDB 모듈을 설치하기 전에 먼저 `다음 중 하나를` 수행해야 합니다:

* Visual C++ build tools 설치: <https://visualstudio.microsoft.com/visual-cpp-build-tools/>
* conda로 `hnswlib` 패키지 설치: `conda install -c conda-forge hnswlib`

---

### Mac에서 Python 요구 사항을 설치할 때 오류

> ERROR: No matching distribution found for torch==2.0.0+cu117

Mac은 CUDA를 지원하지 않으므로 torch 패키지는 CUDA 지원 없이 설치해야 합니다.

대신 `requirements-silicon.txt` 파일을 사용하여 요구 사항을 설치하세요.

---

### 모듈 누락?

* `--enable-modules` 수정자를 사용하여 Python 명령줄에 모듈 이름 목록을 지정해야 합니다.
* [Modules](/extensions/Extras/Installation.md#decide-which-module-to-use) 섹션 참조.

---

### API Key 상자는 무엇을 위한 것인가요?

* SillyTavern의 Extensions 패널의 API Key 상자는 다음을 수행한 경우에만 사용됩니다:
  * Extras 설치 폴더에 선택한 Extras '비밀번호'가 포함된 `api_key.txt`라는 텍스트 파일을 만들었습니다.
  * `--secure` 명령줄 인수로 extras를 시작했습니다.
* 이렇게 하면 Extras API가 '비밀번호 잠금'되어 API Key 상자에 해당 키가 있는 사용자만 액세스할 수 있습니다.
* 이것은 주로 자체 공개 Extras 배포(colab 등)를 만들려는 사람들에게 유용합니다.
* 개인 사용을 위해 PC에서 Extras를 실행하는 사용자는 API Key 상자에 아무것도 입력하지 않아야 합니다.

### 모바일/Android/Termux는 어떻습니까? 🤔

* 커뮤니티의 일부 사람들은 Termux의 Ubuntu를 통해 휴대폰에서 Extras를 실행하는 데 성공하고 있습니다.
* 그러나 Extras는 모바일 지원을 염두에 두고 만들어지지 않았습니다.
* Android 장치에서 Extras를 실행하는 사람들에게는 지원이 제공되지 않습니다.
* 아래 링크된 가이드 작성자에게 모든 질문을 문의하세요.

#### ❗ 이것은 지원되지 않습니다

<https://rentry.org/STAI-Termux#downloading-and-running-tai-extras>
