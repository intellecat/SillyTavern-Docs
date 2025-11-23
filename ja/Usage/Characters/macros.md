---
order: 90
route: /usage/core-concepts/macros/
---

# Macros (replacement tags)

!!! Note
このリストは不完全または時代遅れの可能性があります。SillyTavern チャットで `/help macros` スラッシュ コマンドを使用して、インスタンスで機能するマクロのリストを取得します。
!!!

マクロはキャラクター説明、著者の注記、世界情報など、多くの場所で使用でき、応答を生成するときに対応する値に置き換えられます。ユーザー名やキャラクターの説明など、プロンプトに動的なコンテンツを挿入するために使用できます。マクロはダブル中括弧で囲まれています（例：`{{user}}`）。通常は大文字と小文字を区別しません。**マクロネストは現在サポートされていないことに注意してください。**

注：一部の拡張機能は、特定の領域でのみ機能する特定のコンテキストマクロも追加できます（例：拡張機能プロンプトの特別なプレースホルダー）。マクロが特定の機能にバインドされていない限り、ここに文書化されません。

## General Macros

| Macro | Description |
|-------|-------------|
| `{{pipe}}` | スラッシュ コマンドのバッチ処理のみ。前のコマンドの返されたリスト結果に置き換えられます。 |
| `{{newline}}` | 改行を挿入します。 |
| `{{trim}}` | このマクロを囲む改行をトリミングします。 |
| `{{noop}}` | 操作なし、単なる空の文字列。 |
| `{{user}}` or `<USER>` | ユーザー名。 |
| `{{charPrompt}}` | キャラクターのメイン プロンプト オーバーライド。 |
| `{{charJailbreak}}` | キャラクターのポスト ヒストリー インストラクション プロンプト オーバーライド。 |
| `{{group}}` or `{{charIfNotGroup}}` | グループ メンバー名のカンマ区切りリスト、またはソロ チャットのキャラクター名。 |
| `{{groupNotMuted}}` | `{{group}}` と同じですがミュートされたメンバーを除外します。 |
| `{{notChar}}` | 現在のスピーカー（`{{char}}`）を除く、すべてのチャット参加者のカンマ区切りリスト。グループ チャットではミュートされたキャラクターが含まれ、メッセージが生成されていない場合はロスター内のすべてのキャラクターがリストされます。 |
| `{{char}}` or `<BOT>` | キャラクター名。 |
| `{{description}}` | キャラクターの説明。 |
| `{{scenario}}` | キャラクターのシナリオまたはチャット シナリオ オーバーライド（設定されている場合）。 |
| `{{personality}}` | キャラクター個性。 |
| `{{persona}}` | ユーザー ペルソナの説明。 |
| `{{mesExamples}}` | キャラクターのダイアログ例（インストラクト形式）。 |
| `{{mesExamplesRaw}}`  | キャラクターのダイアログ例（未変更および分割されていない）。 |
| `{{charVersion}}` | キャラクターのバージョン番号。 |
| `{{charDepthPrompt}}` | キャラクターの at-depth プロンプト。 |
| `{{model}}` | 現在選択されている API のテキスト生成モデル名。**不正確な場合があります！** |
| `{{lastMessageId}}` | 最後のチャット メッセージ ID。 |
| `{{lastMessage}}` | 最後のチャット メッセージ テキスト。 |
| `{{firstIncludedMessageId}}` | コンテキストに含まれる最初のメッセージの ID。現在のセッションで少なくとも 1 回、生成を実行する必要があります。 |
| `{{lastCharMessage}}` | キャラクターが送信した最後のチャット メッセージ。 |
| `{{lastUserMessage}}` | ユーザーが送信した最後のチャット メッセージ。 |
| `{{currentSwipeId}}` | 現在表示されている最後のメッセージ スワイプの 1 ベース ID。 |
| `{{lastSwipeId}}` | 最後のチャット メッセージのスワイプ数。 |
| `{{lastGenerationType}}` | 最後のキューに入れられた生成要求のタイプ。値：「normal」、「impersonate」、「regenerate」、「quiet」、「swipe」、「continue」。 |
| `{{original}}` | システム設定からデフォルト プロンプトを含むために、プロンプト オーバーライド フィールドで使用できます。Chat Completion API とインストラクト モードにのみ適用されます。 |
| `{{time}}` | 現在のシステム時刻。 |
| `{{time_UTC±X}}` | 指定された UTC オフセット（タイムゾーン）での現在の時刻。例：UTC+02:00 の場合は `{{time_UTC+2}}` を使用します。 |
| `{{timeDiff::(time1)::(time2)}}` | time1 と time2 の間の時間差。時間と日付のマクロを受け入れます。 |
| `{{date}}` | 現在のシステム日付。 |
| `{{input}}` | ユーザー入力バーの内容。 |
| `{{weekday}}` | 現在の曜日。 |
| `{{isotime}}` | 現在の ISO 時刻（24 時間形式）。 |
| `{{isodate}}` | 現在の ISO 日付（YYYY-MM-DD）。 |
| `{{datetimeformat ...}}` | 指定された形式での現在の日付/時刻（例：`{{datetimeformat DD.MM.YYYY HH:mm}}`）。 |
| `{{idle_duration}}` | 最後のユーザー メッセージが送信された後の時間範囲の人間化された文字列を挿入します（例：4 時間、1 日）。 |
| `{{random:(args)}}` | リストからランダムなアイテムを返します（例：`{{random:1,2,3,4}}` は 4 つの番号のうち 1 つをランダムに返します）。 |
| `{{random::arg1::arg2}}` | コンマをサポートする引数の代替構文。 |
| `{{pick::(args)}}` | ランダムの代替ですが、選択された引数は、ソース文字列が変更されないままであれば、現在のチャット内の後続の評価で安定しています。 |
| `{{roll:(formula)}}` | D&D サイコロ構文を使用してランダム値を生成します：XdY+Z（例：`{{roll:d6}}` は値 1-6 を生成します）。 |
| `{{bias "text here"}}` | 次のユーザー入力まで AI の行動バイアスを設定します。テキストの周りに引用符が必要です。 |
| `{{// (note)}}` | メモを残すことができます。空白のコンテンツに置き換えられます。AI には見えません。 |
| `{{banned "text here"}}` | Text Generation WebUI バックエンド用の禁止された単語シーケンスに動的に引用符で囲まれたテキストを追加します。他のバックエンドでは何もしません。引用符が必要です。 |
| `{{reverse:(content)}}` | マクロの内容を逆にします。 |
| `{{outlet::(name)}}` | 名前付き [World Info outlet](/Usage/worldinfo.md#outlet-name) のコンテンツに置き換えられます。改行で区切られたアクティブ化されたエントリが含まれます。 |

## Instruct Mode and Context Template Macros

(Advanced Formatting 設定で有効)

| Macro | Description |
|-------|-------------|
| `{{exampleSeparator}}` | Context template ダイアログの例の区切り文字。 |
| `{{chatStart}}` | Context template チャット開始行。 |
| `{{instructSystemPrompt}}` | Instruct システム プロンプト。 |
| `{{instructSystemPromptPrefix}}` | システム プロンプト プレフィックス シーケンス。 |
| `{{instructSystemPromptSuffix}}` | システム プロンプト サフィックス シーケンス。 |
| `{{instructUserPrefix}}` | ユーザー メッセージ プレフィックス シーケンス。 |
| `{{instructAssistantPrefix}}` | アシスタント メッセージ プレフィックス シーケンス。 |
| `{{instructSystemPrefix}}` | システム メッセージ プレフィックス シーケンス。 |
| `{{instructUserSuffix}}` | ユーザー メッセージ サフィックス シーケンス。 |
| `{{instructAssistantSuffix}}` | アシスタント メッセージ サフィックス シーケンス。 |
| `{{instructSystemSuffix}}` | システム メッセージ サフィックス シーケンス。 |
| `{{instructFirstAssistantPrefix}}` | アシスタント最初の出力シーケンス。 |
| `{{instructLastAssistantPrefix}}` | アシスタント最後の出力シーケンス。 |
| `{{instructFirstUserPrefix}}` | Instruct ユーザー最初の入力シーケンス。 |
| `{{instructLastUserPrefix}}` | Instruct ユーザー最後の入力シーケンス。 |
| `{{instructSystemInstructionPrefix}}` | システム インストラクション プレフィックス シーケンス。 |
| `{{instructUserFiller}}` | ユーザー フィラー メッセージ テキスト。 |
| `{{instructStop}}` | Instruct stop シーケンス。 |
| `{{maxPrompt}}` | トークン内のプロンプトの最大サイズ（応答の長さで減らしたコンテキスト長）。 |
| `{{systemPrompt}}` | システム プロンプト コンテンツ（許可された場合は利用可能なキャラクター プロンプト オーバーライドを含む）。 |
| `{{defaultSystemPrompt}}` | システム プロンプト コンテンツ（キャラクター プロンプト オーバーライドを除く）。 |

## Chat variables Macros

- Local variables = 現在のチャットに一意
- Global variables = どのチャットのどのキャラクターでも機能

| Macro | Description |
|-------|-------------|
| `{{getvar::name}}` | ローカル変数「name」の値に置き換えられます。 |
| `{{setvar::name::value}}` | 空の文字列に置き換えられ、ローカル変数「name」を「value」に設定します。空の値を許可します。 |
| `{{addvar::name::increment}}` | 空の文字列に置き換えられ、ローカル変数「name」に数値「increment」を追加します。 |
| `{{incvar::name}}` | 変数「name」の値を 1 ずつ増加させた結果に置き換えられます。 |
| `{{decvar::name}}` | 変数「name」の値を 1 ずつ減少させた結果に置き換えられます。 |
| `{{getglobalvar::name}}` | グローバル変数「name」の値に置き換えられます。 |
| `{{setglobalvar::name::value}}` | 空の文字列に置き換えられ、グローバル変数「name」を「value」に設定します。空の値を許可します。 |
| `{{addglobalvar::name::value}}` | 空の文字列に置き換えられ、グローバル変数「name」に数値「increment」を追加します。 |
| `{{incglobalvar::name}}` | グローバル変数「name」の値を 1 ずつ増加させた結果に置き換えられます。 |
| `{{decglobalvar::name}}` | グローバル変数「name」の値を 1 ずつ減少させた結果に置き換えられます。 |
| `{{var::name}}` | スコープ付き変数「name」の値に置き換えられます（STscript のみ）。 |
| `{{var::name::index}}` | スコープ付き変数「name」のインデックスの値に置き換えられます（STscript の配列/オブジェクト用）。 |

## 拡張機能固有のマクロ

拡張機能によって追加され、特定の条件下でのみ機能します。

| Macro | Description |
|-------|-------------|
| `{{summary}}` | 現在のチャット セッションの概要に置き換えられます（利用可能な場合）。 |
| `{{authorsNote}}` | 「Author's Note」の内容に置き換えられます。 |
| `{{charAuthorsNote}}` | キャラクターの「Author's Note」の内容に置き換えられます。 |
| `{{defaultAuthorsNote}}` | デフォルトの「Author's Note」の内容に置き換えられます。 |
| `{{charPrefix}}` | キャラクター固有の Image Generation ポジティブ プロンプト プレフィックスに置き換えられます（利用可能な場合）。 |
| `{{charNegativePrefix}}` | キャラクター固有の Image Generation ネガティブ プロンプト プレフィックスに置き換えられます（利用可能な場合）。 |
