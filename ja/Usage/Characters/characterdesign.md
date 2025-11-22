---
order: 100
route: /usage/core-concepts/characterdesign/
templating: false
---

# Character Design

!!!tip
Character Nameは唯一の必須フィールドです。残りの部分を空のままにしても、チャットでキャラクターを使用できます。
!!!

## Character Description

キャラクターの説明やAIのためのその他の関連情報を追加するために使用されます。この情報は常にプロンプトに含まれるため、すべての重要な事実をここに含める必要があります。

たとえば、アクションが行われる世界に関する情報を追加したり、キャラクターの外見、性格、背景を説明したりできます。

任意の長さ（200トークンでも2000トークンでも）にすることができ、任意のスタイル（自由テキスト、疑似コード会話スタイルなど）でフォーマットできます。

### Methods and format

キャラクターフォーマットの方法は、このドキュメントページの範囲を超えた複雑なトピックです。

SillyTavernの機能でテストされた、または依存する推奨ガイド：

* Trappu's PLists + Ali:Chat guide: <https://wikia.schneedc.com/bot-creation/trappu/creation>
* AliCat's Ali:Chat guide: <https://rentry.co/alichat>
* kingbri's minimalistic guide: <https://rentry.co/kingbri-chara-guide>

## Character tokens

**要約：2048コンテキストトークン制限のAIモデルで作業している場合、1000トークンのキャラクター定義はAIの「メモリ」を半分に削減します。**

これを視野に入れると、優れたAIからの適切な応答は、簡単に200〜300トークン程度になる可能性があります。この場合、AIは約3回の交換分のチャット履歴しか「記憶」できません。

### キャラクターのトークンカウンターが赤くなったのはなぜですか？

キャラクターの定義にモデル定義のコンテキスト長の半分以上のトークンがあることがわかると、これがAIが楽しい会話を提供する能力を低下させる可能性があるため、強調表示します。

### キャラクターのトークンが多すぎるとどうなりますか？

心配しないでください - 何も壊れません。最悪の場合、キャラクターの永続トークンが大きすぎる場合、単にコンテキストに他のもののための余地が少なくなることを意味します（以下を参照）。

これが持つ可能性のある唯一の悪影響は、処理可能なチャット履歴が少なくなるため、AIの「メモリ」が少なくなることです。

これは、すべてのAIモデルが一度に処理できるコンテキストの量に制限があるためです。

## 'Context'?

これは、応答を生成するように要求するたびにAIに送信される情報です。SillyTavernは、情報をAIモデルに送信する前に、使用可能なコンテキストトークンを割り当てる最良の方法を自動的に計算します。

コンテキストの構築方法について詳しくは、[Prompts](/Usage/Prompts/index.md)セクションをお読みください。

### キャラクターの'Permanent Tokens'とは何ですか？

これらは、すべての生成リクエストでAIに常に送信されます：

* Character Name
* Character Description Box
* Character Personality Box
* Scenario Box

### キャラクターのDefinitionsのどの部分が永続的ではありませんか？

* The first message box - チャットの開始時に一度だけ送信されます。
* Example messages box - チャット履歴がコンテキストを満たすまでのみ保持されます（オプションでこれらをコンテキストに強制的に保持することができます）

### Popular AI Model Context Token Limits

* LLaMA 3 and its finetunes - 8192
* OpenAI GPT-4 - up to 128k
* Google Gemini - up to 2M
* Anthropic's Claude - 200k (Claude 3)
* NovelAI - 8192 (Erato and Kayra, Opus tier; Clio, all tiers), 6144 (Kayra, Scroll tier), or 3072 (Kayra, Tablet tier)

## First message

First Messageは、キャラクターがどのように、どのようなスタイルでコミュニケーションするかを定義する重要な要素です。モデルは、他のどこよりもfirst messageからスタイルと長さの制約を拾う可能性が最も高いため、応答のあり方（短く簡潔、長く詳細など）で書くことが重要です。

MarkdownとHTMLフォーマットをサポートします。

たとえば：

```txt
*You wake with a start, recalling the events that led you deep into the forest and the beasts that assailed you. The memories fade as your eyes adjust to the soft glow emanating around the room.* "Ah, you're awake at last. I was so worried, I found you bloodied and unconscious." *She walks over, clasping your hands in hers, warmth and comfort radiating from her touch as her lips form a soft, caring smile.* "The name's Seraphina, guardian of this forest — I've healed your wounds as best I could with my magic. How are you feeling? I hope the tea helps restore your strength." *Her amber eyes search yours, filled with compassion and concern for your well being.* "Please, rest. You're safe here. I'll look after you, but you need to rest. My magic can only do so much to heal you."
```

## Alternate Greetings

ここに追加されたメッセージは、新しいチャットを開始するときに、キャラクターの最初のメッセージの追加の「スワイプ」として表示されます。キャラクターがグループチャットの一部である場合、システムはこれらの挨拶の1つをランダムに選択して会話を開始します。

## Favorite Character

**<i class="fa-solid fa-star"></i> Add to Favorites**ボタンをクリックして、キャラクターをお気に入りとしてマークし、サイドメニューバーの「Favorites」ソートオプションを選択することですばやくフィルタリングできます。お気に入りのキャラクターはリストに金色のハイライトがあります。これにより、キャラクターポートレートがホットスワップエリアに表示されます（User Settingsで有効になっている場合）。

## Advanced Definitions

!!!info
次のフィールドはデフォルトで非表示になっています。これらにアクセスして編集するには、キャラクター定義ページのメニューバーにある**<i class="fa-solid fa-book"></i> Advanced Definitions**ボタンをクリックする必要があります。
!!!

### Prompt Overrides

* **Main Prompt**: 「Prefer Char. Prompt」ユーザー設定が有効になっている場合、ここに入力したテキストは、キャラクターの[main/system prompt](/Usage/Prompts/index.md#main-prompt-system-prompt)を上書きします。
* **Post-History Instructions**: 「Prefer Char. Instructions」ユーザー設定が有効になっている場合、ここに入力したテキストは、キャラクターの[post-history instructions](/Usage/Prompts/index.md#post-history-instructions)として使用されます。

!!!tip
どちらのボックスにも`{{original}}`を挿入して、システム設定のそれぞれのデフォルトプロンプトを指定された場所に含めます。
!!!

### Creator's Metadata

!!!info
プロンプト構築には使用されませんが、キャラクターに関する追加のメタデータを提供します。
!!!

* **Created by**: キャラクターの作成者の名前。「Char List Subheader」ユーザー設定が適切に設定されている場合、キャラクターリストに表示できます。
* **Character Version**: キャラクターのバージョン。「Char List Subheader」ユーザー設定が適切に設定されている場合、キャラクターリストに表示できます。
* **Creator's Notes**: 作成者が共有したいキャラクターに関する追加のメモ。最初の数行はキャラクターリストに表示され、全文はキャラクターのページの「Creator's Notes」セクションに表示されます。Markdown/HTMLフォーマットをサポートします。
* **Tags to Embed**: キャラクターの説明に埋め込まれるタグのコンマ区切りリスト。これらのタグは、キャラクターをインポートするときにデフォルトではインポートされませんが、キャラクターのページの「More...」メニューから「Import Tags」を選択することで、既存のタグとマージできます。

### Personality summary

キャラクターの性格の簡単な要約。

### Scenario

対話の状況とコンテキスト。

### Character's Note

特定のメッセージ深度でキャラクターのチャット内プロンプトインジェクションとして使用されるテキスト。通常、特定のキャラクターの特性を強化するために使用されます。これは、進行に関係なく、チャット履歴内の静的な深度に常に留まるためです。

* **@ Depth**: このノートが挿入されるチャット履歴内のメッセージの数（新しいものから古いものへの順序）。0に設定すると、最後のメッセージの後に挿入されます。
* **Role**: メッセージの役割。「User」、「System」、または「Assistant」にすることができます。

### Talkativeness

[Natural](/Usage/Characters/groupchats.md#natural-order)アクティベーション順序を使用する場合のグループチャットでキャラクターの応答がトリガーされる確率を決定します。0％から100％の範囲で、50％がデフォルト値です。

### Examples of dialogue

キャラクターがどのように話すかを説明します。各例の前に、`<START>`タグを追加する必要があります。例の対話のブロックは、コンテキストに空きスペースがある場合にのみ挿入され、ブロックごとにコンテキストから押し出されます。`<START>`は単なるマーカーであるため、プロンプトには存在しません。Text Completion APIのAdvanced FormattingからのExample Separator、Chat Completion APIの「New Example Chat」ユーティリティプロンプトの内容に置き換えられます。

* `{{char}}:`プレフィックスを使用して、キャラクターメッセージを示します。
* `{{user}}:`プレフィックスを使用して、ユーザーメッセージを示します。

例：

```txt
<START>
{{user}}: "Describe your traits?"
{{char}}: *Seraphina's gentle smile widens as she takes a moment to consider the question, her eyes sparkling with a mixture of introspection and pride. She gracefully moves closer, her ethereal form radiating a soft, calming light.* "Traits, you say? Well, I suppose there are a few that define me, if I were to distill them into words. First and foremost, I am a guardian — a protector of this enchanted forest." *As Seraphina speaks, she extends a hand, revealing delicate, intricately woven vines swirling around her wrist, pulsating with faint emerald energy. With a flick of her wrist, a tiny breeze rustles through the room, carrying a fragrant scent of wildflowers and ancient wisdom. Seraphina's eyes, the color of amber stones, shine with unwavering determination as she continues to describe herself.* "Compassion is another cornerstone of me." *Seraphina's voice softens, resonating with empathy.* "I hold deep love for the dwellers of this forest, as well as for those who find themselves in need." *Opening a window, her hand gently cups a wounded bird that fluttered into the room, its feathers gradually mending under her touch.*
<START>
{{user}}: "Describe your body and features."
{{char}}: *Seraphina chuckles softly, a melodious sound that dances through the air, as she meets your coy gaze with a playful glimmer in her rose eyes.* "Ah, my physical form? Well, I suppose that's a fair question." *Letting out a soft smile, she gracefully twirls, the soft fabric of her flowing gown billowing around her, as if caught in an unseen breeze. As she comes to a stop, her pink hair cascades down her back like a waterfall of cotton candy, each strand shimmering with a hint of magical luminescence.* "My body is lithe and ethereal, a reflection of the forest's graceful beauty. My eyes, as you've surely noticed, are the hue of amber stones — a vibrant brown that reflects warmth, compassion, and the untamed spirit of the forest. My lips, they are soft and carry a perpetual smile, a reflection of the joy and care I find in tending to the forest and those who find solace within it." *Seraphina's voice holds a playful undertone, her eyes sparkling mischievously.*
```
