---
order: 80
route: /usage/core-concepts/chatfilemanagement/
---

# チャットファイル管理

このページでは、AIチャットファイルを管理する方法について説明します。

!!!info Note
これらのオプションの一部は、左下のオプションメニューから開く「Manage chat files」ダイアログで利用できます。
!!!

## ソロチャットとグループチャット

キャラクターカードを使用する最も簡単な方法は、Soloチャットです。カードをクリックするだけでチャットを開始できます。

いくつかのキャラクターカードができたら、「Create New Chat Group」ボタンを使用して、複数のキャラクターを含む[group chat](/Usage/Characters/groupchats.md)を作成することもできます。その後、キャラクターはお互いとあなたとやり取りします。

## チャットのインポート

**Character.AIからSillyTavernにチャットをインポートします。**

Character.AIのチャットとボットをインポートするには、CAI Toolsブラウザ拡張機能を使用します：[https://github.com/irsat000/CAI-Tools](https://github.com/irsat000/CAI-Tools)。

チャットをインポートできる他のプログラムやツールには、次のものがあります：

* TavernAI (original): <https://github.com/TavernAI/TavernAI>
* Text Generation WebUI (oobabooga): <https://github.com/oobabooga/text-generation-webui>
* Agnai: <https://github.com/agnaistic/agnai>
* KoboldAI Lite: <https://github.com/LostRuins/lite.koboldai.net>
* RisuAI: <https://github.com/kwaroran/RisuAI>

## .jsonl としてエクスポート

「Manage chat files」をクリックすると、チャットファイルリストの各エントリには、そのままインポートできる形式でエクスポートするボタンがあります。これを使用して、すべてのメタデータを含むチャットを共有または移行します（ただし、画像とファイルの添付ファイルは除外されます）。

プライバシーを気にする場合は、エクスポートされたJSONLファイルを検査して、共有したくないものをスクラブしてください。

## .txt としてエクスポート

「Download chat as plain text document」ボタンで、簡素化されたテキストのみのバージョンをエクスポートすることもできます。重要なメタデータが失われるため、再インポートすることはできません！

## チェックポイント

「Checkpoints」は、現在のチャットのクローンです。つまり、特定のポイントまで指定されたチャットからすべてのメッセージをコピーし、ソースへのリンク（チャットファイル名による）を保存します。

各チャットメッセージの右側にある3つのドットボタンから、チェックポイントを作成する2つの方法があります：

* 「Create Branch」は、そのメッセージまで現在のチャットをクローンし、それに切り替えます
* 「Create Checkpoint」は、そのメッセージまで現在のチャットをクローンし、名前を尋ねて作成しますが、切り替えません

これらは、ブラウザの「新しいタブでリンクを開く」と「新しいタブをバックグラウンドで開く」のようなものと考えることができます。

チェックポイントから親に戻るには、メッセージテキストボックスの左側にあるバーガーメニューボタンを入力し、「Back to parent chat」をクリックします。

## チャット名の変更

デフォルトでは、チャットファイルには開始された日時の名前が付けられます。

鉛筆アイコンをクリックして新しい名前を入力することで、これを変更できます。

これにより、チェックポイントからそのチャットへのリンクが壊れることに注意してください（チャットファイル名でリンクされているため）。
