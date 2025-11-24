---
icon: paperclip
route: /usage/core-concepts/connection-profiles/
order: 100
---

# 接続プロファイル

異なるAPI、モデル、フォーマットテンプレート間を素早く切り替えるためにConnection Profilesを保存できます。これは複数のAPI接続を頻繁に使用する場合や、メニューを行き来することなく異なる構成に切り替える必要がある場合に便利です。

## 接続プロファイルへのアクセス

この機能はSillyTavern 1.12.6以降でデフォルトで有効になっており、組み込みエクステンションとしてAPI Connectionsメニューから利用できます。*無効化*したい場合は、Extensionsパネルを開き、「Manager extensions」をクリックし、リストからConnection Profilesを見つけて「Enabled」チェックボックスのチェックを外してから「Close」をクリックしてください。

## 保存される内容

Connection Profilesには以下の選択項目が保存されます。

### 共通

* [API type, model and the server URL](/Usage/API_Connections/index.md)
* [Secret Key](/Usage/faq.md#where-are-my-api-keys-stored-why-cant-i-see-them)
* [Settings preset](/Usage/Common-Settings.md)
* [Start Reply With](/Usage/Prompts/advancedformatting.md#start-reply-with) (明示的に空にすることも可能)
* [Custom Stopping Strings](/Usage/Prompts/advancedformatting.md#custom-stopping-strings) (明示的に空にすることも可能)
* [Reasoning Formatting](/Usage/Prompts/reasoning.md#configuration)

### Text Completion API

* [System Prompt and its state](/Usage/Prompts/advancedformatting.md#system-prompt)
* [Instruct Mode state and template](/Usage/Prompts/instructmode.md)
* [Context Template](/Usage/Prompts/advancedformatting.md#context-template)
* [Tokenizer](/Usage/Prompts/advancedformatting.md#tokenizer)

### Chat Completion API

* [Prompt Post-Processing](/Usage/API_Connections/openai.md#prompt-post-processing)
* Proxy preset

## 接続プロファイルの管理

!!!info 注意
Profilesはドロップダウンフィールドの選択のみを保存し、基礎となる設定については何も知りません。つまり、別のプロファイルに切り替えることで未保存の変更が失われます。これを防ぐには、一時的な変更を失いたくない場合、すべてのプリセットとテンプレートを更新してください。
!!!

* プロファイルを保存するには、必要なすべての設定を行い、「Create」ボタンをクリックします。次に設定を確認し、プロファイルの名前を指定します。**名前は一意である必要があります。**
* 選択したプロファイルの詳細情報を表示するには、「Information」ボタンをクリックします。詳細を非表示にするには、もう一度クリックします。
* Connection Profileの設定は、「Update」ボタンを押すまで、関連するプロファイル保存ファイルを変更することなく`settings.json`に保存されます。つまり、プロファイルを設定してから更新せずに別のプロファイルに切り替えると、以前の変更がすべて失われます。
* 保存されたプロファイルから変更された選択を復元するには、「Reload」ボタンをクリックします。
* プロファイルを削除するには、「Delete」ボタンをクリックして削除を確認します。**この操作は元に戻せません。**

## スラッシュコマンド

Connection profilesは以下のslash commandsを使用して管理できます。

1. `/profile [name]` - 引数が指定されている場合はプロファイルに切り替え、指定されていない場合は現在のプロファイルの名前を取得します。
2. `/profile-create [name]` - 現在の設定を指定された名前で新しいプロファイルとして保存します。
3. `/profile-list` - 利用可能なプロファイル名のJSON形式の配列を返します。
4. `/profile-get [name]` - 指定された名前のプロファイルの詳細をJSON形式のオブジェクトとして取得します。
5. `/profile-update` - 選択したプロファイルを現在の設定で更新します。
