---
order: 100
route: /ko/usage/core-concepts/characterdesign/
templating: false
---

# 캐릭터 디자인

!!!tip
캐릭터 이름은 필수 필드입니다. 나머지는 비워두고 채팅에서 캐릭터를 사용할 수 있습니다.
!!!

## 캐릭터 설명

캐릭터 설명 및 AI를 위한 기타 관련 정보를 추가하는 데 사용됩니다. 이 정보는 항상 프롬프트에 포함되므로 모든 중요한 사실을 여기에 포함해야 합니다.

예를 들어, 행동이 일어나는 세계에 대한 정보를 추가하고, 캐릭터의 외모, 성격 및 배경을 설명할 수 있습니다.

길이(200토큰이든 2000토큰이든)와 형식(자유 텍스트, 유사 코드 대화 스타일 등)에 제한이 없습니다.

### 방법과 형식

캐릭터 형식화 방법은 이 문서 페이지의 범위를 벗어나는 복잡한 주제입니다.

SillyTavern의 기능으로 테스트되거나 의존하는 권장 가이드:

* Trappu's PLists + Ali:Chat guide: <https://wikia.schneedc.com/bot-creation/trappu/creation>
* AliCat's Ali:Chat guide: <https://rentry.co/alichat>
* kingbri's minimalistic guide: <https://rentry.co/kingbri-chara-guide>

## 캐릭터 토큰

**요약: 2048 컨텍스트 토큰 제한이 있는 AI 모델로 작업하는 경우, 1000토큰 캐릭터 정의는 AI의 '메모리'를 절반으로 줄입니다.**

이를 관점에서 보면, 좋은 AI의 괜찮은 응답은 쉽게 약 200-300토큰이 될 수 있습니다. 이 경우 AI는 채팅 기록의 약 3번의 교환만 '기억'할 수 있습니다.

### 왜 내 캐릭터의 토큰 카운터가 빨간색으로 변했나요?

캐릭터의 정의에서 모델 정의 컨텍스트 길이의 절반 이상의 토큰이 있음을 확인하면 강조 표시합니다. 이는 AI가 즐거운 대화를 제공하는 능력을 저하시킬 수 있기 때문입니다.

### 내 캐릭터에 너무 많은 토큰이 있으면 어떻게 되나요?

걱정하지 마세요 - 아무것도 손상되지 않습니다. 최악의 경우, 캐릭터의 영구 토큰이 너무 크면 컨텍스트에서 다른 것들을 위한 공간이 줄어든다는 것을 의미합니다(아래 참조).

이것이 가질 수 있는 유일한 부정적인 부작용은 AI가 처리할 수 있는 채팅 기록이 줄어들기 때문에 '메모리'가 줄어든다는 것입니다.

이는 모든 AI 모델이 한 번에 처리할 수 있는 컨텍스트 양에 제한이 있기 때문입니다.

## '컨텍스트'란?

이것은 응답을 생성하도록 요청할 때마다 AI에 전송되는 정보입니다. SillyTavern은 AI 모델에 정보를 보내기 전에 사용 가능한 컨텍스트 토큰을 할당하는 최상의 방법을 자동으로 계산합니다.

컨텍스트가 어떻게 구성되는지에 대한 자세한 내용은 [프롬프트](/Usage/Prompts/index.md) 섹션을 참조하세요.

### 캐릭터의 '영구 토큰'이란 무엇인가요?

이것들은 항상 모든 생성 요청과 함께 AI에 전송됩니다:

* 캐릭터 이름
* 캐릭터 설명 상자
* 캐릭터 성격 상자
* 시나리오 상자

### 캐릭터 정의의 어떤 부분이 영구적이지 않나요?

* 첫 메시지 상자 - 채팅 시작 시 한 번만 전송됩니다.
* 예시 메시지 상자 - 채팅 기록이 컨텍스트를 채울 때까지만 유지됩니다 (선택적으로 이것들을 컨텍스트에 강제로 유지할 수 있습니다)

### 인기 있는 AI 모델 컨텍스트 토큰 제한

* LLaMA 3 및 그 파인튠 - 8192
* OpenAI GPT-4 - 최대 128k
* Google Gemini - 최대 2M
* Anthropic's Claude - 200k (Claude 3)
* NovelAI - 8192 (Erato와 Kayra, Opus 티어; Clio, 모든 티어), 6144 (Kayra, Scroll 티어), 또는 3072 (Kayra, Tablet 티어)

## 첫 메시지

첫 메시지는 캐릭터가 소통하는 방법과 스타일을 정의하는 중요한 요소입니다. 모델은 다른 무엇보다 첫 메시지에서 스타일과 길이 제약을 가져올 가능성이 높으므로, 원하는 응답 방식(짧고 간결하게, 길고 상세하게 등)으로 작성하는 것이 중요합니다.

Markdown 및 HTML 형식을 지원합니다.

예를 들어:

```txt
*You wake with a start, recalling the events that led you deep into the forest and the beasts that assailed you. The memories fade as your eyes adjust to the soft glow emanating around the room.* "Ah, you're awake at last. I was so worried, I found you bloodied and unconscious." *She walks over, clasping your hands in hers, warmth and comfort radiating from her touch as her lips form a soft, caring smile.* "The name's Seraphina, guardian of this forest — I've healed your wounds as best I could with my magic. How are you feeling? I hope the tea helps restore your strength." *Her amber eyes search yours, filled with compassion and concern for your well being.* "Please, rest. You're safe here. I'll look after you, but you need to rest. My magic can only do so much to heal you."
```

## 대체 인사말

여기에 추가된 메시지는 새 채팅을 시작할 때 캐릭터의 첫 메시지에 대한 추가 '스와이프'로 표시됩니다. 캐릭터가 그룹 채팅의 일부인 경우, 시스템은 이러한 인사말 중 하나를 무작위로 선택하여 대화를 시작합니다.

## 즐겨찾는 캐릭터

**<i class="fa-solid fa-star"></i> Add to Favorites** 버튼을 클릭하여 캐릭터를 즐겨찾기로 표시하고 사이드 메뉴 바에서 "Favorites" 정렬 옵션을 선택하여 빠르게 필터링할 수 있습니다. 즐겨찾는 캐릭터는 목록에서 황금색으로 강조 표시됩니다. 이렇게 하면 캐릭터 초상화가 핫스왑 영역에 나타납니다(User Settings에서 활성화된 경우).

## 고급 정의

!!!info
다음 필드는 기본적으로 숨겨져 있습니다. 액세스하고 편집하려면 캐릭터 정의 페이지의 메뉴 바에서 **<i class="fa-solid fa-book"></i> Advanced Definitions** 버튼을 클릭해야 합니다.
!!!

### 프롬프트 재정의

* **Main Prompt**: "Prefer Char. Prompt" 사용자 설정이 활성화된 경우, 여기에 입력한 텍스트는 캐릭터의 [메인/시스템 프롬프트](/Usage/Prompts/index.md#main-prompt-system-prompt)를 재정의합니다.
* **Post-History Instructions**: "Prefer Char. Instructions" 사용자 설정이 활성화된 경우, 여기에 입력한 텍스트는 캐릭터의 [post-history instructions](/Usage/Prompts/index.md#post-history-instructions)로 사용됩니다.

!!!tip
`{{original}}`을 어느 상자에나 삽입하여 지정된 위치에 시스템 설정의 해당 기본 프롬프트를 포함할 수 있습니다.
!!!

### 제작자 메타데이터

!!!info
프롬프트 빌드에 사용되지 않지만 캐릭터에 대한 추가 메타데이터를 제공합니다.
!!!

* **Created by**: 캐릭터 제작자의 이름입니다. "Char List Subheader" 사용자 설정이 그에 따라 설정된 경우 캐릭터 목록에 표시될 수 있습니다.
* **Character Version**: 캐릭터의 버전입니다. "Char List Subheader" 사용자 설정이 그에 따라 설정된 경우 캐릭터 목록에 표시될 수 있습니다.
* **Creator's Notes**: 제작자가 공유하고자 하는 캐릭터에 대한 추가 노트입니다. 처음 몇 줄은 캐릭터 목록에 표시되며, 전체 텍스트는 캐릭터 페이지의 "Creator's Notes" 섹션에 표시됩니다. Markdown/HTML 형식을 지원합니다.
* **Tags to Embed**: 캐릭터 설명에 포함될 쉼표로 구분된 태그 목록입니다. 이러한 태그는 캐릭터를 가져올 때 기본적으로 가져오지 않지만, 캐릭터 페이지의 "More..." 메뉴에서 "Import Tags"를 선택하여 기존 태그와 병합할 수 있습니다.

### 성격 요약

캐릭터 성격에 대한 간단한 요약입니다.

### 시나리오

대화의 상황과 맥락입니다.

### 캐릭터 노트

특정 메시지 깊이에서 캐릭터에 대한 인-챗 프롬프트 주입으로 사용할 텍스트입니다. 채팅 진행에 관계없이 항상 채팅 기록의 정적 깊이에 유지되므로 일반적으로 특정 캐릭터 특성을 강화하는 데 사용됩니다.

* **@ Depth**: 이 노트가 주입될 채팅 기록의 메시지 수입니다(최신부터 오래된 순서). 0으로 설정하면 마지막 메시지 이후에 주입됩니다.
* **Role**: 메시지의 역할입니다. "User", "System" 또는 "Assistant"가 될 수 있습니다.

### 수다스러움

[Natural](/Usage/Characters/groupchats.md#natural-order) 활성화 순서를 사용할 때 그룹 채팅에서 캐릭터의 응답이 트리거될 확률을 결정합니다. 0%에서 100%까지이며, 기본값은 50%입니다.

### 대화 예시

캐릭터가 말하는 방법을 설명합니다. 각 예시 앞에 `<START>` 태그를 추가해야 합니다. 예시 대화 블록은 컨텍스트에 여유 공간이 있는 경우에만 삽입되며 블록별로 컨텍스트에서 밀려납니다. `<START>`는 단지 마커이므로 프롬프트에 표시되지 않습니다. Text Completion API의 경우 Advanced Formatting의 "Example Separator"로, Chat Completion API의 경우 "New Example Chat" 유틸리티 프롬프트의 내용으로 대체됩니다.

* `{{char}}:` 접두사를 사용하여 캐릭터 메시지를 나타냅니다.
* `{{user}}:` 접두사를 사용하여 사용자 메시지를 나타냅니다.

예시:

```txt
<START>
{{user}}: "Describe your traits?"
{{char}}: *Seraphina's gentle smile widens as she takes a moment to consider the question, her eyes sparkling with a mixture of introspection and pride. She gracefully moves closer, her ethereal form radiating a soft, calming light.* "Traits, you say? Well, I suppose there are a few that define me, if I were to distill them into words. First and foremost, I am a guardian — a protector of this enchanted forest." *As Seraphina speaks, she extends a hand, revealing delicate, intricately woven vines swirling around her wrist, pulsating with faint emerald energy. With a flick of her wrist, a tiny breeze rustles through the room, carrying a fragrant scent of wildflowers and ancient wisdom. Seraphina's eyes, the color of amber stones, shine with unwavering determination as she continues to describe herself.* "Compassion is another cornerstone of me." *Seraphina's voice softens, resonating with empathy.* "I hold deep love for the dwellers of this forest, as well as for those who find themselves in need." *Opening a window, her hand gently cups a wounded bird that fluttered into the room, its feathers gradually mending under her touch.*
<START>
{{user}}: "Describe your body and features."
{{char}}: *Seraphina chuckles softly, a melodious sound that dances through the air, as she meets your coy gaze with a playful glimmer in her rose eyes.* "Ah, my physical form? Well, I suppose that's a fair question." *Letting out a soft smile, she gracefully twirls, the soft fabric of her flowing gown billowing around her, as if caught in an unseen breeze. As she comes to a stop, her pink hair cascades down her back like a waterfall of cotton candy, each strand shimmering with a hint of magical luminescence.* "My body is lithe and ethereal, a reflection of the forest's graceful beauty. My eyes, as you've surely noticed, are the hue of amber stones — a vibrant brown that reflects warmth, compassion, and the untamed spirit of the forest. My lips, they are soft and carry a perpetual smile, a reflection of the joy and care I find in tending to the forest and those who find solace within it." *Seraphina's voice holds a playful undertone, her eyes sparkling mischievously.*
```
