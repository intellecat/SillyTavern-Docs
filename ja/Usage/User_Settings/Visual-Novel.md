---
order: 10
tags:
    [
        visual novel,
        vn,
    ]
route: /usage/user-settings/visual-novel/
---

# ビジュアルノベル (VN) モード (Visual Novel (VN) Mode)

Visual Novel Modeは、SillyTavernの特別なスクリーン構成であり、Doki Doki Literature Club、The Fruits of Grisaia、Fate: Stay/nightなどの有名なVNゲームのようなVNのようなスプライト（またはキャラクターカード画像）を持つキャラクターとチャットすることができます。

## Visual Novel Modeを切り替える (Toggling Visual Novel Mode)

### Visual Novel Modeを有効にする (Enabling Visual Novel Mode)

Visual Novel ModeはSillyTavernに組み込まれており、*User Settings*（User Settings Icon）に移動し、*No Text Shadows*の下に**Visual Novel Mode**をチェックして切り替えることができます。

![User Settings](/static/vn/vn-mode-toggle.png)

### Visual Novel Modeを無効にする (Disabling Visual Novel Mode)

Visual Novel Modeの無効化は有効化と同じステップです。Visual Novel Modeをトグル解除して、通常のチャット画面に戻ります。

!!!warning VN Mode with VN Extensionsに関して
一部の拡張機能（Prome VN Extensionなど）は、独自のVNモードを使用する場合、「Visual Novel Mode」をオンに切り替えられます。*User Settings*メニューからVN Modeを有効/無効にすると、これらの拡張機能にも影響します。
!!!

## ビジュアルノベルUI (The Visual Novel UI)

![VN Display](/static/vn/vn-display.png)

Visual Novel Modeでは、UIは、中心に表示されるキャラクタースプライト（またはキャラクターカード画像）に対応するためにわずかに変更されます。複数のキャラクターとのグループチャットでは、キャラクタースプライトは下記のように互いに対応するために拡がります。

![Group VN Display](/static/vn/group-vn-display.png)

### MovingUI付きVNモード (VN Mode with MovingUI)

!!!info
MovingUIを切り替えるには、*User Settings*に移動して**MovingUI**をチェックしてください。この機能は**デスクトップのみで**機能することに注意してください。
!!!

**MovingUI**が*User Settings*で有効な場合、スプライト（またはキャラクターカード画像）は、スクリーン上で移動したい場合は、周りを移動させることができます。

!!!warning スプライトサイズに関して
キャラクタースプライトのサイズが比較的大きい場合、MovingUIでスプライトを周りに移動させるのは課題になります。スプライトの周りをドラッグするボタンが既存のスプライトの下に覆われている可能性があります。より良い配置のため、特に特に画面上により多くのキャラクターがある場合は、通常よりもそれらをもっと移動させるかもしれません。
!!!

![Group VN Display (MovingUI)](/static/vn/vn-group-display-movingui.png)

## キャラクタースプライトの取得 (Acquiring Character Sprites)

キャラクタースプライトを取得するには、Visual NovelまたはDDLC CounterSideなどのVisual Novel機能を使用するゲームなどの既存のキャラクターからスプライトを取得するためにインターネットを閲覧することで行うことができます。希望するキャラクターはスプライトで既にスプライトを持っていない場合、残っているいくつかのオプションがあります。

1. キャラクターポストでスプライトZIPパッケージまたはスプライトパックへのリンクを検索してください。
    !!!info
    一部のボットクリエイターは、スプライトパック（同じポスト内または専用のスプライトチャネル内）でボットをリリースする場合があります。既にスプライトを持つキャラクターに対して誰かがスプライトを作成していないか、これらのポストを検索してください。
    !!!
2. LoRAsと安定した拡散を使用して独自に作成してください。
    !!!warning
    スプライトを最初から生成することは時間がかかり（特にあなたが考えるキャラクターの場合LoRAsが存在しない場合や、使用したい安定した拡散モデルの場合）、スプライト表現が6以上の場合、より多くのハードウェアが必要です。もしあなたが28スプライト式を作っているなら、SDXLを使用している場合や、スプライトをより高い解像度にアップスケーリングしている場合。
    !!!
3. キャラクターカード画像を使用してください。スプライトのようではないかもしれませんが、少なくとも画面上で見られるものを持っています。ただし、複数のキャラクターカードはVN modeで使用できません。
    !!! Prome Visual Novel Extensionを持つキャラクターカード画像
    Prome Visual Novel Extension 1.0.6+では、スプライトと非スプライトの両方のキャラクターを持つグループチャットを持つ「Emulate Character Card as Sprite」という機能があります。キャラクターカードをチャットのスプライトとして使用します。

    ![Character Card Group Chat](/static/vn/extensions/prome/card-emulation.png)
    !!!

## VN拡張機能 (VN Extensions)

### Prome Visual Novel Extension

Prome Visual Novel Extensionは、Bronya RandおよびPrometheusからの推奨されるサードパーティ拡張機能で、Letterbox Modeなどの機能でSillyTavernの視覚的な小説体験をさらに強化します。これにより、視覚的な小説UIは「映画のような」で、集中モードは従来のVN Modeの暗いキャラクタースプライトと多くの計画が含まれています！

|                              Letterbox Mode                              |                          Traditional VN Mode                           |
|:------------------------------------------------------------------------:|:----------------------------------------------------------------------:|
| ![Horizontal Letterbox Mode](/static/vn/extensions/prome/horizontal.png) | ![Traditional VN Mode](/static/vn/extensions/prome/single-message.png) |

|                 Hide Sheld (Message Box)                  |                      Focus Mode (w/ Darken Sprites)                      |
|:---------------------------------------------------------:|:------------------------------------------------------------------------:|
| ![Sheld Hide](/static/vn/extensions/prome/sheld_hide.png) | ![Focus Mode w/ Darken Sprites](/static/vn/extensions/prome/defocus.png) |

Prome Visual Novel Extensionをインストールするには、`Download Extensions & Assets`に移動して*Prome Visual Novel Extension*を見つけるか、[Prome Visual Novel Extension](https://github.com/Bronya-Rand/Prome-VN-Extension?tab=readme-ov-file#installation-and-usage) Github ページのインストール手順に従うことができます。Promeの設定を調整するには、*Extensions* -> **Prome (Visual Novel Extension)**で見つけるか、🪄（Wand）メニューを使用できます。
