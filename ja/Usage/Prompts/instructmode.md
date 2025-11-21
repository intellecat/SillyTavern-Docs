---
order: 80
route: /usage/core-concepts/instructmode/
---

# Instruct Mode

Instruct Mode により、Alpaca、ChatML、Llama2 など、さまざまなプロンプト形式でトレーニングされた命令に従うモデルのプロンプトを調整できます。

!!! Applies to: Text Completion APIs
Chat Completion APIs で同等の設定については、[Prompt Manager](prompt-manager.md) を使用してください。
!!!

## API support

### Text Completion API

完全にサポートされています。これには以下が含まれます：

* Text Completion のすべてのソース
* KoboldAI Classic
* AI Horde

#### Choosing a formatting

選択した instruct テンプレートは、バックエンドで実行されている実際のモデルの期待に一致する必要があります。

これは通常、HuggingFace のモデル カードに反映され、一部は SillyTavern 互換の JSON ファイルも提供しています。

例： [NeverSleep/Noromaid-13b-v0.1.1](https://huggingface.co/NeverSleep/Noromaid-13b-v0.1.1#prompt-template-custom-format-or-alpaca)

### Chat Completion API (OpenAI、Claude など)

これはサポートされていません **（そして必要ありません）**。Chat Completion API。彼らは完全に異なるプロンプト ビルダーを使用します。

### NovelAI

Instruct Mode は NovelAI では *技術的に* サポートされていますが、それらのモデルは命令フォーマットを理解するために訓練されていません。NovelAI モデルは、中括弧で囲まれた指示がチャット メッセージで遭遇した場合に *自動的に* アクティブ化される特別な instruct モジュールを使用できるため、プロンプト全体に Instruct Mode を使用すると **低下した品質**の出力につながります。

NovelAI の instruct モジュール を自動アクティブ化する例を以下に示します：

```txt
User: { Write a happy song about Nintendo Switch. }
```

## Instruct Mode Settings

### System Prompt

!!!warning Recent change
システム プロンプトは現在別の エンティティです。詳細については [Advanced Formatting](advancedformatting.md#system-prompt) ページを参照してください。
!!!

### Templates

よく知られている instruct モデルのためのシーケンスとプリメードテンプレートを提供します。

*テンプレートを変更すると、保存されていない設定が最後に保存された状態にリセットされます！失いたくない変更をした場合は、テンプレートを保存することを忘れないでください。*

### Activation Regex

有効な正規表現として定義されている場合、モデルに接続されてその名前がこの正規表現に一致すると、このテンプレートが自動的に選択されます。

Instruct Mode は事前に有効にする必要があります。テンプレート間で最初に一致する正規表現のみが選択されます（アルファベット順で評価）。

### Wrap Sequences with Newline

各シーケンス テキストは、プロンプトに挿入されるときに改行文字でラップされます。Alpaca とその派生物に必要です。

行終了を完全に制御したい場合は、無効にします。

### Replace Macro in Sequences

有効な場合、既知の \{\{macro\}\} 代替がメッセージ ラップ シーケンスで定義されていれば置き換えられます。

また、特別な \{\{name\}\} マクロをメッセージ プレフィックスで使用して、実際にアクティブな \{\{char\}\} または \{\{user\}\} ではなく、メッセージに添付された実際の名前を参照できます。グループ チャットや /sendas コマンドを使用する場合に役立ちます。名前を決定できない場合は、「System」がフォールバック プレースホルダーとして使用されます。

### Include Names

有効にした場合、チャット履歴ログのプレフィックス シーケンスの後に文字とユーザー名を追加します。

次のオプションを使用できます：

* **Never**: メッセージ コンテンツの前に名前プレフィックスを追加しないでください。
* **Groups and Past Personas**: グループ文字と過去のペルソナからのメッセージにのみ名前プレフィックスを追加します。
* **Always**: メッセージ コンテンツの前に常に名前プレフィックスを追加します。

### Sequences: Story String Wrapping

!!!warning Recent change
システム プロンプト ラップが削除され、Story String ラップに置き換えられました。
!!!

位置が「デフォルト（コンテキストの上部）」に設定されている場合、Story String がどのようにラップされるかを定義します

#### Story String Prefix

Story String の前に挿入されます。

#### Story String Suffix

Story String の後に挿入されます。

### Sequences: Chat Messages Wrapping

これらの設定は、異なるロールに属するメッセージがプロンプト構築時にどのようにラップされるかを定義します。

すべてのプレフィックス シーケンスも自動的に停止文字列として使用されます。

#### User Message Prefix

ユーザー メッセージの前および詐称時の最後のプロンプト行として挿入されます。

#### User Message Suffix

ユーザー メッセージの後に挿入されます。

#### Assistant Message Prefix

アシスタント メッセージの前および AI 応答を生成するときの最後のプロンプト行として挿入されます。

#### Assistant Message Suffix

アシスタント メッセージの後に挿入されます

#### System Message Prefix

システム（スラッシュ コマンドまたは拡張機能によって追加された）メッセージの前に挿入されます。

#### System Message Suffix

システム メッセージの後に挿入されます。

#### System same as User

チェックされている場合は true に設定され、システム メッセージはユーザー ロール メッセージ シーケンスを使用します。

それ以外の場合、システム メッセージは独自のシーケンス（空でない場合）を使用するか、まったくラップを実行しません（空の場合）。

### Misc. Sequences

プロンプト構築をより細かく調整するためのさまざまな高度な設定

#### First Assistant Prefix

最初のアシスタント メッセージの前に挿入されます。

!!!info
チャット履歴の最初のメッセージのみがカウントされます。実際にはプロンプトに最初に入るメッセージではありません！
!!!

#### Last Assistant Prefix

最後のアシスタント メッセージの前または AI 応答を生成するときの最後のプロンプト行として挿入されます。

!!!info
バックグラウンドでテキストを生成するときには使用されません（例：Stable Diffusion プロンプトまたはサマリー）。System Instruction Prefix または Regular Assistant Prefix が代わりに使用されます。
!!!

#### System Instruction Prefix

バックグラウンド（例：Stable Diffusion プロンプトまたはサマリー）でニュートラル/システム テキストを生成するときの最後のプロンプト行として挿入されます。

#### User Filler Message

チャット履歴がユーザー メッセージで開始されない場合、チャット履歴の開始時に挿入されます。

**Use case:** instruct フォーマットが *厳密に* ユーザー最初で要求され、交互ロールのみでメッセージが付いている場合、例：Llama 2 Chat、Mistral Instruct。

#### Stop Sequence

返信の終了を示すテキスト。また、バックエンド API への停止文字列として送信されます。

Stop シーケンスが生成された場合、その後のすべてが出力から削除されます（シーケンス自体を含む）。
