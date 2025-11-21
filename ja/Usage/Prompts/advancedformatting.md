---
order: 100
route: /usage/core-concepts/advancedformatting/
---

# Advanced Formatting

このセクションで提供される設定により、[プロンプト構築](index.md) 戦略をより制御できます（主に Text Completion APIs の場合）。

このパネルのほとんどの設定は Chat Completions APIs には適用されません。これらは Prompt Manager システムで管理されます。

+++ Text Completion APIs
* [System Prompt](#system-prompt)
* [Context Template](#context-template)
* [Tokenizer](#tokenizer)
* [Custom Stopping Strings](#custom-stopping-strings)
+++ Chat Completion APIs
* System Prompt: 適用されません。[Prompt Manager](prompt-manager.md) を使用してください
* Context Template: 適用されません。[Prompt Manager](prompt-manager.md) を使用してください
* [Tokenizer](#tokenizer)
* [Custom Stopping Strings](#custom-stopping-strings)
+++

## Resetting Templates

デフォルトのテンプレートを元の状態に復元できます。これは UI を通じて、または関連するデータ ファイルを手動で削除することで実行できます。

### UI Reset

1. **<i class="fa-solid fa-font"></i> Advanced Formatting** メニューを開きます。
2. リセットするテンプレートを選択します。
3. **<i class="fa-solid fa-recycle"></i> Restore current template** ボタンをクリックします。
4. プロンプトされたときにアクションを確認します。

### Manual Reset

!!!
[config.yaml](/Administration/config-yaml.md#data-configuration) の `skipContentCheck` 設定が `false` に設定されていることを確認します。そうしないと、コンテンツ チェックがトリガーされません。
!!!

1. ユーザー データ ディレクトリに移動します（詳細については [Data paths](/Installation/index.md#data-paths) を参照）。
2. ユーザー データ ディレクトリのルートから `content.log` ファイルを削除します。このファイルは、ユーザー用にコピーされたデフォルト ファイルを追跡します。
3. テンプレート JSON ファイルを関連するサブディレクトリ（`context`、`instruct`、`sysprompt` など）から削除します。
4. SillyTavern サーバーを再起動します。アプリケーションはデフォルト コンテンツを再入力し、削除されたデフォルト テンプレートを復元します。

## Backend-defined templates

!!! Applies to: Text Completion APIs
Chat Completion APIs には適用されません。異なるプロンプト ビルダーを使用します。
!!!

一部の Text Completion ソースでは、モデル作成者が推奨するテンプレートを自動的に選択する機能を提供しています。これは、モデルの `tokenizer_config.json` ファイルで定義されているチャット テンプレートのハッシュを、デフォルトの SillyTavern テンプレートのいずれかと比較することで機能します。

1. **<i class="fa-solid fa-font"></i> Advanced Formatting** メニューで **<i class="fa-solid fa-bolt"></i> Derive templates** オプションを有効にする必要があります。これは Context、Instruct、またはその両方に適用できます。
2. サポートされているバックエンドを Text Completion ソースとして選択する必要があります。現在、llama.cpp と KoboldCpp のみがテンプレートの導出をサポートしています。
3. API への接続が確立されるとき、モデルはメタデータを正しく報告する必要があります。これが機能しなかった場合は、バックエンドを最新バージョンに更新してみてください。
4. 報告されたチャット テンプレート ハッシュは、[既知の SillyTavern テンプレート](https://github.com/SillyTavern/SillyTavern/blob/release/public/scripts/chat-templates.js) のいずれかと一致する必要があります。これにより、Llama 3、Gemma 2、Mistral V7 などのデフォルト テンプレートのみが対象となります。
5. ハッシュが一致する場合、テンプレートは、テンプレート リストに存在する場合（名前変更または削除されていない場合）、自動的に選択されます。

## System Prompt

!!! Applies to: Text Completion APIs
Chat Completion APIs で同等の設定については、[Prompt Manager](prompt-manager.md) を使用してください。**Main Prompt** は Chat Completion APIs のシステム プロンプトと同等です。
!!!

System Prompt はモデルが従うべき一般的な指示を定義します。これは会話のトーンとコンテキストを設定します。たとえば、モデルに AI アシスタント、執筆パートナー、または架空のキャラクターとして行動するよう指示します。

System Prompt は [Story String](context-template.md#story-string) の一部であり、通常、モデルが受け取るプロンプトの最初の部分です。

プロンプト ガイドの詳細については [prompting guide](index.md#main-prompt-system-prompt) を参照してください。

## Context Template

!!! Applies to: Text Completion APIs
Chat Completion APIs で同等の設定については、[Prompt Manager](prompt-manager.md) を使用してください。
!!!

通常、AI モデルでは、キャラクター データを特定の方法で提供する必要があります。SillyTavern には、異なるモデルのプリメードされた変換ルールのリストが含まれていますが、必要に応じてカスタマイズできます。

このセクションのオプションについては [Context Template](context-template.md) で説明しています。

## Tokenizer

Tokenizer は、テキストを「トークン」と呼ばれるより小さなユニットに分割するツールです。これらのトークンは個々の単語、または接頭辞、接尾辞、句読点などの単語の一部です。経験則として、1 つのトークンは通常、テキストの 3～4 文字に対応します。

このセクションのオプションについては [Tokenizer](tokenizer.md) で説明しています。

## Custom Stopping Strings

JSON でシリアル化された停止文字列の配列を受け入れます。例： `["\n", "\nUser:", "\nChar:"]`。形式がわからない場合は、[オンライン JSON バリデーター](https://jsonlint.com/) を使用してください。モデルの出力が停止文字列のいずれかで **終了** する場合、それらは出力から削除されます。

サポートされている API：

1. KoboldAI Classic (バージョン 1.2.2 以上) または KoboldCpp
2. AI Horde
3. Text Completion APIs: Text Generation WebUI (ooba)、Tabby、Aphrodite、Mancer、TogetherAI、Ollama など
4. NovelAI
5. OpenAI (最大 4 文字列) および互換 API
6. OpenRouter (Text と Chat Completion の両方)
7. Claude
8. Google AI Studio
9. MistralAI

## Start Reply With

!!! Note
デフォルトでは、Start Reply With プレフィックスは結果のメッセージに表示されません。「Show reply prefix in chat」を有効にして表示します。
!!!

### Text Completion APIs

プロンプトの最後の行にプリフィルし、モデルがそのポイントから続行するように強制します。これは、定義されたプレフィックスで [Model Reasoning](/Usage/Prompts/reasoning.md) に向かって推し進めるなどのコンテンツの実施に役立ちます：

```txt
<think>
Sure!
```

### Chat Completion APIs

プロンプトの最後にアシスタント ロール メッセージを追加します。一部のバックエンド モデルの場合、これはモデル応答のプリフィルに相当しますが、一部はまったくサポートされていない可能性があり、検証エラーで失敗します。ご不明な場合は、このフィールドを空のままにしてください。
