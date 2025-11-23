---
order: -10
icon: code-review
route: /for-contributors/function-calling/
---

# Function Calling

Function Callingは、LLMが使用できる構造化データを提供し、拡張機能の特定の機能をトリガーできるようにすることで、動的な機能を追加できます。

## 使用例

1. 追加情報のための外部APIのクエリ(ニュース、天気、Web検索など)。
2. ユーザー入力に基づく計算または変換の実行。
3. 重要な記憶や事実の保存と呼び出し(RAGやデータベースクエリを含む)。
4. 会話への真のランダム性の導入(サイコロ、コイントスなど)。

## Function callingを使用する公式拡張機能

1. [Image Generation](/extensions/Stable-Diffusion.md) (内蔵) - ユーザープロンプトに基づいて画像を生成します。
2. [Web Search](/extensions/WebSearch.md) - クエリのWeb検索をトリガーします。
3. [RSS](https://github.com/SillyTavern/Extension-RSS/) - RSSフィードから最新のニュースを取得します。
4. [AccuWeather](https://github.com/SillyTavern/Extension-AccuWeather) - AccuWeatherから天気情報を取得します。
5. [D&D Dice](https://github.com/SillyTavern/Extension-Dice) - D&Dゲーム用のサイコロを振ります。

## 前提条件と制限事項

1. この機能は、特定のChat Completionソースでのみ利用可能です:OpenAI、Claude、MistralAI、Groq、Cohere、OpenRouter、AI21、Google AI Studio、Google Vertex AI、DeepSeek、AI/ML API、Custom APIソース。
2. Text Completion APIはfunction callsをサポートしていませんが、OllamaやTabbyAPIなどの一部のローカルホスト型バックエンドは、Chat Completionの下でCustom OpenAI互換モードで実行できる場合があります。
3. Function callingのサポートは、まずユーザーが明示的に許可する必要があります。これは、AI Response Configurationパネルの「Enable function calling」オプションを有効にすることで行われます。
4. LLMがfunction callsを実行する保証はありません。ほとんどのLLMは、プロンプトを通じた明示的な「アクティベーション」が必要です(例:ユーザーが「サイコロを振る」、「天気を取得する」などを要求する)。
5. すべてのプロンプトがtool callをトリガーできるわけではありません。継続、なりすまし、バックグラウンド('quiet')プロンプトは、tool callをトリガーすることはできません。それらは、過去の成功したtool callsを応答で使用できます。

## Function toolの作成方法

### 機能がサポートされているか確認

Function tool calling機能がサポートされているかどうかを判断するには、`SillyTavern.getContext()`オブジェクトから`isToolCallingSupported`を呼び出します。これにより、現在のAPIがfunction tool callingをサポートしているか、設定で有効になっているかを確認します。機能がサポートされているか確認する方法の例は次のとおりです:

```ts
if (SillyTavern.getContext().isToolCallingSupported()) {
    console.log("Function tool calling is supported");
} else {
    console.log("Function tool calling is not supported");
}
```

### 関数の登録

Function toolを登録するには、`SillyTavern.getContext()`オブジェクトから`registerFunctionTool`関数を呼び出し、必要なパラメータを渡す必要があります。Function toolを登録する方法の例は次のとおりです:

```ts
SillyTavern.getContext().registerFunctionTool({
    // Internal name of the function tool. Must be unique.
    name: "myFunction",
    // Display name of the function tool. Will be shown in the UI. (Optional)
    displayName: "My Function",
    // Description of the function tool. Must describe what the function does and when to use it.
    description: "My function description. Use when you need to do something.",
    // JSON schema for the parameters of the function tool. See: https://json-schema.org/
    parameters: {
        $schema: 'http://json-schema.org/draft-04/schema#',
        type: 'object',
        properties: {
            param1: {
                type: 'string',
                description: 'Parameter 1 description',
            },
            param2: {
                type: 'string',
                description: 'Parameter 2 description',
            },
        },
        required: [
            'param1', 'param2',
        ],
    },
    // Function to call when the tool is triggered. Can be async.
    // If the result is not a string, it will be JSON-stringified.
    action: async ({ param1, param2 }) => {
        // Your function code here
        console.log(`Function called with parameters: ${param1}, ${param2}`);
        return "Function result";
    },
    // Optional function to format the toast message displayed when the function is invoked.
    // If an empty string is returned, no toast message will be displayed.
    formatMessage: ({ param1, param2 }) => {
        return `Function is called with: ${param1} and ${param2}`;
    },
    // Optional function that returns a boolean value indicating whether the tool should be registered for the current prompt.
    // If no shouldRegister function is provided, the tool will be registered for every prompt.
    shouldRegister: () => {
        return true;
    },
    // Optional flag. If set to true, the function call will be performed, but the result won't be recorded to the visible chat history.
    stealth: false,
});
```

### 関数の登録解除

Function toolを無効にするには、`SillyTavern.getContext()`オブジェクトから`unregisterFunctionTool`関数を呼び出し、無効にするfunction toolの名前を渡す必要があります。Function toolの登録を解除する方法の例は次のとおりです:

```ts
SillyTavern.getContext().unregisterFunctionTool("myFunction");
```

## ヒントとコツ

1. 成功したtool callsは、表示可能な履歴の一部として保存され、チャットUIに表示されるため、実際のパラメータと結果を検査できます。これが望ましくない場合は、function toolを登録する際に`stealth: true`フラグを設定してください。
2. チャット履歴にtool callを表示したくない場合。カスタムCSSでスタイライズまたは非表示にしたい場合は、`.mes`要素の`toolCall`クラスをターゲットにします。例:`.mes.toolCall { display: none; }`または`.mes.toolCall { color: #999; }`。
