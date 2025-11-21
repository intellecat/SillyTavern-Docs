---
order: -30
icon: server
route: /for-contributors/server-plugins/
---

# Server Plugins

これらのプラグインは、新しいAPIエンドポイントの作成や、ブラウザ環境では利用できないNode.JSパッケージの使用など、UI extensionsだけでは実現不可能な機能を追加できます。

Pluginsは、SillyTavernの`plugins`ディレクトリに格納され、サーバー起動時に読み込まれますが、`config.yaml`ファイルで`enableServerPlugins`が`true`に設定されている場合*のみ*です。

!!!warning 警告
 **Server Pluginsはサンドボックス化されていません。つまり、ファイルシステム全体にアクセスしたり、通常のUI extensionsではできない方法で広範なセキュリティ脆弱性を引き起こす可能性があります。信頼できる開発者のserver pluginsのみをインストールしてください!**
!!!

すべての公式server pluginsのリストについては、GitHubの組織リストを参照してください:<https://github.com/search?q=topic%3Aplugin+org%3ASillyTavern&type=Repositories>

## Pluginのタイプ

### ファイル

`.js`(CommonJSモジュール用)または`.mjs`(ESモジュール用)拡張子を持つ実行可能なJavaScriptファイルで、`init`関数をエクスポートするモジュールを含みます。この関数は、Expressルーター(プラグイン専用に作成されたもの)を引数として受け取り、Promiseを返します。

モジュールは、プラグインに関する情報(`id`、`name`、`description`文字列)を含む`info`オブジェクトもエクスポートする必要があります。これにより、ローダーにプラグインに関する情報が提供されます。

ルーター経由でルートを登録でき、`/api/plugins/{id}/{route}`パスの下に登録されます。例えば、`example`プラグインの`router.get('/foo')`は、次のようなルートを生成します:`/api/plugins/example/foo`。

プラグインは、サーバーをシャットダウンする際にクリーンアップを実行する`exit`関数を*オプションで*エクスポートすることもできます。引数は持たず、Promiseを返す必要があります。

Plugin exportsのTypeScript契約:

```ts
interface PluginInfo {
    id: string;
    name: string;
    description: string;
}

interface Plugin {
    init: (router: Router) => Promise<void>;
    exit: () => Promise<void>;
    info: PluginInfo;
}
```

以下は"Hello world!"プラグインの例です:

```js
/**
 * Initialize plugin.
 * @param {import('express').Router} router Express router
 * @returns {Promise<any>} Promise that resolves when plugin is initialized
 */
async function init(router) {
    // Do initialization here...
    router.get('/foo', req, res, function () {
       res.send('bar');
    });
    console.log('Example plugin loaded!');
    return Promise.resolve();
}

async function exit() {
    // Do some clean-up here...
    return Promise.resolve();
}

module.exports = {
    init,
    exit,
    info: {
        id: 'example',
        name: 'Example',
        description: 'My cool plugin!',
    },
};
```

### ディレクトリ

`plugins`ディレクトリのサブディレクトリからプラグインを読み込むことができます。以下のいずれかの方法で(優先順に):

1. "main"フィールドに実行可能ファイルへのパスを含む`package.json`ファイル。
2. CommonJSモジュール用の`index.js`ファイル。
3. ESモジュール用の`index.mjs`ファイル。

結果のファイルは、個々のファイルと同じ要件で`init`関数と`info`オブジェクトをエクスポートする必要があります。

ディレクトリプラグインの例(`index.js`ファイル付き):<https://github.com/SillyTavern/SillyTavern-DiscordRichPresence-Server>

### バンドル化

すべての要件を1つのファイルにパッケージ化するバンドラー(WebpackやBrowserifyなど)を使用することが望ましいです。ビルドターゲットとして"Node"を設定してください。

WebpackとTypeScriptを使用したプラグインのテンプレートリポジトリ:<https://github.com/SillyTavern/Plugin-WebpackTemplate>
