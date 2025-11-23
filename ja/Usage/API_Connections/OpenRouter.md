---
order: 10
route: /usage/api-connections/openrouter/
---

# OpenRouter

!!!info
OpenRouterはText CompletionとChat Completionの両方のソースとして利用できます。すべてのモデルはどちらのAPIからでも利用できますが、選択するAPIタイプによって機能が異なる場合があります。例えば、画像のインライン化とツール呼び出しはChat Completion APIでのみ利用できます。
!!!

多数のAPIサービスに登録したくないが、最新のすべてのモデルにアクセスしたいですか？OpenRouterを使用してください。

OpenRouterは、DeepSeek、Claude、Geminiなどのモデルに単一のエンドポイントを使用してアクセスでき、共有クレジットプールを持つ1つのサービスですべてを利用できます。

無料トライアル(約$1)と、その後の有料アクセスがあります。サブスクリプションや月額料金はありません - 実際に使用した分だけ支払います。一部のモデルには、1日あたりのリクエスト数が制限された無料アクセスがあります。

!!!tip
1日の制限が寛大な無料モデルへの永続的なアクセスを取得するには、**一度だけ**少なくとも$10のクレジットを購入する必要があります。

詳細については、[OpenRouter FAQ page](https://openrouter.ai/docs/faq)をご覧ください。
!!!

- OpenRouterアカウントを作成: [openrouter.ai](https://openrouter.ai/)
- [OpenRouter Models List](https://openrouter.ai/models?order=pricing-low-to-high)

![OpenRouter-ConnectionPanel](/static/openrouter-connection.png)

上から下へ(上の画像を参照):

1. 'Chat Completion' APIを選択します。
2. ソースとしてOpenRouterを選択します。
3. 「Authorize」をクリックしてOAuthフローを使用してキーを取得します。または、[ここ](https://openrouter.ai/keys)でAPIキーを生成してボックスに貼り付けます。
4. 「Connect」をクリックしてモデルを選択します。
5. (オプション)「Test Message」ボタンを使用して接続を確認します。
