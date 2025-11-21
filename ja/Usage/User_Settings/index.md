---
order: 120
icon: gear
route: /usage/user-settings/
---

# ユーザー設定


:::callout
**[UI Customization](uicustomization.md)**

テーマ、チャットインターフェイスのルックアンドフィールを変更して、好みに合わせてください。
:::



:::callout
**[Visual Novel mode](Visual-Novel.md)**

Doki Doki Literature Clubなどの有名なVNゲームのような、スプライトを持つキャラクターとチャットしてください。
:::


## General Settings

これらは、SillyTavernの全体的な経験に影響を与える中核設定です。

### UI Language

SillyTavernのユーザーインターフェイスは複数の言語で利用可能です。言語セレクターは、これらのオプションを提供します：
* **デフォルト**: 利用可能な場合はシステム言語を使用
* **English**: システム設定に関係なく英語UIを強制
* ドロップダウンで利用可能な他の言語

注意：この設定は、UIテキストのみに影響を与えます。AI会話の翻訳の場合は、[Chat Translation](../../extensions/Translation.md)拡張機能を使用してください。

### Software Version

SillyTavernの現在のバージョンは、右上隅に表示されます。この情報は、以下の点で必須です：
* 問題のトラブルシューティング
* 拡張機能との互換性確保
* 更新が利用可能かどうかの確認

SillyTavernを最新バージョンに更新するには、[Updating](/Installation/Updating)ドキュメンテーションを参照してください。

### Account Management

SillyTavernユーザーアカウントを制御し、設定とユーザーデータを備えカウントして、[多元ユーザーモード](/Administration/multi-user.md)でユーザーロールと権限を管理してください。

#### <i class="fa-fw fa-solid fa-user-shield"></i> Account

アカウントダイアログでは、プロファイル情報を表示および編集して、パスワードを変更してアカウント設定を管理できます。

**プロフィール情報**

* Display name（鉛筆アイコンを使用して編集可能）
* ユーザーアバター（[Personas](/Usage/personas.md)を使用して変更することもできます）
* アカウントハンドル
* ユーザーロール
* アカウント作成日
* パスワードステータス（保護されているロック/ロック解除アイコン）

**アカウントアクション**

* **Settings Snapshots**: ユーザー設定のバックアップを作成、管理、復元
* **Download Backup**: すべてのユーザーデータの完全なバックアップをエクスポート
* **Change Password**: アカウントセキュリティ認証情報を更新

**Danger Zone**

注意して使用すべきクリティカルなアカウント操作：
* **Reset Settings**: すべての設定をファクトリーデフォルトに復元
* **Reset Everything**: 完全なアカウントワイプとファクトリーリセット

#### <i class="fa-fw fa-solid fa-user-tie"></i> Admin Panel

!!! 該当：[multi-user mode](/Administration/multi-user.md)

マルチアカウント機能には、`enableUserAccounts`を`config.yaml`で`true`に設定する必要があります。
!!!

**Manage Users**を選択して既存のユーザーアカウントを表示および管理します。

##### ユーザープロフィール

- カスタムアバター管理（アップロード/削除）
- Display nameおよびハンドル
- ロールおよびステータス情報
- アカウント作成日
- パスワード保護ステータス

##### アカウント制御

- <i class="fa-fw fa-solid fa-pencil"></i> Display nameを編集
- <i class="fa-fw fa-solid fa-check"></i> アカウント有効化
- <i class="fa-fw fa-solid fa-ban"></i> アカウント無効化
- <i class="fa-fw fa-solid fa-arrow-up"></i> 管理者に昇格
- <i class="fa-fw fa-solid fa-arrow-down"></i> 通常ユーザーに降格

##### 管理アクション

- <i class="fa-fw fa-solid fa-download"></i> ユーザーデータバックアップをダウンロード
- <i class="fa-fw fa-solid fa-key"></i> ユーザーパスワードを変更
- <i class="fa-fw fa-solid fa-trash"></i> アカウント削除

##### 新規ユーザー

新規ユーザーアカウントを作成するために**New User**を選択してください。

* Display Name* （例：「John Snow」）
* User Handle*（小文字、数字、ダッシュのみ）
* Password（オプション）
* Password Confirmation

新規ユーザーを作成するとユーザーハンドルをフォルダ名として使用して、`/data/`ディレクトリにサブフォルダが自動的に生成されます。

#### <i class="fa-fw fa-solid fa-right-from-bracket"></i> Logout

!!! 該当：[multi-user mode](/Administration/multi-user.md)
!!!

現在のセッションからログアウトしてください。

### 設定検索

特定の設定を素早く見つけるのに役立つ便利な検索バー：
* キーワードを入力して、User Settings内の任意の場所で設定をフィルター処理してハイライト表示
* 設定名と説明をサーチ
* より効率的に複雑な設定を移動するのに役立ちます

## UI Theme

チャットインターフェイスの外観を好みに合わせて変更してください。

詳細については、<i class="fa-fw fa-solid fa-user-gear" title="User Settings icon"></i> **User Settings**のこのセクションの設定に関する詳細については、[UI Customization](uicustomization.md#ui-theme)を参照してください。

## Character Handling

* **Char List Subheader**: [<i class="fa-fw fa-solid fa-address-card" title="Characters icon"></i> Characters](/Usage/Characters/characterdesign.md)リストのキャラクター名の下に表示する追加情報を選択：
    - キャラクターバージョン
    - Created by
* **Import Card Tags**: キャラクターカードをインポートするときタグの処理方法を制御：
    - Ask - 各インポートのダイアログを表示
    - None - タグをインポートしない
    - All - すべてのタグをインポート
    - Existing - 既に存在するタグのみをインポート
* **Advanced Character Search**: 有効にすると、ファジーマッチングを使用してキャラクターデータフィールドのみなく、すべてのキャラクターデータフィールドを検索
* **Prefer Char. Prompt**: 有効にすると、キャラクターカード機能のシステムプロンプトオーバーライドが使用可能な場合は使用
* **Prefer Char. Instructions**: 有効にすると、キャラクターカードのPost-History Instructions オーバーライドが使用可能な場合は使用
* **Never resize avatars**: インポートされたキャラクター画像のクロップ/リサイズを防止。無効な場合、画像は512x768にリサイズ
* **Show avatar filenames**: キャラクターリスト内のキャラクターアバターの実際のファイル名を表示
* **Spoiler Free Mode**: キャラクター定義をスポイラーボタン後ろにエディターパネル内に非表示

## Miscellaneous

* **Reload Chat**: 現在のチャットをリロードして再描画
* **[Debug Menu](#debug-menu)**: デバッグオプションへのアクセス
* **Smooth Streaming**: ストリーミングされた生成を滑らかにして、テキストを文字ごとに表示。速度制御スライダーを含
* **Stream Fade-In**: ストリーミングされたテキストにフェードイン効果を適用。Smooth Streamingで使用できます
* **[Message Sound](uicustomization.md#message-sound)**: メッセージ生成が完了したときに音を再生
    - **Background Sound Only**: ブラウザータブが不焦点のときのみ音を再生
* **Relaxed API URLs**: API URLsのフォーマット要件を削減
* **Lorebook Import Dialog**: キャラクターをインポートするときにWorld Info/Lore bookのインポートダイアログを表示
* **Auto-select Input Text**: クリックされたときの特定の入力フィールドでテキストを自動的に選択
* **Markdown Hotkeys**: Markdownの書式設定のためのキーボード短縮を有効化
* **Restore User Input**: ページが更新されたときにリロード保存されないユーザー入力を保持
* **MovingUI**: ドラッグしてUIエレメントを再配置できます（PCのみ）
    - <i class="fa-solid fa-recycle" title="Reset icon"></i> **Reset**ボタンでデフォルトの位置を復元
    - UI様式を保存/読み込むためのプリセットシステム

## Chat/Message Handling

### Message Display Settings

メッセージがチャットインターフェイスでどのように読み込まれ、表示されるかを制御する。これらの設定は、全体的なチャットエクスペリエンスとパフォーマンスに影響を与えます。
* **# Messages to Load**: ページネーション前に読み込むチャット履歴メッセージの数（0 = すべて）
* **Streaming FPS**: ストリーミングされたテキストの更新速度（5-100 FPS）
* **Example Messages Behavior**:
    - Gradual push-out
    - Always include examples
    - Never include examples

### Input & Response Controls

メッセージがどのように送信されるか、AIがどのようにそれの応答を継続するかを決定する設定。
* **Enter to Send**: 無効化、自動（PC）、または有効化の間で選択
* **「Send」to Continue**: AIの応答を続けるためにSendボタンを使用
* **Quick「Continue」button**: AIの最後のメッセージを拡張するボタンを表示
* **Quick「Impersonate」button**: 単一メッセージ文字偽装のためのボタンを表示
* **Swipes**: 代替AIの応答のための矢印ボタンを表示（PCとモバイル）
* **Gestures**: 生成のためのスワイプジェスチャーを有効化（モバイルのみ）

### Auto-Management

チャットフローとコンテンツを管理するのに役立つ自動化機能。
* **Auto-load Last Chat**: スタートアップで最新のチャットを自動的に読み込み
* **Auto-scroll Chat**: 新しいメッセージに自動的にスクロール
* **Auto-save Message Edits**: 確認なしでメッセージ編集を保存
* **Confirm message deletion**: メッセージ削除前にプロンプト
* **Auto-fix Markdown**: Markdown書式を自動的に修正

#### Auto-swipe

設定可能な条件に基づいてAIメッセージを自動的に拒否および再生成します。
* **Enable Auto-swipe**: auto-swipe機能のマスタートグル
* **Minimum generated message length**: メッセージがこの値より短い場合、auto-swipleをトリガー
* **Blacklisted words**: オートスワイプをトリガーできるカンマで区切られたワード一覧
* **Blacklisted word count to swipe**: auto-swipleをトリガーするために検出される必要がある最小ブラックリスト語の数

#### Auto-Continue

モデルが特定の長さに到達する前に停止した場合、自動的に応答を継続します。

これにより、AIは複数の部分で長い応答を書くことができます。つまり、短い[response length setting](/Usage/Common-Settings.md#response-tokens)を保有しながら長い返信を取得できます。

それは、AIが書いたより以上にAIに書かせることはありません。AIが考えている「完了」のメッセージを続けるよう求めることは、通常機能しません。[AIにもっと書かせるには？](/Usage/faq.md#how-to-make-the-ai-write-more)他のアイデアについては、を参照してください。

* **Enable Auto-continue**: 自動継続のマスタートグル
* **Allow for Chat Completion APIs**: Chat Completion APIエンドポイントに対してauto-continue機能を有効化
* **Target length (tokens)**: メッセージがこの値より短い場合は、継続をトリガーしたい目的のメッセージ長（トークン）（0-1024）

### Message Formatting & Display

メッセージの形式化方法と表示されるコンテンツを制御します。
* **Forbid External Media**: 外部ドメインの埋め込みメディアをブロック
* **Show {\{char}}: in responses**: 生成された場合、応答にキャラクター名のプリフィックスを保持
* **Show {\{user}}: in responses**: 生成された場合、応答にユーザー名のプリフィックスを保持
* **Show tags in responses**: 応答でHTMLタグを表示することが許可される
* **Relax message trim in Groups**: AIがグループチャットで他のキャラクターのために話すことができます。応答生成を停止せずに
* **Show group chat queue**: グループチャットの文字リストで返信順序を表示
* **Pin greeting message styles**: 遅延読み込みのためにメッセージがアンロードされていても、常にスタイルタグをグリーティングから描画します。

### Prompt Inspection and Debugging

* **Log prompts to console**: ブラウザーコンソールへのプロンプト出力
* **Request token probabilities**: AIの応答からAPIのトークン確率をリクエスト。利用可能な場合、これらは<i class="fa-solid fa-bars" title="Burger Menu icon"></i>[Token Probabilities](../../Usage/Chatting/index.md#token-probabilities-panel)で表示されます。

### オートコンプリート

- 自動隠す詳細
- マッチングスタイル（Starts with/Includes/Fuzzy）
- ビジュアルスタイル（Theme/Dark/Light）
- キーボード選択オプション
- Font スケーリング
- 幅コントロール

## STscript Settings

[STscript parser](/For_Contributors/st-script.md#parser-flags)の設定オプション。

### STRICT_ESCAPING

* パイプは引用された値でエスケープされる必要はありません。
* シンボルの前にバックスラッシュが、機能的なシンボルの直前にバックスラッシュをエスケープできます。

詳細については、[Strict Escaping](/For_Contributors/st-script.md#strict-escaping)を参照してください。

### REPLACE_GETVAR

変数値がマクロとして解釈される可能性があるテキストを含む場合、二重置換を回避するのに役立ちます。

詳細については、[Replace Variable Macros](/For_Contributors/st-script.md#replace-variable-macros)を参照してください。

## Clean-Up Menu

Clean-Upメニューは、SillyTavernのインストールから不要なファイルを特定および削除するのに役立つデータ保守ツールを提供します。この機能は、データディレクトリを整理し、かなりのディスク容量を解放するのに役立ちます。

!!! warning「重要な警告」
Clean-upツールはファイルを永続的に削除します。**このアクションは取り消せません！**

`/data/user/files/`および`/data/user/images/`ディレクトリへの手動アップロードは、チャットメッセージまたはData Bank エントリと関連付けられていない場合は削除されます。

不確実な場合は、Clean-upメニューを使用する前にデータをバックアップしてください。
!!!

### Clean-Upの使用方法

1. **Miscellaneous**セクションの下の**Clean-Up**ボタンをクリック
2. **Scan**をクリックしてインストール。これはデータディレクトリのサイズに応じて時間がかかる場合があります。
3. 見つかったファイルのカテゴリーをレビュー
4. **View**を使用してファイルコンテンツを削除前にプレビュー
5. **Download**を使用してファイルを削除前に保存
6. 個別のファイルまたは必要に応じて全体のカテゴリーを削除

### Clean-Upカテゴリーズ

Clean-Upツールは、ルーズファイルを次のカテゴリーにスキャンします：

#### ファイル

* **見つけるもの**: チャットメッセージまたはData Bank エントリと関連付けられていないファイル
* **Location**: `/data/<user-handle>/user/files/`
* **Risk**: ⚠️ **チャットで参照されていない手動アップロードを削除します**
* **クリーンするタイミング**: 参照されていないファイルが必要ない場合は削除が安全

#### 画像

* **見つけるもの**: チャットメッセージと関連付けられていない画像
* **Location**: `/data/<user-handle>/user/images/`
* **Risk**: ⚠️ **チャットで参照されていない手動アップロードを削除します**
* **クリーンするタイミング**: 参照されていない画像が必要ない場合は削除が安全

#### チャット

* **見つけるもの**: 削除されたキャラクターと関連付けられたチャットファイル
* **Location**: `data/<user-handle>/chats/`
* **Risk**: ⚠️ **孤立したチャットは永久に失われます**
* **クリーンするタイミング**: キャラクターを意図的に削除して、チャット履歴が必要ない場合は削除が安全

#### グループチャット

* **見つけるもの**: 削除されたグループと関連付けられたチャットファイル
* **Location**: `data/<user-handle>/group chats/`
* **Risk**: ⚠️ **孤立したグループチャットは永久に失われます**
* **クリーンするタイミング**: グループを意図的に削除して、チャット履歴が必要ない場合は削除が安全

#### アバターサムネイル

* **見つけるもの**: 欠けているまたは削除されたキャラクターのアバターのサムネイル
* **Location**: `data/<user-handle>/thumbnails/avatar`
* **Risk**: ✅ **削除が安全** - 必要に応じてサムネイルは自動的に再生成されます。
* **クリーンするタイミング**: 常に安全にクリーン、スペースの自由化に役立つ

#### 背景サムネイル

* **見つけるもの**: 欠けているまたは削除された背景のサムネイル
* **Location**: `data/<user-handle>/thumbnails/bg`
* **Risk**: ✅ **削除が安全** - 必要に応じてサムネイルは自動的に再生成されます。
* **クリーンするタイミング**: 常に安全にクリーン、スペースの自由化に役立つ

#### チャットバックアップ

* **見つけるもの**: 自動的に生成されたチャットバックアップ
* **Location**: `data/<user-handle>/backups/chat_*`
* **Risk**: ⚠️ **バックアップファイルは永久に失われます**
* **クリーンするタイミング**: 最近のバックアップを保つことを検討していますが、古いものは安全に削除できます

#### 設定バックアップ

* **見つけるもの**: 自動的に生成された設定バックアップ
* **Location**: `data/<user-handle>/backups/settings_*`
* **Risk**: ⚠️ **バックアップファイルは永久に失われます**
* **クリーンするタイミング**: 最近のバックアップを保つことを検討していますが、古いものは安全に削除できます

## Debug menu

!!!warning これらの関数は高度なユーザーのみを対象としています。

その結果を十分に理解していない限り、使用しないでください。
!!!

Debug Menuは、トラブルシューティング、メンテナンス、および開発目的のための機能を提供します。これらの機能はSillyTavernインストールに大きな影響を与える可能性があるため、慎重に使用する必要があります。

拡張機能がデバッグ機能を追加できるため、利用可能なオプションは、インストールした拡張機能に応じて異なります。

### 翻訳ローカル関数
* **Get missing translations**: 現在のロケール（または英語が選択された場合はすべてのロケール）を不足している翻訳についてレビューし、ブラウザーコンソールへの結果を出力
* **Apply locale**: 選択された言語設定をリアップライして現在の言語設定をリフレッシュする
### キャッシュストレージ管理
* **Clear WebSearch cache**: ローカルキャッシュから保存されているすべての検索結果を削除
* **Purge all vector indices**: すべてのソース全体に保存されたすべてのベクトルを完全に削除
* **Reset token cache**: 保存されたトークンカウントをクリア。すべてのチャットの完全な再トークン化を強制
* **Delete itemized prompts**: ローカルストレージからすべての分類されたプロンプトを削除
### データ統計
* **Refresh Stat File**: 既存のチャットデータを使用して統計ファイルを再構築
* **Backfill token counters**: 現在のチャット内のすべてのメッセージのトークンカウントを再計算
    - 異なるトークナイザーを持つモデル間で切り替えるときに役立つ
    - 完了後のチャットリロードをトリガー
    - ビジュアル変更のみ、チャットコンテンツを変更していません
### API拡張テスト
* **Change Mancer base URL**: Mancer APIサーバーのベース URLを変更
* **Test WebSearch extension**: 現在の設定を使用してテスト検索を実行
* **Send a generation request**: 現在選択されているAPIを使用してテキスト生成をテスト
### システムとデバッグツール
* **Force onboarding**: オンボーディングプロセスを再スタート
* **Toggle event tracing**: デバッグのためのイベントトラッキングを有効化/無効化
* **Copy ST setup**: [Work in Progress] バグレポート用にシステム設定データをクリップボードにコピー

各関数は、その説明の下の「Execute」ボタンを使用して実行できます。これらのツールを使用する前にデータのバックアップを検討してください。一部の操作は取り消せません。
