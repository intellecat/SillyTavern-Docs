---
order: 60
route: /usage/prompts/cfg/
---

# CFG

작성자: kingbri

기여자: kingbri, Guillaume "Vermeille" Sanchez, AliCat

## 무엇인가요?

CFG, 즉 classifier-free guidance는 프롬프트의 일부를 덜 또는 더 두드러지게 만드는 데 사용되는 방법입니다.

### 지원되는 백엔드 APIs

현재 지원되는 백엔드는 oobabooga의 textgen WebUI, NovelAI 및 TabbyAPI입니다.
NovelAI에는 자체 [CFG 문서](https://web.archive.org/web/20240917150051/https://docs.novelai.net/text/cfg.html)가 있습니다.

경고: CFG는 하나 이상의 프롬프트를 수집하기 때문에 vram 사용량을 증가시킵니다! CFG가 켜진 상태에서 프롬프트를 생성하는 동안 GPU 메모리가 부족하면 컨텍스트 크기를 줄이거나, 더 적은 파라미터 모델을 사용하거나, CFG를 완전히 끄는 것을 고려하세요.

---

## 구성

CFG 설정에 액세스하는 것은 작가 노트에 액세스하는 것과 동일합니다:

![CFGhamburgermenupng](/static/cfg-hamburger.png)

다음은 CFG 패널의 모습입니다:

![CFGchatpanelpng](/static/cfg-panel.png)

CFG 패널에는 네 개의 드롭다운이 있습니다:

- Chat CFG

  - CFG 스케일 및 프롬프트를 이 채팅에만 적용합니다
- Character CFG

  - CFG 스케일 및 프롬프트를 지정된 캐릭터에 적용합니다
- Global CFG

  - CFG 스케일 및 프롬프트를 전역적으로 재정의합니다(모델 프리셋도 재정의합니다!)
- CFG Advanced Settings (이전에는 CFG Prompt Cascading이라고 함)

  - 이전 3개의 드롭다운에서 프롬프트를 결합하고 삽입 깊이를 설정하는 장소입니다.

참고: guidance scale이 1로 설정되면 CFG가 "꺼진" 상태이므로 아무것도 전송되지 않습니다.

#### 그룹 채팅

그룹 채팅에서 CFG 스케일 패널은 다음과 같습니다:

![CFGpanelgcpng](/static/cfg-groups.png)

주요 변경 사항은 character CFG가 제거되고 채팅 CFG 드롭다운에 `Use Character CFG Scales`라는 확인란이 있다는 것입니다. 이를 통해 채팅 CFG 스케일이 설정된 것 대신 현재 캐릭터의 guidance scale을 사용할 수 있습니다.

이 기능의 주요 유용성은 각 캐릭터의 개별 요구에 따라 스케일을 변경하는 것입니다.

또한 프롬프트 cascading에서 `Character Negatives` 상자를 확인하면 채팅 negative 프롬프트와 함께 독립적인 캐릭터 negative 프롬프트가 추가됩니다(활성화된 경우).

---

## 개념

### Stable Diffusion에 있지 않나요?

네이자 아니오. LLM을 사용한 CFG는 Stable Diffusion에서 익숙할 수 있는 것과 다른 방식으로 작동합니다. LLM 기반 CFG는 "프롬프트 믹싱" 원리로 작동합니다. CFG 공식은 positive 및 negative 프롬프트를 가져온 다음 그 사이의 *차이*를 혼합합니다. 거기에서 결합된 프롬프트가 전송되고 응답이 생성됩니다!

다음은 이 개념을 시각화하는 데 도움이 되는 그림입니다. 빨간색은 negative 프롬프트를 나타내고, 파란색은 중립 프롬프트를 나타내며, 보라색은 해석되는 혼합된 결과를 나타냅니다. 모든 흰색 공간은 3개의 프롬프트 모두에서 동일하므로 CFG 믹싱에 사용되지 않습니다.

![stcfgdiagrampng](/static/cfg-diagram.png)

CFG와 LLM에 대해 더 알고 싶다면 Vermifuge의 원본 논문이 여기에 있습니다. 읽거나 듣는 것을 권장합니다:

- Paper - [[2306.17806] Stay on topic with Classifier-Free Guidance (arxiv.org)](https://arxiv.org/abs//2306.17806)

- Audio version - [https://www.youtube.com/watch?v=MGY00YFcyco](https://www.youtube.com/watch?v=MGY00YFcyco)


### CFG 프롬프트가 필요한가요?

아니요! CFG 프롬프트는 완전히 선택 사항입니다. guidance scale을 `1` 이상으로 조정하는 것만으로도 응답에 효과를 내는 데 도움이 되며, 이는 채팅 및 캐릭터 상호작용을 강조할 수 있습니다.

### 좋은 CFG 프롬프트를 만드는 것은 무엇인가요?

그래서, 우리는 CFG 프롬프팅이 Stable Diffusion의 negative 태그 및 임베딩과 같지 않다는 것을 확립했습니다. 어떻게 프롬프트를 만들까요?

경고: 이것은 PLists 및 Ali:Chat을 사용하여 캐릭터를 만들었다고 가정합니다. 그렇지 않은 경우 다양한 프롬프팅 기술을 실험해 보세요.

"John"이라는 캐릭터가 있다고 가정해 봅시다. John은 예시 대화에서 항상 행복하고 흥분한 느낌을 받아야 합니다. 그러나 John과 채팅할 때 때때로 슬프고 우울합니다.

이것을 제거하기 위해 CFG가 구출됩니다! negative 프롬프트 `[John's feelings: sad, depressed]`를 만들어 슬픔 부분을 제거하는 데 도움을 줍니다. 선택적으로 positive 프롬프트 `[John's feelings: happy, joyful]`를 만들어 John의 행복한 부분을 더 끌어낼 수 있습니다.

### Positive Prompts

이전 섹션에서 이것을 다루었지만, 조금 더 다루고 싶습니다. Positive 프롬프트는 캐릭터의 일부를 더 강조하는 데 사용됩니다. John을 다시 예로 들어봅시다. `[John's feelings: happy, joyful]`의 positive 프롬프트로 그를 더 행복하게 만들면 John은 positive 프롬프트가 포함되지 않은 경우보다 더 행복한 느낌으로 대화를 출력하기 시작해야 합니다.

### 하지만...

이것들은 하나의 특정 캐릭터 형식에 대한 경험에서 나온 **느슨한 지침**일 뿐입니다. 실험해야 할 많은 다른 방법으로 프롬프트를 만들 수 있습니다. 다른 사용자와 생각을 공유해 보세요!

### Guidance Scale

다음은 경험 법칙입니다. guidance scale `1`은 CFG가 비활성화되었다는 것을 의미합니다. 실제로 SillyTavern은 guidance scale이 1이면 백엔드에 아무것도 보내지 않습니다. guidance scale `>1`은 다른 섹션에 표시된 결과를 다양한 정도로 제공합니다.

그러나 guidance scale `<1`은 negative 프롬프트가 여기서 기본 프롬프트로 사용되기 때문에 *반대* 효과를 제공합니다.

John을 다시 예로 들어봅시다. negative 프롬프트는 `[John's feelings: sad, depressed]`이고 positive 프롬프트는 `[John's feelings: happy, joyful]`이며 guidance scale은 `0.8`입니다.

이것은 차례로 *negative* 프롬프트를 더 강조하고 John이 행복한 것보다 평소보다 더 슬프게 행동하기 시작하는 것을 볼 수 있습니다.

요약; guidance scale `1.5`를 사용하고 출력에 따라 위아래로 작업하세요.

### Prompt Cascading

Negatives 및 positives는 CFG 유형(채팅별, 캐릭터별 및 전역 재정의 유형) 간에 cascade될 수 있습니다. 자세한 내용은 Configuration 헤더를 참조하세요.

### Insertion Depth

기본 규칙을 따르세요: 프롬프트에서 아래쪽에 위치할수록 응답에 더 영향을 미칩니다. 채팅의 경우 SillyTavern의 다른 구성 요소와 매우 유연하기 때문에 기본 깊이 `1`을 사용하는 것이 좋습니다.

그러나 실험하고 싶다면 삽입 깊이 `0`이 열려 있습니다. 그러나 이것들은 응답이 어떻게 보일지 극적으로 바꿀 수 있으며 여기서 프롬프트 cascading을 사용하는 것은 권장되지 않습니다!
