---
route: /ko/extensions/summarize/
---

# Summarize

## 이게 뭔가요?

이 확장 기능을 사용하면 채팅에서 발생하는 이벤트를 기반으로 자동 생성된 요약을 생성, 저장 및 활용할 수 있습니다. 요약은 스토리에서 발생하는 일반적인 세부 사항을 개요하는 데 도움이 될 수 있으며, 이는 장기 기억으로 *해석될 수* 있지만 이 말을 약간의 회의적인 시각으로 받아들이세요. 요약은 언어 모델에 의해 생성되므로 출력이 일부 중요한 세부 사항을 잃거나 환각을 포함할 수 있으므로 항상 요약 상태를 추적하고 필요한 경우 수동으로 수정하는 것이 좋습니다.

## 일반 구성

요약 확장 기능은 SillyTavern에 기본적으로 설치되므로 다음과 같이 ST의 Extensions 패널(쌓인 큐브 아이콘) 목록에 표시됩니다:

![Summarize Config Panel](/static/extensions/summarize.png)

- **Current summary** - 현재 요약을 표시하고 수정할 수 있는 기능을 제공합니다. 요약은 요약이 생성될 때 컨텍스트에서 마지막이었던 메시지에 대한 채팅 파일의 메타데이터에 업데이트되고 포함됩니다. 요약이 첨부된 채팅에서 메시지를 삭제하거나 편집하면 마지막 유효한 요약으로 상태가 되돌아갑니다.
- **Restore Previous** - 현재 요약을 제거하고 이전 상태로 롤백합니다. 요약자가 특정 시점에서 제대로 작동하지 않을 때 유용합니다.
- **Pause** - 요약이 자동으로 업데이트되지 않도록 하려면 이것을 선택하세요. 자체 요약을 제공하거나 상자를 지우고 업데이트를 중지하여 요약을 효과적으로 비활성화하려는 경우 유용합니다.
- **Popup window** - 요약을 사이드바의 이동 가능한 UI 패널로 분리할 수 있습니다. 확장 메뉴를 탐색하지 않고도 요약 설정에 쉽게 액세스하기 위해 데스크톱 레이아웃에 유용합니다.
- **Injection Template** - 요약이 일반 채팅 프롬프트에 삽입될 때 어떻게 래핑될지 정의합니다. 프롬프트 주입 텍스트에서 현재 요약 상태의 정확한 위치를 나타내기 위해 특수 \{\{summary\}\} 매크로를 사용해야 합니다.
- **Injection Position** - 프롬프트 주입의 위치를 설정합니다. 옵션은 Author's Notes와 동일합니다: 메인 프롬프트 전후 또는 지정된 깊이의 채팅 내.

## 지원되는 요약 소스

### Main API

요약은 현재 선택된 AI 백엔드, 모델 및 설정에 의해 제공됩니다. 이 방법은 추가 설정이 필요하지 않으며 작동하는 API 연결만 있으면 됩니다.

이 옵션에는 요약 프롬프트가 구축되는 방식에 따라 다른 다음 하위 모드가 있습니다:

1. Raw, blocking. 요약은 요약 프롬프트와 채팅 기록만 사용하여 생성됩니다. 후속 프롬프트에는 요약이 생성된 후 전송된 메시지와 함께 이전 요약도 포함됩니다(예 참조). 이 모드는 프롬프트 간에 많은 변동성이 있는 프롬프트를 생성할 수 있으므로(그리고 생성할 것이므로) llama.cpp 및 그 파생 제품과 같이 프롬프트 처리 시간이 느린 백엔드에서 사용하는 것은 권장되지 않습니다.
2. Raw, non-blocking. 위와 동일하지만 채팅 생성이 요약 생성 중에 차단되지 않습니다. 모든 백엔드가 동시 요청을 지원하는 것은 아니므로 요약이 실패하면 blocking 모드로 전환하세요.
3. Classic, blocking. 요약 프롬프트는 캐릭터 카드, 메인 프롬프트, 예제 대화 및 채팅 프롬프트의 다른 부분을 생략하지 않고 중립적인 시스템 지시로 일반 생성 프롬프트의 끝에 전송됩니다. 이것은 일반적으로 처리된 프롬프트를 재사용하는 데 잘 작동하는 프롬프트를 생성하므로 llama.cpp 및 그 형제와 함께 사용하는 것이 좋습니다.

#### 요약 설정 설명

1. **Summary Prompt** - 요약을 생성하는 데 사용될 프롬프트를 정의합니다. 알려진 매크로와 특수 \{\{words\}\} 매크로(아래 참조)를 포함할 수 있습니다.
2. **Target summary length (words)** - Summary Prompt에 삽입할 수 있는 \{\{words\}\} 매크로의 값을 정의합니다. 매크로가 사용되지 않으면 이 설정은 완전히 선택 사항이며 전혀 효과가 없습니다.
3. **API response length (tokens)** - 전역적으로 설정된 값과 다른 요약을 생성하기 위한 재정의 API 응답 길이를 설정할 수 있습니다.
4. **Max messages per request _(raw 모드만)_** - 하나의 요약 프롬프트에 포함될 최대 메시지 수를 제한하도록 설정합니다. `0`은 명시적인 제한이 없음을 의미하지만 요약할 결과 메시지 수는 여전히 최대 컨텍스트 크기에 따라 달라지며 다음 공식을 사용하여 계산됩니다: `max summary buffer = context size - summarization prompt - previous summary - response length`. 큰 컨텍스트 크기를 가진 모델에서 더 집중적인 요약을 얻으려면 이것을 사용하세요.
5. **No WI/AN** - 요약할 텍스트에서 World Info 및 Author's Note를 생략합니다. Classic 프롬프트 빌더를 사용할 때만 효과가 있습니다. Raw 프롬프트 빌더는 항상 WI/AN을 생략합니다.
6. **Update every X messages** - 요약이 생성되는 간격을 설정합니다. `0`은 자동 요약이 비활성화됨을 의미하지만 "Summarize now" 버튼을 클릭하여 수동으로 트리거할 수 있습니다. 프롬프트 버퍼가 채팅 메시지로 완전히 채워지는 속도에 따라 조정해야 합니다. 이상적으로는 메시지가 프롬프트에서 삭제되기 시작할 때 첫 번째 요약이 생성되기를 원할 것입니다.
7. **Update every X words** - 위와 동일하지만 메시지 대신 단어(토큰이 아님!)를 사용합니다. 채팅 메시지의 내용이 일반적으로 얼마나 예측할 수 없는지에 대해 이론적으로 더 정확한 측정이 될 수 있지만 마일리지는 다를 수 있습니다.

두 "Update every" 슬라이더가 모두 0이 아닌 값으로 설정된 경우 둘 다 각각의 간격에서 요약 업데이트를 트리거하며, 먼저 발생하는 것에 따라 달라집니다. 다른 모델로 전환할 때 이러한 값을 적절하게 업데이트하는 것이 좋습니다. 그렇지 않으면 요약 생성이 너무 자주 트리거되거나 전혀 트리거되지 않을 수 있습니다.

간격 설정에 대해 확신이 없는 경우 "Update every" 슬라이더 위의 "마법 지팡이" 버튼을 클릭하여 몇 가지 간단한 휴리스틱을 기반으로 최적 값을 추측할 수 있습니다. 알고리즘에 대한 간단한 설명은 다음과 같습니다:

1. 모든 채팅 메시지의 토큰 및 단어 수 계산
2. 원하는 프롬프트 단어를 기반으로 대상 요약 길이 결정
3. 평균 메시지 길이를 기반으로 프롬프트에 맞을 수 있는 최대 메시지 수 계산
4. "Max messages"가 설정된 경우 요약 제한에 맞지 않는 메시지를 고려하여 평균 조정
5. 조정된 평균 메시지를 5의 배수로 반올림

#### 예제 프롬프트

**Raw 프롬프트**
```
System:
[Summarization prompt]

Previous summary.

User:
Message foo.

Char:
Message bar.
```

**Classic 프롬프트**
```
[Main prompt]

[Character card]

[Example dialogues]

User:
Message foo.

Char:
Message bar.

System:
[Summarization prompt]
```

### Extras API

`summarize` 모듈이 있는 Extras 서버는 보조 요약 모델(BART)을 실행할 수 있습니다.

컨텍스트 크기가 매우 작기 때문에(~1024 토큰) 큰 요약을 처리하는 능력이 상당히 제한적입니다.

Extras 요약 소스를 구성하려면 다음을 수행하세요:

1. [Extras](https://github.com/SillyTavern/SillyTavern-extras)를 최신 버전으로 설치 또는 업데이트하세요.
2. `summarize` 모듈이 활성화된 Extras를 실행하세요: `python server.py --enable-modules=summarize`

#### 요약 모델 변경

기본적으로 Summarize는 요약 목적으로 [Qiliang/bart-large-cnn-samsum-ChatGPT_v3](https://huggingface.co/Qiliang/bart-large-cnn-samsum-ChatGPT_v3) 모델을 사용합니다.

명령줄 인수 `--summarization-model=(###Hugging-Face-Model-URL-Here###)`를 사용하여 변경할 수 있습니다.

알려진 대체 Summarize 모델은 `Qiliang/bart-large-cnn-samsum-ElectrifAi_v10`입니다.
