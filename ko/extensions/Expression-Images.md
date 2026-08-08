---
route: /ko/extensions/expression-images/
---

# Character Expressions

## 이게 뭔가요?

표정 이미지는 채팅 창 옆(또는 뒤)에 표시되는 AI 캐릭터의 이미지(일명 '스프라이트')입니다.

표정 이미지는 AI의 최근 채팅 응답에 표현된 감정에 맞춰 분류에 따라 자동으로 변경될 수 있습니다.

## 캐릭터 표정 이미지 추가

1. Extensions 패널을 열고 'Character Expressions' 섹션을 확장하세요. 캐릭터 채팅이 열려 있으면 이미지 자리 표시자 그리드가 표시됩니다.
![Expression Drawer](/static/extensions/expression-drawer.png)
2. 그리드의 각 이미지 왼쪽 상단에 있는 'Upload image' 버튼을 클릭하고 해당 감정에 적용할 이미지를 선택하세요. 그러면 `/data/<user-handle>/characters/(character_name_here)/` 폴더 안에 올바른 파일 이름으로 이미지가 저장됩니다.
3. 이미지를 할당하려는 모든 표정에 대해 이 작업을 반복하세요.

### 표정 이미지 ZIP 파일 가져오기

'<i class="fa-solid fa-file-zipper"></i> Upload sprite pack (ZIP)' 버튼을 사용하여 표정 이미지 컬렉션이 포함된 zip 파일을 가져올 수 있으며, 이러한 이미지는 **현재 선택된 캐릭터**의 올바른 폴더에 자동으로 추가됩니다. ZIP 파일에는 평면 구조(하위 폴더 없음)와 올바르게 명명된 파일이 모두 포함되어야 합니다. zip을 가져와도 이미지 이름이 감정과 일치하도록 자동으로 이름이 변경되지 않습니다.

## 수동으로 표정 변경

1. 업로드된 표정 이미지(스프라이트) 중 하나를 클릭하여 채팅 인터페이스 근처(기본 UI 모드) 또는 화면 중앙(Visual Novel 모드)에 표시하세요.
2. `/expression-set (name)` 슬래시 명령 또는 일치하는 Quick Reply를 사용하여 확장 메뉴를 열지 않고 스프라이트를 설정하세요.

## 자동으로 표정 변경

캐릭터가 응답할 때 표정을 자동으로 설정하려면 여러 옵션이 있습니다.
메시지 스트리밍이 활성화되면 표정은 메시지당 또는 일정한 간격으로 변경됩니다.

### classify 모듈은 어떻게 작동하나요?

`classify` 모듈은 SillyTavern 서버와 함께 실행되는 작은 '감정 분석' 모델을 사용합니다. 이 모델은 AI의 새 출력을 가져와 텍스트가 표현하는 감정이나 정서의 종류를 감지합니다. 하나의 메시지에 여러 감정이 표현될 수 있지만 모델은 가장 가능성이 높은 감정만 선택하여 SillyTavern에 반환합니다. 그런 다음 프론트엔드 확장 기능이 해당 감정과 연결된 이미지를 표시합니다.

### 설정 지침 (로컬)

1. 확장 패널을 열고 "Character Expressions" 확장 메뉴를 확장하세요.
2. 분류 소스 드롭다운에서 "Local"을 선택하세요.
3. HuggingFace Hub에서 분류 모델의 일회성 다운로드가 시작됩니다(약 ~100 Mb).
4. 분류가 작동하고 스프라이트가 나타나는지 확인하기 위해 메시지를 생성하세요. 서버 콘솔에서 디버그 로그를 확인할 수도 있습니다.

로컬 분류는 기본적으로 28개의 가능한 이미지 레이블로 설정됩니다: [Cohee/distilbert-base-uncased-go-emotions-onnx](https://huggingface.co/Cohee/distilbert-base-uncased-go-emotions-onnx)

6-옵션 분류 모델을 사용하려면 `config.yaml` 파일의 `extensions.models.classification` 변수 값을 [Cohee/bert-base-uncased-emotion-onnx](https://huggingface.co/Cohee/bert-base-uncased-emotion-onnx)로 변경하세요.

### 설정 지침 (LLM 사용)

1. **<i class="fa-solid fa-plug"></i> API Connections**를 통해 지원되고 올바르게 구성된 API에 연결하세요.
2. 위에서 언급한 것과 동일한 방법으로 표정 이미지를 가져오세요.
3. 분류 소스 드롭다운에서 "Main API"를 선택하세요.
4. 선택적으로 분류 지시 프롬프트를 구성하세요.
5. 분류가 작동하고 스프라이트가 나타나는지 확인하기 위해 메시지를 생성하세요. 서버 콘솔에서 디버그 로그를 확인할 수도 있습니다.

#### 프롬프트 구축 전략

Main LLM 소스를 사용하면 분류 프롬프트를 구축하는 방법을 선택할 수 있습니다:

* **Limited Context**: 마지막 메시지와 시스템 지시 프롬프트만 전송됩니다.
* **Full Context**: 캐릭터 카드를 포함한 전체 채팅 기록이 전송됩니다.

### 설정 지침 (WebLLM)

1. 공식 [WebLLM extension](https://github.com/SillyTavern/Extension-WebLLM)을 설치하세요.
2. 위에서 언급한 것과 동일한 방법으로 표정 이미지를 가져오세요.
3. 분류 소스 드롭다운에서 "WebLLM"을 선택하세요.
4. 선택적으로 분류 지시 프롬프트를 구성하세요.
5. 분류가 작동하고 스프라이트가 나타나는지 확인하기 위해 메시지를 생성하세요. 서버 콘솔에서 디버그 로그를 확인할 수도 있습니다.

### 설정 지침 (Extras 사용)

> [!WARNING]
> Extras는 더 이상 사용되지 않으며 향후 업데이트에서 제거될 수 있습니다.

1. `classify` 모듈이 활성화된 Extras를 설치하고 실행하세요: `python server.py --enable-modules=classify`
2. 위에서 언급한 것과 동일한 방법으로 표정 이미지를 가져오세요.
3. 분류 소스 드롭다운에서 "Extras"를 선택하세요.
4. AI가 응답을 보낼 때마다 적절한 표정 이미지가 자동으로 표시됩니다.

Extras API는 기본적으로 6개 옵션이 있는 분류 모델을 사용합니다: [nateraw/bert-base-uncased-emotion](https://huggingface.co/nateraw/bert-base-uncased-emotion)

28개 옵션이 있는 모델도 있습니다: [joeddav/distilbert-base-uncased-go-emotions-student](https://huggingface.co/joeddav/distilbert-base-uncased-go-emotions-student)

이 모델을 사용하려면 Extras 명령줄에 다음 인수를 포함하도록 변경해야 합니다(앞뒤에 공백 포함): `--classification-model=joeddav/distilbert-base-uncased-go-emotions-student`

## 사용자 정의 표정

기본적으로 제공되는 것보다 더 많은 표정 옵션을 얻으려면 어떻게 해야 하나요? 확장 설정에서 **Custom Expressions**를 설정할 수 있습니다. Custom Expressions에 임의의 이름을 할당할 수 있습니다. 표정 이미지 목록에 나타나고 다른 표정처럼 이미지를 할당할 수 있습니다. 사용자 정의 표정임을 나타내는 표시기가 있습니다.

> [!TIP]
> Local과 Extras는 제한된 표정 목록만 지원합니다.
>
> Custom Expressions가 표시되도록 하려면 지원되는 레이블로 분류 모델을 훈련시키거나(이 가이드의 범위 밖), LLM 또는 WebLLM을 분류 소스로 사용할 수 있습니다. 둘 다 기본 및 사용자 정의 표정을 포함한 모든 기존 표정을 자동으로 사용합니다.

## 표정에 지원되는 이미지 형식은 무엇인가요?

webp 및 애니메이션 gif를 포함한 모든 이미지 형식이 허용됩니다.

가장 일반적인 형식은 투명한 배경의 PNG 파일입니다.

## 기본 표정 사용

캐릭터의 모든 표정에 대한 표정 이미지가 없거나 전혀 없는 경우 기본적으로 표시할 항목에 대한 여러 옵션이 있습니다.
'Default / Fallback Expression' 아래의 드롭다운을 통해 모두 선택할 수 있습니다.

1. **Choose a Fallback Expression**: 이미지가 없는 표정이 선택되면 대신 대체 표정이 표시됩니다. 드롭다운에서 사용 가능한 표정 중 하나를 선택하기만 하면 됩니다.
2. **[No Fallback]**: 이미지가 없으면 아무것도 표시하지 않습니다.
3. **[Default emojis]**: SillyTavern에 포함된 기본 제공 기본 표정을 사용할 수 있습니다. 간단한 이모지 스타일 이미지입니다.

## 표정당 여러 이미지 사용

표정당 여러 이미지를 추가하여 표시되는 표정에 더 많은 다양성을 허용할 수 있습니다.
이를 활성화하려면 **Allow multiple sprites per expression**을 토글하기만 하면 됩니다.
이제 둘 이상의 이미지를 업로드할 수 있으며 추가 이미지는 작은 마커와 함께 표시됩니다.

개별 이미지는 클릭하여 수동으로 선택하거나 `/expression-set type=sprite`를 통해 선택할 수 있으며, 이는 표정 대신 사용 가능한 스프라이트 이미지를 나열합니다.

여러 이미지가 있는 표정이 자동으로 선택되면 기존 이미지 중 하나가 무작위로 선택됩니다.
동일한 표정이 여러 번 사용될 때 해당 표정의 새 이미지를 강제로 선택하려면 **Re-roll if same sprite is used again**을 활성화할 수 있습니다.

### 표정당 여러 이미지에 대한 명명 규칙

표정당 여러 이미지의 경우 파일 이름을 특정 방식으로 지정해야 합니다.
파일은 표정 이름으로 시작한 다음 마침표 또는 대시로 구분된 접미사가 와야 합니다. 예: `joy.png`, `joy-1.png`, `joy.expressive.png`
직접 업로드와 ZIP 가져오기 모두 파일 이름이 이 형식을 따라야 합니다.

## 스프라이트 폴더 오버라이드

> [!NOTE]
> 표시 이름(캐릭터 카드 파일 이름이 아님)이 사용되는 이미지 세트를 결정합니다

동일한 표시 이름을 가진 캐릭터가 두 명 이상 있는 경우 모두 동일한 표정 이미지 세트를 사용합니다.

동일한 이름의 캐릭터의 각 버전에 다른 이미지 세트를 사용하려면 스프라이트 폴더 오버라이드를 사용할 수 있습니다.
폴더 오버라이드는 동일한 캐릭터의 다른 스프라이트 세트(의상 등)를 정의하는 데도 사용할 수 있습니다.

### 오버라이드 설정 방법

1. `/data/<user-handle>/characters`에 임의의 이름으로 폴더를 만들고 이미지를 거기에 넣으세요. 예: `/data/<user-handle>/characters/Boris`.
2. 스프라이트를 오버라이드하려는 캐릭터와의 채팅을 엽니다.
3. 오버라이드 폴더의 이름을 "Sprite Folder Override" 입력란에 입력하고 "Submit"을 클릭하세요.
4. Sprites 목록이 다시 로드되고 "Sprite set" 표시기에 오버라이드 폴더가 표시됩니다.
5. 또는 `/costume` 슬래시 명령을 사용하여 동일한 결과를 얻을 수 있습니다: `/costume Boris`.
6. 오버라이드 폴더 이름 앞에 백슬래시를 붙이면 현재 캐릭터 스프라이트 폴더의 하위 폴더로 확인됩니다. 예: Boris라는 캐릭터의 경우 `/costume \tracksuit`는 `/data/<user-handle>/characters/Boris/tracksuit` 폴더로 확인됩니다.
