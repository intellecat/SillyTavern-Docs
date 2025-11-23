---
route: /usage/api-connections/novelai/
---

# NovelAI

NovelAIは、高品質な社内テキスト生成、画像生成、テキスト読み上げモデルへの無制限の月次アクセスを可能にする有料サブスクリプションサービスです。アカウントを登録して始めましょう: <https://novelai.net/>

モデルを評価するための*50回の生成*のみが無料で提供されます。**「Not eligible for this model」**エラーが表示された場合、これはトライアル期間が終了し、有料プランへのサブスクリプションが必要であることを意味します。

## APIキー

NovelAI APIキーを取得するには、次の手順に従います:

1. 左サイドバーの上部にある歯車アイコンを選択します。
![Left Sidebar](/static/novel-side.png)

2. 「User Settings」の下の「Account」を選択します。
![User Settings](/static/novel-user.png)

3. 「Get Persistent API Token」を選択します。
![Account](/static/novel-account.png)

4. コピーアイコンを選択して、NovelAI APIトークンをクリップボードにコピーします。
![Persistent API Token](/static/novel-token.png)

## Models

Opusを持っている場合、Eratoが使用するモデルです。Opusを持っていない場合、Kayraが利用可能な最良のモデルです。

Clioはタブレット/スクロールティアでより大きなコンテキストサイズを持っていますが、Kayraの強さは通常その差を補います。

## Settings

設定ファイルはここにあります(`SillyTavern/data/<user-handle>/NovelAI Settings`)。
独自の設定ファイルを手動で追加することもできます。

### Response Length

メッセージごとに生成したいテキストの量。NovelAIは応答ごとに150トークンの制限があることに注意してください。

### Context Size

任意の時点でコンテキストに保持されるチャットのトークン数。使用できる最大コンテキストサイズは、モデルとサブスクリプションティアによって異なります:

- Kayra (Tablet) - 3072 tokens
- Kayra (Scroll) - 6144 tokens
- Erato (Opus exclusive), Kayra (Opus) and Clio (all tiers) - 8192 tokens

### Preamble

ライティングスタイルを変更するためにチャットのすぐ上に挿入されるテキスト。推奨フォーマットは「[ Style: chat, detailed, sensory ]」のような短いタグのリストです。

## Preset Descriptions
これは、Novel AIによると、デフォルトプリセットが適しているものです。

### Erato

* Golden Arrow - 優れたオールラウンダー。
* Wilder - 単語選択のバリエーションが高く、リロール間の違いが多く、ミスが発生しやすい。
* Zany Scribe - ミスと繰り返しを避ける。より複雑な単語を優先。
* Dragonfruit - 繰り返しが少ない多様で複雑な言語。頻繁なミスと矛盾。
* Shosetsu - 日本語での執筆用に設計。英語でもうまく機能。

### Kayra

* Asper - クリエイティブライティング用。予期しない展開が期待できます。
* Carefree - 優れたオールラウンダー。
* Fresh-Coffee - 物事を軌道に乗せます。instructをうまく処理。
* Pro_Writer - ベストセラー小説のペースと雰囲気を模倣。
* Stelenes - 合理的な代替案を選択する可能性が高い。再試行時のバリエーション。
* Tea_Time - 勢いがつくと良くなる。
* Writers-Daemon - 極めて想像力豊かで、時には過剰。

### Clio

* Edgewise - さまざまな生成スタイルをうまく処理。
* Fresh Coffee - 物事を軌道に乗せます。
* Long-Press - クリエイティブな散文を意図。
* Talker Chat - チャットスタイルの生成用に設計。
* Vingt-Un - 散文に傾いた優れたオールラウンドデフォルト。

## NovelAIをSillyTavernで使用するためのヒントとFAQ

NovelAIを別のSTバックエンドAPIから切り替える際に発生する多くの一般的な問題と質問があります。違いは、モデルがトレーニングされている目的に起因します。おそらく、OpenAIまたはAnthropicモデル(またはそれらに似たローカルモデル)を使用したことがあるでしょう。これらはユーザーの指示に従うように構築されています。NovelAIのモデルは、純粋にテキスト補完を中心に構築されています。入力をメッセージとして受け取り、応答を策定する代わりに、NAIのモデルは受信プロンプトを続けようとします。この違いにより、他のAPIで機能する多くのヒントと一般的な知識は、NAIでは機能しません。

### NovelAIの設定の調整

Advanced Formatting(Aアイコン)の下:
- 「Context Template」を「NovelAI」に設定
- 「Tokenizer」を「Best match」に設定
- 「Always add character's name to prompt」をチェック
- 「Collapse Consecutive Newlines」をチェック
- 「Instruct Mode」の下の「Enabled」ボックスのチェックを外す

User Settings(歯車付きの人)の下:
- 「Swipes」をオンにする(NAI固有ではありませんが、非常に便利なのでオンにすべきです)

### NovelAI用のキャラクターカードの構築/適応

NovelAI用にキャラクターカードを最適化するには、キャラクターの説明を書くためのいくつかの推奨方法があります: proseとattributes。

Proseは非常にシンプルで、機能するとは思えないほどです: 「Sylpheedは若く見えるが実際には900歳のニンフです。彼女は低身長で小柄、長い白髪が編み込まれたサイドポニーテールで緑のグラデーションに変わり、十字架の形をしたエメラルドグリーンの目をしています。[...]」 いいえ、本当に、それだけです。キャラクターがどのように見えるか、どのように行動するかなどを通常の文章で書くだけで、AIがそれを拾います。

ライティング能力を信頼していない場合、またはより構造化された方法を望む場合は、NovelAIのトレーニングデータに存在するattributesメソッドを使用できます。これは、さまざまなタイプのキャラクター特性の単純なリストとして機能します。NovelAIのモデルで効果的であることがテストされた可能性のあるattributesのリストは次のとおりです:

```
Name:
AKA:
Type: character
Setting:
Nationality:
Species:
Gender:
Age:
Height:
Weight:
Appearance:
Clothing:
Attire:
Personality:
Mind:
Mental:
Likes:
Dislikes:
Sexuality:
Speech:
Voice:
Abilities:
Skills:
Quote:
Affiliation:
Occupation:
Reputation:
Secret:
Family:
Allies:
Enemies:
Background:
Description:
Attributes:
```

「Type: character」は、これがキャラクター(場所、オブジェクト、または他のタイプのものとは対照的に)を説明していることをAIに伝えるためにあります。残りのattributesはオプションで、一部は冗長です(例えば、Personality、Mind、Mentalはすべて基本的に同じことを意味します)が、これらはテストされ、NovelAIのモデルでうまく機能します。キャラクターに関連するものを記入してください。attributesは小文字で書き、カンマで区切る必要があり、単語の周りに引用符は必要ありません。例:

```
Skills: lockpicking, stealth, running away very fast
```

これらのメソッドは、NovelAIのトレーニングデータに存在するため推奨されており、モデルで特にうまく機能します。

#### カードの例

NovelAI用に作成され、NovelAI専用のカードを作成するさまざまな方法を示す、いくつかのカードの例を以下に示します。最初のカード、Valkaは、キャラクターの説明にattributesメソッドを使用し、2番目のカード、Erisは、大量のexample dialogueと共にprose descriptionを使用しています。

<div style="display:flex;gap:2em;justify-content:center">

[![Valka](/static/Valka.png)](/static/Valka.png)

[![Eris](/static/Eris.png)](/static/Eris.png)

</div>

#### してはいけないこと

既存のキャラクターカードフォーマットのほとんどは、NovelAIには適していません。いくつかの良い結果、さらにはいくつかの良いものを得ることができますが、多くの問題があります。W++は最大の違反者の1つで、NovelAIのモデルがトレーニングされたものに似ていないため、ブラケット/ブレース/引用符を常に使用すると大量のトークンを消費し、実際の利益なしにカードのサイズを肥大化させます。

NovelAIに組み込まれていない既存のフォーマットの中で、AliChatが最も機能する可能性が高いです。これは、example messagesを使用して、キャラクターに関する情報とその声を同時に伝えることに依存し、AIが出力したいメッセージのタイプのフォーマットで行います。

ほとんどの他のフォーマットは、通常、特定のキャラクターのさまざまな特性をリストアウトする方法であるため、attributesメソッドにかなり簡単に変換できます。

### どのモジュールを使用すべきですか？

おそらくNo Module。Prose Augmenterは、キャラクターがより華麗な方法で話すようにしたい場合に便利ですが、やりすぎないように注意してください。Text Adventureは、テキストアドベンチャースタイルのカード/ストーリーに役立つ可能性があります。

### instructモジュールではないのですか？

必要なときにInstructモジュールを呼び出すことができます。メッセージに改行を作成し、次のように中括弧に指示を入れます: `{ CharName is offended by that seemingly innocuous statement }` (テキストと括弧の間のスペースは_必須_です)。これを行うと、AIは短時間自動的にInstructモジュールに切り替わります。Instructモジュールは他のモジュールよりも創造性の低い出力を生成する傾向があるため、常にInstructモジュールを使用したくありません。特定の方向に強くAIをガイドする必要がある場合にのみ使用してください。

### 応答が切り捨てられ続けるのはなぜですか？

NovelAIは、スライダーをそれより高く設定していても、応答の長さを最大約150トークンに制限します。スライダーのトークン数または150のいずれか低い方に達すると、最大20トークンを追加で生成し、停止シーケンスまたは文の終わりを探すため、応答の有効な制限は170トークンであり、その時点で停止し、切り捨てられます。

切り捨てられた場合は、continueオプション(テキストボックスの左側の3行メニュー)を選択して、キャラクターに応答を続けさせることができます。

定期的に170トークンより長い応答が必要な場合は、次のように制限を回避できます:

- Response lengthを150トークンに保ちます。
- Advanced Formattingの下で、Auto-continueを有効にします。
- 「Target length」を希望する長さに設定します。

これにより、複数の生成が連鎖してより長いメッセージが得られますが、モデルが停止を決定した場合、返信が希望する長さの100%になることを保証するものではありません。

### ボットがより長い応答を書くようにするにはどうすればよいですか？

応答が切り捨てられることについての上記を読んでください。これにより、生成長の制限に達することで応答が早期に切り捨てられないようにするのに役立ちます。

応答が切り捨てられていないがまだ短すぎる場合は、「garbage in, garbage out」に対処している可能性があります - モデルに悪い例を与えると、悪い出力が生成されます。キャラクターカードにexample dialogueがないか、短いexample dialogueがあり、ボットに送信するメッセージが短い場合、モデルはそれを拾い、受け入れられた方法と見なし、応答は短くなります。したがって、より長いexample dialogueとボットへのより長いメッセージを書いてください。(自分で書くのではなく、常にNovelAIを使用してexample dialogueを書くことができます。)

### ボットが自分の代わりに話すのを止めるにはどうすればよいですか？

- キャラクターカードの最初のメッセージとexample dialogueに、キャラクターがあなたのために行動を取ることが含まれていないことを確認してください - 含まれている場合は、それを書き直して、あなたのために行動するのを取り除いてください
- 「Always add character's name to prompt」がチェックされていることを確認してください
- チャットの残りの部分と同じuser personaを現在使用していることを確認してください。user personasを変更して戻さなかった場合(またはそのチャットにロックされたpersonaがない場合)、あなたのために生成を停止する通常のルールが失敗します
- ["\n\{\{user\}\}:"]をCustom Stopping Stringsに追加します(必要ないはずですが、時々役立ちます)

### キャラクターが応答しないのはなぜですか？

これを引き起こす可能性のあるものは多数あるため、いくつかの場所を確認する必要があります:

- Advanced Formattingで「Always add character's name to prompt」がチェックされていることを確認してください
- APIからエラーが発生していないことを確認してください。NAI無料トライアルでSillyTavernを使用できますが、終了すると、エラーが発生します
- 「Custom Stopping Strings」に何があるかを確認してください - これらが応答の開始時に生成されている場合、早期に切り捨てられる可能性があります

### Author's Noteをどのように使用すべきですか？

一般的に、おそらく使用すべきではありません。コンテキストの終わりに非常に近い場所に挿入され、NAIのモデルでは、コンテキストの他のすべてを頻繁に圧倒します。これは主に、より古い、より弱いモデルからの遺物で、より必要でした。

### シーンブレーク/タイムジャンプをどのように行いますか？

次をsystem messageとして、または次のメッセージの開始時の改行に配置します:
```
***
[ 2 days later ]
```

次に、メッセージの残りの部分を次の行に配置します。括弧内のテキストは、タイムジャンプ、新しい場所、またはその他のものにすることができます。「***」(滑稽なことに「dinkus」と呼ばれる)は、シーンが変わったことをAIに伝え、括弧内のテキストはそれにさらにコンテキストを与えます。

### AIが特定の単語/フレーズを繰り返し続ける場合、どうすればよいですか？

上記のように、repetition penaltyスライダーを少し上げることができますが、あまり押しすぎると出力が支離滅裂になる可能性があります。
問題をより徹底的に修正するには、コンテキスト、特に最近のメッセージを遡り、繰り返される単語/フレーズを削除します。コンテキストから削除すると、AIがそもそもそれを言い始める理由が少なくなります。
