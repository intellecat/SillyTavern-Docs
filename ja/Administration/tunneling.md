---
label: VPNとトンネリング
order: -40
icon: lock
route: /administration/tunneling/
---

VPNとトンネルは、世界中のどこからでもホームネットワークに安全にアクセスする方法です。このガイドでは、VPNまたはトンネルを使用して、どこからでもSillyTavernインスタンスにアクセスする方法を説明します。

## 方法

1. **自家製VPN**を使用する。

   いくつかのルーターには、ルーター管理ページでVPNサーバー(主にOpenVPNまたはWireGuard)をホストする機能が付属しています。VPNを設定し、デバイスをVPNに追加するには、ルーターのマニュアルを参照してください。接続したら、SillyTavern用に設定したプライベートIPにアクセスするだけで、問題なく接続できます。ユーザーやWindows使用者にとってより簡単です。

2. [**Cloudflare Zero Trust**](https://developers.cloudflare.com/cloudflare-one/)を使用する。

   Cloudflare Zero Trustは、50ユーザーまで追加できるCloudflareの無料の組織機能です。これにより、Cloudflareを介してトラフィックがプロキシされ、`cloudflared`を使用してST PCをトンネルとして追加することで、自宅にいるかのようにSTインスタンスに接続できます。

   トンネルを作成した後、Cloudflare Zero Trustを使用して外出先でローカルアクセスを完全に行うには、ルーターのプライベートIPアドレスへのルートを追加し、IP CIDR値を計算する必要があることに注意してください。

3. スタンドアロンの**Cloudflare**または**[ngrok](https://ngrok.com)**トンネルを使用する。

   AIバックエンドが接続できる方法と同様に、Cloudflare Tunnel経由でSTインスタンスに接続し、Cloudflare Tunnelページを開くこともできます。ただし、外出先でSTを使用するたびに、CloudflareまたはNGROKによって生成された新しいリンクをコピーして貼り付ける必要があります。

4. **Tailscale**を使用する。

   TailscaleはPCへの安全なリモート接続を可能にするVPNプロバイダーです。

## Tailscaleのセットアップ

TailscaleはPCへの安全なリモート接続を可能にするVPNプロバイダーです。Tailscaleサーバーのオープンソース実装が存在し、[Headscale](https://github.com/juanfont/headscale)を使用してサーバーをホストすることもできますが、それはこのチュートリアルの範囲外です。

### 1. アカウントの作成

* [TailscaleのWebサイト](https://tailscale.com/)にアクセスし、新しいアカウントを作成します。

**注意:** 1人による日常的な使用の場合、Tailscaleは永久に無料です。隠れたコストを恐れる場合は、支払いオプションを追加しないでください。

### 2. クライアントのセットアップ

* [Tailscaleのダウンロードページ](https://tailscale.com/download)にアクセスし、SillyTavernを実行しているデバイスとリモートロケーションから使用するデバイスにクライアント/アプリをダウンロードします。
* 以前に作成したアカウントで両方のデバイスにログインします。
* [Tailscaleの管理ページ](https://login.tailscale.com/admin/machines)にアクセスし、両方のデバイスを承認します。
* 接続された両方のデバイスの名前をメモします。

### 3. デバイスをホワイトリストに追加

* [ホワイトリストに登録されたIPの管理](./remote-connections.md#whitelist-based-access-control)に従って、接続デバイスのマシン名(SillyTavernで使用するもの)をSillyTavernのホワイトリストに追加します。

### 4. 接続

どこからでもSillyTavernを使用したい場合は、次の手順を実行するだけです:

* SillyTavernをホストしているPCとリモートで使用したいデバイスの両方でTailscaleをオンにします。
* 接続したいデバイスでブラウザを開き、`http://<STを実行しているPCのマシン名>:8000/`にアクセスします。

### 5. 友達とSillyTavernインスタンスを共有(オプション)

* 友達に自分のTailscaleアカウントを作成し、デバイスにクライアントをダウンロードするよう伝えます。
* [Tailscaleの管理ページ](https://login.tailscale.com/admin/machines)にアクセスします。
* SillyTavernをホストしているPC上の3点ボタンにマウスを合わせて「Share...」を押すか、3点ボタンを押して「Sharing settings...」を押します。
* 「Allow use as an exit node」のチェックを外します(友達がPCを介してすべてのインターネットトラフィックをルーティングできるようにしたい場合を除く)。
* メールとしてリンクを送信するか、タブを「Copy share link」に変更し、同じテキストの大きな青いボタンを押して、他の方法で友達に送信します。
* 共有リンクをクリックすると、友達のTailscaleネットワークにあなたのPCがポップアップ表示されます。
* 最後のステップで説明したように、SillyTavernにアクセスするために使用するのと同じリンクを友達に送信します。

**注意:** これにより、友達はSillyTavern、automatic1111など、PC上でローカルに実行されているサービスに完全にアクセスできるようになります。本当に信頼できる友達にのみこれを行ってください。
