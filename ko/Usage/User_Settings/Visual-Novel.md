---
order: 10
tags:
    [
        visual novel,
        vn,
    ]
route: /ko/usage/user-settings/visual-novel/
---

# Visual Novel (VN) 모드

Visual Novel 모드는 Doki Doki Literature Club, The Fruits of Grisaia, Fate: Stay/night 및 기타 유명한 VN 게임과 같은 비주얼 노벨과 유사한 스프라이트(또는 캐릭터 카드 이미지)가 있는 캐릭터와 채팅할 수 있도록 하는 SillyTavern의 특수 화면 레이아웃입니다.

## Visual Novel 모드 토글

### Visual Novel 모드 활성화

Visual Novel 모드는 SillyTavern에 내장되어 있으며 *User Settings*(User Settings 아이콘)로 이동하여 *No Text Shadows* 아래의 **Visual Novel Mode**를 선택하여 토글할 수 있습니다.

![User Settings](/static/vn/vn-mode-toggle.png)

### Visual Novel 모드 비활성화

Visual Novel 모드를 비활성화하는 것은 활성화하는 것과 동일한 단계입니다. Visual Novel Mode를 토글 해제하면 일반 채팅 화면으로 돌아갑니다.

!!!warning VN Extensions와 함께 VN Mode에 대해
일부 확장 프로그램(Prome VN Extension과 같은)은 자체 VN 모드를 사용하는 경우 'Visual Novel Mode'를 켭니다. *User Settings* 메뉴에서 VN Mode를 활성화/비활성화하는 것도 이러한 확장 프로그램에 영향을 미칩니다.
!!!

## Visual Novel UI

![VN Display](/static/vn/vn-display.png)

Visual Novel 모드에서 UI는 중앙에 표시되는 캐릭터 스프라이트(또는 캐릭터 카드 이미지)를 수용하기 위해 약간 변경됩니다. 그러나 여러 캐릭터가 있는 그룹 채팅에서는 아래와 같이 캐릭터 스프라이트가 서로를 수용하여 펼쳐집니다.

![Group VN Display](/static/vn/group-vn-display.png)

### MovingUI와 함께 VN Mode

!!!info
MovingUI를 토글하려면 *User Settings*로 이동하여 **MovingUI**를 선택하세요. 이 기능은 **데스크톱에서만** 작동합니다.
!!!

*User Settings*에서 **MovingUI**가 활성화된 경우, 스프라이트(또는 캐릭터 카드 이미지)를 이동하거나 화면의 더 특정 영역에 배치하려는 경우 이동할 수 있습니다.

!!!warning 스프라이트 크기에 대해
캐릭터 스프라이트의 크기가 상대적으로 큰 경우 스프라이트를 드래그하는 버튼이 기존 스프라이트 아래에 가려질 수 있으므로 MovingUI를 사용하여 특정 스프라이트를 이동하는 것이 어려울 수 있습니다. 더 나은 배치를 위해 특히 화면에 더 많은 캐릭터가 있는 경우 평소보다 조금 더 이동해야 할 수 있습니다.
!!!

![Group VN Display (MovingUI)](/static/vn/vn-group-display-movingui.png)

## 캐릭터 스프라이트 얻기

캐릭터 스프라이트를 얻는 것은 인터넷에서 기존 스프라이트를 찾아볼 수 있습니다. 예를 들어 DDLC 또는 CounterSide와 같은 Visual Novel 기능을 사용하는 게임의 기존 캐릭터에 대한 것입니다. 스프라이트를 원하는 캐릭터에 스프라이트가 이미 함께 제공되지 않는 경우 남은 옵션이 여러 가지 있습니다.

1. 스프라이트 ZIP 패키지 또는 스프라이트 팩에 대한 링크를 찾기 위해 캐릭터 게시물을 검색합니다.
    !!!info
    일부 봇 제작자는 스프라이트 팩과 함께 봇을 릴리스할 수 있습니다(동일한 게시물 내에서 또는 스프라이트 채널에서). 원하는 캐릭터의 스프라이트를 이미 만든 사람이 없는지 해당 게시물을 검색하세요.
    !!!
2. LoRAs 및 Stable Diffusion을 사용하여 직접 만듭니다.
    !!!warning
    처음부터 스프라이트를 생성하는 것은 시간이 많이 걸립니다(특히 캐릭터에 대한 LoRAs가 없거나 사용하려는 Stable Diffusion 모델에 대한 LoRAs가 없는 경우). 6개보다 28개의 스프라이트 표현을 만들 계획이고 SDXL을 사용하거나 스프라이트를 더 높은 해상도로 업스케일하는 경우 더욱 그렇습니다. 이를 생성하려면 괜찮은 하드웨어가 필요합니다.
    !!!
3. 캐릭터 카드 이미지를 사용합니다. 스프라이트처럼 보이지 않을 수 있지만 적어도 화면에서 볼 수 있는 것이 있습니다. 그러나 VN 모드에서는 여러 캐릭터 카드를 사용할 수 없습니다.
    !!! Prome Visual Novel Extension과 함께 캐릭터 카드 이미지
    Prome Visual Novel Extension 1.0.6+에서는 캐릭터 카드를 채팅의 스프라이트로 사용하여 스프라이트 및 비스프라이트 캐릭터와 그룹 채팅을 할 수 있는 `Emulate Character Card as Sprite`라는 기능이 있습니다.

    ![Character Card Group Chat](/static/vn/extensions/prome/card-emulation.png)
    !!!

## VN Extensions

### Prome Visual Novel Extension

Prome Visual Novel Extension은 Bronya Rand 및 Prometheus의 승인된 타사 확장 프로그램으로, Letterbox Mode(비주얼 노벨 UI를 더 "영화적"으로 만듦), Darken Character Sprites가 있는 Focus Mode, 채팅의 마지막 메시지만 채팅에 나타나는 Traditional VN Mode 등과 같은 기능으로 SillyTavern의 비주얼 노벨 경험을 더욱 향상시키며 더 많은 것이 계획되어 있습니다!

|                              Letterbox Mode                              |                          Traditional VN Mode                           |
|:------------------------------------------------------------------------:|:----------------------------------------------------------------------:|
| ![Horizontal Letterbox Mode](/static/vn/extensions/prome/horizontal.png) | ![Traditional VN Mode](/static/vn/extensions/prome/single-message.png) |

|                 Hide Sheld (Message Box)                  |                      Focus Mode (w/ Darken Sprites)                      |
|:---------------------------------------------------------:|:------------------------------------------------------------------------:|
| ![Sheld Hide](/static/vn/extensions/prome/sheld_hide.png) | ![Focus Mode w/ Darken Sprites](/static/vn/extensions/prome/defocus.png) |

Prome Visual Novel Extension을 설치하려면 `Download Extensions & Assets`로 이동하여 *Prome Visual Novel Extension*을 찾거나 [Prome Visual Novel Extension](https://github.com/Bronya-Rand/Prome-VN-Extension?tab=readme-ov-file#installation-and-usage) Github 페이지의 설치 지침을 따르세요. Prome의 설정 조정은 *Extensions* -> **Prome (Visual Novel Extension)** 또는 🪄 (Wand) 메뉴를 통해 찾을 수 있습니다.
