---
icon: cpu
route: /administration/config-yaml/
---

# 設定ファイル

!!!warning 免責事項

このドキュメントは古く、不完全、または不正確である可能性があります。最新の設定一覧については、インストール先の[デフォルトconfig.yaml](https://github.com/SillyTavern/SillyTavern/blob/release/default/config.yaml)を参照してください。

**警告: デフォルト設定を直接編集しないでください。これは何の効果もありません。代わりにリポジトリルートにあるそのコピーを編集してください。**
!!!

`config.yaml`はSillyTavernサーバーのメイン設定ファイルで、[インストールを完了](/Installation/index.md)後にリポジトリのルートディレクトリにあります。これはネットワーク、セキュリティ、バックエンド固有のオプションなど、さまざまな設定を含むYAMLファイルです。**このファイルに加えた変更は、サーバーを再起動した後に有効になります。**

上流で追加された新しい設定は、[リポジトリを更新](/Installation/Updating/index.md)した後に`npm install`（具体的には`post-install.js`スクリプト）を実行すると、デフォルト値で自動的に入力されます。その後、必要に応じてこれらの設定を変更できます。

ネストされた設定の場合、ドット記法を使用して階層を示します。例えば、`protocol.ipv6: false`は`protocol`セクション下の`ipv6`設定を指し、値は`false`です。

```yaml
protocol:
  ipv6: false
```

## コマンドライン引数

SillyTavernサーバーを起動する際にコマンドライン引数を渡して、[config.yaml](../Administration/config-yaml.md)の一部の設定を上書きできます。

### 例

```shell
node server.js --port 8000 --listen false
# or
npm run start -- --port 8000 --listen false
# or (Windows only)
Start.bat --port 8000 --listen false
```

### サポートされている引数

!!!tip
引数はすべて任意です。これらを指定しない場合、SillyTavernは`config.yaml`の設定を使用します。
!!!

| オプション                      | 説明                                                                 | 型       |
|---------------------------------|----------------------------------------------------------------------|----------|
| `--version`                     | バージョン番号を表示します                                           | boolean  |
| `--global`                      | アプリケーションデータにシステム全体のパスを強制的に使用します       | boolean  |
| `--configPath`                  | config.yamlファイルへのパスを上書きします（standaloneモードのみ）    | string   |
| `--dataRoot`                    | データストレージのルートディレクトリを設定します（standaloneモードのみ） | string   |
| `--port`                        | SillyTavernが実行されるポートを設定します                            | number   |
| `--listen`                      | すべてのネットワークインターフェースでSillyTavernをリッスンさせます  | boolean  |
| `--whitelist`                   | whitelistモードを有効にします                                        | boolean  |
| `--basicAuthMode`               | basic認証を有効にします                                              | boolean  |
| `--enableIPv4`                  | IPv4プロトコルを有効にします                                         | boolean  |
| `--enableIPv6`                  | IPv6プロトコルを有効にします                                         | boolean  |
| `--listenAddressIPv4`           | リッスンするIPv4アドレスを指定します                                 | string   |
| `--listenAddressIPv6`           | リッスンするIPv6アドレスを指定します                                 | string   |
| `--dnsPreferIPv6`               | DNSにIPv6を優先します                                                | boolean  |
| `--ssl`                         | SSLを有効にします                                                    | boolean  |
| `--certPath`                    | 証明書ファイルへのパスを設定します                                   | string   |
| `--keyPath`                     | 秘密鍵ファイルへのパスを設定します                                   | string   |
| `--browserLaunchEnabled`        | ブラウザでSillyTavernを自動的に起動します                            | boolean  |
| `--browserLaunchHostname`       | ブラウザ起動のホスト名を設定します                                   | string   |
| `--browserLaunchPort`           | ブラウザ起動のポートを上書きします                                   | string   |
| `--browserLaunchAvoidLocalhost` | autoモードでブラウザ起動時に'localhost'の使用を避けます              | boolean  |
| `--corsProxy`                   | CORSプロキシを有効にします                                           | boolean  |
| `--requestProxyEnabled`         | 発信リクエストにプロキシの使用を有効にします                         | boolean  |
| `--requestProxyUrl`             | リクエストプロキシURLを設定します（HTTPまたはSOCKSプロトコル）        | string   |
| `--requestProxyBypass`          | リクエストプロキシバイパスリストを設定します（スペース区切りのホストリスト） | array    |
| `--disableCsrf`                 | CSRF保護を無効にします（非推奨）                                     | boolean  |

## 環境変数

設定は環境変数を介して設定することもでき、これは`config.yaml`ファイルの値を上書きします。

環境変数は`SILLYTAVERN_`というプレフィックスを付け、設定名には大文字を使用する必要があります。例えば、`dataRoot`設定は`SILLYTAVERN_DATAROOT`環境変数で上書きできます。

ネストされた設定はアンダースコアで区切る必要があります。例えば、`protocol.ipv6`は`SILLYTAVERN_PROTOCOL_IPV6`環境変数で上書きできます。

!!!warning
配列やオブジェクトを期待する設定はJSON文字列化する必要があります。例えば、`whitelist`設定を`SILLYTAVERN_WHITELIST`環境変数で上書きするには、JSON文字列として設定する必要があります: `SILLYTAVERN_WHITELIST='["127.0.0.1", "::1"]'`。
!!!

Node.js v20以降を使用している場合、`.env`ファイルに環境変数を保存し、`--env-file`フラグでサーバーに渡すこともできます。例えば、リポジトリルートにある`.env`ファイルを使用するには、次のコマンドでサーバーを起動できます:

```bash
node --env-file=.env server.js
```

または、コマンドラインから直接環境変数を渡すこともできます:

```bash
SILLYTAVERN_LISTEN=true SILLYTAVERN_PORT=8000 node server.js
```

環境変数の使用の詳細については、[Node.jsドキュメント](https://nodejs.org/en/learn/command-line/how-to-read-environment-variables-from-nodejs)を参照してください。

## Data設定

| 設定 | 説明 | デフォルト | 許可される値 |
|---------|-------------|---------|-----------------|
| `dataRoot` | ユーザーデータストレージのルートディレクトリ（standaloneモードのみ） | `./data` | 任意の有効なディレクトリパス |
| `skipContentCheck` | 新しいデフォルトコンテンツのチェックをスキップします | `false` | `true`, `false` |
| `enableDownloadableTokenizers` | オンデマンドのtokenizerダウンロードを有効にします | `true` | `true`, `false` |

## Logging設定

| 設定 | 説明 | デフォルト | 許可される値 |
|---------|-------------|---------|------------------|
| `logging.minLogLevel` | ターミナルに表示する最小ログレベル | `0` (DEBUG) | (DEBUG = 0, INFO = 1, WARN = 2, ERROR = 3) |
| `logging.enableAccessLog` | サーバーアクセスログをファイルとコンソールに書き込みます | `true` | `true`, `false` |

## [Network設定](/Administration/remote-connections.md)

| 設定 | 説明 | デフォルト | 許可される値 |
|---------|-------------|---------|-----------------|
| `listen` | 受信接続のリッスンを有効にします | `false` | `true`, `false` |
| `port` | サーバーリスニングポート | `8000` | 任意の有効なポート番号 (1-65535) |
| `protocol.ipv4` | IPv4プロトコルでのリッスンを有効にします | `true` | `true`, `false`, `auto` |
| `protocol.ipv6` | IPv6プロトコルでのリッスンを有効にします | `false` | `true`, `false`, `auto` |
| `listenAddress.ipv4` | 特定のIPv4アドレスでリッスンします | `0.0.0.0` | 有効なIPv4アドレス |
| `listenAddress.ipv6` | 特定のIPv6アドレスでリッスンします | `'[::]'` | 有効なIPv6アドレス |
| `dnsPreferIPv6` | DNS解決にIPv6を優先します | `false` | `true`, `false` |

## SSL設定

| 設定 | 説明 | デフォルト | 許可される値 |
|---------|-------------|---------|------------------|
| `ssl.enabled` | SSL/TLS暗号化とHTTPSプロトコルを有効にします | `false` | `true`, `false` |
| `ssl.keyPath` | SSL秘密鍵へのパス（サーバーディレクトリからの相対パス） | `"./certs/privkey.pem"` | 有効なファイルパス |
| `ssl.certPath` | SSL証明書へのパス（サーバーディレクトリからの相対パス） | `"./certs/cert.pem"` | 有効なファイルパス |
| `ssl.keyPassphrase` | SSL秘密鍵のパスフレーズ。不要な場合は空のままにします | `""` | 任意の文字列 |

## Security設定

### IP Whitelisting

| 設定 | 説明 | デフォルト | 許可される値 |
|---------|-------------|---------|-----------------|
| `whitelistMode` | IP whitelistフィルタリングを有効にします | `true` | `true`, `false` |
| `enableForwardedWhitelist` | whitelistに登録されたIPの転送ヘッダーをチェックします | `true` | `true`, `false` |
| `whitelist` | 許可されたIPアドレスのリスト | `["::1", "127.0.0.1"]` | 有効なIPアドレスの配列 |
| `whitelistDockerHosts` | Docker host IPを自動的にwhitelistに追加します | `true` | `true`, `false` |

### Host Whitelisting

| 設定 | 説明 | デフォルト | 許可される値 |
|---------|-------------|---------|-----------------|
| `hostWhitelist.enabled` | host whitelistingを有効にします | `false` | `true`, `false` |
| `hostWhitelist.scan` | 信頼されていないホストからの受信リクエストをログに記録します | `true` | `true`, `false` |
| `hostWhitelist.hosts` | 信頼されたホスト名のリスト | `[]` | 有効なホスト名の配列 |

### Securityオーバーライド

!!!danger
**セキュリティ対策を無効にすることは強く推奨されません。変更を加える前に、何をしているかを理解していることを確認してください。**
!!!

| 設定 | 説明 | デフォルト | 許可される値 |
|---------|-------------|---------|------------------|
| `allowKeysExposure` | UIでマスクされていないAPIキーの公開を許可します | `false` | `true`, `false` |
| `disableCsrfProtection` | CSRF保護を無効にします（非推奨） | `false` | `true`, `false` |
| `securityOverride` | 起動時のセキュリティチェックを無効にします（非推奨） | `false` | `true`, `false` |

## [ユーザー認証](/Administration/multi-user.md)

| 設定 | 説明 | デフォルト | 許可される値 |
|---------|-------------|---------|-----------------|
| `basicAuthMode` | basic認証を有効にします | `false` | `true`, `false` |
| `basicAuthUser.username` | Basic認証のユーザー名 | `"user"` | 任意の文字列 |
| `basicAuthUser.password` | Basic認証のパスワード | `"password"` | 任意の文字列 |
| `enableUserAccounts` | マルチユーザーモードを有効にします | `false` | `true`, `false` |
| `enableDiscreetLogin` | ログイン画面でユーザーリストを非表示にします | `false` | `true`, `false` |
| `sessionTimeout` | ユーザーセッションのタイムアウト（秒） | `-1` (無効) | 任意の数値 (-1で無効化、0でブラウザを閉じたとき、>0でタイムアウト) |
| `perUserBasicAuth` | basic認証にアカウント認証情報を使用します | `false` | `true`, `false` |

### SSO自動ログイン

| 設定 | 説明 | デフォルト | 許可される値 |
|---------|-------------|---------|------------------|
| `sso.autheliaAuth` | Autheliaベースの自動ログインを有効にします。参照: [SSO](/Administration/sso.md) | `false` | `true`, `false` |
| `sso.authentikAuth` | Authentikベースの自動ログインを有効にします。参照: [SSO](/Administration/sso.md) | `false` | `true`, `false` |

## Rate Limiting設定

| 設定 | 説明 | デフォルト | 許可される値 |
|---------|-------------|---------|------------------|
| `rateLimiting.preferRealIpHeader` | レート制限にソケットIPの代わりにX-Real-IPヘッダーを使用します | `false` | `true`, `false` |

## Request Proxy設定

| 設定 | 説明 | デフォルト | 許可される値 |
|---------|-------------|---------|-----------------|
| `requestProxy.enabled` | 発信リクエストのプロキシを有効にします | `false` | `true`, `false` |
| `requestProxy.url` | プロキシサーバーURL | `null` | 有効なプロキシURL（例: `"socks5://username:password@example.com:1080"`） |
| `requestProxy.bypass` | プロキシをバイパスするホスト | `["localhost", "127.0.0.1"]` | ホスト名/IPの配列 |

## CORS Proxy設定

!!!
有効なCORSプロキシは一部の拡張機能で必要になる場合があります。組み込み機能では必要ありません。
!!!

| 設定 | 説明 | デフォルト | 許可される値 |
|---------|-------------|---------|-----------------|
| `enableCorsProxy` | CORSプロキシミドルウェアを有効にします | `false` | `true`, `false` |

## Browser Launch設定

> 以前は「Autorun」設定として知られていました。

| 設定 | 説明 | デフォルト | 許可される値 |
|---------|-------------|---------|-----------------|
| `browserLaunch.enabled` | サーバー起動時にブラウザを自動的に開きます | `true` | `true`, `false` |
| `browserLaunch.browser` | URLを開くために使用するブラウザ | `"default"` | `"default"`, `"chrome"`, `"firefox"`, `"edge"`, `"brave"` |
| `browserLaunch.hostname` | ブラウザ起動のホスト名を上書きします | `"auto"` | `"auto"`, 任意の有効なホスト名（例: `"localhost"`, `"st.example.com"`） |
| `browserLaunch.port` | ブラウザ起動のポートを上書きします | `-1` | `-1` (サーバーポートを使用)、任意の有効なポート番号 |
| `browserLaunch.avoidLocalhost` | 起動URLで'localhost'の使用を避けます | `false` | `true`, `false` |

## Performance設定

| 設定 | 説明 | デフォルト | 許可される値 |
|---------|-------------|---------|------------------|
| `performance.lazyLoadCharacters` | キャラクターデータを遅延ロードします | `true` | `true`, `false` |
| `performance.useDiskCache` | キャラクターカードのディスクキャッシュを有効にします | `true` | `true`, `false` |
| `performance.memoryCacheCapacity` | 最大メモリキャッシュ容量 | `100mb` | 人間が読める形式のサイズ（例: `100mb`, `1gb`） |

## Cache Buster設定

!!!warning
localhostまたはHTTPSを持つドメインが必要です。それ以外の場合は機能しません！
!!!

| 設定 | 説明 | デフォルト | 許可される値 |
|---------|-------------|---------|------------------|
| `cacheBuster.enabled` | 最初の読み込み時または画像ファイルのアップロード後にブラウザキャッシュをクリアします | `false` | `true`, `false` |
| `cacheBuster.userAgentPattern` | 指定された正規表現パターンに一致するユーザーエージェントのみキャッシュをクリアします。例: `'firefox'` (大文字小文字を区別しない) | `''` | 任意の有効な正規表現文字列 |

## Thumbnailing設定

| 設定 | 説明 | デフォルト | 許可される値 |
|---------|-------------|---------|-----------------|
| `thumbnails.enabled` | サムネイル生成を有効にします | `true` | `true`, `false` |
| `thumbnails.quality` | JPEGサムネイルの品質 | `95` | 0-100 |
| `thumbnails.format` | サムネイルの画像フォーマット | `jpg` | `jpg`, `png` |
| `thumbnails.dimensions.bg` | 背景サムネイルのサイズ | `[160, 90]` | 2つの数値の配列（幅、高さ） |
| `thumbnails.dimensions.avatar` | アバターサムネイルのサイズ | `[96, 144]` |  2つの数値の配列（幅、高さ） |
| `thumbnails.dimensions.persona` | ペルソナサムネイルのサイズ | `[96, 144]` |  2つの数値の配列（幅、高さ） |

## Backup設定

| 設定 | 説明 | デフォルト | 許可される値 |
|---------|-------------|---------|-----------------|
| `backups.chat.enabled` | 自動チャットバックアップを有効にします | `true` | `true`, `false` |
| `backups.chat.checkIntegrity` | 保存前にチャットファイルの整合性を検証します | `true` | `true`, `false` |
| `backups.common.numberOfBackups` | 保持するバックアップの数 | `50` | 任意の正の整数 |
| `backups.chat.throttleInterval` | バックアップスロットル間隔（ミリ秒） | `10000` | 任意の正の整数 |
| `backups.chat.maxTotalBackups` | 保持する最大チャットバックアップ数 | `-1` | 任意の正の整数または-1 |

## [Extensions設定](/extensions/index.md)

| 設定 | 説明 | デフォルト | 許可される値 |
|---------|-------------|---------|-----------------|
| `extensions.enabled` | UI拡張機能を有効にします | `true` | `true`, `false` |
| `extensions.autoUpdate` | 拡張機能を自動更新します（拡張機能マニフェストで有効になっている場合） | `true` | `true`, `false` |
| `extensions.models.autoDownload` | モデルの自動ダウンロードを有効にします | `true` | `true`, `false` |
| `extensions.models.classification` | 分類用のHuggingFaceモデルID | `"Cohee/distilbert-base-uncased-go-emotions-onnx"` | 有効なモデルID |
| `extensions.models.captioning` | 画像キャプション用のHuggingFaceモデルID | `"Xenova/vit-gpt2-image-captioning"` | 有効なモデルID |
| `extensions.models.embedding` | 埋め込み用のHuggingFaceモデルID | `"Cohee/jina-embeddings-v2-base-en"` | 有効なモデルID |
| `extensions.models.speechToText` | 音声認識用のHuggingFaceモデルID | `"Xenova/whisper-small"` | 有効なモデルID |
| `extensions.models.textToSpeech` | 音声合成用のHuggingFaceモデルID | `"Xenova/speecht5_tts"` | 有効なモデルID |

## [Server Plugins](/For_Contributors/Server-Plugins.md)

| 設定 | 説明 | デフォルト | 許可される値 |
|---------|-------------|---------|-----------------|
| `enableServerPlugins` | サーバーサイドプラグインを有効にします | `false` | `true`, `false` |
| `enableServerPluginsAutoUpdate` | 起動時にサーバープラグインの自動更新を試みます | `true` | `true`, `false` |

## [API統合設定](/Usage/API_Connections/index.md)

### OpenAI設定

| 設定 | 説明 | デフォルト | 許可される値 |
|---------|-------------|---------|-----------------|
| `promptPlaceholder` | 空のプロンプトのデフォルトメッセージ | `"[Start a new chat]"` | 任意の文字列 |
| `openai.randomizeUserId` | API呼び出しのユーザーIDをランダム化します | `false` | `true`, `false` |
| `openai.captionSystemPrompt` | キャプション補完のシステムメッセージ | `""` | 任意の文字列 |

### MistralAI設定

| 設定 | 説明 | デフォルト | 許可される値 |
|---------|-------------|---------|-----------------|
| `mistral.enablePrefix` | 返信のプリフィルを有効にします。**プレフィックスはレスポンスにエコーバックされます** | `false` | `true`, `false` |

### Ollama設定

| 設定 | 説明 | デフォルト | 許可される値 |
|---------|-------------|---------|-----------------|
| `ollama.keepAlive` | モデルのキープアライブ期間（秒） | `-1` | `-1` (無期限)、`0` (即座にアンロード)、正の整数 |
| `ollama.batchSize` | 生成リクエストの"num_batch"（バッチサイズ）パラメータを制御します | `-1` | `-1` (モデルのデフォルト)、正の整数 |

### Claude設定

!!!warning **重要！**

静的でリクエスト間で変更されないプロンプトプレフィックスの場合にのみ、注意して使用してください。\{\{random\}\}マクロ、lorebooks、vectors、summariesなどはキャッシュを無効化する可能性が高く、キャッシュミスで単にお金を無駄にすることになります。動作は予測不可能であり、保証はできませんし、されません。

参照: [Prompt Caching](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching)
!!!

| 設定 | 説明 | デフォルト | 許可される値 |
|---------|-------------|---------|-----------------|
| `claude.enableSystemPromptCache` | システムプロンプトキャッシュを有効にします | `false` | `true`, `false` |
| `claude.cachingAtDepth` | メッセージ履歴キャッシュを有効にします | `-1` | `-1` (無効)、`0`または正の整数 |
| `claude.extendedTTL` | デフォルトの5分の代わりに1時間のTTLを使用します。これによりリクエストのコストも増加することに注意してください。 | `false` | `true`, `false` |

### Google AI Studio設定

| 設定 | 説明 | デフォルト | 許可される値 |
|---------|-------------|---------|-----------------|
| `gemini.apiVersion` | APIエンドポイントバージョン | `v1beta` | `v1beta`, `v1alpha` |

### DeepL設定

| 設定 | 説明 | デフォルト | 許可される値 |
|---------|-------------|---------|-----------------|
| `deepl.formality` | 翻訳の形式レベル | `"default"` | `"default"`, `"more"`, `"less"`, `"prefer_more"`, `"prefer_less"` |
