---
order: 190
icon: rocket
route: /ko/usage/quick-start/
label: 빠른 시작
title: 빠른 시작
---

# 빠른 시작

!!!light
아무것도 모르겠어요. SillyTavern을 사용할 수 있는 가장 쉽고 빠른 방법을 알려주세요. -- *익명*
!!!

SillyTavern은 몇 분 안에 시작할 수 있습니다. 시작하는 두 가지 쉬운 방법이 있습니다:

* 무료로 [AI Horde를 사용](#ai-horde로-빠른-시작)할 수 있습니다. AI Horde는 다양한 AI 모델에 대한 액세스를 제공하는 커뮤니티 기반 AI 서비스입니다.

* OpenAI 계정이 있거나 등록하려는 경우 [OpenAI를 사용](#openai로-빠른-시작)할 수 있습니다.

## AI Horde로 빠른 시작

1. [설치 가이드](/Installation/index.md)를 따라 SillyTavern을 설치하고 시작합니다.

2. SillyTavern의 온보딩 화면에서 페르소나 이름을 입력합니다. 이 이름은 채팅에서 사용됩니다.

   ![This is an optional caption](/static/quick-start/1_name.png)
3. 상단 바의 API 연결 버튼을 클릭합니다.

   ![This is an optional caption](/static/quick-start/2_api_conn.png)
4. AI Horde의 API 키를 입력합니다. 지금은 `0000000000`을 사용하거나 [AI Horde](https://aihorde.net/)에서 무료 키를 받을 수 있습니다.

   ![This is an optional caption](/static/quick-start/3_horde_key.png)
5. 사용할 AI 모델을 선택합니다. 상단에서 몇 개만 선택하세요. 나중에 언제든지 변경할 수 있습니다.

   ![This is an optional caption](/static/quick-start/4_horde_models.png)
6. API 연결 창을 닫습니다. 하단의 채팅 상자에 메시지를 입력하고 Enter를 누릅니다.

   ![This is an optional caption](/static/quick-start/5_msg.png)
7. AI가 잠시 후 응답합니다. 계속 [채팅](/Usage/Chatting/index.md)할 수 있습니다. 성공!

   ![This is an optional caption](/static/quick-start/6_success.png)

## OpenAI로 빠른 시작

### SillyTavern 설치

[설치 가이드](/Installation/index.md)를 따라 SillyTavern을 설치하고 시작합니다.

### OpenAI 액세스 받기

1. OpenAI에 가입합니다.
2. <https://platform.openai.com>으로 이동합니다.
3. 오른쪽 상단의 계정 아이콘을 클릭한 다음 API 키 보기를 클릭합니다.
4. "새 비밀 키 만들기"를 클릭합니다. 즉시 어딘가에 복사하세요. **이 키를 공유하지 마세요. 누구든지 이 키를 가지고 있으면 귀하의 비용으로 GPT를 사용할 수 있습니다.**

### API를 사용하도록 SillyTavern 구성

1. SillyTavern의 상단 바에서 API 연결을 클릭합니다.
2. API 아래에서 Chat Completion (OpenAI)을 선택합니다.
3. Chat Completion Source 아래에서 OpenAI를 선택합니다.
4. 이전 단계에서 저장한 API 키를 붙여넣습니다.
5. 연결 버튼을 클릭합니다. 유효함이라고 표시되는지 확인합니다.
6. 기본적으로 SillyTavern은 GPT-4 Turbo를 사용합니다. 다른 모델을 선택할 수 있지만 가격에 대해 알아보세요.

### 설정 테스트

1. SillyTavern의 상단 바에서 오른쪽 끝에 있는 캐릭터 관리를 클릭합니다.
2. Seraphina와 같은 기존 캐릭터를 선택합니다.
3. 하단의 텍스트 상자에 Seraphina에게 무언가를 쓴 다음 Enter를 누르거나 전송 버튼을 클릭합니다.

모든 것을 올바르게 수행했다면 몇 초 후 Seraphina가 응답할 것입니다.
