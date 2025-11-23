---
route: /extensions/vrm/
---

# VRM

이 가이드는 SillyTavern 경험을 위한 VRM 확장 기능을 설정하고 사용자 정의하는 과정을 안내합니다. 이 확장 기능을 사용하면 캐릭터에 VRM 애니메이션 모델을 사용하여 가상 캐릭터에 동적이고 대화형 요소를 제공할 수 있습니다.

## 사전 요구 사항

시작하기 전에 다음 사전 요구 사항을 충족했는지 확인하세요:

1. **브랜치 선택**: 최신 기능과 업데이트에 액세스하려면 SillyTavern의 최신 버전 브랜치를 사용하고 있는지 확인하세요.

2. **확장 기능 설치**: Extensions 패널(쌓인 블록 아이콘으로 표시됨)의 "Download Extensions & Assets" 메뉴에서 "VRM" 확장 기능을 설치합니다.

3. **모델 폴더 배치**: VRM 모델 파일(.vrm)을 `/data/<user-handle>/assets/vrm/model` 디렉토리에, 애니메이션 파일을 `/data/<user-handle>/assets/vrm/animation` 디렉토리에 배치합니다. 현재 지원되는 애니메이션 파일 형식은 VRM 모델과 호환되는 .fbx 및 .bvh입니다. 여기에는 Mixamo (https://www.mixamo.com/)에서 얻을 수 있는 모든 애니메이션과 XR Animator (https://github.com/ButzYung/SystemAnimatorOnline)와 같은 도구에서 내보낼 수 있는 모든 애니메이션이 포함됩니다.

## 확장 기능 설정

VRM 확장 기능은 애니메이션 모델의 동작을 사용자 정의할 수 있는 다양한 설정을 제공합니다. 주요 설정은 다음과 같습니다:

![UI global settings](/static/extensions/vrm-global.png)

### 전역 설정

1. **Enabled**:
   - VRM 모델이 SillyTavern 내에서 상호 작용할 수 있도록 확장 기능을 활성화하려면 이 체크박스를 활성화합니다.
   - 일반 스프라이트만 사용하려면 확장 기능을 비활성화할 수 있습니다.

2. **Look at camera**:
   - VRM 모델의 눈이 카메라를 바라보도록 하려면 이 체크박스를 활성화합니다.

3. **Blink**:
   - VRM 모델의 눈이 무작위 간격으로 깜박이도록 하려면 이 체크박스를 활성화합니다. 모델 표정은 깜박임 가중치 속성을 적절하게 정의해야 합니다. 그렇지 않으면 예를 들어 눈을 감은 상태로 모델이 깜박일 수 있습니다. 이런 경우:
    - .vroid 파일이 있는 경우 모델을 수정합니다
    - 잘못된 얼굴 표정을 사용하지 않습니다
    - 이 체크박스로 깜박임을 완전히 비활성화합니다

4. **TTS Lip sync**
    - TTS가 재생될 때 VRM 입 움직임이 소리를 따라가도록 하려면 이 체크박스를 활성화합니다. XTTS(스트리밍 모드가 아닌)와 같이 SillyTavern 자체에서 소리를 재생하는 TTS에서만 작동합니다. 비활성화하면 새 캐릭터 메시지를 받을 때 메시지 텍스트 길이에 따라 입이 애니메이션됩니다.

5. **Auto-send Interaction**:
   - 매핑된 메시지가 있는 영역을 클릭할 때 캐릭터 상호 작용을 자동으로 트리거하려면 이 체크박스를 활성화합니다(히트 영역 섹션 참조).

### 성능 설정

1. **Body hitboxes**
    - 모델에 따라 VRM 모델의 여러 부분에 대한 클릭 감지를 활성화하려면 이 체크박스를 활성화합니다. 다음 영역을 감지할 수 있습니다: head/chest/hands/groin/butt/legs/feets. 히트박스 위치는 각 프레임에서 계산되며 신체 애니메이션을 따릅니다. 이 옵션을 비활성화하면 성능이 향상될 수 있습니다.

2. **Use model cache**
    - 모델을 전환할 때 메모리에 VRM 모델을 유지하려면 이 체크박스를 활성화합니다. 이전 모델로 더 빠르게 전환할 수 있습니다. 예를 들어 의상이나 형태를 변경하기 위해 같은 캐릭터에 다른 모델을 사용하는 경우 유용합니다. 성능에 영향을 줄 수 있습니다.

3. **Use animation cache**
    - 세션 중에 재생된 모든 애니메이션을 메모리에 유지하려면 이 체크박스를 활성화합니다. 모델에 할당된 모든 애니메이션도 모델이 처음 나타날 때 로드됩니다. 처음 모델을 로드하는 시간이 증가하지만 모든 애니메이션 전환이 즉시 이루어집니다. 성능에 영향을 줄 수 있습니다.

### 디버그 설정

1. **Show grid**
    - 3D 그리드, 모델 드래그 상자 및 신체 히트박스를 시각화하려면 이 체크박스를 활성화합니다.

2. **Reload button**
    - 3D 장면을 다시 로드하고 캐시와 모든 VRM 모델을 지우려면 이 버튼을 클릭합니다. 버그가 발생하거나 캐시가 성능에 영향을 미치기 시작하면 사용하세요.

### 장면 설정

![UI scene settings](/static/extensions/vrm-scene.png)

1. **Light Color**
    - 3D 장면의 조명 색상을 설정합니다. 기본 흰색으로 다시 설정하려면 재설정 버튼을 클릭합니다. 브라우저에 따라 색상 선택기를 사용할 수 있습니다. 예를 들어 배경 이미지의 색상을 선택하여 더 몰입감을 높일 수 있습니다.

2. **Light intensity**
    - 슬라이더를 사용하여 백분율로 조명 강도를 설정합니다. 기본값인 100%로 다시 설정하려면 재설정 버튼을 클릭합니다. VRM 모델은 모델에 포함된 셰이더에 따라 조명에 다르게 반응할 수 있으므로 값을 조정하여 어떻게 되는지 확인하세요.

![UI model settings](/static/extensions/vrm-model.png)

## 캐릭터 선택

이 설정을 사용하면 캐릭터를 관리하고 VRM 모델을 할당할 수 있습니다.

1. **Refresh Button**:
   - 새로 고침 버튼을 클릭하여 현재 채팅의 캐릭터 목록을 업데이트합니다.

2. **Select Character**:
   - 드롭다운 목록을 사용하여 VRM 모델을 할당할 캐릭터를 선택합니다.

3. **Remove Button**:
   - 이 버튼을 클릭하여 캐릭터에 할당된 모델을 삭제합니다.

## 모델 선택

1. **Refresh Button**:
   - VRM 모델이 목록에 표시되지 않으면 새로 고침 버튼을 클릭합니다.

2. **Select Model**:
   - 목록에서 모델을 선택하여 선택한 캐릭터에 할당합니다.
   - 모델은 `/data/<user-handle>/assets/vrm/model` 디렉토리에 있어야 합니다.

3. **Reset button**
    - 이 버튼을 클릭하여 모델 설정을 기본값으로 재설정합니다. 기본값에 해당하는 애니메이션 파일이 있으면 자동으로 매핑됩니다. 이 README 끝부분의 이름 매핑을 참조하세요.

## 모델 설정

1. **Model Scale**:
   - 슬라이더를 사용하여 모델 크기를 조정하여 더 크게 또는 더 작게 만듭니다.

2. **Model Center X/Y Offset**:
   - 이 슬라이더를 사용하여 창 중심에 대한 모델의 가로/세로 위치를 변경합니다.

3. **Model X/Y Rotation**
    - 이 슬라이더를 사용하여 모델 엉덩이에 대한 모델의 가로/세로 회전을 변경합니다.

### 비고
    - 설정은 캐릭터별로가 아니라 모델별로 저장되며 다른 채팅에도 적용됩니다.
    - 다른 설정으로 두 개의 다른 캐릭터에 같은 모델을 사용하려면 .vrm 파일의 복사본을 만드세요.
    - 마우스로 모델을 드래그할 수도 있으며 이러한 설정이 업데이트되고 저장됩니다. 왼쪽 클릭하고 길게 눌러 모델을 화면 주위로 드래그합니다. 중간 마우스 클릭하고 길게 눌러 모델을 회전하거나 shift-왼쪽 클릭을 사용합니다. 모델에 커서를 놓고 마우스 휠을 사용하여 확대 또는 축소하거나 ctrl+왼쪽 클릭을 사용합니다.
    - 어떻게든 화면 밖으로 모델을 만든 경우 이러한 UI 설정을 사용하여 모델을 화면으로 되돌립니다. 또한 "Show frame" 체크박스를 선택하여 모델을 드래그하기 위해 클릭할 수 있는 위치를 명확하게 확인하세요.

![UI hitboxes settings](/static/extensions/vrm-hitboxes.png)

## 히트박스 매핑

    - 모델 뼈 정의에 따라 일부 히트박스 영역이 생성될 수 있으며 UI의 이 부분에 나열되며 클릭할 때 트리거될 각 영역에 표정/애니메이션/메시지를 할당할 수 있습니다.

![UI classify settings](/static/extensions/vrm-classify.png)

## 분류된 표정 매핑

1. **요구 사항**
    - classify expression 확장 기능을 사용해야 합니다. 그렇지 않으면 기본 애니메이션으로 대체됩니다.

2. **매핑**
    - classify 확장 기능에서 감지된 각 감정에 대해 표정/모션/메시지를 할당할 수 있습니다. 메시지에는 명령이 포함될 수 있습니다.

## 명령

1. **/vrmlightcolor**
    - 조명 색상을 설정합니다
    - 인수: color
    - 예: "/vrmlightcolor white" 또는 "/vrmlightcolor purple".
2. **/vrmlightintensity**
    - 조명 강도를 백분율로 설정합니다
    - 인수: intensity
    - 예: "/vrmlightintensity 0" 또는 "/vrmlightintensity 100
3. **/vrmmodel**
    - vrm 모델을 캐릭터에 할당합니다
    - 인수: character, model
    - 예: 솔로 채팅에서 "/vrmmodel Seraphina.vrm" 또는 그룹 채팅에서 "/vrmmodel character=Seraphina model=Seraphina.vrm"
4. **/vrmexpression**
    - 모델의 표정을 변경합니다
    - 인수: character, expression
    - 예: 솔로 채팅에서 "/vrmexpression happy" 또는 그룹 채팅에서 "/vrmexpression character=Seraphina expression=happy"

5. **/vrmmotion**
    - 모델의 애니메이션을 변경합니다
    - 인수: character, motion, loop, random
    - "/vrmmotion idle" 또는 "/vrmmotion character=Seraphina motion=idle loop=true random=false"

## 애니메이션 기본 매핑
애니메이션 파일 이름이 다음과 같은 방식으로 지정되면 모델 설정을 재설정할 때 자동으로 매핑됩니다. 예를 들어 "assets/vrm/animation/neutral.bvh" 및 "assets/vrm/animation/neutral1.fbx"라는 파일은 기본 및 neutral 분류 애니메이션에 대한 그룹으로 자동 매핑됩니다. 히트박스도 마찬가지입니다.

    // Fallback
    "default": "assets/vrm/animation/neutral",

    // Classify class
    "admiration": "assets/vrm/animation/admiration",
    "amusement": "assets/vrm/animation/amusement",
    "anger": "assets/vrm/animation/anger",
    "annoyance": "assets/vrm/animation/annoyance",
    "approval": "assets/vrm/animation/approval",
    "caring": "assets/vrm/animation/caring",
    "confusion": "assets/vrm/animation/confusion",
    "curiosity": "assets/vrm/animation/curiosity",
    "desire": "assets/vrm/animation/desire",
    "disappointment": "assets/vrm/animation/disappointment",
    "disapproval": "assets/vrm/animation/disapproval",
    "disgust": "assets/vrm/animation/disgust",
    "embarrassment": "assets/vrm/animation/embarrassment",
    "excitement": "assets/vrm/animation/excitement",
    "fear": "assets/vrm/animation/fear",
    "gratitude": "assets/vrm/animation/gratitude",
    "grief": "assets/vrm/animation/grief",
    "joy": "assets/vrm/animation/joy",
    "love": "assets/vrm/animation/love",
    "nervousness": "assets/vrm/animation/nervousness",
    "neutral": "assets/vrm/animation/neutral",
    "optimism": "assets/vrm/animation/optimism",
    "pride": "assets/vrm/animation/pride",
    "realization": "assets/vrm/animation/realization",
    "relief": "assets/vrm/animation/relief",
    "remorse": "assets/vrm/animation/remorse",
    "sadness": "assets/vrm/animation/sadness",
    "surprise": "assets/vrm/animation/surprise",

    // Hitboxes
    "head": "assets/vrm/animation/hitarea_head",
    "chest": "assets/vrm/animation/hitarea_chest",
    "groin": "assets/vrm/animation/hitarea_groin",
    "butt": "assets/vrm/animation/hitarea_butt",
    "leftHand": "assets/vrm/animation/hitarea_hands",
    "rightHand": "assets/vrm/animation/hitarea_hands",
    "leftLeg": "assets/vrm/animation/hitarea_leg",
    "rightLeg": "assets/vrm/animation/hitarea_leg",
    "rightFoot": "assets/vrm/animation/hitarea_foot",
    "leftFoot": "assets/vrm/animation/hitarea_foot"

이 가이드를 따라 주셔서 감사합니다! 이제 SillyTavern 경험이 애니메이션 및 대화형 3D 모델로 풍부해졌습니다.

## 비고
    - 이 확장 기능에서 로드하는 VRM 모델은 .vroid 파일이 아닌 .vrm 파일입니다.
    - 애니메이션 파일은 VRM과 호환되어야 합니다. XR animation (https://github.com/ButzYung/SystemAnimatorOnline)과 같은 도구를 사용하여 fbx/bvh 애니메이션 파일을 변환할 수 있습니다.
    - 같은 이름으로 끝나는 다른 숫자의 파일을 포함하여 애니메이션 그룹을 만들 수 있습니다. 예를 들어: "idle1.bvh", "idle2.bhv", "idle3.bvh"는 하나의 그룹 "idle"로 간주되며 매핑에서 선택하면 트리거될 때 무작위로 재생되어 애니메이션에 다양성을 추가할 수 있습니다.
    - 이 저장소에서 큐레이팅된 애니메이션을 얻을 수 있습니다: https://github.com/test157t/VRM-Animations-Pack-For-Silly-Tavern
    - Nitral에는 확장 기능 및 애니메이션 저장소 사용 방법에 대한 튜토리얼 비디오가 있습니다: https://www.youtube.com/@nitralai
