---
route: /usage/api-connections/tabbyapi/
---

# TabbyAPI
Exllamav2バックエンドを使用してLLMでテキストを生成できるFastAPIベースのアプリケーションで、Exl2、GPTQ、およびFP16モデルをサポートします。

* [GitHub](https://github.com/theroyallab/tabbyAPI)

### Quickstart
1. 公式TabbyAPI GitHubの[installation instructions](https://github.com/theroyallab/tabbyAPI/wiki/01.-Getting-Started)に従ってください。
2. [config.ymlを作成](https://github.com/theroyallab/tabbyAPI/wiki/02.-Server-options)して、モデルパス、デフォルトモデル、シーケンス長などを設定します。ほとんど(すべてではないにしても)のこれらの設定を無視できます。
3. TabbyAPIを起動します。成功すると、次のようなものが表示されるはずです:

    ![TabbyAPI terminal](/static/tabby-terminal.png)

4. SillyTavernのText Completion APIで、TabbyAPIを選択します。
5. TabbyAPIターミナルから`Tabby API key`にAPIキーをコピーし、`API URL`が正しいことを確認します(デフォルトでは`http://127.0.0.1:5000`である必要があります)。

すべてを正しく行った場合、SillyTavernで次のようなものが表示されるはずです:

![TabbyAPI SillyTavern](/static/tabby-config.png)

これでTabbyAPIを使用してチャットできます！

### TabbyAPI Loader
TabbyAPIの開発者は、SillyTavernから直接モデルをロード/アンロードするための公式エクステンションを作成しました。インストールは簡単です:
1. SillyTavernで、Extensionsタブをクリックし、Download Extensions & Assetsに移動します。
2. Assets URLに`https://raw.githubusercontent.com/theroyallab/ST-repo/main/index.json`をコピーし、右側のプラグボタンをクリックします。
3. 次のようなものが表示されるはずです。Tabby Loaderの横のダウンロードボタンをクリックします。

    ![Tabby Loader](/static/tabby-assets.png)

4. インストールが成功した場合、画面の上部に緑色のポップアップメッセージが表示されるはずです。extensionsタブで、TabbyAPI Loaderに移動し、TabbyAPIターミナルからAdmin KeyにAdmin Keyをコピーします。
5. Model Selectの横の更新ボタンをクリックします。すぐ下のテキストボックスをクリックすると、モデルディレクトリ内のすべてのモデルが表示されるはずです。

![Tabby Loader Extension](/static/tabby-loader.png)

これでSillyTavernから直接モデルをロードおよびアンロードできます！

### Support
まだヘルプが必要ですか？開発者の公式Discordサーバーへのリンクについては[TabbyAPI GitHub](https://github.com/theroyallab/tabbyAPI)にアクセスし、[wikiを読んでください](https://github.com/theroyallab/tabbyAPI/wiki/1.-Getting-Started)。
