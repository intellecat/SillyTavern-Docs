---
order: -20
icon: file-added
templating: false
route: /for-contributors/writing-extensions/
---

# UI Extensions

UI extensionsは、SillyTavernのイベントとAPIにフックすることで機能を拡張します。これらはブラウザコンテキストで実行され、DOM、JavaScript API、SillyTavernコンテキストへの事実上無制限のアクセス権を持ちます。Extensionsは、UIの変更、内部APIの呼び出し、チャットデータとの対話が可能です。このガイドでは、独自のextensionsの作成方法を説明します(JavaScriptの知識が必要です)。

!!!tip 単にextensionsをインストールしたいだけですか?
こちらへ:[Extensions](../extensions/index.md)。
!!!

Node.jsサーバーの機能を拡張するには、[Server Plugins](./Server-Plugins.md)ページを参照してください。

**JavaScriptを書けませんか?**

* 本格的なextensionを書く代わりに、より簡単な代替手段として[STscript](./st-script.md)を検討してください。
* [MDN Course](https://developer.mozilla.org/en-US/docs/Learn/JavaScript)を受講して、完了したら戻ってきてください。

## Extensionの投稿

[公式コンテンツリポジトリ](https://github.com/SillyTavern/SillyTavern-Content)にextensionを投稿したいですか? お問い合わせください!

すべてのextensionが安全で使いやすいことを保証するために、いくつかの要件があります:

1. Extensionはオープンソースで、libre license(自由ライセンス)を持つ必要があります([Choose a License](https://choosealicense.com/licenses/)参照)。不明な場合は、AGPLv3が良い選択です。
2. Extensionは、SillyTavernの最新リリースバージョンと互換性がある必要があります。コアに何か変更がある場合は、extensionを更新する準備をしてください。
3. Extensionは十分にドキュメント化されている必要があります。これには、インストール手順、使用例、機能リストを含むREADMEファイルが含まれます。
4. 機能するためにserver pluginの要件があるextensionは受け入れられません。

## 例

シンプルなSillyTavern extensionの実例を参照してください:

* <https://github.com/city-unit/st-extension-example> - 基本的なextensionテンプレート。manifestの作成、ローカルスクリプトのインポート、設定UIパネルの追加、永続的なextension設定の使用方法を紹介しています。
* <https://github.com/search?q=topic%3Aextension+org%3ASillyTavern&type=Repositories> - GitHubのすべての公式SillyTavern extensionのリスト。

## バンドル化

Extensionは、バンドル化を利用して、他のモジュールから分離し、Vue、Reactなどのフレームワークを含むNPMの任意の依存関係を使用することもできます。

* <https://github.com/SillyTavern/Extension-WebpackTemplate> - TypeScriptとWebpackを使用したextensionのテンプレートリポジトリ(Reactなし)。
* <https://github.com/SillyTavern/Extension-ReactTemplate> - ReactとWebpackを使用した基本的なextensionのテンプレートリポジトリ。

バンドルから相対インポートを使用するには、インポートラッパーを作成する必要がある場合があります。Webpackの例は次のとおりです:

```js
/**
 * Import a member from a module by URL, bypassing webpack.
 * @param {string} url URL to import from
 * @param {string} what Name of the member to import
 * @param {any} defaultValue Fallback value
 * @returns {Promise<any>} Imported member
 */
export async function importFromUrl(url, what, defaultValue = null) {
    try {
        const module = await import(/* webpackIgnore: true */ url);
        if (!Object.hasOwn(module, what)) {
            throw new Error(`No ${what} in module`);
        }
        return module[what];
    } catch (error) {
        console.error(`Failed to import ${what} from ${url}: ${error}`);
        return defaultValue;
     }
}

// Import a function from 'script.js' module
const generateRaw = await importFromUrl('/script.js', 'generateRaw');
```

## manifest.json

すべてのextensionは、`data/<user-handle>/extensions`にフォルダと`manifest.json`ファイルを持つ必要があります。このファイルには、extensionに関するメタデータと、extensionのエントリポイントであるJSスクリプトファイルへのパスが含まれています。

ダウンロード可能なextensionは、HTTP経由で提供される際に`/scripts/extensions/third-party`フォルダにマウントされるため、それに基づいて相対インポートを使用する必要があります。ローカル開発を容易にするために、extensionリポジトリを`/scripts/extensions/third-party`フォルダに配置することを検討してください("Install for all users"オプション)。

```json
{
    "display_name": "The name of the extension",
    "loading_order": 1,
    "requires": [],
    "optional": [],
    "dependencies": [],
    "js": "index.js",
    "css": "style.css",
    "author": "Your name",
    "version": "1.0.0",
    "homePage": "https://github.com/your/extension",
    "auto_update": true,
    "minimum_client_version": "1.0.0",
    "i18n": {
        "de-de": "i18n/de-de.json"
    }
}
```

### Manifestフィールド

* `display_name`は必須です。"Manage Extensions"メニューに表示されます。
* `loading_order`はオプションです。数値が大きいほど後に読み込まれます。
* `js`はメインJSファイルの参照で、必須です。
* `css`はオプションのスタイルファイルの参照です。
* `author`は必須です。作成者の名前または連絡先情報を含む必要があります。
* `auto_update`は、STパッケージのバージョンが変更されたときにextensionを自動更新する場合は`true`に設定します。
* `i18n`は、サポートされているロケールとそれに対応するJSONファイルを指定するオプションのオブジェクトです(以下を参照)。
* `dependencies`は、このextensionが依存する他の**extensions**を指定する文字列のオプションの配列です。
* `generate_interceptor`は、テキスト生成リクエスト時に呼び出されるグローバル関数の名前を指定するオプションの文字列です。
* `minimum_client_version`は、このextensionが動作するために必要な最小SillyTavernバージョンを指定するオプションの文字列です。

### 依存関係

Extensionは、他のSillyTavern extensionに依存することもできます。これらの依存関係のいずれかが欠落しているか無効になっている場合、extensionは読み込まれません。

依存関係は、`public/extensions`ディレクトリに表示される**フォルダ名**で指定されます。

例:

* 内蔵extensions:`"vectors"`、`"caption"`
* サードパーティextensions:`"third-party/Extension-WebLLM"`、`"third-party/Extension-Mermaid"`

### 非推奨フィールド

* `requires`は、必要な**Extrasモジュール**を指定する文字列のオプションの配列です。接続されたExtras APIがリストされたすべてのモジュールを提供しない場合、extensionは読み込まれません。
* `optional`は、オプションの**Extrasモジュール**を指定する文字列のオプションの配列です。これらが欠落している場合でも、extensionは読み込まれ、extensionはそれらの不在を適切に処理する必要があります。

接続されたExtras APIによって現在提供されているモジュールを確認するには、`scripts/extensions.js`から`modules`配列をインポートしてください。

## スクリプティング

### getContextの使用

`SillyTavern`グローバルオブジェクトの`getContext()`関数は、すべてのメインアプリの状態オブジェクト、便利な関数、ユーティリティのコレクションであるSillyTavernコンテキストへのアクセスを提供します。

```js
const context = SillyTavern.getContext();
context.chat; // Chat log - MUTABLE
context.characters; // Character list
context.characterId; // Index of the current character
context.groups; // Group list
context.groupId; // ID of the current group
// And many more...
```

利用可能なプロパティと関数の完全なリストは、[SillyTavernソースコード](https://github.com/SillyTavern/SillyTavern/blob/staging/public/scripts/st-context.js)で確認できます。

!!!
`getContext`で欠落している関数/プロパティがある場合は、開発者に連絡するか、pull requestを送信してください!
!!!

### 共有ライブラリ

SillyTavernフロントエンドで内部的に使用されているnpmライブラリのほとんどは、`SillyTavern`グローバルオブジェクトの`libs`プロパティで共有されています。

* `lodash` - ユーティリティライブラリ。[ドキュメント](https://lodash.com/)。
* `localforage` - ブラウザストレージライブラリ。[ドキュメント](https://localforage.github.io/localForage/)。
* `Fuse` - ファジー検索ライブラリ。[ドキュメント](https://www.fusejs.io/)。
* `DOMPurify` - HTMLサニタイズライブラリ。[ドキュメント](https://github.com/cure53/DOMPurify)。
* `Handlebars` - テンプレートライブラリ。[ドキュメント](https://handlebarsjs.com/)。
* `moment` - 日付/時刻操作ライブラリ。[ドキュメント](http://momentjs.com/)。
* `showdown` - Markdownコンバーターライブラリ。[ドキュメント](https://showdownjs.com/)。

エクスポートされたライブラリの完全なリストは、[SillyTavernソースコード](https://github.com/SillyTavern/SillyTavern/blob/staging/public/lib.js)で確認できます。

**例:** DOMPurifyライブラリの使用。

```js
const { DOMPurify } = SillyTavern.libs;

const sanitizedHtml = DOMPurify.sanitize('<script>"dirty HTML"</script>');
```

### TypeScript注意事項

`getContext()`と`libs`を含む`SillyTavern`グローバルオブジェクトのすべてのメソッドのオートコンプリートにアクセスしたい場合(おそらくアクセスしたいでしょう)、TypeScript `.d.ts`モジュール宣言を追加する必要があります。この宣言は、extensionの場所に応じて、SillyTavernのソースからグローバルタイプをインポートする必要があります。以下は、"all users"と"current user"の両方のインストールタイプで動作する例です。

**global.d.ts** - このファイルをextensionディレクトリのルート(`manifest.json`の隣)に配置します:

```ts
export {};

// 1. Import for user-scoped extensions
import '../../../../public/global';
// 2. Import for server-scoped extensions
import '../../../../global';

// Define additional types if needed...
declare global {
    // Add global type declarations here
}
```

### 他のファイルからのインポート

!!!warning
SillyTavernコードからのインポートの使用は信頼性が低く、STのモジュールの内部構造が変更された場合、いつでも破損する可能性があります。`getContext`は、より安定したAPIを提供します。
!!!

バンドルされたextensionを構築していない限り、他のJSファイルから変数や関数をインポートできます。

例えば、このコードスニペットは、現在選択されているAPIからバックグラウンドで返信を生成します:

```js
import { generateQuietPrompt } from "../../../../script.js";

async function handleMessage(data) {
    const text = data.message;
    const translated = await generateQuietPrompt({ quietPrompt: text });
    // ...
}
```

## 状態管理

### 永続的な設定

Extensionがその状態を永続化する必要がある場合、`getContext()`関数から`extensionSettings`オブジェクトを使用してデータを保存および取得できます。Extensionは、設定オブジェクトに任意のJSONシリアライズ可能なデータを保存でき、他のextensionとの競合を避けるために一意のキーを使用する必要があります。

設定を永続化するには、`saveSettingsDebounced()`関数を使用します。これにより、設定がサーバーに保存されます。

```js
const { extensionSettings, saveSettingsDebounced } = SillyTavern.getContext();

// Define a unique identifier for your extension
const MODULE_NAME = 'my_extension';

// Define default settings
const defaultSettings = Object.freeze({
    enabled: false,
    option1: 'default',
    option2: 5
});

// Define a function to get or initialize settings
function getSettings() {
    // Initialize settings if they don't exist
    if (!extensionSettings[MODULE_NAME]) {
        extensionSettings[MODULE_NAME] = structuredClone(defaultSettings);
    }

    // Ensure all default keys exist (helpful after updates)
    for (const key of Object.keys(defaultSettings)) {
        if (!Object.hasOwn(extensionSettings[MODULE_NAME], key)) {
            extensionSettings[MODULE_NAME][key] = defaultSettings[key];
        }
    }

    return extensionSettings[MODULE_NAME];
}

// Use the settings
const settings = getSettings();
settings.option1 = 'new value';

// Save the settings
saveSettingsDebounced();
```

### チャットメタデータ

特定のチャットにデータをバインドするには、`getContext()`関数から`chatMetadata`オブジェクトを使用できます。このオブジェクトを使用すると、チャットに関連付けられた任意のデータを保存でき、extension固有の状態を保存するのに役立ちます。

メタデータを永続化するには、`saveMetadata()`関数を使用します。これにより、メタデータがサーバーに保存されます。

!!!warning
チャットが切り替えられると参照が変更されるため、`chatMetadata`への参照を長期間保持する変数に保存しないでください。現在のチャットメタデータにアクセスするには、常に`SillyTavern.getContext().chatMetadata`を使用してください。
!!!

```js
const { chatMetadata, saveMetadata } = SillyTavern.getContext();

// Set some metadata for the current chat
chatMetadata['my_key'] = 'my_value';

// Get the metadata for the current chat
const value = chatMetadata['my_key'];

// Save the metadata to the server
await saveMetadata();
```

!!!tip
`CHAT_CHANGED`イベントは、チャットが切り替えられたときに発行されるため、このイベントをリッスンして、extensionの状態を適切に更新できます。詳細については、[イベントのリッスン](#listening-to-events)セクションを参照してください。
!!!

### キャラクターカード

SillyTavernは、キャラクターカードJSONデータに任意のデータを保存できる[Character Cards V2 Specification](https://github.com/malfoyslastname/character-card-spec-v2/blob/main/spec_v2.md)を完全にサポートしています。

これは、キャラクターに関連付けられた追加データを保存し、キャラクターカードをエクスポートする際に共有可能にする必要があるextensionに役立ちます。

キャラクターカードの[extensions](https://github.com/malfoyslastname/character-card-spec-v2/blob/main/spec_v2.md#extensions)データフィールドにデータを書き込むには、`getContext()`関数から`writeExtensionField`関数を使用します。この関数は、キャラクターID、文字列キー、および書き込む値を受け取ります。値はJSONシリアライズ可能である必要があります。

!!!warning 奇妙な点
`characterId`と呼ばれていますが、「本当の」一意の識別子ではなく、`characters`配列内のキャラクターのインデックスです。

現在のキャラクターのインデックスは、コンテキストの`characterId`プロパティによって提供されます。現在選択されているキャラクターにデータを書き込む場合は、`SillyTavern.getContext().characterId`を使用してください。別のキャラクターのデータを保存する必要がある場合は、`characters`配列でキャラクターを検索してインデックスを見つけてください。

**注意:`characterId`は、グループチャットまたはキャラクターが選択されていない場合は`undefined`です!**
!!!

```js
const { writeExtensionField, characterId } = SillyTavern.getContext();

// Write some data to the character card
await writeExtensionField(characterId, 'my_extension_key', {
    someData: 'value',
    anotherData: 42
});

// Read the data back from the character card
const character = SillyTavern.getContext().characters[characterId];
// The data is stored in the `extensions` object of the character's data
const myData = character.data?.extensions?.my_extension_key;
```

### 設定プリセット

メインAPIタイプの設定プリセットに任意のJSONデータを保存できます。プリセットJSONと一緒にエクスポートおよびインポートされるため、プリセットのextension固有の設定を保存するために使用できます。以下のAPIタイプは、設定プリセットのデータ拡張をサポートしています:

* Chat Completion
* Text Completion
* NovelAI
* KoboldAI / AI Horde

データを読み書きするには、まずコンテキストからPresetManagerインスタンスを取得する必要があります:

```js
const { getPresetManager } = SillyTavern.getContext();

// Get the preset manager for the current API type
const pm = getPresetManager();

// Write data to the preset extension field:
// - path: the path to the field in the preset data
// - value: the value to write
// - name (optional): the name of the preset to write to, defaults to the currently selected preset
await pm.writePresetExtensionField({ path: 'hello', value: 'world' });

// Read data from the preset extension field:
// - path: the path to the field in the preset data
// - name (optional): the name of the preset to read from, defaults to the currently selected preset
const value = pm.readPresetExtensionField({ path: 'hello' });
```

!!!tip
`PRESET_CHANGED`および`MAIN_API_CHANGED`イベントは、プリセットが変更されたときまたはメインAPIが切り替えられたときに発行されるため、これらのイベントをリッスンして、extensionの状態を適切に更新できます。詳細については、[イベントのリッスン](#listening-to-events)セクションを参照してください。
!!!

## 国際化

!!!
翻訳の提供に関する一般的な情報については、[Internationalization](/For_Contributors/i18n.md)ページを参照してください。
!!!

Extensionは、`t`、`translate`関数およびHTMLテンプレートの`data-i18n`属性で使用するための追加のローカライズ文字列を提供できます。

サポートされているロケールのリストは、こちら(`lang`キー)を参照してください:<https://github.com/SillyTavern/SillyTavern/blob/release/public/locales/lang.json>

### 直接`addLocaleData`呼び出し

ロケールコードと翻訳を含むオブジェクトを`addLocaleData`関数に渡します。既存のキーのオーバーライドは*許可されていません*。渡されたロケールコードが現在選択されているロケールでない場合、データはサイレントに無視されます。

```js
SillyTavern.getContext().addLocaleData('fr-fr', { 'Hello': 'Bonjour' });
SillyTavern.getContext().addLocaleData('de-de', { 'Hello': 'Hallo' });
```

### Extension manifestを介して

manifestにi18nオブジェクトを追加し、サポートされているロケールとそれに対応するJSONファイルパス(extensionディレクトリからの相対パス)のリストを記載します。

```json
{
  "display_name": "Foobar",
  "js": "index.js",
  // rest of the fields
  "i18n": {
    "fr-fr": "i18n/french.json",
    "de-de": "i18n/german.json"
  }
}
```

## Slash commandsの登録(新しい方法)

後方互換性のために`registerSlashCommand`は引き続き存在しますが、新しいslash commandsは`SlashCommandParser.addCommandObject()`を通じて登録して、コマンドとそのパラメータに関する拡張詳細をパーサー(ひいてはオートコンプリートとコマンドヘルプ)に提供する必要があります。

```javascript
SlashCommandParser.addCommandObject(SlashCommand.fromProps({ name: 'repeat',
    callback: (namedArgs, unnamedArgs) => {
        return Array(namedArgs.times ?? 5)
            .fill(unnamedArgs.toString())
            .join(isTrueBoolean(namedArgs.space.toString()) ? ' ' : '')
        ;
    },
    aliases: ['example-command'],
    returns: 'the repeated text',
    namedArgumentList: [
        SlashCommandNamedArgument.fromProps({ name: 'times',
            description: 'number of times to repeat the text',
            typeList: ARGUMENT_TYPE.NUMBER,
            defaultValue: '5',
        }),
        SlashCommandNamedArgument.fromProps({ name: 'space',
            description: 'whether to separate the texts with a space',
            typeList: ARGUMENT_TYPE.BOOLEAN,
            defaultValue: 'off',
            enumList: ['on', 'off'],
        }),
    ],
    unnamedArgumentList: [
        SlashCommandArgument.fromProps({ description: 'the text to repeat',
            typeList: ARGUMENT_TYPE.STRING,
            isRequired: true,
        }),
    ],
    helpString: `
        <div>
            Repeats the provided text a number of times.
        </div>
        <div>
            <strong>Example:</strong>
            <ul>
                <li>
                    <pre><code class="language-stscript">/repeat foo</code></pre>
                    returns "foofoofoofoofoo"
                </li>
                <li>
                    <pre><code class="language-stscript">/repeat times=3 space=on bar</code></pre>
                    returns "bar bar bar"
                </li>
            </ul>
        </div>
    `,
}));
```

登録されたすべてのコマンドは、[STscript](/For_Contributors/st-script.md)で可能なあらゆる方法で使用できます。

## イベント

### イベントのリッスン

`eventSource.on(eventType, eventHandler)`を使用してイベントをリッスンします:

```js
const { eventSource, event_types } = SillyTavern.getContext();

eventSource.on(event_types.MESSAGE_RECEIVED, handleIncomingMessage);

function handleIncomingMessage(data) {
    // Handle message
}
```

主なイベントタイプは次のとおりです:

* `APP_READY`:アプリが完全に読み込まれ、使用可能になりました。アプリの準備が整った後に新しいリスナーがアタッチされるたびに自動起動されます。
* `MESSAGE_RECEIVED`:LLMメッセージが生成され、`chat`オブジェクトに記録されますが、まだUIにレンダリングされていません。
* `MESSAGE_SENT`:メッセージがユーザーによって送信され、`chat`オブジェクトに記録されますが、まだUIにレンダリングされていません。
* `USER_MESSAGE_RENDERED`:ユーザーが送信したメッセージがUIにレンダリングされます。
* `CHARACTER_MESSAGE_RENDERED`:生成されたLLMメッセージがUIにレンダリングされます。
* `CHAT_CHANGED`:チャットが切り替えられました(例:別のキャラクターに切り替えられた、または別のチャットが読み込まれた)。
* `GENERATION_AFTER_COMMANDS`:slash commandsの処理後、生成が開始されようとしています。
* `GENERATION_STOPPED`:生成がユーザーによって停止されました。
* `GENERATION_ENDED`:生成が完了したか、エラーが発生しました。
* `SETTINGS_UPDATED`:アプリケーション設定が更新されました。

残りは[ソース](https://github.com/SillyTavern/SillyTavern/blob/staging/public/scripts/events.js)で見つけることができます。

!!!info イベントデータ
各イベントがリスナーにデータを渡す方法は統一されていません。一部のイベントはデータを発行しません。一部はオブジェクトまたはプリミティブ値を渡します。イベントが発行されるソースコードを参照して、どのようなデータを渡すかを確認するか、デバッガーで確認してください。
!!!

### イベントの発行

Extensionからアプリケーションイベント(カスタムイベントを含む)を生成するには、`eventSource.emit(eventType, ...eventData)`を呼び出します:

```js
const { eventSource } = SillyTavern.getContext();

// Can be a built-in event_types field or any string.
const eventType = 'myCustomEvent';

// Use `await` to ensure all event handlers complete before continuing execution.
await eventSource.emit(eventType, { data: 'custom event data' });
```

## Prompt Interceptors

Prompt Interceptorsは、テキスト生成リクエストが行われる前に、チャットデータの変更、インジェクションの追加、または生成の中止などのアクティビティをextensionが実行する方法を提供します。

異なるextensionからのインターセプターは順次実行されます。順序は、それぞれの`manifest.json`ファイルの`loading_order`フィールドによって決定されます。`loading_order`値が低いextensionが早く実行されます。`loading_order`が指定されていない場合、`display_name`がフォールバックとして使用されます。どちらも指定されていない場合、順序は未定義です。

### Interceptorの登録

Prompt interceptorを定義するには、extensionの`manifest.json`ファイルに`generate_interceptor`フィールドを追加します。値は、SillyTavernによって呼び出されるグローバル関数の名前である必要があります。

```json
{
    "display_name": "My Interceptor Extension",
    "loading_order": 10, // Affects execution order
    "generate_interceptor": "myCustomInterceptorFunction",
    // ... other manifest properties
}
```

### Interceptor関数

`generate_interceptor`関数は、ドライランではない生成リクエスト時に呼び出されるグローバル関数です。グローバルスコープで定義する必要があり(例:`globalThis.myCustomInterceptorFunction = async function(...) { ... }`)、非同期操作を実行する必要がある場合は`Promise`を返すことができます。

Interceptor関数は、次の引数を受け取ります:

* `chat`:プロンプト構築に使用されるチャット履歴を表すメッセージオブジェクトの配列。この配列を直接変更できます(例:メッセージの追加、削除、または変更)。メッセージはミュータブルであるため、配列に加えた変更は、実際のチャット履歴に反映されることに注意してください。変更を一時的なものにしたい場合は、`structuredClone`を使用してメッセージオブジェクトのディープコピーを作成してください。
* `contextSize`:今後の生成のために計算された現在のコンテキストサイズ(トークン単位)を示す数値。
* `abort`:呼び出されると、テキスト生成が続行されないようにシグナルを送る関数。`true`の場合、後続のインターセプターの実行を防ぐブール値パラメータを受け入れます。
* `type`:生成のタイプまたはトリガーを示す文字列(例:`'quiet'`、`'regenerate'`、`'impersonate'`、`'swipe'`など)。これにより、インターセプターは、生成がどのように開始されたかに基づいて条件付きでロジックを適用できます。

**実装例:**

```javascript
globalThis.myCustomInterceptorFunction = async function(chat, contextSize, abort, type) {
    // Example: Add a system note before the last user message
    const systemNote = {
        is_user: false,
        name: "System Note",
        send_date: Date.now(),
        mes: "This was added by my extension!"
    };
    // Insert before the last message
    chat.splice(chat.length - 1, 0, systemNote);
}
```

## テキストの生成

SillyTavernは、現在選択されているLLM APIを使用して、さまざまなコンテキストでテキストを生成するためのいくつかの関数を提供しています。これらの関数を使用すると、チャットのコンテキスト内でテキストを生成したり、コンテキストなしでraw generationを行ったり、構造化された出力を使用したりできます。

### チャットコンテキスト内

`generateQuietPrompt()`関数は、チャットのコンテキストで追加された「quiet」プロンプト(履歴後の指示)をバックグラウンドで使用してテキストを生成するために使用されます(出力はUIにレンダリングされません)。これは、要約や画像プロンプトの生成など、ユーザーエクスペリエンスを中断することなく、関連するチャットおよびキャラクターデータを保持したままテキストを生成するのに役立ちます。

```js
const { generateQuietPrompt } = SillyTavern.getContext();

const quietPrompt = 'Generate a summary of the chat history.';

const result = await generateQuietPrompt({
    quietPrompt,
});
```

### Raw generation

`generateRaw()`関数は、チャットコンテキストなしでテキストを生成するために使用されます。プロンプト構築プロセスを完全に制御したい場合に役立ちます。

これは、Text Completion文字列またはChat Completionオブジェクトの配列として`prompt`を受け入れ、選択されたAPIタイプに応じて適切な形式でリクエストを構築します。例:チャット/テキストモード間の変換、instruct formattingの適用など。さらに詳細な制御のために、`systemPrompt`と`prefill`を関数に渡すこともできます。

```js
const { generateRaw } = SillyTavern.getContext();

const systemPrompt = 'You are a helpful assistant.';
const prompt = 'Generate a story about a brave knight.';
const prefill = 'Once upon a time,';

/*
In Chat Completion mode, will produce a prompt like this:
[
  {role: 'system', content: 'You are a helpful assistant.'},
  {role: 'user', content: 'Generate a story about a brave knight.'},
  {role: 'assistant', content: 'Once upon a time,'}
]
*/

/*
In Text Completion mode (no instruct), will produce a prompt like this:
"You are a helpful assistant.\nGenerate a story about a brave knight.\nOnce upon a time,"
*/

const result = await generateRaw({
    systemPrompt,
    prompt,
    prefill,
});
```

### 構造化出力

!!!info
現在、Chat Completion APIでのみサポートされています。可用性は、選択されたソースとモデルによって異なります。選択されたモデルが構造化出力をサポートしていない場合、生成は失敗するか、空のオブジェクト(`'{}'`)を返します。構造化出力がサポートされているかどうかについては、使用している特定のAPIのドキュメントを確認してください。
!!!

構造化出力機能を使用して、提供された[JSON Schema](https://json-schema.org/learn)に準拠した有効なJSONオブジェクトをモデルが生成するようにできます。これは、状態追跡、データ分類などの構造化データを必要とするextensionに役立ちます。

構造化出力を使用するには、`generateRaw()`または`generateQuietPrompt()`にJSONスキーマオブジェクトを渡す必要があります。モデルは、スキーマに一致する応答を生成し、文字列化されたJSONオブジェクトとして返されます。

!!!warning
出力はスキーマに対して検証されません。生成された出力の解析と検証は、自分で処理する必要があります。モデルが有効なJSONオブジェクトの生成に失敗した場合、関数は空のオブジェクト(`'{}'`)を返します。

[Zod](https://zod.dev/json-schema)は、JSONスキーマを生成および検証するための人気のあるライブラリです。その使用については、ここではカバーしません。
!!!

```js
const { generateRaw, generateQuietPrompt } = SillyTavern.getContext();

// Define a JSON schema for the expected output
const jsonSchema = {
    // Required: a name for the schema
    name: 'StoryStateModel',
    // Optional: a description of the schema
    description: 'A schema for a story state with location, plans, and memories.',
    // Optional:  the schema will be used in strict mode, meaning that only the fields defined in the schema will be allowed
    strict: true,
    // Required: a definition of the schema
    value: {
        '$schema': 'http://json-schema.org/draft-04/schema#',
        'type': 'object',
        'properties': {
            'location': {
                'type': 'string'
            },
            'plans': {
                'type': 'string'
            },
            'memories': {
                'type': 'string'
            }
        },
        'required': [
            'location',
            'plans',
            'memories'
        ],
    },
};

const prompt = 'Generate a story state with location, plans, and memories. Output as a JSON object.';

const rawResult = await generateRaw({
    prompt,
    jsonSchema,
});

const quietResult = await generateQuietPrompt({
    quietPrompt: prompt,
    jsonSchema,
});
```

## カスタムマクロの登録

マクロ置換がサポートされている場所であればどこでも使用できるカスタムマクロを登録できます。例:キャラクターカードフィールド、STscriptコマンド、プロンプトテンプレートなど。

マクロを登録するには、`SillyTavern.getContext()`オブジェクトから`registerMacro()`関数を使用します。この関数は、一意の文字列である必要があるマクロ名と、文字列または文字列を返す関数を受け入れます。関数は、各`substituteParams`呼び出し間で異なる一意の`nonce`文字列で呼び出されます。

```js
const { registerMacro } = SillyTavern.getContext();

// Simple string macro
registerMacro('fizz', 'buzz');
// Function macro
registerMacro('tomorrow', () => {
    return new Date(Date.now() + 24 * 60 * 60 * 1000).toLocaleDateString();
});
```

カスタムマクロが不要になったら、`unregisterMacro()`関数を使用して削除します:

```js
const { unregisterMacro } = SillyTavern.getContext();

// Unregister the 'fizz' macro
unregisterMacro('fizz');
```

**カスタムマクロに関する重要な詳細と既知の制限事項:**

1. 現在、シンプルな文字列置換マクロのみがサポートされています。将来的により複雑なマクロのサポートを追加する予定です。
2. 値を提供するために関数を使用するマクロは、同期的である*必要があります*。`Promise`を返すことは機能しません。
3. マクロ名を登録する際に、二重中括弧(`{{ }}`)で囲む必要はありません。SillyTavernがそれを行います。
4. マクロは単純な正規表現置換であるため、多くのマクロを登録すると、パフォーマンスの問題が発生するため、控えめに使用してください。

## Extrasリクエストの実行

!!!warning
Extras APIは非推奨です。新しいextensionでの使用は推奨されません。
!!!

`doExtrasFetch()`関数を使用すると、SillyTavern Extras APIサーバーにリクエストを送信できます。

例えば、`/api/summarize`エンドポイントを呼び出すには:

```js
import { getApiUrl, doExtrasFetch } from "../../extensions.js";

const url = new URL(getApiUrl());
url.pathname = '/api/summarize';

const apiResult = await doExtrasFetch(url, {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'Bypass-Tunnel-Reminder': 'bypass',
    },
    body: JSON.stringify({
        // Request body
    })
});
```

`getApiUrl()`は、ExtrasサーバーのベースURLを返します。

`doExtrasFetch()`関数は:

* `Authorization`および`Bypass-Tunnel-Reminder`ヘッダーを追加します
* 結果のフェッチを処理します
* 結果(responseオブジェクト)を返します

これにより、extensionからExtras APIを簡単に呼び出すことができます。

次を指定できます:

* リクエストメソッド:GET、POSTなど。
* 追加のヘッダー
* POSTリクエストのボディ
* その他のfetchオプション
