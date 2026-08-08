---
route: /ko/usage/api-connections/horde/
label: AI Horde
title: AI Horde
---

# AI Horde

## 고지 사항

- AI Horde는 전적으로 자원봉사자가 운영하는 크라우드소싱 분산 GPU 클러스터입니다.
- 기본적으로 입력은 익명으로 전송되며 Horde Worker를 실행하는 사람이 응답을 볼 수 없습니다.
- 그러나 오픈 소스 프로그램이므로 악의적인 Worker가 코드를 수정하여:
  - 활동(입력 프롬프트, AI 응답)을 로그할 수 있습니다.
  - 나쁜 응답이나 공격적인 응답을 생성할 수 있습니다.

!!!warning
Horde를 사용할 때 이름, 이메일 주소 등과 같은 개인 정보를 **절대 보내지 마세요**.
!!!

"Trusted Workers Only" 확인란을 켜면 사용 가능한 worker 선택을 Horde에서 잠시 호스팅하고 있으며 일반적으로 신뢰할 수 있는 것으로 간주되는 worker로만 제한합니다. 그러나 예를 들어 설명되지 않은 소프트웨어를 사용하여 호스팅하여 프롬프트를 계속 볼 수 있습니다.

이 문제를 줄이는 데 도움이 되도록 SillyTavern에는 다음 기능이 내장되어 있습니다:

- Horde Worker가 채팅 응답을 생성하면 SillyTavern은 Worker의 ID와 사용 중인 모델을 기록합니다.
- 이 정보는 채팅 항목 위로 마우스 커서를 가져가면 볼 수 있습니다(아래 이미지 참조).
- 악의적인 응답을 받았다고 생각되면 이 정보를 [AI Horde Discord](https://discord.gg/3DxrhksKzn)의 Horde 관리자에게 전달하여 검토하고 해당 Worker에 대한 징계 조치를 취할 수 있습니다.

![Horde Worker Info Popup](/static/horde-worker.png)

## 설정

- SillyTavern은 추가 설정 없이 즉시 Horde에 연결할 수 있습니다.
- ST API 패널의 API 드롭다운 선택기에서 'AI Horde'를 선택합니다.
- 패널 하단의 모델 선택기에서 하나 이상의 모델(캐릭터용 'AI brains')을 선택합니다.
- 캐릭터를 선택하고 채팅을 시작합니다.

![ST Kobold Horde API Connection Panel](/static/horde-config.png)

!!!warning
기본적으로 SillyTavern 인스턴스는 Horde의 낮은 우선순위 'guest account'에 연결됩니다.
이는 응답을 기다리는 데 오랜 시간이 걸릴 수 있음을 의미합니다.
대기 시간을 줄이려면 아래 팁을 따르세요.
!!!

## 팁

- [Horde 웹사이트에서 계정을 등록](https://aihorde.net/register)한 다음 Horde 키를 SillyTavern Horde API Key 상자에 추가합니다.
- [Horde Worker 설정](https://github.com/Haidra-Org/AI-Horde-Worker#readme)하여 다른 사람들을 위해 GPU를 제공합니다.
  - 다른 사람들이 GPU를 사용하도록 허용하면 ['Kudos', Horde 전용 통화의 일종](https://github.com/Haidra-Org/AI-Horde/blob/main/FAQ.md#kudos)을 얻습니다.
  - 계정에 kudos가 많을수록 다른 Horde Worker로부터 채팅 응답을 더 빨리 받을 수 있습니다.
  - Kudos는 [Stable Horde](https://stablehorde.net)에서 AI 이미지를 만드는 데도 사용할 수 있습니다.
    - SillyTavern은 즉시 Stable Horde 이미지 생성을 지원합니다.
- GPU가 AI를 실행할 만큼 강력하지 않거나 컴퓨터가 없는 경우에도 [다양한 방법으로 Horde 커뮤니티에 참여하여 Kudos를 얻을 수 있습니다](https://github.com/Haidra-Org/AI-Horde/blob/main/FAQ.md#i-dont-have-a-powerful-gpu-how-can-i-get-kudos).
