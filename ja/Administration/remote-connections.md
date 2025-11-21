---
icon: rss
order: -30
route: /usage/remoteconnections/
---

# リモート接続

これは主に、同じWiFiネットワーク内でPCがSTサーバーを実行している間に、モバイルフォンでSillyTavernを使用したい人のためのものです。

また、ローカルネットワーク外からのリモート接続を許可するための最初のステップでもあります。

!!!warning
STサーバーをインターネットに公開するためにポートフォワーディングを使用すべきではありません。代わりに、Cloudflare Zero Trust、ngrok、TailscaleなどのVPNまたはトンネリングサービスを使用してください。詳細については、[VPNとトンネリング](tunneling.md)ガイドを参照してください。
!!!

!!!danger 免責事項
**適切なセキュリティ対策を確保せずに、インスタンスをオープンインターネットにホストしないでください。**

**不適切または不十分なセキュリティ実装による不正アクセスの場合の損害または損失について、当社は一切責任を負いません。**
!!!

## リモート接続の許可

デフォルトでは、STサーバーは実行されているマシン(localhost)からの接続のみを受け付けます。他のデバイスからの接続を待ち受けるには、`config.yaml`の`listen`オプションを`true`に設定します。

!!! SillyTavernフォルダで`config.yaml`を直接検索すると、2つのファイルが見つかる場合があります。
このドキュメントの`config.yaml`へのすべての変更は、SillyTavernルートディレクトリ(/SillyTavern/config.yaml)にあるものを参照しており、`/SillyTavern/default/config.yaml`ではありません。
!!!

```yaml
# Listen for incoming connections
listen: true
```

STがリモート接続を待ち受けている場合、コンソールに次のメッセージが表示されます:

```txt
SillyTavern is listening on IPv4: 0.0.0.0:8000
```

そしてその意味についての説明が表示されます。

STがリモート接続を待ち受けて**いない**場合、コンソールに次のメッセージが表示されます:

```txt
SillyTavern is listening on IPv4: 127.0.0.1:8000
```

## アクセス制御の設定

リモート接続の待ち受けを有効にした後、少なくとも1つのアクセス制御方法を設定する必要があります。そうしないと、サーバーは起動しません。

### ホワイトリストベースのアクセス制御

ホワイトリスト経由でアクセス制御を有効にするには、SillyTavernルートディレクトリ(`/SillyTavern/config.yaml`)の`config.yaml`ファイルを編集します:

1. 必要な設定ファイルを生成するために、少なくとも一度SillyTavernを起動します。
2. テキストエディタで`/SillyTavern/config.yaml`を開きます。
3. `whitelist`セクションを見つけて、許可したいIPアドレスを追加します:
    * 各IPアドレスを個別にリストします。
    * `127.0.0.1`を含めてください。含めないとホストマシンから接続できなくなります。
    * 個別のIP、CIDRマスク(例: `10.0.0.0/24`)、ワイルドカード(`*`)範囲をサポートしています。
4. `config.yaml`ファイルを保存します。
5. **SillyTavernサーバーを再起動します。**

#### `config.yaml`ホワイトリスト設定例

1. ローカルネットワーク上の任意のデバイスを許可:

    ```yaml
    whitelist:
      - ::1
      - 127.0.0.1
      - 10.0.0.0/8
      - 172.16.0.0/12
      - 192.168.0.0/16
    ```

    ローカルネットワークのアドレス範囲がわからない場合は、上記のホワイトリストを使用してください。

2. 2つの特定のデバイスの接続を許可:

    ```yaml
    whitelist:
      - ::1
      - 127.0.0.1
      - 192.168.0.2
      - 192.168.0.5
    ```

3. `192.168.0.*`サブネット上の任意のデバイスの接続を許可:

    ```yaml
    whitelist:
      - ::1
      - 127.0.0.1
      - 192.168.0.*
    ```

4. すべてのIPv4デバイスのネットワーク接続を許可:

    ```yaml
    whitelist:
      - 0.0.0.0/0
    ```

### ホワイトリストベースのアクセス制御の無効化

ホワイトリスト経由のアクセス制御を無効にするには:

* `/SillyTavern/config.yaml`の`whitelistMode`を`false`に設定します。
* SillyTavernベースインストールフォルダにある`whitelist.txt`を削除または名前変更します(存在する場合)。
* SillyTavernサーバーを再起動します。

### 非推奨: `whitelist.txt`の使用

!!!info
`whitelist.txt`が存在する場合、`config.yaml`のホワイトリスト設定よりも優先されます。

ただし、他のすべての設定は`config.yaml`内で管理されており、`whitelist.txt`は権限の問題が発生したりロックされたりする可能性があるため、システムは`config.yaml`のホワイトリストを使用するように暗黙的に戻る可能性があります。

**config.yamlを直接編集する方が、よりシンプルで信頼性が高いです。**
!!!

それでもwhitelist.txtを使用したい場合:

1. SillyTavernベースインストールフォルダに`whitelist.txt`という名前の新しいテキストファイルを作成します。
2. テキストエディタで開き、許可するIPアドレスを追加します。
3. ファイルを保存し、SillyTavernサーバーを再起動します。

#### `whitelist.txt`設定例

```txt
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
127.0.0.1
::1
```

これにより、ローカルネットワーク上の任意のデバイスが接続できるようになります。

### HTTP Basic Authenticationによるアクセス制御

!!!warning
HTTP Basic Authenticationは強力なセキュリティを提供しません。

ブルートフォース攻撃を防ぐためのレート制限はありません。これが懸念される場合は、TLSとレート制限を備えたリバースプロキシ、および専用の[認証サービス](sso.md)を使用することをお勧めします。
!!!

サーバーは、クライアントがHTTP経由で接続するたびにユーザー名とパスワードを要求します。**これは、リモート接続(listen: true)が有効になっている場合にのみ機能します。**

HTTP BAを有効にするには、SillyTavernベースディレクトリの`config.yaml`を開き、`basicAuthMode`を検索します。basicAuthModeをtrueに設定し、ユーザー名とパスワードを設定します。注意: `config.yaml`は、STが少なくとも一度実行されている場合にのみ存在します。

```yaml
basicAuthMode: true
basicAuthUser:
  username: "MyUsername"
  password: "MyPassword"
```

または、次のようにbasic authを有効にすることもできます:

```yaml
basicAuthMode: true
enableUserAccounts: true
perUserBasicAuth: true
```

この`perUserBasicAuth`モードでは、basic authのユーザー名とパスワードは、パスワードを持つ有効なマルチユーザーアカウントと同じになります。さらに、SillyTavernはそのアカウントに直接ログインします。**`perUserBasicAuth`を有効にする前に、パスワードを持つアカウントがあることを確認してください。**

ファイルを保存し、既に実行中の場合はSillyTavernを再起動します。STに接続すると、ユーザー名とパスワードの入力を求められます。ユーザー名とパスワードの両方がプレーンテキストで送信されます。これが懸念される場合は、HTTPS経由でSTを提供できます。

### ホストホワイトリスト

HTTPSなしでネットワーク経由でサーバーをホストする場合、リクエストホスト検証を有効にすることを強くお勧めします。これにより、DNSリバインディングなどのさまざまな攻撃を防ぐことができます。デフォルトでは、SillyTavernサーバーは認識されていないホストからの最初の接続時にコンソールメッセージをログに記録します。

### ホストホワイトリストの切り替え

ホストホワイトリストを有効にするには、SillyTavernルートディレクトリの`config.yaml`ファイルを編集します:

```yaml
hostWhitelist:
    enabled: true
```

### 信頼できるホストの追加

信頼できるホストのリストにホスト名を追加するには、`hostWhitelist.hosts`セクションに含めます:

!!!tip ヒント
`localhost`やIP(`127.0.0.1`や`::1`など)は追加しないでください。これらは常に信頼されているものと見なされます。

ホストの範囲を追加するには、先頭にドットを使用します。たとえば、`.trycloudflare.com`を追加すると、`trycloudflare.com`と`example.trycloudflare.com`のようなサブドメインが信頼されます。
!!!

```yaml
hostWhitelist:
  hosts:
    - "example.com"
    - ".trycloudflare.com"
```

### コンソールメッセージの切り替え

認識されていないホストのコンソールメッセージを無効にするには、`hostWhitelist.scan`オプションを`false`に設定します:

```yaml
hostWhitelist:
    scan: false
```

## SillyTavernインスタンスへの接続

### STホストマシンのIPアドレスの取得

ホワイトリストが設定された後、STホスティングデバイスのIPが必要になります。

STホスティングデバイスが同じwifiネットワーク上にある場合、STホストの内部wifi IPを使用します:

* Windowsの場合: windowsボタン > 検索バーに`cmd.exe`と入力 > コンソールに`ipconfig`と入力し、Enterキーを押す > `IPv4`リストを探します。

あなた(または他の誰か)が同じネットワーク上にいない間にホストされたSTに接続したい場合、STホスティングデバイスのパブリックIPが必要になります。

* STホスティングデバイスを使用している間に、[このページ](https://whatismyipaddress.com/)にアクセスし、`IPv4`を探します。これは、リモートデバイスから接続するために使用するものです。

### STサーバーへの接続

状況に応じて最終的に取得したIPを、リモートデバイスのWebブラウザにそのIPアドレスとポート番号を入力します。

同じwifiネットワーク上のSTホストの典型的なアドレスは次のようになります:

`http://192.168.0.5:8000`

https://ではなく、http://を使用してください

### 接続ログ

サーバーへの新しい接続は、コンソールウィンドウに表示され、SillyTavernデータディレクトリの`access.log`ファイルに記録されます。

サーバーと同じマシン上のブラウザのコンソールメッセージは次のようになります:

```txt
New connection from 127.0.0.1; User Agent: ...
```

サーバーと同じネットワーク上の別のマシン上のブラウザのコンソールメッセージは次のようになる場合があります:

```txt
New connection from 192.168.116.187; User Agent: ...
```

接続が拒否された場合、コンソールメッセージは次のようになります:

```txt
New connection from 192.168.116.211; User Agent: ...

Forbidden: Connection attempt from 192.168.116.211. If you are attempting to connect,
please add your IP address in whitelist or disable whitelist mode in config.yaml in
root of SillyTavern folder.
```

`access.log`には、タイムスタンプ付きの接続情報が含まれますが、接続が受け入れられたか拒否されたかは含まれません。

### トラブルシューティング

まだ接続できませんか?

* 接続の試行が[コンソールに表示される](#connection-logging)が、禁止されている場合は、[ホワイトリストの問題](#whitelist-based-access-control)です。
* STがリモート接続を待ち受けているが、接続の試行がコンソールに表示されない場合は、[ネットワークの問題](#network-issues)です。
* STがリモート接続を待ち受けていない場合は、[読み取りの問題](#allowing-remote-connections)です。

#### ネットワークの問題

* Windowsでは、アプリケーションがアプリケーションファイアウォールによってブロックされている可能性があります。これを修正する最も簡単な方法は、node.jsをアンインストールして再インストールし、ファイアウォールから求められたときにネットワークへのアクセスを許可することです。そうしないと、Windowsアプリケーションファイアウォールを介してnode.jsアプリケーションを手動で許可する必要があります。
* Windows 11では、設定 > ネットワークとインターネット > イーサネットでプライベートネットワークプロファイルタイプを有効にします。これはWindows 11では非常に重要です。そうしないと、前述のファイアウォールルールがあっても接続できません。
* Linuxでは、ファイアウォールを介してポートを許可する必要がある場合があります。これを行うコマンドは`sudo ufw allow 8000`です。これにより、ポート8000でのトラフィックが許可されます。

ルーターのポートフォワーディング設定を変更しないでください。これは、ローカルネットワーク内でSTにアクセスするためには必要なく、サーバーをインターネットに公開する可能性があります。

[ローカルネットワーク外](remote-connections.md)からSTサーバーにアクセスしようとしていて、うまくいかない場合は、問題がリモートデバイスとトンネル/VPNエンドポイントの間にあるのか、サーバー上のトンネルエンドポイントとSTサービスの間にあるのかを特定してください。そうしないと、間違ったことのトラブルシューティングに多くの時間を費やすことになります。

## HTTPS

### TLS/SSLでSillyTavernを起動

!!!tip
SSLは`config.yaml`ファイルを使用して設定することもできます: [SSL設定](/Administration/config-yaml.md#ssl-configuration)。
!!!

STインスタンスへの、およびSTインスタンスからのトラフィックを暗号化するには、`--ssl`フラグを使用してサーバーを起動します。

例:

```bash
node server.js --ssl
```

デフォルトでは、STは`certs`フォルダ内で証明書を検索します。ファイルが他の場所にある場合は、`--keyPath`および`--certPath`引数を使用できます。

例:

```bash
node server.js --ssl --keyPath /home/user/certificates/privkey.pem --certPath /home/user/certificates/cert.pem
```

SillyTavernを実行しているユーザーには、証明書ファイルの読み取り権限が必要です。

### 証明書の取得方法

証明書を取得する最も簡単で迅速な方法は、[certbot](https://letsencrypt.org/getting-started/)を使用することです。
