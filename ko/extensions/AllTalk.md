---
order: tts-alltalk
route: /extensions/alltalk/
---
# AllTalk TTS V2

AllTalk은 Coqui XTTS, F5-TTS, VITS, Piper 및 기타 TTS 모델 엔진을 기반으로 하는 음성 복제 시스템으로 고품질 음성 재현(제로샷 음성 복제 또는 내장 음성)을 생성하도록 설계되었습니다. AllTalk V2에서는 여러 TTS 엔진 지원, 확장된 사용자 정의 및 성능 최적화를 포함하여 기능과 사용 편의성을 향상시키는 중요한 업데이트가 있습니다. 포괄적인 기능 목록은 [AllTalk Wiki 여기](https://github.com/erew123/alltalk_tts/wiki)를 참조하세요.

---

## 🟩 AllTalk V2의 주요 기능
- **다중 엔진 지원**: Coqui XTTS, VITS, Piper, Parler, F5 및 사용자 정의 엔진 간에 쉽게 전환할 수 있습니다.
- **음성 변환(RVC)**: 향상된 검색 기반 음성 복제 파이프라인.
- **사용자 정의 가능한 설정**: 엔진별 설정을 조정하고 시작 구성을 저장합니다.
- **내레이터 기능**: 내레이션과 캐릭터에 대해 별도의 음성을 지정합니다.
- **독립형 및 통합 사용**: SillyTavern과 원활하게 통합됩니다.
- **DeepSpeed 및 Low VRAM 모드**: 리소스가 제한된 환경에 대한 성능 최적화.
- **스크린샷**: AllTalk V2의 인터페이스를 [여기](https://github.com/erew123/alltalk_tts/discussions/237)에서 확인하세요.

---

## 🟨 설정 및 설치 옵션

AllTalk은 독립형 및 통합 설치 방법을 모두 제공합니다. 가장 빠른 설정은 제공된 빠른 설치 옵션 중 하나를 사용하는 것이며 스크립트가 대부분의 프로세스를 자동화합니다.

- **독립형 설치**: 대부분의 사용자에게 권장됨 ([독립형 가이드](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Standalone-Installation))
- **Text-generation-webui 통합**: Text-generation-webui에 통합하기 위해 ([TGWUI 설치 가이드](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Text%E2%80%90generation%E2%80%90webui-Installation))

#### 🟩 자동 설치
**이 방법은 Windows 사용자 전용입니다.**
빠른 설정을 원하는 새 사용자의 경우 자동 설치는 SillyTavern-Launcher를 사용합니다.
참고: 이는 SillyTavern-Launcher를 이미 설치했다고 가정합니다. 설치하지 않은 경우 https://github.com/SillyTavern/SillyTavern-Launcher를 방문하여 readme.md 파일의 지침에 따라 설치하세요.
SillyTavern-Launcher가 설치되면:
1. Launcher.bat 실행
2. `Home > Toolbox > App Installer > Voice Generation`으로 이동
3. **Install AllTalk V2** 레이블이 있는 옵션 선택

#### 🟩 수동 설치
자세한 제어가 필요한 고급 사용자의 경우 Windows, Linux 또는 Mac(테스트되지 않음)에서 단계별 설정을 위해 [수동 설치 가이드](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Manual-Installation-Guide)를 따르세요.

#### 🟩 Google Colab 설치
로컬로 설치하지 않으려는 사용자를 위해 클라우드 환경에서 AllTalk을 실행하려면 [Google Colab 설치](https://github.com/erew123/alltalk_tts/wiki/Google-COLAB)를 사용하세요.

---

## 🟨 SillyTavern 내에서 AllTalk 사용

AllTalk이 로드되면 SillyTavern의 TTS 페이지에서 선택하고 설정에서 올바른 AllTalk 서버 버전을 선택했는지 확인하세요.

- **설정 관리**: AllTalk은 선택한 구성에 따라 특정 설정을 활성화하거나 비활성화할 수 있습니다.
- **로딩 순서**: AllTalk 전에 SillyTavern이 로드된 경우 TTS 확장 페이지를 다시 로드하세요.
- **성능 최적화**: 시스템 리소스에 따라 성능을 향상시키기 위해 DeepSpeed 및 Low VRAM 모드를 선택적으로 활성화하세요.
- **내레이터 기능**: 내레이터 기능에 대한 자세한 내용은 [AllTalk Wiki](https://github.com/erew123/alltalk_tts/wiki/Narrator-Function)에서 찾을 수 있습니다.

SillyTavern AllTalk Extension의 전체 세부 정보는 [AllTalk Wiki 페이지 for SillyTavern](https://github.com/erew123/alltalk_tts/wiki/SillyTavern-Extension)에서 업데이트될 예정입니다.

TGWUI AllTalk 확장을 사용하는 TGWUI 사용자는 TGWUI 채팅 인터페이스에서 `Enable TGWUI TTS`를 비활성화해야 합니다. 그렇지 않으면 중복 TTS 오디오가 생성됩니다.

---

## 🟨 문제 해결

SillyTavern 내 AllTalk과 관련된 문제가 발생하면 최신 정보는 [AllTalk Wiki 페이지 for SillyTavern](https://github.com/erew123/alltalk_tts/wiki/SillyTavern-Extension)을 참조하세요.

---

### 🟪 지원, 지원 및 기능 요청

추가 지원:
- [Wiki](https://github.com/erew123/alltalk_tts/wiki) 및 내장 문서를 참조하세요.
- [Discussion Board](https://github.com/erew123/alltalk_tts/discussions/245)에서 토론에 참여하세요.
- [Issue Tracker](https://github.com/erew123/alltalk_tts/issues)를 통해 버그 또는 기능 요청을 제출하세요.

---
