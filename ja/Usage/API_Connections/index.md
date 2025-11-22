---
order: 150
icon: repo-forked
expanded: false
route: /usage/api-connections/
---

# API Connections

SillyTavernは幅広いLLM APIに接続できます。
以下は、それぞれの長所、短所、使用例の説明です。

## ELI5: Chat Completions vs Text Completions

STで初めて「API Connections」ページに移動すると、「Chat Completion」や「Text Completion」などの用語を使用してオプションを選択するドロップダウンオプションがあります。これが何を意味するかを理解すると役立ちます。

そうでないもの: 「Text Completion」をローカルモデル、「Chat Completion」をクラウドベースのLLMと考えがちですが、そうではありません。STのAPIドロップダウンの別個のオプションになっていても、例えば「Novel AI」や「Kobold」が実際には完全に別のタイプのモデルであるわけでもありません。適切なバックエンドを使用してモデルを異なるAPI構造に強制できますが、それはこのセクションの目的ではありません。

STを使用してメッセージを送信すると、チャット、キャラクターの説明、およびlorebooksやauthor's notesなどの他のプロンプトが単一の「プロンプト」に構築されてモデルに送信されます。使用しているモデルのAPI「タイプ」は、このプロンプトがどのように正確に構築されるかを決定します(STがバックグラウンドで自動的に処理します - STターミナルを開いて、AIに送信されるプロンプトがどのように見えるかを正確に確認できます)。

### Chat Completions

Chat Completionモデルは、その名前が示すように、プロンプトをUser(あなた)とAssistant(AI)またはSystem(中立)の間の一連のメッセージに構造化します。Chat Completionのためにトレーニングされたモデルは、AIが最後のメッセージに「応答」する「チャット」の感覚を作り出すのに役立ちます。ChatGPT Webサイトを使用しているとき、バックグラウンドでChat Completions APIを扱っています。

### Text Completions (a.k.a just "Completions")

一方、Text Completionは、その名前が示すように、プロンプトを1つの長い文字列に変換し、モデルは単純にこれを続けようとします(文字通り、すべてのテキスト、数百のメッセージ、すべてのフォーマット、改行などが1つの非常に長い文に圧縮されることを想像してください)。

STでのメッセージがYourPersona:とCharacter:の間の一連のメッセージとしてフォーマットされている場合、Text Completionモデルはこのパターンを続けようとし、STはそれを新しいチャットメッセージとしてレンダリングしますが、実際にはモデルは単にテキストを続けようとしているだけです。「The Sun rises in the」という入力を提供した場合、text completionモデルはおそらく「East」でそのメッセージを完成させます。

ほとんどのText Completionモデルには、Chat Completionモデルと同じようにメッセージと指示に「応答」するのに役立つ推奨される「Instruct Template」があります(通常、モデルのドキュメントまたはダウンロードページに記載されています)。STには通常、「Advanced Formatting」ページで選択できるほとんど(すべてではないにしても)のInstruct Templatesがあります。

## Local APIs

- これらのLLM APIはPCで実行できます。
- 使用は無料で、コンテンツフィルターがありません。
- インストールプロセスは複雑になる可能性があります(**SillyTavern開発チームはこれに対するサポートを提供していません**)。
- [HuggingFace](https://huggingface.co/models?other=LLM)からLLMモデルを個別にダウンロードする必要があり、それぞれ5-50GBになる可能性があります。
- ほとんどのモデルはクラウドLLM APIほど強力ではありません。

### KoboldCpp

- CPUオフロードを備えた使いやすいAPI(低VRAM ユーザーに役立つ)とストリーミング
- Windows、Mac、Linuxで単一のバイナリファイルから実行
- GGUFモデルをサポート
- AutoGPTQやExllama/v2などのGPU専用ローダーよりも遅い
- [GitHub](https://github.com/LostRuins/koboldcpp)、[Setup Instructions](/Usage/API_Connections/koboldcpp.md)

### llama.cpp

- KoboldCppとOllamaがフォークされた元のソース
- プリコンパイルされたバイナリとソースからコンパイルするオプションを提供
- GGUFモデルをサポート
- llama-server用の軽量CLIインターフェース
- [GitHub](https://github.com/ggml-org/llama.cpp)

### Ollama

- すべてのllama.cppベースのAPIの中で最もセットアップと使用が簡単
- ワンクリックダウンロード可能なモデルの優れた[カタログ](https://ollama.com/library)
- Ollama独自のフォーマットでラップされたGGUFモデルをサポート
- [GitHub](https://github.com/ollama/ollama)、[Website](https://ollama.com/)

### Oobabooga TextGeneration WebUI

- ストリーミングを備えたオールインワンGradio UI
- 量子化(AWQ、Exl2、GGML、GGUF、GPTQ)およびFP16モデルの最も広範なサポート
- ワンクリックインストーラーが利用可能
- 定期的な更新により、SillyTavernとの互換性が損なわれることがある
- [GitHub](https://github.com/oobabooga/text-generation-webui#one-click-installers)

**SillyTavernをOobaの新しいOpenAI APIに接続する正しい方法:**

1. Oobabooga's TextGenの最新アップデートを使用していることを確認してください(2023年11月14日時点)。
2. CMD_FLAGS.txtファイルを編集し、そこに`--api`フラグを追加します。次にOobaのサーバーを再起動します。
3. STを`http://localhost:5000/`(デフォルト)に接続し、「Legacy API」ボックスをチェックしないでください。Oobaのコンソールが提供するURLから`/v1`接尾辞を削除できます。

*`--api-port 5001`フラグを使用してAPIホスティングポートを変更できます。ここで5001はカスタムポートです。*

### TabbyAPI

- ストリーミングを備えた軽量の[Exllamav2](https://github.com/turboderp/exllamav2)ベースのAPI
- Exl2、GPTQ、およびFP16モデルをサポート
- [公式エクステンション](https://github.com/theroyallab/ST-tabbyAPI-loader)により、SillyTavernから直接モデルのロード/アンロードが可能
- 低VRAMユーザーには推奨されません(CPUオフロードなし)
- [GitHub](https://github.com/theroyallab/tabbyAPI)、[Setup Instructions](/Usage/API_Connections/tabbyapi.md)

### KoboldAI Classic (deprecated, abandoned)

- PCで実行、100%プライベート、幅広いモデルが利用可能
- AIの生成設定を最も直接的に制御
- GPU内に大量のVRAMが必要(LLMモデルに応じて6-24GB)
- モデルは2kコンテキストに制限
- ストリーミングなし
- 人気のあるKoboldAIバージョン:
  - [Henky's United](https://github.com/henk717/KoboldAI)
  - [0cc4m's 4bit-supporting United](https://github.com/0cc4m/KoboldAI)

## Cloud LLM APIs

- これらのLLM APIはクラウドサービスとして実行され、PC上のリソースを必要としません
- ほとんどのローカルLLMよりも強力/スマート
- ただし、すべてさまざまな程度のコンテンツフィルタリングがあり、ほとんどは支払いが必要です

### AI Horde

- SillyTavernは追加設定なしでこのAPIにそのままアクセスできます
- 個々のボランティア(Horde Workers)のGPUを使用してチャット入力の応答を処理
- 生成待ち時間、AI設定、利用可能なモデルに関してWorkerの裁量に委ねられる
- [Website](https://aihorde.net/)、[Setup Instructions](./horde.md)

### OpenAI (ChatGPT)

- セットアップとAPIキーの取得が簡単
- クレジットの前払いが必要で、プロンプトごとに課金
- 非常に論理的。創造的なスタイルは反復的で予測可能な場合がある
- 新しいモデルのほとんど(gpt-4-turbo、gpt-4o)はマルチモダリティをサポート
- [Website](https://platform.openai.com/)、[Setup Instructions](/Usage/API_Connections/openai.md#openai)

### Claude (by Anthropic)

- AIチャットに創造的でユニークなライティングスタイルを求めるユーザーに推奨
- クレジットの前払いが必要で、プロンプトごとに課金
- 最新のモデル(Claude 3)はマルチモダリティをサポート
- 特定のプロンプトスタイルと返信操作のための[prefills](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/prefill-claudes-response)の利用が必要
- [Website](https://console.anthropic.com/)、[Setup Instructions](/Usage/API_Connections/openai.md#claude)

### Google AI Studio and Vertex AI

- レート制限付きの無料ティアがあり(Gemini Flash)、請求情報が必要な場合があります
- [AI Studio](https://aistudio.google.com/)は通常、最新のモデルと機能を持っています
- [Vertex AI](https://console.cloud.google.com/vertex-ai/studio)はセットアップが難しいですが、より安定しています
- [Setup Instructions](/Usage/API_Connections/google.md)

### Mistral (by Mistral AI)

- さまざまなサイズと使用例の効率的なモデル。[プラットフォーム](https://console.mistral.ai/api-keys/)でアカウントとAPIキーを作成できます。
- 一般使用の場合は32kから128kのコンテキストサイズ、コーディングの場合は32kから256kのコンテキストサイズ。
- レート制限付きの無料ティア。
- 合理的なモデレーション、Mistralの主な原則は中立でユーザーをエンパワーすること、詳細は[こちら](https://mistral.ai/terms/)。
- [Website](https://console.mistral.ai/)、[Setup Instructions](/Usage/API_Connections/openai.md#mistral-ai)

### OpenRouter

- 市場のすべての主要LLMにアクセスするための統一APIを提供
- トークンごとの課金クレジットシステム、および1日あたりのリクエスト数が制限された無料モデル
- LLMベンダーが要求しない限り、モデレーションは強制されません
- [Website](https://openrouter.ai)、[Setup Instructions](/Usage/API_Connections/OpenRouter.md)

### DeepSeek

- 非常に人気のあるDeepSeek V3(`deepseek-chat`)とDeepSeek R1(`deepseek-reasoner`)モデルの最新バージョンへのアクセスを提供
- クレジットの支払いが必要($2最小)ですが、モデルは品質に対してかなり安価
- APIにモデレーションはありませんが、モデルは特定のプロンプトを拒否する場合があります
- [Website](https://platform.deepseek.com/)、[Setup Instructions](/Usage/API_Connections/openai.md#deepseek)

### AI21

- Jambaファミリーのオープンモデルへのアクセスを提供
- 無料トライアルがあり(3か月で$10)、その後はトークンごとに月額支払いが必要
- [Website](https://ai21.com/)、[Setup Instructions](/Usage/API_Connections/openai.md#ai21)

### Cohere

- Cohereの最新モデル(command-r、command-a、c4ai-ayaなど)へのアクセスを提供
- カジュアル使用に十分なレート制限を持つ無料ティア(Trial Keys)があります
- [Website](https://cohere.com/)、[Setup Instructions](/Usage/API_Connections/openai.md#cohere)

### Perplexity

- APIを介してユニークなPerplexity Sonarオンライン対応モデルへのアクセスを提供
- 請求の設定とクレジットの購入が必要
- [Website](https://perplexity.ai/)、[Setup Instructions](/Usage/API_Connections/openai.md#perplexity)

### Mancer AI

- さまざまなファミリーの制約のないモデルをホストするサービス
- さまざまなモデルのトークンに対して「クレジット」を使用して支払い
- デフォルトではプロンプトをログに記録しませんが、有効にしてトークンのクレジット割引を受けることができます。
- `Oobabooga TextGeneration WebUI`に似たAPIを使用、詳細は[Mancer docs](https://mancer.tech/docs/clients/#sampling-parameters)を参照。
- [Website](https://mancer.tech/)、[Setup Instructions](/Usage/API_Connections/mancer.md)

### DreamGen

- ステアラブルな創造的なライティングのために調整された検閲なしのモデル
- 無料の月次クレジット、および有料サブスクリプション
- 7Bから70Bまでのモデル
- [Setup Instructions](DreamGen.md)

### Pollinations

- セットアップ不要、そのまま使用可能
- 幅広いモデルへの無料アクセスを提供
- 出力には時折、サードパーティサービスへのリンクを含む広告が含まれる場合があります

### NovelAI

- コンテンツフィルターなし、最新モデルはLlama 3ベース
- 有料サブスクリプションが必要、ティアによって最大コンテキスト長が決定されます
- [Website](https://novelai.net/)、[Setup Instructions](/Usage/API_Connections/novelai.md)

### Electron Hub

- 1つのAPIキーで複数のベンダー(OpenAI、Anthropic、DeepSeekなど)のテキストおよび画像生成モデルへのアクセスを解除
- 毎日$0.25の無料クレジット、有料プランあり
- [Website](https://www.electronhub.ai/)、[Setup Instructions](/Usage/API_Connections/openai.md#electron-hub)

### AI/ML API

- Claude、GPT-4o、Gemini、LLaMA 3、Mistralなどを含む300以上のモデルの統一API
- レート制限付きの無料ティア、サブスクリプションプラン、および従量課金制オプションがあります
- [Website](https://aimlapi.com)、[Docs](https://docs.aimlapi.com)、[Models](https://aimlapi.com/models)
