---
order: 40
tags: ['>=1.12.12']
route: /usage/prompts/reasoning/
---

# Reasoning

言語モデルでは、推論（モデル思考とも呼ばれる）は、段階的な分析を通じて人間の問題解決を反映するchain-of-thought（CoT）技術を指しています。SillyTavernは、推論モデルの使用をサポートされたバックエンド全体でより効率的で一貫性のあるものにするいくつかの機能を提供します。

## 一般的な問題

1. 推論モデルを使用する場合、モデルの内部推論プロセスは、この推論が最終出力に表示されない場合でも（例えば、o3-miniまたはGemini Thinking）、応答トークンの割り当ての一部を消費します。応答が不完全またはemptyで返されていることに気付いた場合は、**<i class="fa-solid fa-sliders"></i> AI Response Configuration**パネルで見つかる Max Response Length設定を調整してみてください。推論モデルの場合、標準会話モデルと比較して、1024～4096トークンのどこかから、大幅に高いトークン制限を使用することが典型的です。

## 設定

!!!
ほとんどの推論関連の設定は、**<i class="fa-solid fa-font"></i> Advanced Formatting**パネルの「Reasoning」セクションで設定できます。
!!!

推論ブロックはチャットで折りたたみ可能なメッセージセクションとして表示されます。それらは、手動でバックエンドで自動的に、または応答解析を通じて追加することができます（以下を参照）。

デフォルトでは、推論ブロックは空間を節約するために折りたたまれます。ブロックをクリックしてその内容を表示してください。ブロックを自動的に展開するように設定できます。**Auto-Expand**を推論設定で有効化してください。

推論ブロックが展開されると、**<i class="fa-solid fa-copy"></i> Copy**および**<i class="fa-solid fa-pencil"></i> Edit**ボタンを使用してその内容をコピーまたは編集できます。

一部のモデルは推論をサポートしていますが、その考えを返信しません。**Show Hidden**設定を切り替えることで、推論ブロックを推論時間で表示するのは引き続き可能です。

## Reasoningを追加

### 手動で

メッセージの追加を通じて、任意のメッセージに推論ブロックを追加します。**<i class="fa-solid fa-pencil"></i> Message Edit**メニュー。編集中に**<i class="fa-solid fa-lightbulb"></i>**をクリックして、推論セクションを追加します。サードパーティの拡張機能は、チャットに追加する前に、メッセージオブジェクトの`extra.reasoning`フィールドに書き込むことにより、推論を追加することもできます。

### コマンド使用

`/reasoning-set` STscriptコマンドを使用して、メッセージへの推論を追加します。このコマンドは`at`（メッセージID、デフォルトは最後のメッセージ）および推論テキストをパラメーターとして使用します。

```stscript
/reasoning-set at=0 This is the reasoning for the first message.
```

### Backend

選択されたLLMバックエンドとモデルが推論出力をサポートしている場合、**<i class="fa-solid fa-sliders"></i> AI Response Configuration**パネルで「Request model reasoning」を有効化すると、モデルの思考プロセスを含む推論ブロックが追加されます。

サポートされているソース：

- Claude
- DeepSeek
- Google AI Studio
- Google Vertex AI
- OpenRouter
- xAI (Grok)
- AI/ML API

「Request model reasoning」は、モデルが推論するかどうかは判断されません。ClaudeおよびGoogle（2.5 Flash）は思考モードの切り替えを許可します；[Reasoning Effort](#reasoning-effort)を参照してください。

### パース で

**<i class="fa-solid fa-font"></i> Advanced Formatting**パネルで「Auto-Parse」を有効化して、モデルの出力から推論を自動的に解析します。

応答には、設定されたプリフィックスおよびサフィックスシーケンスにラップされた推論セクションを含める必要があります。デフォルトで提供されるシーケンスは、DeepSeek R1推論形式に対応しています。

プリフィックス`<think>`とサフィックス`</think>`の例：

```txt
<think>
This is the reasoning.
</think>

This is the main content.
```

## 推論でのプロンプト

デフォルトでは、認識された推論ブロックの内容はモデルに返送されません。推論をプロンプトに含めるには、**<i class="fa-solid fa-font"></i> Advanced Formatting**パネルで「Add to Prompts」を有効化します。推論のコンテンツは、設定されたプリフィックスおよびサフィックスシーケンスにラップされ、主なコンテキストから分離器で分離されます。Max Additions数値設定は、プロンプトの終わりからカウントして、含めることができる推論ブロックの数を制御します。

!!!
ほとんどのモデルプロバイダーは、マルチターン会話でCoTをモデルに返送することをお勧めしません。
!!!

### 推論から続行

推論を「Add to Prompts」切り替えなくモデルに返送できる特別な場合は、生成が継続される場合（例えば、**<i class="fa-solid fa-bars"></i> Options**メニューから「Continue」を押して）ただし、継続されるメッセージが実際のコンテンツなしで推論のみを含む場合です。これにより、モデルは不完全な推論を終了し、メインコンテンツを生成し始める機会が与えられます。プロンプトは以下の様に送信されます：

```txt
<think>
Incomplete reasoning...
```

## Regex Scripts

[Regex extension](/extensions/Regex.md)からのRegular Expression Scriptsは、推論ブロックの内容に適用できます。スクリプトエディターの「Affects」セクションで「Reasoning」をチェックして、推論ブロックを特に対象にします。

異なるEphemerality オプションは、推論ブロックに以下の方法で影響を与えます：

1. No ephemerality: 推論内容は永続的に変更されます。
2. Run on edit: regex scriptは推論ブロックが編集されるときに再評価されます。
3. Alter chat display: regexが基になるコンテンツではなく推論ブロックの表示テキストに適用されます。
4. Alter outgoing prompts: regexはモデルに送信される前に推論ブロックにのみ適用されます。

## Reasoning Effort

Reasoning Effortは、**<i class="fa-solid fa-sliders"></i> AI Response Configuration**パネルのChat Completionの設定で、推論に潜在的に使用される可能性があるトークンの数に影響を与えます。各オプションの効果は、接続されているソースに依存しています。以下のソースでは、自動は単に関連するパラメーターがリクエストに含まれていないことを意味しています。

| Option  | Claude (≤ 21333 if no streaming) | OpenAI (keyword)     | OpenRouter (keyword)             | xAI (Grok) (keyword) | Perplexity (keyword) |
| ------- | -------------------------------- | -------------------- | -------------------------------- | -------------------- | -------------------- |
| Models  | Opus 4, Sonnet 4/3.7             | o4-mini, o3\*, o1\*  | applicable models                | grok-3-mini          | sonar-deep-research  |
| Auto    | not specified, **no thinking**   | not specified        | not specified, effect depends    | not specified        | not specified        |
| Minimum | budgets 1024 tokens              | "low"                | "low", or 20% of max response    | "low"                | "low"                |
| Low     | 15% of max response, min 1024    | "low"                | "low", or 20% of max response    | "low"                | "low"                |
| Medium  | 25% of max response, min 1024    | "medium"             | "medium", or 50% of max response | "low"                | "medium"             |
| High    | 50% of max response, min 1024    | "high"               | "high", or 80% of max response   | "high"               | "high"               |
| Maximum | 95% of max response, min 1024    | "high"               | "high", or 80% of max response   | "high"               | "high"               | 

- Claudeの場合、ストリーミングが無効な場合、バジェットは21333にキャップされます。計算されたバジェットが1024未満の場合、maxレスポンスは2048に変更されます。
- OpenRouter、Perplexity、およびAI/ML APIの場合、OpenAI形式のキーワードのみが送信されます。

Google AI StudioおよびVertex AIは、以下の通りです：

| Model          | Auto (dynamic thinking) | Minimum            | Low                          | Medium     | High       | Maximum               |
| -------------- | ----------------------- | ------------------ | ---------------------------- | ---------- | ---------- | --------------------- | 
| 2.5 Pro        | thinkingBudget = -1     | 128                | 15% of max response, min 128 | 25% of max | 50% of max | lower of max or 32768 |
| 2.5 Flash      | thinkingBudget = -1     | 0, **no thinking** | 15% of max response          | 25% of max | 50% of max | lower of max or 24576 |
| 2.5 Flash Lite | thinkingBudget = -1     | 0, **no thinking** | 15% of max response, min 512 | 25% of max | 50% of max | lower of max or 24576 |

- Gemini 2.5 ProおよびGemini 2.5 Flash/Liteの場合、ストリーミング設定に関係なく、予算は32768または24576トークンそれぞれにキャップされています。
