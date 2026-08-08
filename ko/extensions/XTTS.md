---
order: tts-xtts
route: /ko/extensions/xtts/
---

# 음성 복제 기능이 있는 XTTS

안녕하세요! AI 텍스트 음성 변환 기술이 얼마나 발전했는지 보여주는 Reddit 게시물에 감명을 받으셨나요?

로봇 와이푸/허즈번도에게 새로운 멋진 음성 변조기를 주고 싶으신가요?

두려워하지 마세요, 이 놀라운 획기적인 기술은 이미 로컬 SillyTavern에서 사용할 수 있습니다. 간단한...

## 사전 요구 사항

1. SillyTavern 최신 버전.
2. [Miniconda](https://docs.conda.io/projects/miniconda/en/latest/miniconda-install.html) 설치.
3. (Windows) [Visual C++ Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/) 설치.
4. 복제할 음성 클립이 있는 WAV 파일(파일당 ~10초). 파일 요구 사항: PCM, Mono, 22050Hz, 16-bit (Audacity를 통해 변환).
5. "speakers" 및 "output" 하위 폴더가 있는 폴더를 만듭니다. WAV 파일을 "speakers"에 넣습니다.

예제 폴더 구조:
```
C:\xtts
  - speakers
    - alice.wav
    - bob.wav
  - output
```

## 설치

[daswer123](https://github.com/daswer123)는 컴퓨터에서 XTTSv2 모델을 실행하고 SillyTavern의 TTS 확장 기능에 연결하는 API 서버를 만들었습니다.

Extras API와 완전히 독립적이며 별도의 환경을 사용합니다.

**매우 중요:** 다음 요구 사항을 Extras 환경이나 시스템 Python에 설치하지 마세요.
다른 패키지가 손상되고 불필요한 다운그레이드 등이 발생합니다.

다음 지침은 Miniconda를 사용하여 제공되지만 venv로도 수행할 수 있습니다(여기서는 다루지 않음).
Anaconda 명령 프롬프트를 열고 지침을 한 줄씩 따르세요.

### 서버 시작 및 실행

1. 사전 요구 사항 4단계에서 만든 폴더로 이동합니다.
    ```
    cd C:\xtts
    ```
2. 새 conda 환경을 만듭니다. 이제부터 `xtts`라고 부르겠습니다.
    ```
    conda create -n xtts
    ```
3. 새로 만든 환경을 활성화합니다.
    ```
    conda activate xtts
    ```
4. 환경에 Python 3.10을 설치합니다. 메시지가 표시되면 "y"로 확인합니다.
    ```
    conda install python=3.10
    ```
5. 요구 사항과 함께 XTTS 서버를 설치합니다.
    ```
    pip install xtts-api-server pydub
    ```
6. PyTorch를 설치합니다. 시간이 걸릴 수 있습니다. 다음 줄은 GPU 가속 지원(CUDA)과 함께 PyTorch를 설치합니다.
CPU 추론만 사용하려면 `--index-url`로 시작하는 마지막 부분을 삭제하세요.
    ```
    pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
    ```
7. 기본 호스트 및 포트에서 XTTS 서버를 시작합니다: <http://localhost:8020>
    ```
    python -m xtts_api_server
    ```
8. 첫 번째 시작 중에 모델이 다운로드됩니다(약 ~2GB).
Coqui AI의 법적 고지를 매우 주의 깊게 읽는 것을 잊지 마세요. 농담입니다, 다시 "y"를 누르세요.

### SillyTavern에 연결

1. extensions 패널을 열고 TTS 메뉴를 확장한 다음 provider 목록에서 "XTTSv2"를 선택합니다.
2. Language 드롭다운에서 텍스트 음성 변환 언어를 선택합니다(폴란드어가 아니라면 슬플 것입니다).
3. provider endpoint가 <http://localhost:8020>을 가리키고 "Available voices"에 음성 샘플 목록이 표시되는지 확인합니다.
4. 캐릭터를 선택하고 음성 샘플과 캐릭터 간의 매핑을 설정합니다.
캐릭터 목록이 비어 있으면 "Reload"를 몇 번 누르세요.
5. 기본 설정에 따라 나머지 TTS 설정을 구성합니다.

### 이제 모두 준비되었습니다!

모든 메시지의 context actions 메뉴에서 확성기 아이콘을 클릭하고 스피커에서 나오는 아름다운 복제된 음성을 들으세요. 생성에는 시간이 걸리며 고급 RTX GPU에서도 실시간이 아닙니다.

### 스트리밍?

최신 버전의 XTTS 서버와 함께 HTTP 스트리밍을 사용하여 사용 가능한 즉시 생성된 오디오 청크를 받을 수 있습니다!

#### RVC와 함께 작동하지 않습니다!

오디오는 여전히 생성되고(최신 버전의 RVC 확장 기능을 사용한다고 가정) 변환되지만 RVC가 변환을 시작하기 전에 전체 오디오 파일이 필요하므로 *스트리밍되지 않습니다*. 스트리밍 RVC는 여전히 조사 중입니다...

#### 스트리밍 지원을 받는 방법은?

1. SillyTavern을 최신 버전으로 업데이트합니다.
2. XTTS 서버를 최신 버전으로 업데이트합니다.

    ```bash
    conda activate xtts
    pip install xtts-api-server --upgrade
    ```

3. 평소와 같이 XTTS를 시작하고 ST에 연결합니다.
4. SillyTavern에서 "Streaming" XTTS 확장 기능 설정을 활성화합니다.

### 오디오가 끊기나요?

"chunk size" 설정을 늘려보세요.

참고: chunk size가 200일 때 RTX 3090은 약간 증가된 오디오 대기 시간을 희생하여 중단 없는 오디오를 생성할 수 있습니다.

### TTS 서버를 다시 시작하는 방법은?

설치 지침의 1, 3, 7단계만 수행하면 됩니다.

### Android??

unlikely, PyTorch가 필요한 앱을 실행할 수 없습니다. 지원하지 않는 신비한 흑마술 없이는요. 자신의 위험을 감수하고 시도할 수 있지만 문제가 발생하면 지원이 제공되지 않습니다.

최선의 해결책은 로컬 네트워크를 통해 PC에서 TTS API를 호스팅하는 것입니다. 수신할 호스트와 포트를 지정하는 것을 잊지 마세요 - [README](https://github.com/daswer123/xtts-api-server/blob/main/README.md)를 참조하세요.
