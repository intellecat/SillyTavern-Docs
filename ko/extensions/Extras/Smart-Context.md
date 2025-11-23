---
route: /extensions/smart-context/
---

# Smart Context

## **이 확장 기능은 더 이상 유지 관리되지 않으며 사용을 권장하지 않습니다. 가능한 대안으로 [Chat Vectorization](/extensions/Chat-vectorization.md)을 고려하세요.**

!!!warning 면책 조항
이 확장 기능의 사용이 더 나은 채팅 경험이나 향상된 메모리를 보장하지 않습니다. 벡터 데이터베이스 활용의 모든 의미를 이해하는 경우에만 사용하세요.
!!!

### 이게 뭔가요?

Smart Context는 [ChromaDB 라이브러리](https://www.trychroma.com)를 사용하여 AI 캐릭터가 일반 채팅 기록 컨텍스트 제한 외부에 존재하는 정보에 액세스할 수 있도록 하는 SillyTavern 확장 기능입니다.

### 왜 유용한가요?

매우 긴 채팅이 있는 경우 대부분의 내용이 일반적인 컨텍스트 창 외부에 있으므로 AI가 응답을 작성할 때 사용할 수 없습니다.

Smart Context는 채팅 파일의 전체 기록을 자동으로 가져와 벡터 데이터베이스에 넣습니다. 그런 다음 채팅에 새로운 것을 입력할 때마다 이 데이터베이스를 검색하고 일치하는 키워드가 있는 메시지가 발견되면 해당 채팅 메시지가 컨텍스트에 배치되어 AI가 다음 응답을 작성할 때 볼 수 있습니다.

***

### 설정 지침

1. SillyTavern을 최소 버전 1.10.6으로 업데이트하세요.
2. Extensions 패널(쌓인 블록 아이콘)의 "Download Extensions & Assets" 메뉴에서 "Smart Context" 확장 기능을 설치하세요.
3. [Extras](https://github.com/SillyTavern/SillyTavern-extras)를 최신 버전으로 설치 또는 업데이트하세요. 또는 [Colab 노트북](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb)을 사용하세요.
4. *로컬 설치만:* Extras에 대한 requirements-complete.txt를 설치하세요(이전 설치에서 한 번 수행한 경우에도).
5. chromadb 모듈이 활성화된 Extras를 실행하세요: `python server.py --enable-modules=chromadb`

#### ChromaDB를 설치할 때 오류가 발생하나요?

```
ERROR: Could not build wheels for hnswlib, which is required to install pyproject.toml-based projects
```

chromadb 패키지를 설치하려면 다음 중 하나가 필요합니다:

- Visual C++ 빌드 도구가 설치되어 있어야 합니다: <https://visualstudio.microsoft.com/visual-cpp-build-tools/>
- conda에서 hnswlib 설치: `conda install -c conda-forge hnswlib`

***

### 구성

Smart Context가 활성화되면 SillyTavern UI에서 구성해야 합니다.
Smart Context 구성은 Extensions 메뉴 ![STExtensionMenuIcon](/static/extensions/menu-icon.png) 내에서 수행할 수 있습니다.

![Smart Context Config Panel](/static/extensions/smart-context.png)

알아야 할 4가지 주요 개념이 있습니다:

- Chat History Preservation
- Memory Injection Amount
- Individual Memory Length
- Injection Strategy

***

#### SmartContext는 채팅 기록에 10개의 메시지가 있은 후에만 시작됩니다

- 새 채팅이 시작될 때 ChromaDB는 비활성 상태입니다.
- 채팅에 10개의 메시지가 누적되면 데이터베이스에 모든 메시지를 기록하기 시작하고 필요에 따라 메시지를 불러옵니다.

#### Chat History Preservation ('kept mesages')

기본적으로 ChromaDB는 슬라이더에 지정된 만큼 최근 자연 채팅 기록 메시지를 유지합니다.
이 양을 초과하는 메시지는 전송된 프롬프트에서 제거되고 데이터베이스에 '메모리'가 있으면 이전 채팅 기록 메시지 대신 추가됩니다(아래 Strategy 참조).

***

#### Memory Injection Amount

Smart Context가 컨텍스트에 삽입할 최대 '메모리' 수입니다.
모든 주입 시도가 이 전체 양을 얻는 것은 아닙니다.
'개'와 관련된 입력을 보내고 DB에서 개와 관련된 다른 메시지가 하나뿐이면 1개 항목만 삽입됩니다.

***

#### Individual Memory Length

각 주입된 '메모리'에 허용되는 최대 길이입니다.
이것은 **문자** 단위입니다(토큰이 아님).
너무 작게 설정하면 메모리가 중간에 잘릴 수 있습니다.

예:

`Ross: I like dogs with long fur and fluffy tails. I dislike dogs with short fur and short tails.`

이 데이터베이스 '메모리'는 103자이므로 전체를 컨텍스트로 가져오려면 슬라이더를 최소 `103`으로 설정해야 합니다.

슬라이더가 103보다 작으면 메시지가 잘려서 그렇게 주입됩니다.

***

### Injection Strategy

#### Replace oldest history

이 전략은 X개의 최근 메시지를 유지하고 그 이전의 모든 메시지를 제거한 다음 '메모리'로 교체합니다.

장점

- 컨텍스트 제한을 초과할 가능성이 적음
- 컨텍스트 상단 근처에 존재하는 메모리는 '배경 정보'를 제공하면서 응답에 즉각적인 영향을 미칠 가능성이 적음.

단점

- 오래된 메시지는 특별한 구분 없이 채팅 기록에 직접 삽입되며 일반적으로 보존된 자연 채팅 기록 메시지와 즉각적인 자연 관련성이 없습니다. 이것은 덜 지능적인 AI 모델을 혼란스럽게 할 수 있습니다.

#### Add to Bottom

이 전략은 채팅 기록을 자연 상태로 유지하고 형식화된 [bracket header] 안에 '메모리'를 **뒤에** 추가합니다.
이것은 'kept messages' 슬라이더가 효과적으로 비활성화됨을 의미합니다.

장점

- 현재 자연 채팅 기록을 단축하거나 변경하지 않음
- '메모리'가 채팅 뒤에 존재하고 다음 AI 응답에 더 강한 영향을 미침

단점

- 채팅 항목이 제거/교체되지 않기 때문에 컨텍스트 제한을 초과할 가능성이 더 높음.
- 메모리가 프롬프트 끝에 매우 가까이 존재하기 때문에 AI의 응답에 너무 많은 영향을 미칠 수 있음.

#### Custom Depth

이 전략은 채팅 기록을 자연 상태로 유지하고 지정한 템플릿 내에서 결정한 깊이에 '메모리'를 추가합니다.
이것은 'kept messages' 슬라이더가 효과적으로 비활성화됨을 의미합니다.
사용자 정의 주입 메시지에는 모든 쿼리된 메모리가 배치될 위치인 `{{memories}}` 템플릿 단어가 포함되어야 합니다.

장점

- 메모리 배치를 실험할 수 있는 유연성
- 컨텍스트 내 메모리에 대한 사용자 정의 가능한 소개

단점

- 채팅 항목이 제거/교체되지 않기 때문에 컨텍스트 제한을 초과할 가능성이 더 높음.


#### Use % Strategy

참고: 이것은 'Add to Bottom' 전략과 호환되지 않습니다. 이 전략은 메시지를 전혀 제거하지 않습니다.

'Replace Oldest History' 전략을 사용하는 동안 이 상자를 선택하면 SmartContext 메모리로 교체할 컨텍스트 내 채팅 기록의 백분율을 선택하기 위한 슬라이더가 활성화됩니다. 또한 메시지 수를 수동으로 선택하기 위한 두 슬라이더가 비활성화됩니다.

이 전략은 고정된 메시지 수 대신 SmartContext 메모리로 교체할 채팅 기록의 백분율을 자동으로 계산합니다.

장점

- 메시지 수를 수동으로 계산하는 것보다 쉬움
- 사용 가능한 컨텍스트 크기에 맞춰 조정되어 작고 큰 프롬프트 공간에 동일한 백분율 적용

단점

- 제거할 기록의 양에 대한 계산은 메시지당 추정 토큰을 기반으로 하므로 약간 부정확할 수 있음
- 제거할 메시지 수를 5로 나눌 수 있는 가장 가까운 숫자로 반올림하므로(0, 5, 10, 15, 20 등) 수동 숫자 선택만큼 세밀하지 않음.

***

### Memory Recall Strategy

#### Recall only from this chat

이것은 smart-context의 기본 동작이며 이 특정 채팅에 대한 ChromaDB 컬렉션에서만 '메모리'를 가져옵니다.

#### Recall from all character chats

이것은 선택한 캐릭터에 대한 모든 ChromaDB 컬렉션에서 '메모리'를 가져오는 smart-context의 실험적 동작입니다.
가설적으로 이것은 많은 상호 작용에 걸쳐 더 강력한 메모리 세트의 개발을 허용해야 합니다.
'Add to Bottom' 또는 'Custom Depth' 전략과 함께 사용하고 'kept messages'를 낮은 숫자로 설정하여 ChromaDB가 메모리에서 더 빨리 가져오도록 하는 것이 좋습니다.

### Smart Context 사용

활성화되고 구성되면 Smart Context는 자동으로 발생합니다.

ChromaDB는 SillyTavern 내에서 열리는 각 채팅에 대해 새 데이터베이스를 만듭니다.
이 데이터베이스는 전체 채팅 기록으로 자동으로 채워집니다.

텍스트 파일을 데이터베이스에 수동으로 삽입할 수도 있습니다.

이러한 텍스트 파일은 채팅일 필요가 없습니다. 무엇이든 될 수 있습니다(위키백과 항목, 팬픽 등).

#### 데이터베이스 제거

'Purge DB' 버튼을 사용하여 현재 채팅에 대한 데이터베이스를 지울 수 있습니다.

이것은 부정확한 메모리가 저장된 경우(예: 삭제하거나 편집한 채팅 메시지) 유용할 수 있습니다.

***

### FAQ

#### 채팅이 끝나면 데이터베이스는 어떻게 되나요? 저장할 수 있나요?

로컬로 설치된 Extras 서버의 경우 Smart Context는 데이터베이스를 저장합니다. 일반적인 사용 사례에서는 수동으로 저장할 필요가 없습니다.

colab 사용자의 경우 extras 서버가 종료되면 데이터베이스가 삭제됩니다. export 버튼을 사용하여 데이터베이스를 JSON 파일로 저장하고 다음에 사용하려면 import하세요.

**일반적으로 Smart Context 데이터베이스를 저장할 필요가 없습니다.**

현재 채팅의 DB를 저장하고 나중에 다시 사용할 수 있는 Import/Export 기능이 있습니다.

#### 모든 채팅이 참조할 하나의 큰 데이터베이스를 만들 수 있나요?

이것은 Smart Context의 기능을 잘 사용하는 것이 아닙니다.
이 목적으로는 World Info를 사용하는 것이 좋습니다.
