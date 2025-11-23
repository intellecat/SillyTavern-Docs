---
label: 拡張機能
icon: plug
expanded: true
order: 35
route: /extensions/
---

# 拡張機能

SillyTavernには、拡張機能パネルで有効または無効にできる多くの拡張機能が付属しています。拡張機能は新機能を追加し、既存機能の動作を変更し、AIがコンテンツを使用できるようにします。拡張機能パネルの「拡張機能とアセットをダウンロード」メニューからより多くの拡張機能をインストールできます。

## 拡張機能パネル

拡張機能パネルを開いたり閉じたりするには、トップバーで**<i class="fa-solid fa-cubes fa-fw"></i>拡張機能**を選択してください。

- **<i class="fa-solid fa-cubes"></i>拡張機能の管理**：拡張機能を有効化、無効化、更新します
- **拡張機能とアセットをダウンロード**：SillyTavernリポジトリから[詳細な拡張機能](#インストール可能な拡張機能)、キャラクター、サウンド、背景をインストールします
- **拡張機能更新の通知**：インストール済み拡張機能の更新が利用可能な場合に通知されるようにします
- **<i class="fa-solid fa-cloud-arrow-down"></i>拡張機能のインストール**：Gitリポジトリ URLから[サードパーティ拡張機能](#サードパーティ拡張機能)をインポートします

## 組み込み拡張機能

これらの拡張機能はSillyTavernに組み込まれており、インストールの必要はありません。拡張機能パネルで有効または無効にすることができます。

:::callout
**[チャット翻訳](Translation.md)**

チャットメッセージを別の言語に翻訳します
:::

:::callout
**[画像キャプション](captioning.md)**

画像からテキストを生成して、AIが会話で視覚的コンテンツを「見て」応答できるようにします
:::

:::callout
**[画像生成](Stable-Diffusion.md)**

ローカルまたはクラウドベースのStable Diffusion、FLUX、またはDALL-E APIを使用して画像を生成します
:::

:::callout
**[表情画像](Expression-Images.md)**

チャットウィンドウの横または後ろに表示されるAIキャラクター（スプライト）の画像
:::

:::callout
**[要約](Summarize.md)**

チャット履歴の自動要約
:::

:::callout
**[チャット ベクトル化](Chat-vectorization.md)**

チャット履歴から関連するメッセージを探してコンテキストに追加します
:::

:::callout
**[テキスト音声合成](TTS.md)**

ElevenLabs、Silero、システムTTS、**[AllTalk](AllTalk.md)**、**[XTTS](XTTS.md)**などを使用したチャットメッセージの音声ナレーション
:::

:::callout
**[クイック返信](/For_Contributors/st-script.md#quick-replies-script-library-and-auto-execution)**

1回のクリックでチャットメッセージに返信し、コマンドとSTスクリプトを実行します
:::

:::callout
**トークンカウンター**

テキストをトークンに変換し、トークン数をカウントします
:::

---

## インストール可能な拡張機能

!!!tip
拡張機能をダウンロードするには、gitがインストールされている必要があります。インストールされていない場合は、[Git インストールページ](https://git-scm.com/downloads)の指示に従ってください。
!!!

**<i class="fa-solid fa-cubes"></i>拡張機能** => **拡張機能とアセットをダウンロード**メニューに移動して**<i class="fa-solid fa-plug-circle-exclamation"></i>アセットリストを読み込み**ボタンをクリックすることで、アプリから直接利用可能なすべての拡張機能を参照できます。拡張機能をインストールするには、**<i class="fa-solid fa-download"></i>ダウンロード**ボタンをクリックします。拡張機能について詳しく知るには、その名前の横にある**<i class="fa-solid fa-arrow-up-right-from-square"></i>リンク**ボタンをクリックしてGitHubページを開きます。

!!!info 拡張機能はExtrasではありません
Extrasプロジェクトは2024年4月に中止されました。拡張機能を使用するためにExtrasをインストールする必要はありません。
!!!

:::callout
**[Blip](Blip.md)**

キャラクターメッセージのテキストを可変速度でアニメーション化し、アニメーション中に音を再生します。
:::

:::callout
**[動的オーディオ](Dynamic-Audio.md)**

チャットに没入感のある背景音楽と環境音を追加します。
:::

:::callout
**[EmulatorJS](EmulatorJS.md)**

SillyTavernチャット内でレトロコンソールゲームを直接再生します。
:::

:::callout
**[Live2d](Live2d.md)**

Live2dモデルのサポートを追加します。カスタマイズ可能な表情、アニメーション、相互作用。
:::

:::callout
**[目標](Objective.md)**

チャット中にAIが目指す目標を設定します。
:::

:::callout
**[RVC](RVC.md)**

テキスト音声合成モジュールにリアルタイム音声クローン機能を追加します。
:::

:::callout
**[音声認識](Speech-Recognition.md)**

ブラウザまたはextrasを使用して音声をテキストに変換します。
:::

:::callout
**[VRM](VRM.md)**

VRMモデルのサポートを追加します。カスタマイズ可能な表情、アニメーション、相互作用。
:::

:::callout
**[ウェブサーチ](WebSearch.md)**

Webサーチ結果をLLMプロンプトに追加します。
:::

:::callout
**[AccuWeather](https://github.com/SillyTavern/Extension-AccuWeather)**

AccuWeatherAPIを使用して、スラッシュコマンドまたは関数ツールとして天気情報を提供します。
:::

:::callout
**[チャットトップバー](https://github.com/SillyTavern/Extension-TopInfoBar)**

チャットウィンドウにトップバーを追加し、クイックアクションへのショートカットを表示します。
:::

:::callout
**[チェス](https://github.com/SillyTavern/SillyTavern-Chess)**

LLMとチェスゲームをプレイします。
:::

:::callout
**[コード実行](https://github.com/SillyTavern/Extension-CodeRunner)**

チャット内のコードブロックからJavaScriptとSTスクリプトコードを実行できます。
:::

:::callout
**[D&Dダイス](https://github.com/SillyTavern/Extension-Dice)**

すべてのサイコロ振りニーズのための7つのクラシックD&Dダイス。
:::

:::callout
**[重複検索](https://github.com/SillyTavern/Extension-DupeFinder)**

キャラクターを類似グループでクラスタ化して、重複を簡単に見つける機能を追加します。
:::

:::callout
**[絵文字ピッカー](https://github.com/SillyTavern/Extension-EmojiPicker)**

チャットメッセージに絵文字をすばやく挿入するボタンを追加します。
:::

:::callout
**[グループ挨拶](https://github.com/SillyTavern/Extension-GroupGreetings)**

グループチャットに固有の代替挨拶の設定を許可します。
:::

:::callout
**[グループSendAs](https://github.com/SillyTavern/SillyTavern-GroupSendAs)**

選択したグループメンバーの/sendas コマンドテンプレートを素早く挿入するボタンを追加します。
:::

:::callout
**[HypeBot](https://github.com/SillyTavern/Extension-HypeBot)**

NovelAIのHypeBotエンジンを使用して、最近のチャットに基づいてパーソナライズされたお勧めを表示します。アクティブなNovelAI購読が必要です。
:::

:::callout
**[アイドル](https://github.com/SillyTavern/Extension-Idle)**

ユーザーがアイドル状態になってからしばらく経った後に「アイドルプロンプト」を追加して、会話を自然に続行します。
:::

:::callout
**[画像メタデータビューアー](https://github.com/SillyTavern/Extension-ImageMetadataViewer)**

チャットに添付された拡大画像のメタデータを表示します。
:::

:::callout
**[LaTeX](https://github.com/SillyTavern/Extension-LaTeX)**

チャットメッセージでLaTeXおよびAsciiMath公式をレンダリングします。
:::

:::callout
**[Mermaid](https://github.com/SillyTavern/Extension-Mermaid)**

SillyTavernチャットにMermaidダイアグラムとフローチャートレンダリングを追加します。
:::

:::callout
**[ノートブック](https://github.com/SillyTavern/Extension-Notebook)**

メモを保存する場所を追加します。リッチテキスト書式をサポートします。
:::

:::callout
**[パラメータランダマイザ](https://github.com/SillyTavern/Extension-Randomizer)**

すべての生成のたびにAPIセッティングスライダーをランダム化する機能を追加します。
:::

:::callout
**[Prome Visual Novel拡張](https://github.com/Bronya-Rand/Prome-VN-Extension)**

現在のビジュアルノベル体験をさらに多くの機能（フォーカスモード、レターボックスモードなど）で強化します！
:::

:::callout
**[プロンプトインスペクター](https://github.com/SillyTavern/Extension-PromptInspector)**

サーバーに送信する前に出力プロンプトを検査および編集するオプションを追加します。
:::

:::callout
**[プッシュ通知](https://github.com/SillyTavern/SillyTavern-PushNotifications)**

着信チャットメッセージのプッシュ通知を受け取ることができます。
:::

:::callout
**[クイックペルソナ](https://github.com/SillyTavern/Extension-QuickPersona)**

チャットバーからユーザーペルソナを選択するためのドロップダウンメニューを追加します。
:::

:::callout
**[RSS](https://github.com/SillyTavern/Extension-RSS)**

RSSフィードから最新ニュースをスラッシュコマンドまたは関数ツールとして取得します。
:::

:::callout
**[スクリーン共有](https://github.com/SillyTavern/Extension-ScreenShare)**

メッセージを送信するときにマルチモーダルモデルのスクリーン画像を提供します。
:::

:::callout
**[サイレンスプレーヤー](https://github.com/SillyTavern/Extension-Silence)**

拡張機能メニューにサイレンスオーディオプレーヤーを追加します。ブラウザタブがバックグラウンドで終了されるのを防ぐのに役立つかもしれません。
:::

:::callout
**[タイムライン](https://github.com/SillyTavern/SillyTavern-Timelines)**

チャット履歴へのタイムラインナビゲーションを追加します。
:::

:::callout
**[変数ビューアー](https://github.com/LenAnderson/SillyTavern-Variable-Viewer)**

変数を表示および変更する簡単な方法。
:::

:::callout
**[WebLLM](https://github.com/SillyTavern/Extension-WebLLM)**

拡張機能がブラウザで直接言語モデルを使用するためのインターフェースを提供します。
:::

## サードパーティ拡張機能

!!!danger
サードパーティ拡張機能を使用すると、予期しない副作用が生じる可能性があり、セキュリティリスクが生じる可能性があります。
拡張機能を**<i class="fa-solid fa-cloud-arrow-down"></i>拡張機能をインストール**経由でインポートする前に、常にソースを信頼していることを確認してください。
サードパーティ拡張機能によって引き起こされた損害については、弊社は責任を負いません。
!!!

サードパーティ拡張機能をインストールするには、**<i class="fa-solid fa-cubes"></i>拡張機能** => **<i class="fa-solid fa-cloud-arrow-down"></i>拡張機能のインストール**メニューに移動し、拡張機能リポジトリのURLを貼り付けてください。必要に応じて、ブランチと（[マルチユーザー](../Administration/multi-user.md)シナリオでは）インストール対象（すべてのユーザーまたは現在のユーザーのみ）を指定します。拡張機能は自動的にダウンロードされロードされます。
