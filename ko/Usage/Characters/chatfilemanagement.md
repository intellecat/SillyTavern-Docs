---
order: 80
route: /usage/core-concepts/chatfilemanagement/
---

# 채팅 파일 관리

이 페이지는 AI 채팅 파일을 관리하는 방법을 설명합니다.

!!!info 참고
이러한 옵션 중 일부는 왼쪽 하단 옵션 메뉴에서 열리는 "Manage chat files" 대화상자에서 사용할 수 있습니다.
!!!

## 단독 채팅 vs 그룹 채팅

캐릭터 카드를 사용하는 가장 간단한 방법은 단독 채팅입니다. 카드를 클릭하고 채팅을 시작하면 됩니다.

몇 개의 캐릭터 카드가 있으면 "Create New Chat Group" 버튼을 사용하여 여러 캐릭터를 포함하는 [그룹 채팅](/Usage/Characters/groupchats.md)을 만들 수도 있습니다. 그러면 캐릭터들이 서로 및 사용자와 상호작용합니다.

## 채팅 가져오기

**Character.AI에서 SillyTavern으로 채팅을 가져옵니다.**

Character.AI 채팅 및 봇을 가져오려면 CAI Tools 브라우저 확장 프로그램을 사용하세요: [https://github.com/irsat000/CAI-Tools](https://github.com/irsat000/CAI-Tools).

채팅을 가져올 수 있는 다른 프로그램 및 도구는 다음과 같습니다:

* TavernAI (original): <https://github.com/TavernAI/TavernAI>
* Text Generation WebUI (oobabooga): <https://github.com/oobabooga/text-generation-webui>
* Agnai: <https://github.com/agnaistic/agnai>
* KoboldAI Lite: <https://github.com/LostRuins/lite.koboldai.net>
* RisuAI: <https://github.com/kwaroran/RisuAI>

## .jsonl로 내보내기

"Manage chat files"를 클릭하면 채팅 파일 목록의 각 항목에는 그대로 다시 가져올 수 있는 형식으로 내보내는 버튼이 있습니다. 이것을 사용하여 모든 메타데이터를 포함하여 채팅을 공유하거나 마이그레이션합니다(단, 이미지 및 파일 첨부 파일 제외).

개인정보를 염두에 두고 있다면 내보낸 JSONL 파일을 검사하고 공유하고 싶지 않은 내용을 제거하세요.

## .txt로 내보내기

"Download chat as plain text document" 버튼으로 간단한 텍스트 전용 버전을 내보낼 수도 있습니다. 중요한 메타데이터를 잃기 때문에 다시 가져올 수 없습니다!

## 체크포인트

"체크포인트"는 현재 채팅의 복제본으로, 주어진 채팅에서 특정 지점까지의 모든 메시지를 복사하고 소스에 대한 링크(채팅 파일 이름별)를 저장합니다.

각 채팅 메시지 오른쪽의 점 세 개 버튼에서 체크포인트를 만드는 두 가지 방법이 있습니다:

* "Create Branch"는 해당 메시지까지 현재 채팅을 복제하고 전환합니다
* "Create Checkpoint"는 해당 메시지까지 현재 채팅을 복제하고 이름을 묻고 생성하지만 전환하지 않습니다

이것들을 대략 브라우저의 "새 탭에서 링크 열기" 및 "백그라운드의 새 탭에서 링크 열기"로 생각할 수 있습니다.

체크포인트에서 부모로 돌아가려면 메시지 텍스트 상자 왼쪽의 햄버거 메뉴 버튼을 누른 다음 "Back to parent chat"을 클릭합니다.

## 채팅 이름 변경

기본적으로 채팅 파일은 시작된 날짜와 시간으로 이름이 지정됩니다.

연필 아이콘을 클릭하고 새 이름을 입력하여 이것을 변경할 수 있습니다.

이것은 체크포인트에서 해당 채팅에 대한 링크를 끊을 것입니다(채팅 파일 이름으로 연결되기 때문에).
