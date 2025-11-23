---
order: 20
route: /usage/api-connections/openai/
---

# Chat Completions

## ソース固有の手順

!!!warning **重要！**
ほとんどのAPIプラットフォームでは、生成されたAPIキーは作成時に一度だけ表示できます。紛失した場合は、新しいキーを生成する必要があります。安全に保管してください！
!!!

### OpenAI

OpenAIの開発者プラットフォームを使用して、gpt-4o、gpt-4.1、o3などのさまざまなOpenAIモデルにアクセスします。

**APIキーの取得方法:**

1. [OpenAI](https://platform.openai.com/)に移動してサインインします。
2. 「[View API keys](https://platform.openai.com/account/api-keys)」オプションを使用して新しいAPIキーを作成します。

### Claude

ClaudeはAnthropicによって開発されたAIモデルのファミリーです。Anthropicコンソールを通じてClaudeモデルにアクセスできます。

**APIキーの取得方法:**

1. [Anthropic Console](https://console.anthropic.com/)に移動してサインインします。
2. 「[Get API Key](https://console.anthropic.com/settings/keys)」セクションを使用して新しいAPIキーを作成します。

### Mistral AI

Mistral AIは、高い科学的基準とオープンさに焦点を当てて、オープンモデルとプロプライエタリモデルの両方を開発しているチームです。モデルをローカルで実行するか、APIサービスのLa Plateformeを通じて実行できます。

**APIキーの取得方法:**

1. 最初のステップは、[La Plateforme](https://console.mistral.ai/)でアカウントを作成することです。
2. それが完了したら、[plan](https://console.mistral.ai/billing/plans)を選択して支払い情報を設定するか、Free Tierを選択できます。
3. 次に、[API key](https://console.mistral.ai/api-keys/)を作成できます。キーが有効になるまで数分待つ必要がある場合があります！

### DeepSeek

DeepSeek Platformは、API経由で最新のDeepSeekモデルへのアクセスを提供します。DeepSeek V3とDeepSeek R1を含むさまざまなモデルを提供しています。

**APIキーの取得方法:**

1. [DeepSeek Platform](https://platform.deepseek.com/)でサインアップします。
2. サインアップしてアカウントをトップアップした後、「[API keys](https://platform.deepseek.com/api_keys)」セクションでAPIキーを作成できます。

### AI21

AI21 Labsは、フラッグシップのJambaシリーズを含むさまざまなAIモデルを提供しています。AI21 Studio APIを通じてモデルにアクセスできます。

**APIキーの取得方法:**

1. [AI21 Studio](https://studio.ai21.com/)に移動してサインインします。
2. 「Settings => API Keys」セクションに移動して新しいAPIキーを作成します。

### Cohere

Cohereは、テキスト生成とembeddingsを含むさまざまなタスク用のAIモデルのスイートを提供しています。Cohere APIを通じてモデルにアクセスできます。

**APIキーの取得方法:**

1. [Cohere](https://cohere.com/)に移動してサインインします。
2. アカウント設定の「[API Keys](https://dashboard.cohere.com/api-keys)」セクションに移動して新しいAPIキーを作成します。

### Perplexity

Perplexity AIは、リアルタイム調査と情報検索のためにAPI経由でオンライン対応Sonarモデルへのアクセスを提供しています。

公式Getting Startedガイド: [Perplexity Quickstart](https://docs.perplexity.ai/getting-started/quickstart)

**APIキーの取得方法:**

1. [Perplexity](https://perplexity.ai/)に移動してサインインします。
2. API使用のためのクレジットを購入するために「[API billing](https://www.perplexity.ai/account/api/billing)」セクションに移動します。
3. 設定の「[API keys](https://www.perplexity.ai/account/api/keys)」セクションに移動して新しいAPIキーを作成します。

### Fireworks AI

Fireworks AIは、最先端のオープンソース言語モデルへの高速でコスト効率の良いアクセスを提供する高性能プラットフォームです。このプラットフォームは、OpenAI互換APIを備えたサーバーレスデプロイメントを提供し、最大256,000トークンのコンテキストウィンドウをサポートします。

**APIキーの取得方法:**

1. [Fireworks AI](https://fireworks.ai/)に移動してアカウントを作成するかサインインします。
2. アカウント設定の[API Keys page](https://app.fireworks.ai/settings/users/api-keys)に移動します。
3. 「Create API key」をクリックして、説明的な名前(例: 「SillyTavern」)を指定します。

## Electron Hub

Electron Hubは、単一のAPIキーを通じて複数のベンダーのモデルへのアクセスを提供する統一されたOpenAI互換プラットフォームです。

**APIキーの取得方法:**

1. [Electron Hub](https://playground.electronhub.ai/console)でアカウントを作成します。
2. **Console → API Keys**ページからAPIキーを生成します。

## Custom OpenAI-compatible endpoint

!!!warning
発生する可能性のある問題についてサポートを提供していないことに注意してください！
すべての可能なAPIエンドポイントとの互換性を保証するものではありません！
!!!

!!!
この機能を使用してTabbyAPI、Oobabooga、Aphrodite、またはそのような他のローカルエンドポイントを使用する場合は、代わりに[組み込みの互換性](/Usage/API_Connections/index.md)を確認してください。カスタムエンドポイント機能は主に、OpenAI互換API Chat Completionエンドポイントをエクスポーズする他のサービスやプログラムでの使用を目的としています。

ほとんどのText Completion APIは、OpenAIの標準が許可するよりもはるかに多くのカスタマイズオプションをサポートしています。Min-Pサンプラーなどのこれらのより大きなカスタマイズオプションは、SillyTavernユーザーがチェックする価値があるかもしれません。これにより、生成の品質が大幅に向上する可能性があります。
!!!

Chat Completionsバックエンド用の代替エンドポイントを設定できます。このカスタムエンドポイントは、一般的なOpenAI APIスキーマをサポートする任意のサーバーに接続できます。

互換性のあるバックエンドの例には以下が含まれます:

* [LM Studio](https://lmstudio.ai/)
* [LiteLLM](https://www.litellm.ai/)
* [LocalAI](https://localai.io/)

### 接続

この機能にアクセスするには:

1. 'Chat Completion' APIタイプに切り替えます
2. 'Chat Completion Source'で'Custom (OpenAI-compatible)'を選択します

カスタムエンドポイントURLと、必要に応じてAPIキーを入力します。例えば、TabbyAPIは認証にAPIキーを必要とします。

!!!tip
**ヒント:** 接続の問題が発生した場合は、エンドポイントURLの最後に`/v1`を追加してみてください。`/chat/completions`接尾辞は追加しないでください。
!!!

### モデルの選択

カスタムAPIが利用可能なモデルのリストを提供するために`/v1/models`エンドポイントを実装している場合、ドロップダウンリストから選択できます。それ以外の場合は、テキストフィールドを使用してモデルIDを手動で入力します。

'Bypass API status check'をチェックして、SillyTavernが機能していないAPIエンドポイントについて警告しないようにします。APIエンドポイントが正しく機能しているにもかかわらず、SillyTavernが警告を表示し続ける場合は、このオプションを有効にします。

「Test Message」をクリックして、シンプルなプロンプトをモデルに送信して接続を確認します。

## Prompt Post-Processing

!!!warning
**注意:** 「no tools」を使用したPost-Processingオプションを使用すると、Tool Callingはサポートされません！
!!!

一部のエンドポイントは、1つのsystem messageのみを要求する、または厳密に交互の役割を要求するなど、受信プロンプトのフォーマットに特定の制限を課す場合があります。

SillyTavernは、これらの要件を満たすのに役立つ組み込みのプロンプトコンバーターを提供しています(最も制限が少ないものから最も制限が多いものまで):

1. None - APIによって厳密に要求されない限り、明示的な処理は適用されません
2. Merge consecutive messages from the same role
3. Semi-strict - ロールをマージし、オプションの1つのsystem messageのみを許可
4. Strict - ロールをマージし、オプションの1つのsystem messageのみを許可し、user messageを最初に要求
5. Single user message - すべてのロールからのすべてのメッセージを単一のuser messageにマージ

Merge、semi-strict、strictは、「with tools」バリアントが選択されていない限り、プロンプトからtool callsを追加で削除します。これは、tool callingをサポートしていないAPIで、既存のプロンプトにtool callsが含まれている場合に便利です。

より制限の少ないオプションは、「Custom OpenAI-compatible」以外のSillyTavernで実装されているより制限的なエンドポイントに影響を与えません。Customは無効なリクエストでエラーになる可能性があります。

strict modeで、最初のassistant messageの前にuser messageが存在しない場合、`config.yaml`の`promptPlaceholder`が挿入されます。デフォルトでは「\[Start a new chat]」です。
