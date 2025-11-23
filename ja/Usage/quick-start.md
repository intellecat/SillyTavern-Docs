---
order: 190
icon: rocket
route: /usage/quick-start/
---

# クイックスタート

!!!light
困っています。SillyTavernの使用を開始する最も簡単で最速の方法を教えてください。-- *匿名*
!!!

わずか数分でSillyTavernを始められます。始めるための2つの簡単な方法があります：

* [AI Horde](#quick-start-with-ai-horde)を無料で使用できます。AI Hordeは、さまざまなAIモデルへのアクセスを提供するコミュニティ主導のAIサービスです。

* OpenAIアカウントを持っているか、登録したい場合は、[OpenAIを使用](#quick-start-with-openai)できます。

## AI Hordeでクイックスタート

1. [Installation Guide](/Installation/index.md)に従ってSillyTavernをインストールして起動します。

2. SillyTavernのオンボーディング画面でペルソナの名前を入力してください。この名前はチャットで使用されます。

   ![This is an optional caption](/static/quick-start/1_name.png)
3. トップバーのAPI接続ボタンをクリックしてください。

   ![This is an optional caption](/static/quick-start/2_api_conn.png)
4. AI Hordeのアクセスキーを入力してください。今は`0000000000`を使用するか、[AI Horde](https://aihorde.net/)から無料キーを取得できます。

   ![This is an optional caption](/static/quick-start/3_horde_key.png)
5. 使用するいくつかのAIモデルを選択してください。上部からいくつか選択するだけです。後でいつでも変更できます。

   ![This is an optional caption](/static/quick-start/4_horde_models.png)
6. API接続ウィンドウを閉じます。チャットボックスの下部にメッセージを入力してEnterキーを押します。

   ![This is an optional caption](/static/quick-start/5_msg.png)
7. あなたのAIは数刻後に応答します。[チャット](/Usage/Chatting/index.md)を続けることができます。成功！

   ![This is an optional caption](/static/quick-start/6_success.png)

## OpenAIでクイックスタート

### SillyTavernをインストール

[Installation Guide](/Installation/index.md)に従ってSillyTavernをインストールして起動します。

### OpenAIへのアクセス取得

1. OpenAIにサインアップします。
2. <https://platform.openai.com>に移動
3. 右上隅のアカウントアイコンをクリックし、View API Keysをクリック
4. 「Create new secret key」をクリックしてください。すぐにどこかにコピーしてください。**このキーを共有しないでください。これを持つ人は誰でもあなたのアカウントを使用してあなたの費用でGPTを使用できます。**

### SillyTavernを設定してAPIを使用

1. SillyTavernのトップバーで、API接続をクリックしてください。
2. APIの下で、Chat Completion（OpenAI）を選択してください。
3. Chat Completion Sourceの下で、OpenAIを選択してください。
4. 前のステップで保存したAPIキーを貼り付けてください。
5. 「Connect」ボタンをクリックしてください。有効であると言っていることを確認してください。
6. デフォルトでは、SillyTavernはGPT-4ターボを使用します。別のモデルを選択できますが、価格について自分自身を教育してください。

### セットアップをテスト

1. SillyTavernのトップバーで、Character Management（遠い右）をクリックしてください。
2. Seraphinaなどの既存のキャラクターを選択してください。
3. テキストボックスの下で、Seraphinaに何かを書き、EnterキーまたはSendボタンをクリックしてください。

すべてを正しく実行した場合、数秒後にSeraphinaが応答する必要があります。
