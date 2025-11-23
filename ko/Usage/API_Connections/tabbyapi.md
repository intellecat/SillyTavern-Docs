---
route: /usage/api-connections/tabbyapi/
label: TabbyAPI
title: TabbyAPI
---

# TabbyAPI
Exl2, GPTQ 및 FP16 모델을 지원하는 Exllamav2 백엔드를 사용하여 LLM을 사용하여 텍스트를 생성할 수 있는 FastAPI 기반 애플리케이션입니다.

* [GitHub](https://github.com/theroyallab/tabbyAPI)

### 빠른 시작
1. 공식 TabbyAPI GitHub의 [설치 지침](https://github.com/theroyallab/tabbyAPI/wiki/01.-Getting-Started)을 따르세요.
2. 모델 경로, 기본 모델, 시퀀스 길이 등을 설정하기 위해 [config.yml을 만듭니다](https://github.com/theroyallab/tabbyAPI/wiki/02.-Server-options). 원하는 경우 이러한 설정의 대부분(전부는 아니더라도)을 무시할 수 있습니다.
3. TabbyAPI를 시작합니다. 작동하면 다음과 같이 표시되어야 합니다:

    ![TabbyAPI terminal](/static/tabby-terminal.png)

4. SillyTavern의 Text Completion API에서 TabbyAPI를 선택합니다.
5. TabbyAPI 터미널에서 API 키를 복사하여 `Tabby API key`에 붙여넣고 `API URL`이 올바른지 확인합니다(기본적으로 `http://127.0.0.1:5000`이어야 함).

모든 것을 올바르게 수행했다면 SillyTavern에서 다음과 같이 표시되어야 합니다:

![TabbyAPI SillyTavern](/static/tabby-config.png)

이제 TabbyAPI를 사용하여 채팅할 수 있습니다!

### TabbyAPI Loader
TabbyAPI 개발자는 SillyTavern에서 직접 모델을 로드/언로드하는 공식 확장 기능을 만들었습니다. 설치는 간단합니다:
1. SillyTavern에서 Extensions 탭을 클릭하고 Download Extensions & Assets로 이동합니다.
2. Assets URL에 `https://raw.githubusercontent.com/theroyallab/ST-repo/main/index.json`을 복사하고 오른쪽의 플러그 버튼을 클릭합니다.
3. 다음과 같이 표시되어야 합니다. Tabby Loader 옆의 다운로드 버튼을 클릭합니다.

    ![Tabby Loader](/static/tabby-assets.png)

4. 설치가 성공하면 화면 상단에 녹색 팝업 메시지가 표시됩니다. extensions 탭에서 TabbyAPI Loader로 이동하고 TabbyAPI 터미널에서 admin key를 Admin Key에 복사합니다.
5. Model Select 옆의 새로 고침 버튼을 클릭합니다. 바로 아래 텍스트 상자를 클릭하면 모델 디렉터리의 모든 모델이 표시되어야 합니다.

![Tabby Loader Extension](/static/tabby-loader.png)

이제 SillyTavern에서 직접 모델을 로드하고 언로드할 수 있습니다!

### 지원
아직도 도움이 필요하신가요? [TabbyAPI GitHub](https://github.com/theroyallab/tabbyAPI)를 방문하여 개발자의 공식 Discord 서버 링크를 확인하고 [위키를 읽어보세요](https://github.com/theroyallab/tabbyAPI/wiki/1.-Getting-Started).
