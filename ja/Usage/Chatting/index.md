---
icon: report
order: 170
expanded: false
route: /usage/chatting/
---

# チャット (Chatting)

[API に接続](</Usage/API_Connections/index.md>) されている場合、画面下部のチャット バーにメッセージを入力して AI にメッセージを送信します。その後、<i class="fa-solid fa-paper-plane"></i> **Send** をクリックするか、Enter キーを押します。
![Chat bar](/static/chatbox.png)

AI は会話を続けるメッセージで応答します。

![Chat message](/static/chatmessage.png)

これで以下を実行できます：

* **別のメッセージを送信**
* **応答をスワイプ**: メッセージの <i class="fa-solid fa-chevron-right"></i> **Swipe** ボタンをクリックして、別の応答を生成します。
* **メッセージを編集**: 任意のメッセージの <i class="fa-solid fa-pencil"></i> **Edit** ボタンをクリックして [メッセージ コンテンツを編集](#edit-message-content) します。
* **メッセージ アクション**: メッセージの <i class="fa-solid fa-ellipsis"></i> **Message actions** ボタンをクリックして、[翻訳](../../extensions/Translation.md)、画像生成、ストーリー分岐などのより多くの [メッセージ オプション](#message-actions-panel) にアクセスします。
* **チャット オプション**: チャット バーの横の <i class="fa-solid fa-bars"></i> **Options** ボタンをクリックして、著者の注記やチャット ファイル管理などの追加 [チャット オプション](#chat-options-panel) にアクセスします。

!!! Edit and swipe
別のことを言ったことを望む場合は、メッセージを編集してから AI の応答をスワイプして新しい応答を取得できます。
!!!

!!! Keyboard shortcuts
また、**Right** 矢印キーを使用してスワイプしたり、**Up** 矢印キーを使用してチャットの最後のメッセージを編集したりすることもできます。より多くのホットキーについては、チャットで `/help hotkeys` [スラッシュ コマンド](/Usage/Chatting/slashcommands.md) を使用するか、[HotKeys](/Usage/Chatting/hotkeys.md) ページを確認してください。
!!!

## メッセージアクションパネル (Message actions panel)

省略記号（•••）ボタンを使用して個々のチャット メッセージを管理します。

チャット内のすべてのメッセージに対してこれらのオプションを表示するには、ユーザー設定で [Expand Message Actions](/Usage/User_Settings/uicustomization.md#theme-toggles) 設定を有効にします。

### コア機能 (Core Functions)

* <i class="fa-solid fa-language"></i> **Translate**: メッセージを別の言語に変換
* <i class="fa-solid fa-paintbrush"></i> **Generate Image**: [メッセージ コンテンツから画像を作成](/extensions/Stable-Diffusion.md)
* <i class="fa-solid fa-bullhorn"></i> **Narrate**: [テキスト音声変換](/extensions/TTS.md) 変換
* <i class="fa-solid fa-square-poll-horizontal"></i> **Prompt**: 生成プロンプトとトークン使用状況を表示

### メッセージの可視性 (Message Visibility)

* <i class="fa-solid fa-eye"></i> **Included**: AI はこのメッセージを見る。クリックして除外
* <i class="fa-solid fa-eye-slash"></i> **Excluded**: AI はこのメッセージを見ない。クリックして含める

### コンテンツ管理 (Content Management)

* <i class="fa-solid fa-paperclip"></i> **Embed**: [ファイルまたは画像を添付](/Usage/Characters/data-bank.md#about-documents)
* <i class="fa-solid fa-flag-checkered"></i> **Checkpoint**: ストーリー チェックポイントを作成
* <i class="fa-solid fa-flag"></i> **Checkpoint Navigation**: クリックしてチェックポイント チャットを開く、Shift+クリックで既存のチェックポイントを更新
* <i class="fa-solid fa-code-branch"></i> **Branch**: 別のストーリー パスを開始
* <i class="fa-solid fa-copy"></i> **Copy**: メッセージ テキストをコピー
* <i class="fa-solid fa-pencil"></i> **Edit**: メッセージ コンテンツを編集

## メッセージコンテンツの編集 (Edit message content)

チャット メッセージを <i class="fa-solid fa-pencil"></i> **Edit** するときに表示されるメッセージ操作ツールのコンパクト パネル。

### コアアクション (Core Actions)

* <i class="fa-solid fa-check"></i> **Confirm**: メッセージの変更を保存
* <i class="fa-solid fa-xmark"></i> **Cancel**: メッセージの変更を破棄

### メッセージ操作 (Message Operations)

* <i class="fa-solid fa-copy"></i> **Copy**: メッセージ コンテンツを複製
* <i class="fa-solid fa-trash-can"></i> **Delete**: メッセージを削除

### メッセージ位置 (Message Position)

* <i class="fa-solid fa-chevron-up"></i> **Move Up**: メッセージをチャットの上方にシフト
* <i class="fa-solid fa-chevron-down"></i> **Move Down**: メッセージをチャットの下方にシフト

注：メッセージの位置に基づいて、移動コントロールが無効になる場合があります。

## チャットオプションパネル (Chat options panel)

チャット インターフェイスの左下の <i class="fa-solid fa-bars"></i> **Options** ボタンを使用してチャット設定と操作を管理します。

### 表示コントロール (Display Controls)

* <i class="fa-lg fa-solid fa-times"></i> **Close chat**: 現在のチャット セッションを終了
* <i class="fa-lg fa-solid fa-cog"></i> **Toggle Panels**: [インターフェイス パネル](/Usage/index.md#control-panels) を表示/非表示

### 生成設定 (Generation Settings)

* <i class="fa-lg fa-solid fa-note-sticky"></i> **[Author's Note](/Usage/Characters/Author's-Note.md)**: カスタム コンテキスト インストラクション
* <i class="fa-lg fa-solid fa-scale-balanced"></i> **[CFG Scale](/Usage/Prompts/CFG.md)**: 応答の創意性を調整
* <i class="fa-lg fa-solid fa-pie-chart"></i> **[Token Probabilities](#token-probabilities-panel)**: トークン生成統計を表示

### チャットナビゲーション (Chat Navigation)

* <i class="fa-lg fa-solid fa-left-long"></i> **Back to parent chat**: メイン会話に戻る
* <i class="fa-lg fa-solid fa-flag"></i> **Save checkpoint**: ストーリー チェックポイントを作成
* <i class="fa-lg fa-solid fa-people-arrows"></i> **Convert to group**: [グループ チャット](/Usage/Characters/groupchats.md) に変換

### チャット管理 (Chat Management)

* <i class="fa-lg fa-solid fa-comments"></i> **Start new chat**: 新しい会話を開始
* <i class="fa-lg fa-solid fa-address-book"></i> **Manage chat files**: インポート、エクスポート、名前変更などの [チャット ファイル操作](/Usage/Characters/chatfilemanagement.md)

### メッセージコントロール (Message Controls)

* <i class="fa-lg fa-solid fa-trash-can"></i> **Delete messages**: 複数のメッセージを選択して削除
* <i class="fa-lg fa-solid fa-repeat"></i> **Regenerate**: 新しい応答を作成
* <i class="fa-lg fa-solid fa-user-secret"></i> **Impersonate**: AI はメッセージをユーザーとして書く
* <i class="fa-lg fa-solid fa-arrow-right"></i> **Continue**: 最後のメッセージを拡張

注：一部のオプションはコンテキストとチャット状態に応じて非表示になる場合があります。

## トークン確率パネル (Token Probabilities Panel)

Token Probabilities パネルを使用すると、テキスト生成の AI のサンプリング プロセスを詳しく調べることができます。AI が書いたものだけでなく、テキストの各ポイントで検討した他のオプションも表示されます。

これを開くには、<i class="fa-solid fa-bars" title="Burger Menu icon"></i> **Chat Options** パネルの <i class="fa-solid fa-pie-chart"></i> **Token Probabilities** ボタンをクリックします。

![Example message](/static/token-probs/fling-msg.png){ width=500}

![Token probabilities display for example message](/static/token-probs/fling-probs.png){ width=500}

生成されたテキストの任意のトークン（単語、句読点、または形式化文字）をクリックすると、パネルにはその位置で AI が検討した代替トークンとその確率スコアが表示されます。これにより、AI の「思考プロセス」に洞察が得られ、応答が進んだ可能性のある他の方向が示されます。これらの代替案を見ると、いくつかの可能性のあるオプションがあったか、単一の明確な選択肢があったかどうかを理解するのに役立ちます。

![Alternative tokens and probabilities](/static/token-probs/fling-probs-logprob.png){ width=500}

AI が異なる選択をすべき だと思うトークンが表示される場合は、代替を選択すると、メッセージがそのポイントから再生成され、別の応答が得られる可能性があります。

### 再ロール (Rerolling)

特定のトークンを変更して応答を再生成すると、変更されたトークンの前の新しい応答の部分は元の応答と同じになります。この部分は灰色で表示されます。生成されなかったため、この部分の確率情報はありません。

代替トークンに基づいて生成された他の応答を見たい場合があります。

灰色の部分をクリックして「再ロール」して、テキストの新しいバリエーションを生成できます。灰色の部分のいずれかをクリックすると、灰色の部分全体が保持され、白/着色部分全体が再生成されます。

灰色の部分でトークンをクリックする際に Ctrl キーを押したまま にすると、クリックされたトークンまでの灰色部分が保持され、テキストの残りが再生成されます。代替トークンの選択はこの場合保持できません。

### コントロール (Controls)

**Token Display**:

* 生成されたテキストは個々のトークンに分割されます
* 各トークンはインタラクティブであり、トークンをクリックして AI が検討した代替案を表示します
* トークンは視覚的な補助として着色されていますが、これは確率を示しません
* 特殊文字（スペース、改行）は目に見えるようにマークされています

**Token Selection**:

* トークンをクリックして代替案を表示
* 代替をクリックしてトークンを置き換え、応答を再生成
* トークンにマウスを置いて、その生ログ確率スコアを表示

**Window Controls**:

* <i class="fa-solid fa-grip"></i> パネル再配置用ドラッグ ハンドル（MovingUI のみ）
* <i class="fa-solid fa-window-maximize"></i> パネル サイズを最大化/復元
* <i class="fa-solid fa-circle-chevron-up"></i> パネル コンテンツを展開/折りたたむ
* <i class="fa-solid fa-circle-xmark"></i> パネルを閉じる

### 可用性 (Availability)

[ユーザー設定](/Usage/User_Settings/index.md#chatmessage-handling) で **Request token probabilities** を選択して、この機能を有効にする必要があります。

トークン確率は最新のメッセージでのみ利用可能で、チャットに保存されません。メッセージのトークン確率情報が利用可能でなくなった場合、パネルはこれを示すメッセージを表示します。

Smooth Streaming を使用している場合、トークン確率は利用できません。

トークン確率は、すべての API で利用可能ではありません。トークン確率をサポートしていない API を使用している場合、パネルは開きますが、情報は表示されません。

#### Text Completion
* **LlamaCPP**: 利用可能
* **Text Generation WebUI** (oobabooga): 利用可能
* **TabbyAPI**: 利用可能
* **NovelAI**: 利用可能
* **KoboldCPP**: 利用可能
* **Ollama**: 利用不可のようです
* **OpenRouter Text**: 利用不可のようです

#### Chat Completion
* **OpenAI** or **Custom**: 利用可能ですが、再ロールはサポートされていません
* **Anthropic**: 利用不可のようです
* **Google AI Studio**: 利用不可のようです
* **OpenRouter Chat**: 利用不可のようです
