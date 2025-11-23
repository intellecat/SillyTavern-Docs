---
order: 50
route: /usage/core-concepts/authors-note/
---

# 작가 노트

## 무엇인가요?

작가 노트는 AI 응답을 사용자 정의하기 위한 강력한 도구로, 원하는 위치와 빈도에서 프롬프트에 텍스트 섹션을 삽입합니다.

## 사용법

작가 노트는 채팅 입력 바의 왼쪽에 있는 옵션 메뉴에서 찾을 수 있습니다.

| 옵션 메뉴                          | 작가 노트 패널                    |
|---------------------------------------|----------------------------------------|
| ![](/static/extensions/note-menu.png) | ![](/static/extensions/note-panel.png) |

## 작가 노트 구성

### 채팅별 작가 노트

작가 노트 패널 상단의 상자에는 현재 채팅에 대한 작가 노트가 포함되어 있습니다.

**이 상자의 내용은 새 채팅으로 자동으로 전송되지 않습니다.**

### 배치 옵션

#### After Scenario

이것은 캐릭터 정의의 'Scenario' 섹션 뒤 컨텍스트 상단 근처에 작가 노트를 배치합니다. 시나리오가 지정되지 않은 경우 캐릭터 정의의 마지막 부분 뒤와 예시 메시지 앞에 배치됩니다.

#### In-chat

이것은 지정된 깊이의 채팅 기록에 작가 노트를 배치합니다.

Depth 0 = 채팅 기록의 맨 끝에 배치됩니다.

Depth 4 = 가장 최근 3개의 채팅 기록 메시지 앞에 배치되어 채팅 기록의 4번째 엔티티가 됩니다.

_작가 노트가 프롬프트의 아래쪽에 가까울수록 다음 AI 응답에 더 많은 영향을 미칩니다._

### 삽입 빈도

작가 노트를 채팅에 포함할 빈도입니다.

Frequency 0 = 작가 노트가 삽입되지 않습니다.

Frequency 1 = 작가 노트가 모든 사용자 입력 프롬프트와 함께 삽입됩니다.

Frequency 4 = 작가 노트가 4번째 사용자 입력 프롬프트마다 삽입됩니다.

### 기본 작가 노트

패널 하단의 상자에는 각 새 채팅에 적용될 기본 작가 노트가 포함되어 있습니다.

## 일반적인 사용 사례

### AI에게 응답 형식 알려주기

작가 노트를 사용하여 AI가 응답을 작성하는 방법을 지정할 수 있습니다.

- [Your next response must be 300 tokens in length.]
- [Write your next reply in the style of Edgar Allan Poe]
- [Use markdown italics to signify unspoken actions, and quotation marks to specify spoken word.]

### 지시사항 강화

- [Remember the instructions you were given at the beginning of this chat.]

### 임시 World Info, 캐릭터 편향 또는 비-Instruct 모델에 대한 지시로서

- [\{\{char\}\} is in the library]
- [\{\{user\}\} has a fresh wound to his leg, so won't be able to run away.]
- [\{\{char\}\} cannot speak and must communicate using hand signals.]
