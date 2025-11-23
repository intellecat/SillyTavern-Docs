---
route: /extensions/talkinghead/
tags: ['obsolete']
---

# talkinghead

!!!warning
**TALKINGHEAD에 대한 지원은 SILLYTAVERN 1.12.13에서 중단되었습니다. 이 페이지는 역사적 목적으로 유지됩니다.**
!!!

### 이게 뭔가요?

AITuber용 Talking Head Anime 3 Demo의 구현입니다. 다음 기능을 보유하고 있습니다:

- 단일 정적 이미지에서 무작위 Live 2D 유사 모션 동작을 생성합니다.
- 모든 TTS 출력의 사운드 출력에 립싱크합니다.

이 확장 기능에는 Talking Head(?) Anime from a Single Image 3: Now the Body Too 프로젝트에 대한 원래 데모 프로그램이 포함되어 있습니다. 이름에서 알 수 있듯이 이 프로젝트를 사용하면 애니메이션 캐릭터를 애니메이션화할 수 있으며 해당 캐릭터의 단일 이미지만 있으면 됩니다. 두 가지 데모 프로그램이 있습니다:

manual_poser를 사용하면 그래픽 사용자 인터페이스를 통해 캐릭터의 표정, 머리 회전, 몸 회전 및 호흡으로 인한 가슴 확장을 조작할 수 있으므로 Happy, sad, joy 등의 기본 표정으로 저장할 수 있습니다.
ifacialmocap_puppeteer를 사용하면 얼굴 모션을 애니메 캐릭터로 전송할 수 있습니다.

### 하드웨어 요구 사항

CPU 또는 GPU 모드를 사용할 수 있습니다(CPU가 기본값). 그러나 CPU 모드에서는 약 1 FPS를 예상하고 RTX3060의 GPU 모드에서는 약 9-10 FPS를 얻고 있습니다.

ifacialmocap_puppeteer는 비디오 피드에서 블렌드 셰이프 매개변수를 계산할 수 있는 iOS 기기가 필요합니다. 즉, 기기가 iOS 11.0 이상을 실행할 수 있어야 하며 TrueDepth 전면 카메라가 있어야 합니다. (자세한 내용은 이 페이지를 참조하세요.) 즉, iPhone X 이상이 있으면 준비가 된 것입니다.

### 사용 방법

talkinghead가 작동하려면 다음 모듈로 extras를 시작해야 합니다: `classify` 및 `talkinghead`!
classify는 talkinghead.png 파일 처리에 필요합니다. 또한 `--talkinghead-gpu`를 사용하여 블렌드 모델을 GPU 메모리에 로드하고 애니메이션을 10배 더 빠르게 만들 수도 있습니다. GPU 가속을 사용하는 것이 좋습니다! 기본적으로 프로그램이 시작되면 기본 이미지 SillyTavern-extras\talkinghead\tha3\images\lambda_00.png를 로드합니다. http://localhost:5100/api/talkinghead/result_feed 또는 `YOUR EXT URL:PORT/api/talkinghead/result_feed`로 이동하여 작동하는지 확인할 수 있습니다.

- 서버가 시작되면 Extension API 탭으로 이동하여 연결하세요. 그런 다음 로드할 캐릭터 카드를 선택하기만 하면 됩니다. (`--enable-modules=classify,talkinghead --talkinghead-gpu` server.py 시작 시)

- 이제 Character Expressions를 선택하고 이미지 유형 talkinghead 상자를 선택하면 스크립트가 현재 캐릭터 표정을 `YOUR EXT URL:PORT/api/talkinghead/result_feed`의 결과로 교체합니다. 상자를 선택 해제하면 이미지가 원래 표정으로 돌아가야 하지만 때로는 채팅에 새 메시지를 보내 이미지를 "다시 로드"해야 합니다.

- 캐릭터 디렉토리에 talkinghead.png 파일이 없으면 기본 이미지 또는 talkinghead.png 파일이 있는 마지막 캐릭터 카드만 표시됩니다. 캐릭터 카드가 변경되면 애니메이션 소스 이미지가 변경됩니다.

- 이제 문자 표정을 열고 talkinghead 이미지로 스크롤한 다음 아래 "입력 이미지에 대한 제약 조건"이라는 섹션의 요구 사항을 충족하는 이미지 파일을 업로드하세요.

- 그런 다음 talkinghead 상자를 선택하고 선택 해제하여 캐릭터를 다시 로드하세요. 이미지가 이상하게 보이면 투명하지 않거나 알파 레이어가 없기 때문일 가능성이 높습니다. 그렇지 않으면 아래 지침 및 템플릿을 따르세요.

### 입력 이미지에 대한 제약 조건
시스템이 잘 작동하려면 입력 이미지가 다음 제약 조건을 준수해야 합니다:

해상도가 512 x 512여야 합니다. (프로그램이 다른 크기의 입력 이미지를 받으면 이미지 크기를 이 해상도로 조정하고 이 해상도로 출력합니다.)
알파 채널이 있어야 합니다.
하나의 인간형 캐릭터만 포함해야 합니다.
캐릭터는 똑바로 서서 앞을 향하고 있어야 합니다.
캐릭터의 손은 머리 아래와 멀리 떨어져 있어야 합니다.
캐릭터의 머리는 대략 이미지 상단 절반 중간에 있는 128 x 128 상자에 포함되어야 합니다.
캐릭터에 속하지 않는 모든 픽셀(즉, 배경 픽셀)의 알파 채널은 0이어야 합니다.

![Input Constraints](/static/input_spec.png)

### 고급 섹션

### Python 환경

기본 기능(app.py) 외에도 manual_poser와 ifacialmocap_puppeteer는 모두 데스크톱 애플리케이션으로 사용할 수 있습니다. 이를 실행하려면 Python 언어로 작성된 프로그램을 실행하기 위한 환경을 설정해야 합니다. 환경에는 다음 소프트웨어 패키지가 있어야 합니다:

* Python >= 3.8
* PyTorch >= 1.11.0 with CUDA support
* SciPY >= 1.7.3
* wxPython >= 4.1.1
* Matplotlib >= 3.5.1

한 가지 방법은 Anaconda를 설치하고 셸에서 다음 명령을 실행하는 것입니다:

> conda create -n talking-head-anime-3-demo python=3.8
> conda activate talking-head-anime-3-demo
> conda install pytorch torchvision torchaudio cudatoolkit=11.3 -c pytorch
> conda install scipy
> pip install wxpython
> conda install matplotlib

### 추가 블렌드 모델

하나의(가장 가벼운) 모델만 포함되어 있으며 추가 블렌드 모델을 원하면 https://www.dropbox.com/s/y7b8jl4n2euv8xe/talking-head-anime-3-models.zip?dl=0에서 모델 파일을 다운로드하고 SillyTavern-extras\talkinghead\tha3\models 폴더에 압축을 풀어야 합니다. 결국 데이터 폴더는 다음과 같아야 합니다:

+ tha3
  + models
    + separable_float
      - editor.pt
      - eyebrow_decomposer.pt
      - eyebrow_morphing_combiner.pt
      - face_morpher.pt
      - two_algo_face_body_rotator.pt
    + separable_half
      - editor.pt
          :
      - two_algo_face_body_rotator.pt
    + standard_float
      - editor.pt
          :
      - two_algo_face_body_rotator.pt
    + standard_half
      - editor.pt
          :
      - two_algo_face_body_rotator.pt

모델 파일은 Creative Commons Attribution 4.0 International License로 배포되므로 상업적 목적으로 사용할 수 있습니다. 그러나 Pramook Khungurn. Talking Head(?) Anime from a Single Image 3: Now the Body Too. <https://github.com/pkhungurn/talking-head-anime-3-demo>가 제작자입니다.

### manual_poser 데스크톱 애플리케이션 실행
셸을 엽니다. 작업 디렉토리를 리포지토리의 루트 디렉토리로 변경합니다. 그런 다음 실행하세요:

> python tha3/app/manual_poser.py
위 명령을 실행하기 전에 필요한 패키지가 포함된 Python 환경을 활성화해야 할 수 있습니다.

> conda activate extras
환경을 아직 활성화하지 않은 경우.
