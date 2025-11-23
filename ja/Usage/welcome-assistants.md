---
tags: ['>=1.13.0']
icon: people
route: /usage/welcome-assistants/
---

# Welcome Page Assistants

SillyTavernの機能には、Welcome Screenが含まれており、指定された「Assistant」キャラクターであなたにグリーティングを提供できます。このスクリーンは、SillyTavernを起動するときにアクティブなチャットがない場合、またはラストチャットセッションを閉じた後に表示されます。

!!! 注意
アプリの起動時にWelcome Screenが表示されない場合は、**<i class="fa-solid fa-user-cog"></i> User Settings**パネルの「Chat/Message Handling」セクションで「Auto-Load Last Chat」オプションが無効になっていることを確認してください。このオプションが有効になっている場合、SillyTavernは Welcome Screen を表示する代わりに最後のチャットを自動的にロードします。
!!!

## Welcome Screen

チャットがアクティブでない場合、Welcome Screenは次の有用な要素を提供します：

* **SillyTavern Version：** アプリケーションのロゴと現在のバージョンを表示します。
* **クイックリンク：** 簡単なアクセス：
  * **Docs：** 公式のSillyTavernドキュメントを開きます（あなたはすでにここにいます！）。
  * **GitHub：** SillyTavern GitHub リポジトリに移動します（<https://github.com/SillyTavern/SillyTavern>）。
  * **Discord：** 公式のSillyTavern Discordサーバーへのリンクを提供します（<https://discord.gg/sillytavern>）。
* **一時チャットボタン：** デフォルトの中立的なアシスタントとの新しい一時的なチャットセッションをすばやく開始できます。このチャットは、明示的に保存しない限り、チャット履歴に保存されません。
* **Recent Chats Section：** 簡単なアクセスのための最新の会話をリストします。次のことができます：
  * このセクションを表示または非表示にします。
  * 3つ以上のチャットが利用可能な場合、リストを展開します（最大15個の最近のチャット）。

## 一時チャット

!!! 注意
技術的な制限のため、一時チャット機能は、カスタマイズされたWelcome Page Assistant を使用しません。常に追加のプロンプトやキャラクター情報なしで空のチャットを開始します。
!!!

一時チャットボタンは、チャット履歴に保存せずに新しいチャットセッションをすばやく開始することができます。これは、チャットを保存されたチャットで散らかしことなくテストまたはカジュアルな会話に役立ちます。このチャットは、閉じたり別のチャットに切り替えたりするとすぐに削除されます。

* **Save**ボタンを使用すると、一時チャットをJSONLファイルとしてエクスポートでき、その後インポートできます。
* **Load**ボタンを使用すると、以前に保存されている一時チャットファイルを復元できます。

## Welcome Page Assistantとは何ですか？

Welcome Page Assistantは、Welcome Screenに表示することを選択するキャラクターです。これにより、個人化されたグリーティング、そしてスタートからすぐに知りつくしたキャラクターとチャットを開始する簡単な方法が可能になります。

### アシスタントの設定と設定解除

キャラクターをWelcome Page Assistantとして機能させるように選択できます。

**アシスタントを設定**：

1. **Character Management**パネルに移動します（通常は右側のサイドバーの<i class="fa-solid fa-address-card"></i>アイコンを経由）。
2. リストでアシスタントとして設定したいキャラクターを見つけてください。
3. 「More...」をクリックして、ドロップダウンメニューから**「Set / Unset as Welcome Page Assistant」**を選択してください。
4. 小さなアイコン（<i class="fa-solid fa-user-graduate"></i>）がキャラクター名の横に表示され、アクティブなWelcome Page Assistantであることを示します。

**アシスタントの設定解除**：

1. Character Management パネルに移動します。
2. 現在のWelcome Page Assistantを見つけてください（<i class="fa-solid fa-user-graduate"></i>アイコンがあります）。
3. 「More...」をクリックして、**「Set / Unset as Welcome Page Assistant」**を再度選択してください。
4. キャラクターはアシスタントではなくなり、<i class="fa-solid fa-user-graduate"></i>アイコンが消えます。
5. SillyTavernはデフォルトAssistantを使用するように戻ります（下記を参照）。

### アシスタントとのやり取り

Welcome Screenが選択したアシスタントで表示されたら、チャット入力バーの下部にメッセージを入力し、Enterキーを押すか、送信ボタンをクリックします。これにより、Welcome Page Assistantとのチャットセッションが新しく開始します。

アシスタントとの以前のチャットを開くには、Recent Chatsセクションを使用するか、**<i class="fa-solid fa-bars"></i> Options**メニューからアクセス可能な**Manage chat files**ダイアログでチャットを見つけてください。

## デフォルトアシスタント

SillyTavernは、Welcome Screenと初めてやり取りすると「Assistant」という名前のデフォルトキャラクターを自動的に作成します。このキャラクターは、Welcome Page Assistantとして特定のキャラクターを設定していない場合のフォールバックオプションとして機能します。

デフォルトアシスタントには、それに附属されている特定のプロンプトはなく、自由にカスタマイズできます（例えば、名前変更、画像の追加、またはパーソナリティの設定）。

* 明示的にWelcome Page Assistantとしてキャラクターを設定していない場合、このデフォルトアシスタントが使用されます。
* 選択したアシスタントの設定を解除する場合、システムはこのデフォルトアシスタントに戻ります。
* Welcome Page Assistantとして設定していたキャラクターが削除された場合、システムはこのデフォルトアシスタントにも戻ります。

**注意：** デフォルトシステムアシスタント 同じ方法で選択したキャラクターのように「設定解除」することはできません。デフォルトアシスタントから変更するには、他のキャラクターの1つをアシスタントとして設定する必要があります。
