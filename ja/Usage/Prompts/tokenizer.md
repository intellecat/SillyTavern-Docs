---
order: 70
route: /usage/prompts/tokenizer/
---

# トークナイザー (Tokenizer)

Tokenizerは、テキストをトークンと呼ばれるより小さい単位に分割するツールです。これらのトークンは個別の単語またはプリフィックス、サフィックス、または句読点などの単語の一部である可能性があります。経験則として、1つのトークンは通常、テキストの3～4文字に対応しています。

SillyTavernは、使用するAPIプロバイダーに応じて、次のルールを使用してトークナイザーのマッチングを試みる「Best match」オプションを提供しています。

Text Completion APIs**(オーバーライド可能)**：

1. NovelAI Clio: NerdStash tokenizer。
2. NovelAI Kayra: NerdStash v2 tokenizer。
3. Text Completion: APIトークナイザー（サポートされている場合）またはLlamaトークナイザー。
4. KoboldAI Classic / AI Horde: Llamaトークナイザー。
5. KoboldCpp: model APIトークナイザー。

不正確な結果が得られた場合または実験したい場合は、AIバックエンドへのリクエストを形成するときにSillyTavernが使用する_override tokenizer_を設定できます：

1. None. 各トークンは～3.3文字として推定され、最も近い整数に四捨五入されます。**プロンプトが高いコンテキスト長でカットオフされる場合、これを試してください。** このアプローチはKoboldAI Liteで使用されます。
2. Llamaトークナイザー。Llama 1/2モデルファミリーで使用：Vicuna、Hermes、Airoboros、等。**Llama 1/2モデルを使用する場合に選択してください。**
3. Llama 3トークナイザー。Llama 3/3.1モデルで使用。**Llama 3/3.1モデルを使用する場合に選択してください。**
4. NerdStashトークナイザー。NovelAIのClioモデルで使用。**Clioモデルを使用する場合に選択してください。**
5. NerdStash v2トークナイザー。NovelAIのKayraモデルで使用。**Kayraモデルを使用する場合に選択してください。**
6. Mistral V1トークナイザー。古いMistralモデルファミリーとそのfinetun es で使用。**古いMistralモデルを使用する場合に選択してください。**
7. Mistral Nemoトークナイザー。Mistral Nemoモデルファミリーとそのfinetunesで使用。**Mistral Nemo/Pixti ralモデルを使用する場合に選択してください。**
8. Yiトークナイザー。Yiモデルで使用。**Yiモデルを使用する場合に選択してください。**
9. Gemmaトークナイザー。Gemini/Gemmaモデルで使用。**Gemmaモデルを使用する場合に選択してください。**
10. DeepSeekトークナイザー。DeepSeekモデル（R1など）で使用。**DeepSeekモデルを使用する場合に選択してください。**
11. APIトークナイザー。生成APIに照会してトークン数をモデルから直接取得。既知のバックエンド：Text Generation WebUI（ooba）、koboldcpp、TabbyAPI、Aphrodite API。**サポートされているバックエンドを使用する場合に選択してください。**

Chat Completion APIs**(オーバーライド不可）：

1. OpenAI: [tiktoken](https://github.com/openai/tiktoken)を使用したモデル依存トークナイザー。
2. Claude: [WebTokenizers](https://github.com/mlc-ai/tokenizers-cpp)を使用したモデル依存トークナイザー。
3. OpenRouter: それぞれのモデルのLlama、Mistral、Gemma、Yiトークナイザー。
4. Google AI Studio: Gemmaトークナイザー。
5. AI21 API: Jambaトークナイザー（ワンタイム ダウンロードが必要）。
6. Cohere API: Command-RまたはCommand-Aトークナイザー（ワンタイムダウンロードが必要）。
7. MistralAI API: Mistral V1またはV3トークナイザー（ワンタイムダウンロードが必要）。
8. DeepSeek API: DeepSeekトークナイザー（ワンタイムダウンロードが必要）。
9. Fallbackトークナイザー：GPT-3.5ターボトークナイザー。

#### 追加トークナイザー (Additional Tokenizers)

これらのトークナイザーは、サイズが大きいため、デフォルトインストールに含まれていません。初回使用時にワンタイムダウンロードが必要です。

1. Qwen2 tokenizer。
2. Command-R / Command-Aトークナイザー。Chat Completionでのコホアソースで使用。
3. Mistral V3 (Nemo)トークナイザー。Chat CompletionのMistralAIソースで使用（NemoおよびPixtralモデル）。
4. DeepSeek (deepseek-chat)トークナイザー。Chat CompletionのDeepSeekソースで使用。

インターネットダウンロードを使用したくない場合は、config.yamlのオプトアウトオプションが存在します：`enableDownloadableTokenizers`。ダウンロードを無効化するために`false`に設定します。

[SillyTavern-Tokenizers](https://github.com/SillyTavern/SillyTavern-Tokenizers)リポジトリからトークナイザーを手動でダウンロードすることもできます。JSONファイルをダウンロードし、データルートの`_cache`サブディレクトリに配置します。デフォルトでパスは`./data/_cache`です。`_cache`ディレクトリが存在しない場合は、作成してください。その後、SillyTavernサーバーを再開してトークナイザーを再初期化します。

必要なトークナイザーモデルがキャッシュされていなく、ダウンロードが無効な場合、フォールバックトークナイザー（Llama 3）が数値に使用されます。

### トークンパディング (Token Padding)

!!! 該当：Text Completion APIs
SillyTavernは常にChat Completionモデルのマッチングトークナイザーを使用するため、トークンパディングの必要はありません。
!!!

SillyTavernが、モデルを実行するリモートバックエンドAPIが提供するトークナイザーを使用しない限り、プロンプト生成中に仮定されるすべてのトークン数は、選択した[tokenizer](#tokenizer)タイプに基づいて推定されます。

選択されたモデルの最大値に近いコンテキストサイズでトークン化の結果が不正確である可能性があるため、プロンプトの一部がトリムまたはドロップされ、キャラクター定義の一貫性に悪影響を与える可能性があります。

これを防ぐため、SillyTavernはコンテキストサイズのある部分をパディングとして割り当てて、モデルがサポートできるより多くのチャットアイテムが追加されることを避けてください。最も一致するトークナイザーでさえ、プロンプトの一部がトリムされていることに気付いた場合、説明が短くならないようにパディングを調整してください。

負のパディング値を入力して逆パディングを行うことができます。これにより、設定された最大トークン数以上の割り当てが可能になります。
