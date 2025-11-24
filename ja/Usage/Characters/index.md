---
order: 100
icon: person-fill
route: /usage/characters/
---

# キャラクター

キャラクターは、AI と会話でのロールを形作るために作成および管理できる AI アイデンティティです。各キャラクターには、名前、個性、会話履歴があります。必要なだけ多くのキャラクターを作成し、いつでもそれらを切り替えることができます。

キャラクターはソロ チャットで使用することも、複数のキャラクターをグループ チャットに追加して相互に作用させることもできます。

## キャラクター管理パネル

ナビゲーション バーから <i class="fa-solid fa-address-card"></i> **Characters** パネルを開いて、キャラクター リストにアクセスします。キャラクターまたはグループをクリックしてチャットまたは編集したり、<i class="fa-solid fa-user-plus"></i> **Create New Character** を選択して新しいキャラクターを追加したりします。

### パネルコントロール

* <i class="fa-solid fa-lock"></i> **Pin Panel**: インタラクション中にパネルを開いたままにする
* <i class="fa-solid fa-list-ul"></i> **Character List**: キャラクター リスト ビューに戻る
* **HotSwap Bar**: お気に入りのキャラクターにすばやくアクセス

### キャラクターリスト

* <i class="fa-solid fa-user-plus"></i> **Create New Character**: 新しいキャラクターを追加
* <i class="fa-solid fa-file-import"></i> **Import Character**: ファイルからキャラクターを読み込む
* <i class="fa-solid fa-cloud-arrow-down"></i> **External Import**: URL からインポート
* <i class="fa-solid fa-users-gear"></i> **Create Group**: 新しいグループ チャットを開始

#### 検索と並べ替え

* **Search Bar**: 名前または属性でキャラクターをフィルター
* **Sort Dropdown**: 複数のソート オプション：
    - Alphabetical (A-Z, Z-A)
    - Chronological (Newest, Oldest)
    - Usage-based (Recent, Most/Least chats)
    - Size-based (Most/Least tokens)
    - Special (Favorites, Random)

#### タイプまたはタグでキャラクターをフィルタリング

* <i class="fa-solid fa-star"></i> **Favorites Filter**: お気に入りのキャラクターを表示
* <i class="fa-solid fa-users"></i> **Groups Filter**: グループ チャットのみを表示
* <i class="fa-solid fa-folder-plus"></i> **Tags as Folders**: タグ階層で整理
* <i class="fa-solid fa-gear"></i> **Manage Tags**: [Tag configuration](/Usage/Characters/Tags.md)
* <i class="fa-solid fa-tags"></i> **Tag List**: 利用可能なすべてのタグを表示
* <i class="fa-solid fa-filter-circle-xmark"></i> **Clear Filters**: すべてのフィルターをリセット

### キャラクター作成/編集パネル

* **Avatar Image**: キャラクター プロフィール画像をアップロードしてプレビュー
* **Token Count**: キャラクターの [Token usage](characterdesign.md#character-tokens)
* <i class="fa-solid fa-ranking-star"></i> **Stats**: チャット履歴と使用統計
* [Tag management](/Usage/Characters/Tags.md)

#### クイックアクション

- <i class="fa-solid fa-star"></i> お気に入りトグル
- <i class="fa-solid fa-book"></i> 高度な定義
- <i class="fa-solid fa-globe"></i> キャラクター ロア
- <i class="fa-solid fa-passport"></i> チャット ロア：チャットを [World Info](/Usage/worldinfo.md) にリンク
- <i class="fa-solid fa-file-export"></i> キャラクターをエクスポート
- <i class="fa-solid fa-clone"></i> 複製
- <i class="fa-solid fa-skull"></i> 削除

#### 拡張オプション

* World Info linking
* Card lore import
* Scenario override
* Persona conversion
* Character rename
* Source linking
* Replace/Update
* Tag import
* Gallery view

#### コンテンツフィールド

* **[Character Description](characterdesign.md#character-description)**: キャラクターの簡潔な説明
* **[First Message](characterdesign.md#first-message)**: チャットを開始する際の初期グリーティングまたはプロンプト
* **Alternative greetings**: チャットを開始するときにスワイプできる複数の最初のメッセージを定義

### Advanced Definitions パネル

<i class="fa-solid fa-book"></i> **Advanced Definitions** ボタンをクリックして、拡張キャラクター設定にアクセスします。

#### Prompt Overrides (Chat Completion/Instruct Mode)

* **Main Prompt**: デフォルト [main/system prompt](/Usage/Prompts/index.md#main-prompt-system-prompt) を置き換えます。\{\{original\}\} プレースホルダーを使用して元のプロンプトを含めることができます
* **Post-History Instructions**: デフォルト [post-history instructions](/Usage/Prompts/index.md#post-history-instructions) をオーバーライド

#### Creator's Metadata

キャラクターに関する非プロンプト情報：

- Creator name/contact
- キャラクター版
- Creator's notes
- Embedded tags list

#### Character Personality

* **[Personality Summary](characterdesign.md#personality-summary)**: キャラクターの特性の簡潔な概要
* **[Scenario](characterdesign.md#scenario)**: ダイアログのコンテキストと状況
* **Character's Note**: カスタマイズ可能な深さとメッセージ ロール（[Author's Note](/Usage/Characters/Author's-Note.md) も参照）
* **Talkativeness** (Group Chats): Shy → Normal → Chatty のスライダー
* **Example Messages**: キャラクターの執筆スタイルの例

### グループチャット管理

これがグループ チャットの場合は、このパネルからグループ メンバーと設定を管理できます。

詳細は [Group Chats](/Usage/Characters/groupchats.md) を参照してください。
