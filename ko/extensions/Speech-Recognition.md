---
route: /ko/extensions/speech-recognition/
---

# Speech Recognition

이 가이드는 SillyTavern 내에서 음성을 텍스트로 변환하기 위한 음성 인식 설정 과정을 안내합니다.

## 사전 요구 사항

시작하기 전에 다음 사전 요구 사항을 충족했는지 확인하세요:

- 최신 버전의 SillyTavern을 사용하고 있는지 확인하세요.
- Extensions 패널(쌓인 블록 아이콘)의 "Download Extensions & Assets" 메뉴에서 "Speech Recognition" 확장 기능을 설치하세요.

## 음성 인식 설정 (브라우저)

1. **SillyTavern 구성**:
   - SillyTavern을 시작하고 **Extensions** > **Speech Recognition**으로 이동하세요.
   - 드롭다운 옵션에서 "Browser"를 선택하세요.
   - 브라우저가 음성 인식을 지원하지 않으면 오류 팝업이 나타납니다.

2. **Message Mode 선택**:
   - 원하는 "Message Mode"를 선택하세요:
     - **Append**: 메시지가 현재 사용자 메시지 텍스트 영역에 추가됩니다.
     - **Replace**: 메시지가 텍스트 영역의 현재 사용자 메시지를 대체합니다.
     - **Auto send**: 음성 끝이 감지되면 메시지가 자동으로 전송됩니다.

3. **Enable Message Mapping** *(선택 사항)*:
   - 음성 단축키를 위한 구문 매핑을 설정하세요.
   - 예를 들어 "command delete = /del2"를 추가하면 "command delete"가 감지될 때 음성 메시지가 "/del2" 명령으로 대체됩니다.
   - 완전한 음성 제어를 위해 auto send 모드와 결합하면 유용합니다. "Enable messages mapping"을 선택하여 활성화하세요.

4. **언어 선택**:
   - 말하고 싶은 언어를 선택하세요(참고: 모든 브라우저가 모든 언어를 지원하는 것은 아닙니다).

5. **녹음**:
   - 녹음을 시작하려면 전송 버튼 옆 메시지 영역 오른쪽에 있는 마이크 버튼을 클릭하세요. 녹음을 중지하려면 다시 클릭하세요. 음성이 감지되지 않으면 녹음이 자동으로 중지될 수 있습니다.

## 음성 인식 설정 (API 소스)

음성-텍스트 변환 API를 제공하는 OpenAI, MistralAI, Groq, Chutes, Z.AI 등의 소스를 지원합니다.

설정 방법:

1. Chat Completion API 설정에서 선택한 제공업체의 API 키를 입력하세요.
2. SillyTavern을 시작하고 **Extensions** > **Speech Recognition**으로 이동하세요.
3. 드롭다운 옵션에서 원하는 API 소스를 선택하세요.
4. "Browser" provider 설정과 마찬가지로 필요에 따라 추가 설정을 구성하세요.

## 음성 인식 설정 (Extras) - 지원 중단됨

!!!
ffmpeg 바이너리가 설치되어 있어야 합니다. 자세한 내용은 [RVC 설정](RVC.md#rvc-setup)을 참조하세요.
!!!

1. **Provider 활성화**:
   - 다음 명령을 사용하여 extras 서버에서 원하는 음성 인식 provider를 활성화하세요:
     ```shell
     python server.py --enable-modules=whisper-stt
     ```
     또는
     ```shell
     python server.py --enable-modules=vosk-stt
     ```
   - `--stt-vosk-model-path` 또는 `--stt-whisper-model-path` 옵션에 모델 경로를 추가하여 사용자 정의 모델을 사용할 수도 있습니다.

2. **SillyTavern 구성**:
   - SillyTavern을 시작하고 **Extensions** > **Speech Recognition**으로 이동하세요.
   - 드롭다운 옵션에서 "Vosk" 또는 "Whisper"를 선택하세요(whisper가 더 정확합니다).
   - 설정은 "Browser" provider 설정과 유사합니다(언어 제외) 위를 참조하세요.

## 음성 인식 설정 (스트리밍) - 지원 중단됨

!!!
ffmpeg 바이너리가 설치되어 있어야 합니다. 자세한 내용은 [RVC 설정](RVC.md#rvc-setup)을 참조하세요.
!!!

1. **Provider 활성화**:
   - 다음 명령으로 Sillytavern-extras에서 스트리밍 음성 인식 모듈을 활성화하세요:
     ```shell
     python server.py --enable-modules=streaming-stt
     ```

2. **SillyTavern 구성**:
   - (선택 사항) 위의 Whisper 설정과 같이 사용자 정의 Whisper 모델을 지정하세요.
   - (선택 사항이지만 권장) SillyTavern에서 트리거 단어를 설정하세요. 이러한 트리거 단어로 시작하는 메시지만 실제 메시지로 SillyTavern에 전송됩니다. 이렇게 하면 임의의 음성이나 소음이 전사되는 것을 방지합니다. 체크박스로 활성화하세요. 트리거 단어는 체크박스를 사용하여 실제 메시지에 포함/제외할 수 있습니다.
   - 다른 설정은 다른 provider와 유사합니다.

이제 SillyTavern에서 음성 인식을 사용하여 음성을 텍스트로 변환할 준비가 되었습니다.
