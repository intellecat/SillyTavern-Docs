---
order: 50
templating: false
route: /usage/prompts/prompt-manager/
---

# プロンプトマネージャー

Prompt Manager は Chat Completion APIs の [プロンプト構築](index.md) 戦略を より制御できるシステムです。

!!! Applies to: Chat Completion APIs
Text Completion APIs で同等の設定については、[Advanced Formatting](advancedformatting.md) を使用してください。
!!!

!!!tip Naming Presets
プリセットがキャラクター カードのいずれかと同じ名前を共有する場合、そのキャラクターとのチャットを開始するときに自動的に選択されます。この動作を回避するため、プリセットに一意の名前を付けます。
!!!

Prompt Manager にアクセスするには、ナビゲーション バーの「AI Response Configuration」ボタンをクリックします。Prompt Manager は [一般的な設定](/Usage/Common-Settings.md) パネルの下にあります。

## Quick Prompts Edit

**Main Prompt**、**Auxiliary Prompt**、**Post-History Instructions** などの一般的なプロンプト セクションをすばやく編集するスペースを提供します。これらのプロンプトの詳細については、[プロンプト構築](index.md) ページをご覧ください。

## Utility Prompts

これらのプロンプトは Chat Completion モデルに送信され、送信されている情報を理解するのに役立てるか、特定の種類の相互作用中に特定の方法で行動するよう指示します。

### Format Templates

!!!tip
形式テンプレートが設定されていない場合、情報は何もラップされずに送信されます。
!!!

これらは [World Info](/Usage/worldinfo.md) および [Character Cards](/Usage/Characters/characterdesign.md) から取得された情報をラップするために使用される文字列テンプレートです。

情報が挿入される場所を示すために特別なマーカーが使用されます：

- `{0}` （World Info 形式テンプレートの場合。
- `{{scenario}}` （Scenario 形式テンプレートの場合。
- `{{personality}}` （Personality 形式テンプレートの場合。

### Group Nudge Prompt Template

グループ チャットでのみ使用されます。特定のキャラクターからの返信を強制するためにプロンプトの最後に配置されます。

グループ Nudge 機能を無効にするには、これを空のままにします。

### New Chat, New Group Chat, New Example Chat

これらはチャット履歴の前および各 [Example Dialogue](/Usage/Characters/characterdesign.md#examples-of-dialogue) ブロックの前に送信され、背景情報が終了し、チャット履歴が開始される場所をモデルに通知します。

- **New Chat:** 個人チャットに使用されます。
- **New Group Chat:** グループ チャットに使用されます。
- **New Example Chat:** ダイアログ ブロックの例に使用されます。

この機能を無効にするには、これらを空のままにします。

### Continue Nudge

Continue がトリガーされた場合（Continue ボタンが押されたなど）、またはSTScript によってトリガーされた場合に何をするかをモデルに指示するためにプロンプトの最後に送信されます。

!!! Chat Completion 'Continues'
Chat Completion モデルは Text Completion モデルとは異なる方法で Continues を処理し、関係なく常にシームレスな結果が得られるとは限らないことに注意してください。
!!!

### Replace Empty Message

テキスト ボックスが空で **Send a message** が押されたときに代わりに送信されるフィールドの内容を送信します。

## Character Names Behavior

モデルにどのようにメッセージをキャラクターに関連付けるかについて指示するためのさまざまな戦略を提供します。Chat Completion モデルがメッセージがどのキャラクターに属しているかを判断するのに問題がある場合、別の戦略を選択する必要がある場合があります。

## Continue Postfix

Continue がトリガーされると、モデルによって返される「継続された」メッセージの先頭に、選択された Continue Postfix が前に追加されます。例えば、継続されたテキストの前にスペースを追加できます。

## Additional Settings

### Wrap in Quotes

!!!warning
非推奨オプション。代わりに [Regex scripts](/extensions/Regex.md) を使用することをお勧めします。
!!!

送信する前に隠れた引用符で全ユーザー メッセージをラップします。これは、キャラクターが音声を示すために引用符を使用しないセッション用です。セッションが音声を示すために引用符を使用する場合は、これをチェックしていない状態のままにします。

### Continue Prefill

!!!warning
Chat Completion ソースのすべてで機能しない可能性があります。
!!!

Continue Nudge をシステム メッセージではなくアシスタント ロール メッセージとして送信します。これが有効な場合、Continue Nudge プロンプトは使用されません。

### Squash system messages

!!!warning
非推奨オプション。代わりに [Prompt Post-Processing](/Usage/API_Connections/openai.md#prompt-post-processing) を使用することをお勧めします。
!!!

連続したシステム メッセージを 1 つの統合メッセージに統合します（Example Dialogue を除く）。

### Enable web search

!!!
[Web Search 拡張機能](/extensions/WebSearch.md) と混同しないでください。
!!!

Chat Completion バックエンドによって提供される Web 検索機能を有効にします。プロンプトは通常、モデル プロバイダーによる検索結果で充実させられ、追加のコストが発生する可能性があります。

### Enable function calling

[Function Calling](/For_Contributors/Function-Calling.md) を参照してください

### Send inline images, Send inline videos

!!!
[Image Captioning 拡張機能](/extensions/captioning.md) と混同しないでください。
!!!

Chat Completion モデルが送信されたイメージとビデオを処理するためのマルチモーダル機能を持っている場合、これはそれをするその機能を切り替えます。プロンプトにメディアを追加するには、「Magic Wand」メニューの **Attach A File** オプションを使用します。

### Request inline images

!!!
[Image Generation 拡張機能](/extensions/Stable-Diffusion.md) と混同しないでください。
!!!

モデルが画像添付ファイルを返すことを許可します。

### Use system prompt

!!!
Google Gemini および Anthropic Claude バックエンドのみでサポートされています。

これら 2 つに対して非常に似ている設定があります。ただし、これらは技術的には個別のオプションなので、個別に構成できます。
!!!

最初の非システム ロール（User/Assistant）を持つメッセージまでのすべてのシステム メッセージをマージし、別のシステム指示フィールドとして送信します。

## Reasoning Settings

Chat Completion モデルが推論を使用する場合、これらの設定はその表示と機能に影響します。

### Request model reasoning

[Adding Reasoning: By Backend](/Usage/Prompts/reasoning.md#by-backend) を参照してください。

### Reasoning Effort

[Reasoning Effort](/Usage/Prompts/reasoning.md#reasoning-effort) を参照してください。

## "プロンプト"

Prompt Manager は Chat Completion モデルに送信されるプロンプトのバックボーンを形成します。これは何が送信され、その *順序* を制御します。

### The 'Prompts' Dropdown

現在の Chat Completion プリセットが含むすべての（デフォルト以外の）プロンプトのドロップダウン リストが含まれます。これらのプロンプトのいずれかが発信メッセージに追加されるには、ドロップダウン リストから選択してから、**Insert prompt** ボタンを押してプロンプト マネージャーに追加する必要があります。このドロップダウン リストに追加するために新しいプロンプトを作成するには、**New prompt** ボタンを押します。新しいプロンプトが書き込まれて保存されると、ドロップダウンに追加され、その後、挿入できます。

### プロンプト List

これは、Chat Completion モデルに送信される可能性がある選択されたプロンプトをリストする drag-and-drop インターフェイスです。**top** に近いプロンプトが最初に送信されます。リストの**下部**は最後に送信されます（通常、これはあなたの **Post-History Instructions** です）。

!!! 'Pinned' prompts = デフォルト プロンプト
デフォルト プロンプトは選択されたプロンプトのリストから削除できません。これには Main Prompt、World Info (before/after)、Persona Description、Character Description、Character Personality、Scenario、Enhance Definitions、Auxiliary Prompt、Chat Examples、Chat History、および Post-History Instructions が含まれます。これらが必要ない場合は、**OFF** にトグルできますが、全体を削除または削除することはできません。
!!!

## プロンプトの編集

プロンプットの **pencil ボタン** をクリックすると、**Edit インターフェイス** に移動します。ここでプロンプトを直接編集できます。

!!! 変更を保存することを忘れずに！
これらのプロンプトへの変更を Chat Completion プリセットに永続的に保存するには、**Edit インターフェイス** の右下の **Save** ボタンをクリックし、**AI Response Configuration** セクションの上部にある **Save** ボタンを使用してプリセット自体を保存する必要があります！そうしないと、Chat Completion プリセットが別のプリセットに切り替えられたときに行われた変更は失われます。
!!!

### Name

プロンプトの名前。これは Chat Completion モデルに送信されません。参照用にのみ Prompt Manager 内にあります。

### Role

プロンプトを送信するロール。System、AI Assistant、または User から選択できます。

### Triggers

このプロンプトが送信される生成タイプ。何も選択されていない場合、プロンプトはすべての生成タイプに対して送信されます。1 つ以上が選択されている場合、プロンプトはその特定の生成タイプでのみ送信されます：

- **Normal:** 通常のメッセージ生成リクエスト。
- **Continue:** Continue ボタンが押されたとき。
- **Impersonate:** 詐称ボタンが押されたとき。
- **Swipe:** スワイプによってリクエストがトリガーされたとき。
- **Regenerate:** ソロ チャットで Regenerate ボタンが押されたとき。
- **Quiet:** バックグラウンド生成リクエスト。通常、[拡張機能](/extensions/index.md) または [STscript](/For_Contributors/st-script.md) コマンドによってトリガーされます。

!!!
「Regenerate」トリガーはグループ チャットでは利用できません。異なる再生成ロジックを使用しているため：最後の応答のすべてのメッセージが削除され、選択された [Group reply strategy](/Usage/Characters/groupchats.md#reply-order-strategies) に従って「Normal」生成タイプを使用してメッセージがキューに入れられます。
!!!

### Position

Position が **Relative** に設定されている場合、このプロンプトは、他のすべてのプロンプトとともに drag-and-drop インターフェイスに配置される場所に送信されます。それが **In-Chat** に設定され、**Depth** が与えられる場合、それは代わりにチャット履歴内の選択されたロールとして送信され、drag-and-drop インターフェイスの順序を無視します。

### Depth

Position が **In-Chat** に設定されている場合、これはプロンプトがチャット履歴内でどのくらい深く送信されるかを定義します。数が多いほど、より深く送信されます。たとえば、0 の深さは最後のチャット メッセージの後に送信され、1 の深さは最後のチャット メッセージの前に送信され、2 の深さは 2 番目から最後のチャット メッセージの前に送信されます。

### Order

!!!
同じロールと深さを持つプロンプトはグループ化され、Order 値でソートされます。
順序は次のとおりです（上から下へ）： User、AI Assistant、System。
!!!

Position が **In-Chat** に設定されている場合、これはプロンプトがチャット履歴内で送信される順序を定義します。数字が低いほど、その送信時期が早くなります。

## プロンプトの構築：ヒントとコツ

効果的なプロンプトを書く方法の詳細については、SillyTavern ドキュメントの [プロンプト構築](index.md) セクションにアクセスしてください。情報は大部分が Chat Completion プリセットに適用できます。
