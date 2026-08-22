---
order: tts-minimax
route: /ko/extensions/minimaxtts/
---

# MiniMax TTS

이 페이지에서는 MiniMax TTS provider를 올바르게 사용하는 방법을 알려드립니다.

## 사전 요구 사항

1. API 액세스 권한이 있는 MiniMax 계정
2. MiniMax의 유효한 API Key 및 Group ID

## API 자격 증명 얻기

### 1. MiniMax 계정 만들기

1. [MiniMax 웹사이트(국제)](https://www.minimax.io/) 방문
2. "Sign Up" 또는 "Login" 클릭
3. 계정 등록 프로세스 완료

!!!warning 지역별 차이점
MiniMax에는 중국어 및 국제 버전이 별도로 있습니다. 다음 사항에 유의하세요:
- 중국어 버전은 음성 복제 기능을 지원하지 않습니다
- 중국어 버전은 `api.minimax.chat` API 호스트만 지원합니다
!!!

### 2. API Key 및 Group ID 얻기

1. [MiniMax 콘솔(국제)](https://www.minimax.io/platform/user-center/basic-information)에 로그인
2. Basic Information 페이지에서 GroupId를 찾을 수 있습니다
3. 왼쪽 사이드바의 Settings → API Keys로 이동하여 API Key를 생성하고 얻으세요

## SillyTavern에서 구성

### 1. 기본 설정

1. SillyTavern 열기
2. "Extensions" → "TTS"로 이동
3. TTS provider로 "MiniMax" 선택
4. 다음 설정 구성:
    - **API Key**: MiniMax API key
    - **Group ID**: MiniMax Group ID
    - **API Host**: 지역에 따라 적절한 서버 선택:
        - `api.minimax.io` (공식 국제 서버)
        - `api.minimaxi.chat` (다른 국제 서버 호스트)
        - `api.minimax.chat` (중국 본토 서버)

### 2. 모델 선택

사용 가능한 모델은 다음과 같습니다:
- **Speech-02-HD**: 고품질 음성 합성(권장)
- **Speech-02-Turbo**: 빠른 음성 합성
- **Speech-01**: 레거시 모델
- **Speech-01-240228**: 레거시 모델(특정 버전)

### 3. 음성 매개변수

음성 출력을 사용자 정의하려면 다음 매개변수를 조정하세요:
- **Speed**: 0.5 - 2.0 (1.0 = 일반 속도)
- **Volume**: 0.1 - 2.0 (1.0 = 일반 볼륨)
- **Pitch**: 0.5 - 2.0 (1.0 = 일반 피치)
- **Audio Format**: MP3, WAV, FLAC

## 사용자 정의 음성

### 1. Voice ID 얻기

1. [MiniMax TTS 페이지(국제)](https://www.minimax.io/audio/text-to-speech)에 액세스
2. 오른쪽의 "Voice"를 클릭하여 Voice Selection 인터페이스 입장
3. 사용하려는 음성 찾기
4. 음성 이름 옆의 복사 버튼을 클릭하여 Voice ID 복사

### 2. 사용자 정의 음성 추가

1. MiniMax TTS 설정에서 "Custom Voice Management" 섹션 찾기
2. 다음 정보 입력:
    - **Voice Name**: 식별을 위해 원하는 이름 선택
    - **Voice ID**: MiniMax 플랫폼에서 얻은 음성 ID
    - **Language**: 음성에 해당하는 언어 선택
3. "Add Custom Voice" 클릭

## 사용자 정의 모델

### 1. 사용자 정의 모델 추가

1. "Custom Model Management" 섹션에서
2. 다음 입력:
    - **Model ID**: 모델 식별자
    - **Model Name**: 모델의 표시 이름
3. "Add Custom Model" 클릭

### 2. Model ID 얻기

1. 공식 [MiniMax 문서](https://www.minimax.io/platform/document/Model?key=684261f14c5738213294faa7)에서 모델 목록 확인
2. 또는 콘솔에서 사용 가능한 사용자 정의 모델 보기
3. 해당 Model ID 복사

## 문제 해결

### 일반적인 문제

1. **API 인증 실패**
    - API Key가 올바른 API Host에 해당하는지 확인
    - Group ID가 올바른지 확인
    - 계정에 충분한 잔액이 있는지 확인

2. **음성 생성 실패**
    - 선택한 Voice ID가 유효한지 확인
    - 음성이 선택한 모델과 호환되는지 확인

3. **연결 시간 초과**
    - 다른 API Host로 전환 시도
    - 네트워크 연결 확인
    - 방화벽 설정 확인

4. **오디오 품질 문제**
    - 다른 모델 사용 시도(최상의 품질을 위해 Speech-02-HD)
    - 음성 매개변수 조정(속도, 피치, 볼륨)
    - 오디오 형식 호환성 확인
