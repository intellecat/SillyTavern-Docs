---
route: /usage/api-connections/dreamgen/
---

# DreamGen

DreamGenはAIによるロールプレイングとストーリー執筆のためのアプリおよびAPIです。無料プランに加え、ステアラブルAIロールプレイングとストーリー執筆専用に作られた高品質な社内テキスト生成モデルへの無制限の月次アクセスを可能にする有料サブスクリプションがあります。アカウントを作成して始めましょう: <https://dreamgen.com/>。

(無料の)クレジットは毎月初めにリセットされます。各モデルのクレジットコストについては[pricing](https://dreamgen.com/pricing)を、残りのクレジットについては[usage](https://dreamgen.com/account/usage)をご覧ください。

## DreamGenへの接続

### APIキーの取得

[DreamGen API keys](https://dreamgen.com/account/api-keys)ページに移動し、「New API Key」ボタンをクリックします。APIキーがクリップボードにコピーされていることを確認してください。

![Create New DreamGen API key](/static/dreamgen/dreamgen_api_keys_new.jpg)
![Copy DreamGen API key](/static/dreamgen/dreamgen_api_keys_copy.jpg)

### 接続

1. SillyTavernの接続設定に移動します。
2. API: Text Completionを選択します。
3. API Type: DreamGenを選択します。
4. APIキーを入力します。
5. (オプション)モデルを選択します。

![Connecting to DreamGen](/static/dreamgen/dreamgen_st_connection.png)

## Models

DreamGen APIは異なるサイズのいくつかのモデルを提供しています。

- Lucid Max (APIでは`lucid-v1-max`または`lucid-v1-extra-large`と呼ばれる)
- Lucid Base (APIでは`lucid-v1-base`または`lucid-v1-medium`と呼ばれる) -- 重みが利用可能な[Lucid V1 Nemo](https://dreamgen.com/docs/models/lucid-v1/huggingface)に対応。

Lucid Baseは使用するクレジットが少なく高速ですが、Lucid Maxはより創造的で、より複雑な指示とナラティブを処理できます。

## Settings

Lucid V1 DreamGenモデルは、ロールプレイとライティングに最適化されたLlama 3チャットテンプレートの拡張を使用します。特定のsystem promptで最も効果的に機能します。

以下のマスタープリセットのいずれかから始めることを強くお勧めします:

- instruct modeが有効であることを確認し、インポート時にすべてのチェックボックスを選択してください。
- [DreamGen Lucid V1 Role-Play preset](https://dreamgen.com/docs/models/lucid-v1/sillytavern/master-preset/role-play)
- [DreamGen Lucid V1 Story preset](https://dreamgen.com/docs/models/lucid-v1/sillytavern/master-preset/story)

これらのプリセットには、モデルに指示を送信するための`/sys`の組み込みサポートが付属しています。これらを使用してプロットを操作したり、キャラクターの行動を制御したりできます。

![DreamGen preset selected](/static/dreamgen/dreamgen_st_preset.png)

その他のリソース:

- [**Detailed DreamGen + SillyTavern guide**](https://dreamgen.com/docs/models/lucid-v1/sillytavern)
- [Detailed Lucid V1 prompt format documentation](https://dreamgen.com/docs/models/lucid-v1).
- [DreamGen + SillyTavern role-play demo](https://imgur.com/a/dreamgen-lucid-sillytavern-roleplay-demo-bhzQpto)
- [DreamGen + SillyTavern story-writing demo](https://imgur.com/a/dreamgen-lucid-sillytavern-writing-demo-JLv5iO3)
- [Tips for making your own scenarios](https://v2.dreamgen.com/docs/scenario-editor)

## FAQ

### 応答を長く、または短くするにはどうすればよいですか？

フォーマットプリセットで`Last Assistant Prefix`を設定できます。

長いメッセージの場合:

```txt
<|start_header_id|>user<|end_header_id|>

The next message is from {{char}} and is at least 100 words long<|eot_id|><|start_header_id|>writer character {{char}}<|end_header_id|>

```

短いメッセージの場合:

```txt
<|start_header_id|>user<|end_header_id|>

The next message is from {{char}} and is at most 50 words long<|eot_id|><|start_header_id|>writer character {{char}}<|end_header_id|>

```

最後の2つの改行を含め、すべての改行を保持してください。

![Long Message Prefix](/static/dreamgen/dreamgen_st_long_response_prefix.png)

カードやsystem promptにライティングスタイルの説明を含めることもできます。例:

```txt
## Style

<your description>
```

詳細といくつかの例については、["Style" documentation](https://v2.dreamgen.com/docs/scenario-editor#style)をご覧ください。

### ロールプレイ/ストーリーを操作するにはどうすればよいですか？

`/sys`オプションを使用してモデルに指示を送信します。いくつかの例:

> The inkeeper offers Daria and the others a pint of ale.

> The next message is from Draco and should be at least 200 words, focusing on his inner conflict about the decision.

[See it in action.](https://imgur.com/a/dreamgen-lucid-sillytavern-roleplay-demo-bhzQpto)
