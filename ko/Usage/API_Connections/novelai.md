---
route: /ko/usage/api-connections/novelai/
label: NovelAI
title: NovelAI
---

# NovelAI

NovelAI는 자체 고품질 텍스트 생성, 이미지 생성 및 텍스트 음성 변환 모델에 대한 무제한 월간 액세스를 허용하는 유료 구독 서비스입니다. 시작하려면 여기에서 계정을 등록하세요: <https://novelai.net/>

모델을 평가하기 위해 *50회 생성*만 무료로 받을 수 있습니다. **"Not eligible for this model"** 오류가 나타나면 평가 기간이 소진되었으며 유료 플랜을 구독해야 함을 의미합니다.

## API 키

NovelAI API 키를 받으려면 다음 단계를 따르세요:

1. 왼쪽 사이드바 상단의 톱니바퀴 아이콘을 선택합니다.
![Left Sidebar](/static/novel-side.png)

2. "User Settings" 아래의 "Account"를 선택합니다.
![User Settings](/static/novel-user.png)

3. "Get Persistent API Token"을 선택합니다.
![Account](/static/novel-account.png)

4. 복사 아이콘을 선택하여 NovelAI API 토큰을 클립보드에 복사합니다.
![Persistent API Token](/static/novel-token.png)

## 모델

Opus가 있으면 Erato가 사용할 모델입니다. Opus가 없으면 Kayra가 사용 가능한 최고의 모델입니다.

Clio는 Tablet/scroll 계층에서 컨텍스트 크기가 더 크지만 Kayra의 강점이 일반적으로 그 차이를 보완합니다.

## 설정

설정이 있는 파일은 여기에 있습니다(`SillyTavern/data/<user-handle>/NovelAI Settings`).
자체 설정 파일을 수동으로 추가할 수도 있습니다.

### 응답 길이

메시지당 생성하려는 텍스트 양. NovelAI에는 응답당 150개의 토큰 제한이 있습니다.

### 컨텍스트 크기

주어진 시간에 컨텍스트에 유지되는 채팅의 토큰 수. 사용할 수 있는 최대 컨텍스트 크기는 모델과 구독 계층에 따라 다릅니다:

- Kayra (Tablet) - 3072 tokens
- Kayra (Scroll) - 6144 tokens
- Erato (Opus exclusive), Kayra (Opus) and Clio (all tiers) - 8192 tokens

### Preamble

글쓰기 스타일을 수정하기 위해 채팅 바로 위에 삽입되는 텍스트. 권장 형식은 "[ Style: chat, detailed, sensory ]"와 같은 짧은 태그 목록입니다.

## 프리셋 설명
NovelAI에 따르면 기본 프리셋이 무엇에 좋은지입니다.

### Erato

* Golden Arrow - 좋은 올라운더.
* Wilder - 더 다양한 단어 선택, 다시 굴리기 간의 더 많은 차이, 실수가 더 많습니다.
* Zany Scribe - 실수와 반복을 피합니다. 더 복잡한 단어를 우선시합니다.
* Dragonfruit - 반복이 거의 없는 다양하고 복잡한 언어. 더 빈번한 실수와 모순.
* Shosetsu - 일본어로 글쓰기를 위해 설계되었습니다. 영어에도 잘 작동합니다.

### Kayra

* Asper - 창의적 글쓰기용. 예상치 못한 반전을 기대하세요.
* Carefree - 좋은 올라운더
* Fresh-Coffee - 일을 제대로 진행합니다. instruct를 잘 처리합니다.
* Pro_Writer - 베스트셀러 소설의 페이스와 느낌을 모방합니다
* Stelenes - 합리적인 대안을 선택할 가능성이 더 높습니다. 재시도 시 다양성.
* Tea_Time - 진행될수록 좋아집니다.
* Writers-Daemon - 매우 상상력이 풍부하며 때때로 너무 많습니다.

### Clio

* Edgewise - 다양한 생성 스타일을 잘 처리합니다
* Fresh Coffee - 일을 제대로 진행합니다.
* Long-Press - 창의적인 산문을 위한 것입니다.
* Talker Chat - 채팅 스타일 생성을 위해 설계되었습니다.
* Vingt-Un - 산문 쪽으로 기울어진 좋은 올라운드 기본값.

## SillyTavern과 함께 NovelAI를 사용하기 위한 팁과 FAQ

다른 ST 백엔드 API에서 NovelAI로 전환할 때 발생하는 많은 일반적인 문제와 질문이 있습니다. 차이점은 모델이 훈련된 목적으로 귀결됩니다. OpenAI 또는 Anthropic 모델(또는 이들을 닮도록 만들어진 로컬 모델)을 사용했을 가능성이 높으며 사용자의 지시를 따르도록 구축되었습니다. NovelAI의 모델은 순수하게 텍스트 완성을 위해 구축되었습니다: 입력을 메시지로 받아 응답을 공식화하는 대신 NAI의 모델은 들어오는 프롬프트를 계속하려고 시도합니다. 이러한 차이로 인해 다른 API에 적용되는 많은 팁과 일반적인 지식이 NAI에는 적용되지 않습니다.

### NovelAI용 설정 조정

Advanced Formatting(A 아이콘) 아래:
- "Context Template"을 "NovelAI"로 설정
- "Tokenizer"를 "Best match"로 설정
- "Always add character's name to prompt" 선택
- "Collapse Consecutive Newlines" 선택
- "Instruct Mode" 아래의 "Enabled" 상자 선택 취소

User Settings(톱니바퀴가 있는 사람) 아래
- "Swipes" 켜기 (NAI 특정이 아니지만 매우 유용하므로 그냥 수행해야 합니다)

### NovelAI용 캐릭터 카드 구축/적응

NovelAI용 캐릭터 카드를 최적화하려면 캐릭터 설명을 작성하는 몇 가지 권장 방법이 있습니다: 산문 및 속성.

산문은 너무 간단해서 작동해야 하는 것처럼 느껴지지 않습니다: "Sylpheed는 젊어 보이지만 실제로는 900살 된 님프입니다. 키가 작고 몸집이 작으며 녹색 그라데이션으로 페이드되는 긴 흰 머리를 땋은 사이드 포니테일로 묶고 십자가 모양의 에메랄드 녹색 눈을 가지고 있습니다.[...]" 아니요, 정말로, 그게 다입니다. 캐릭터가 어떻게 생겼는지, 어떻게 행동하는지 등을 일반 문장으로 작성하기만 하면 AI가 이를 인식합니다.

글쓰기 능력을 신뢰하지 않거나 이에 대해 더 구조화된 방법을 원한다면 NovelAI 훈련 데이터에 있는 속성 방법을 사용할 수 있습니다. 이것은 다양한 유형의 캐릭터 특성의 간단한 목록으로 작동합니다. 다음은 NovelAI의 모델에 효과적인 것으로 테스트된 가능한 속성 목록입니다:

```
Name:
AKA:
Type: character
Setting:
Nationality:
Species:
Gender:
Age:
Height:
Weight:
Appearance:
Clothing:
Attire:
Personality:
Mind:
Mental:
Likes:
Dislikes:
Sexuality:
Speech:
Voice:
Abilities:
Skills:
Quote:
Affiliation:
Occupation:
Reputation:
Secret:
Family:
Allies:
Enemies:
Background:
Description:
Attributes:
```

"Type: character"는 AI에게 이것이 캐릭터를 설명하고 있음을 알려줍니다(위치, 개체 또는 기타 유형의 것과 반대). 나머지 속성은 선택 사항이며 일부는 중복됩니다(예: Personality, Mind 및 Mental은 모두 기본적으로 동일한 의미). 그러나 이들은 테스트되었으며 NovelAI의 모델과 잘 작동합니다. 캐릭터와 관련된 것을 채웁니다. 속성은 소문자로 작성해야 하며 쉼표로 구분되며 단어 주위에 따옴표가 필요하지 않습니다. 예:

```
Skills: lockpicking, stealth, running away very fast
```

이러한 방법은 NovelAI의 훈련 데이터에 있기 때문에 권장됩니다. 따라서 모델과 특히 잘 작동합니다.

#### 예제 카드

다음은 NovelAI용으로 만들어진 몇 가지 예제 카드로 NovelAI 전용 카드를 만드는 다양한 방법을 보여줍니다. 첫 번째 카드인 Valka는 캐릭터 설명에 속성 방법을 사용하는 반면, 두 번째 카드인 Eris는 많은 양의 예제 대화와 함께 산문 설명을 사용합니다.

<div style="display:flex;gap:2em;justify-content:center">

[![Valka](/static/Valka.png)](/static/Valka.png)

[![Eris](/static/Eris.png)](/static/Eris.png)

</div>

#### 하지 말아야 할 것

대부분의 기존 캐릭터 카드 형식은 NovelAI에 잘 맞지 않습니다. 일부 좋은 결과도 제공하지만 많은 문제가 있습니다. W++는 가장 큰 범죄자 중 하나로 NovelAI의 모델이 훈련된 것과 유사하지 않으며 대괄호/중괄호/따옴표의 지속적인 사용은 실제 이점 없이 카드 크기를 부풀려 많은 토큰을 소비합니다.

NovelAI에 구워지지 않은 기존 형식 중 작동할 가능성이 가장 높은 것은 AliChat입니다. 예제 메시지를 사용하여 캐릭터에 대한 정보와 그들의 목소리를 동시에 전달하는 데 의존하며 AI가 출력하기를 원하는 메시지 유형의 형식으로 제공됩니다.

대부분의 다른 형식은 일반적으로 특정 캐릭터의 다양한 특성을 나열하는 방법이므로 속성 방법으로 상당히 간단하게 변환할 수 있습니다.

### 어떤 모듈을 사용해야 하나요?

아마도 모듈 없음. Prose Augmenter는 캐릭터가 더 화려한 방식으로 말하게 하려는 경우 유용하지만 과도하게 사용하지 않도록 주의하세요. Text Adventure는 텍스트 어드벤처 스타일 카드/스토리에 유용할 수 있습니다.

### instruct 모듈은 아닌가요?

필요할 때 Instruct 모듈을 호출할 수 있습니다. 메시지에 줄 바꿈을 만들고 다음과 같이 중괄호에 지시를 넣습니다: `{ CharName is offended by that seemingly innocuous statement }` (텍스트와 대괄호 사이에 공백이 _필수_입니다). 이렇게 하면 AI가 짧은 시간 동안 자동으로 Instruct 모듈로 전환됩니다. AI를 특정 방향으로 강력하게 안내해야 할 때만 필요하므로 항상 Instruct 모듈을 사용하고 싶지는 않습니다. 다른 모듈보다 덜 창의적인 출력을 생성하는 경향이 있기 때문입니다.

### 응답이 계속 잘리는 이유는 무엇인가요?

NovelAI는 슬라이더를 그보다 높게 설정하더라도 응답 길이를 ~150 토큰 전체로 제한합니다. 슬라이더의 토큰 수 또는 150 중 낮은 값에 도달하면 최대 20개의 추가 토큰을 생성하여 중지 시퀀스 또는 문장 끝을 찾으므로 응답에 대한 효과적인 제한은 170 토큰이며 이 시점에서 그냥 중지하여 잘립니다.

잘리면 계속 옵션을 선택하여(텍스트 상자 왼쪽의 3줄 메뉴에서) 캐릭터가 응답을 계속하도록 할 수 있습니다.

정기적으로 170 토큰보다 긴 응답을 원하는 경우 다음과 같이 제한을 해결할 수 있습니다:

- 응답 길이를 150 토큰으로 유지합니다.
- Advanced Formatting에서 Auto-continue를 활성화합니다.
- "Target length"를 원하는 길이로 설정합니다.

이것은 여러 생성을 함께 연결하여 더 긴 메시지를 제공하지만 모델이 중지하기로 결정하면 응답이 원하는 길이의 100%가 될 것이라고 보장하지 않습니다.

### 봇이 더 긴 응답을 쓰도록 하려면 어떻게 해야 하나요?

응답이 잘리는 것에 대한 위의 내용을 읽으세요. 이것은 생성 길이 제한에 도달하여 응답이 조기에 잘리지 않도록 하는 데 도움이 됩니다.

응답이 잘리지 않지만 여전히 너무 짧으면 "garbage in, garbage out"을 다루고 있을 가능성이 높습니다. 모델에 나쁜 예를 제공하면 나쁜 출력을 생성합니다. 캐릭터 카드에 예제 대화가 없거나 짧은 예제 대화가 있고 봇에 보내는 메시지가 짧으면 모델이 이를 인식하고 받아들여진 방식으로 받아들이고 응답이 짧아집니다. 따라서 더 긴 예제 대화와 봇에 더 긴 메시지를 작성하세요. (항상 NovelAI를 사용하여 직접 수행하는 대신 일부 예제 대화를 작성할 수 있습니다.)

### 봇이 나를 위해 말하는 것을 멈추게 하려면 어떻게 해야 하나요?

- 캐릭터 카드의 첫 번째 메시지와 예제 대화에 나를 위해 행동하는 캐릭터가 포함되어 있지 않은지 확인하세요. 포함되어 있으면 나를 위해 행동하는 것을 제거하도록 다시 작성하세요
- "Always add character's name to prompt"가 선택되어 있는지 확인하세요
- 현재 채팅의 나머지 부분과 동일한 사용자 페르소나를 사용하고 있는지 확인하세요. 사용자 페르소나를 변경하고 되돌리지 않은 경우(또는 해당 채팅에 잠긴 페르소나가 없는 경우) 나를 위해 생성을 중지하는 일반적인 규칙이 실패합니다
- ["\n\{\{user\}\}:"]를 Custom Stopping Strings에 추가합니다 (필요하지 않아야 하지만 때때로 도움이 됩니다)

### 캐릭터가 응답하지 않는 이유는 무엇인가요?

이것을 유발할 수 있는 많은 것들이 있으므로 몇 군데를 살펴봐야 합니다:

- Advanced Formatting에서 "Always add character's name to prompt"가 선택되어 있는지 확인하세요
- API에서 오류가 발생하지 않는지 확인하세요. NAI 무료 평가판과 함께 SillyTavern을 사용할 수 있지만 평가판이 소진되면 오류만 발생합니다
- "Custom Stopping Strings"에 있는 것을 확인하세요. 응답 시작 시 생성되는 경우 조기에 잘릴 수 있습니다

### Author's Note를 어떻게 사용해야 하나요?

일반적으로 사용하지 않아야 합니다. 컨텍스트 끝에 매우 가깝게 삽입되며 NAI의 모델에서는 컨텍스트의 다른 모든 것을 자주 압도합니다. 필요성이 더 많았던 이전의 더 약한 모델의 유물입니다.

### 장면 전환/시간 점프를 어떻게 수행하나요?

다음을 시스템 메시지로 또는 다음 메시지 시작 부분의 줄 바꿈에 넣으세요:
```
***
[ 2 days later ]
```

그런 다음 다음 줄에 메시지의 나머지 부분을 넣습니다. 대괄호 안의 텍스트는 시간 점프, 새 위치 또는 다른 것일 수 있습니다. "***"(재미있게도 "dinkus"라고 함)는 AI에게 장면이 바뀌었음을 알려주고 대괄호 안의 텍스트가 더 많은 컨텍스트를 제공합니다.

### AI가 특정 단어/구문을 계속 반복하는데 어떻게 해야 하나요?

위에서 언급했듯이 반복 페널티 슬라이더를 조금 더 올릴 수 있지만 너무 멀리 밀면 출력이 일관성이 없어질 수 있습니다.
문제를 더 철저히 해결하려면 컨텍스트, 특히 최근 메시지를 다시 살펴보고 반복된 단어/구문을 삭제하세요. 컨텍스트에서 제거하면 AI가 처음에 말할 이유가 줄어듭니다.
