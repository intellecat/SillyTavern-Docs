---
# icon: container
label: Docker
route: /installation/docker/
---

# Dockerインストール

!!!
この手順は、Dockerがインストールされており、コンテナのインストールのためにコマンドラインにアクセスでき、その一般的な操作に精通していることを前提としています。
!!!

## GitHub Container Registryの使用

ビルド済みイメージを使用することが、DockerでSillyTavernを始める最速かつ最も簡単な方法です。GitHub Container Registryから最新のイメージをpullできます。

### Docker Compose (推奨)

[GitHubリポジトリ](https://github.com/SillyTavern/SillyTavern/blob/release/docker/docker-compose.yml)から`docker-compose.yml`ファイルをダウンロードし、ファイルが配置されているディレクトリで次のコマンドを実行します。これにより、GitHub Container Registryから最新のreleaseイメージがpullされ、コンテナが起動し、必要なボリュームが自動的に作成されます。

```sh
docker compose up
```

ファイルを編集して、ニーズに合わせて追加のカスタマイズを適用できます:

- デフォルトのポートは8000です。`ports`セクションを変更することで変更できます。
- 安定版releaseの代わりに開発ブランチを使用したい場合は、`image`タグを`staging`に変更します。
- 環境変数を使用してサーバー設定を調整したい場合は、[Environment Variables](/Administration/config-yaml.md#environment-variables)ページを確認してください。

### Docker CLI (上級者向け)

SillyTavernを機能させるには、2つの必須ディレクトリマッピングと1つのポートマッピングが必要です。コマンドで、次の場所で選択したものを置き換えます:

#### コンテナ変数

##### ボリュームマッピング

- `CONFIG_PATH` - ホストマシン上でSillyTavern設定ファイルが保存されるディレクトリ
- `DATA_PATH` - ホストマシン上でSillyTavernユーザーデータ（キャラクターを含む）が保存されるディレクトリ
- `PLUGINS_PATH` - （オプション）ホストマシン上でSillyTavernサーバープラグインが保存されるディレクトリ
- `EXTENSIONS_PATH` - （オプション）ホストマシン上でグローバルUI extensionsが保存されるディレクトリ

##### ポートマッピング

- `PUBLIC_PORT` - トラフィックを公開するポート。これは、仮想マシンコンテナの外部からインスタンスにアクセスするため必須です。セキュリティのための別のサービスを実装せずに、これをインターネットに公開しないでください。

##### 追加設定

- `SILLYTAVERN_VERSION` - [GitHub Packagesページ](https://github.com/SillyTavern/SillyTavern/pkgs/container/sillytavern)で、タグ付きイメージバージョンのリストを確認できます。イメージタグ"latest"は、現在のreleaseで最新の状態を保ちます。それぞれのブランチのnightlyイメージを指す"staging"も利用できます。

#### コンテナの実行

1. コマンドラインを開きます
2. 設定ファイルとデータファイルを保存したいフォルダで、次のコマンドを実行します:

```bash
SILLYTAVERN_VERSION="latest"
PUBLIC_PORT="8000"
CONFIG_PATH="./config"
DATA_PATH="./data"
PLUGINS_PATH="./plugins"
EXTENSIONS_PATH="./extensions"

docker run \
  --name="sillytavern" \
  -p "$PUBLIC_PORT:8000/tcp" \
  -v "$CONFIG_PATH:/home/node/app/config:rw" \
  -v "$DATA_PATH:/home/node/app/data:rw" \
  -v "$EXTENSIONS_PATH:/home/node/app/public/scripts/extensions/third-party:rw" \
  -v "$PLUGINS_PATH:/home/node/app/plugins:rw" \
  ghcr.io/sillytavern/sillytavern:"$SILLYTAVERN_VERSION"
```

!!!tip
デフォルトでは、コンテナはフォアグラウンドで実行されます。バックグラウンドで実行したい場合は、`docker run`コマンドに`-d`フラグを追加してください。
!!!

## Dockerイメージのビルド

!!!info
次のセクションでは、非root（非管理者）フォルダにSillyTavernをインストールしたことを前提としています。rootフォルダにSillyTavernをインストールした場合、これらのコマンドの一部を管理者権限［`sudo`、`doas`、Command Prompt (Administrator)］で実行する必要がある場合があります。
!!!

Dockerイメージを自分でビルドしたい場合は、次の手順に従ってください。これは、イメージをカスタマイズしたり、開発目的で使用したりする場合に便利です。

### Linux

1. [こちら](https://docs.docker.com/engine/install/)のDockerインストールガイドに従ってDockerをインストールします。
   !!!danger
   Docker Desktopをインストール**しないでください**。
   !!!
2. Dockerの[Post-Installation Guide](https://docs.docker.com/engine/install/linux-postinstall/)の**Manage Docker as a non-root user**の手順に従ってください。
3. パッケージマネージャーを使用して[Git](https://git-scm.com/download/linux)をインストールします。

    - Debian (Ubuntu/Pop! OS/etc.)

        ```sh
        sudo apt install git
        ```

    - Arch Linux (Manjaro/EndeavourOS/etc.)

        ```sh
        sudo pacman -S git
        ```

    - Fedora, Red Hat Enterprise Linux (RHEL), etc.
        ```sh
        sudo dnf install git
        ```

4. SillyTavernリポジトリをcloneします。

    - Release (Stable Branch)

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    - Staging (Development Branch)
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

5. Dockerフォルダ内で次のコマンドを実行して`docker compose`を実行します。

    ```sh
    docker compose up -d
    ```

6. 新しいブラウザを開いて[http://localhost:8000](http://localhost:8000)にアクセスします。数秒でSillyTavernが読み込まれるはずです。

### Windows

!!!warning Regarding Docker on Windows
WindowsでDockerを使用することは**_本当に_**複雑です。_Turn Windows features on or off_内で_Windows Subsystem for Linux_を有効にするだけでなく、Virtualization（Intel VT-d/AMD SVM）のためにシステムを構成する必要があり、これはPCメーカーごと（またはマザーボードメーカーごと）に異なります。場合によっては、一部のシステムでこのオプションが存在しないことがあります。

[Windows](/Installation/Windows.md)ガイドに従ってSillyTavernをインストールすることを強くお勧めします。このセクションは、Windowsでどのように実行できるかの_おおまかな_アイデアです。
!!!

1.  [こちら](https://docs.docker.com/desktop/setup/install/windows-install/)のDockerインストールガイドに従ってDocker Desktopをインストールします。
2.  [Git for Windows](https://git-scm.com/download/win)をインストールします。
3.  SillyTavernリポジトリをcloneします。

    -   Release (Stable Branch)

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    -   Staging (Development Branch)
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

4.  Dockerフォルダ内で次のコマンドを実行して`docker compose`を実行します。

    ```sh
    docker compose up -d
    ```

5.  新しいブラウザを開いて[http://localhost:8000](http://localhost:8000)にアクセスします。数秒でSillyTavernが読み込まれるはずです。

### macOS

!!!
macOSはLinuxに似ていますが、Docker Engineがありません。Windowsと同様にDocker Desktopをインストールする必要があります。
MacにGitをインストールするには、[Homebrew](https://brew.sh/)もインストールする必要があります。このセクションは、macOSでどのように実行できるかの_おおまかな_アイデアです。
!!!

1.  [こちら](https://docs.docker.com/desktop/setup/install/mac-install/)のDockerインストールガイドに従ってDocker Desktopをインストールします。
2.  Homebrewを使用して`git`をインストールします。

    ```sh
    brew install git
    ```

3.  SillyTavernリポジトリをcloneします。

    -   Release (Stable Branch)

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    -   Staging (Development Branch)
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

4.  Dockerフォルダ内で次のコマンドを実行して`docker compose`を実行します。

    ```sh
    docker compose up -d
    ```

5.  新しいブラウザを開いて[http://localhost:8000](http://localhost:8000)にアクセスします。数秒でSillyTavernが読み込まれるはずです。

## SillyTavernの設定

SillyTavernの設定ファイル（config.yaml）は、`config`フォルダ内にあります。configファイルの設定は、Dockerなしで設定する場合と変わりありませんが、変更を保存するには、管理者権限で`nano`またはコードエディタを実行する必要があります。

!!!warning
変更を適用するために、SillyTavern用のDockerコンテナを忘れずに再起動してください！`docker`フォルダ内でこのコマンドを実行してください。

```sh
docker compose restart sillytavern
```

!!!

## ユーザーデータの場所

SillyTavernのdataフォルダは、`data`フォルダ内にあります。ファイルのバックアップは簡単に行えますが、復元またはコンテンツの追加には、管理者権限で行う必要がある場合があります。

## サーバープラグインの実行

[HoYoWiki-Scraper-TS](https://github.com/Bronya-Rand/HoYoWiki-Scraper-TS)や[SillyTavern-Fandom-Scraper](https://github.com/SillyTavern/SillyTavern-Fandom-Scraper)のようなpluginsをDocker内で実行することは、Dockerなしでシステムで実行する場合と変わりありませんが、そのためにはDocker Composeスクリプトにわずかな変更を加える必要があります。

!!! Note
`docker`フォルダ内に既に_plugins_フォルダが表示されている場合は、ステップ1-2をスキップできます。
!!!

1. `nano`またはコードエディタを使用して、_docker-compose.yml_を開き、`volumes`の下に次の行を追加します。

    ```sh
        volumes:
            - "./config:/home/node/app/config"
            - "./data:/home/node/app/data"
            - "./plugins:/home/node/app/plugins"
    ```

2. `docker`フォルダ内に_plugins_という新しいフォルダを作成します。
3. pluginのインストール手順に従ってください。
4. `nano`または管理者権限のコードエディタを使用して、_config.yaml_（`config`フォルダ内）を開き、`enableServerPlugins`を有効にします

    ```sh
    enableServerPlugins: true
    ```

5. Dockerコンテナを再起動します。

    ```sh
    docker compose restart sillytavern
    ```

## Dockerの一般的な問題

### マウントされたボリュームでのSELinux権限の問題

SELinuxが有効なLinuxディストリビューション（RHEL、CentOS、Fedoraなど）では、セキュリティポリシーのため、Dockerコンテナがマウントされたボリュームにアクセスできない場合があります。これにより、コンテナがマウントされたディレクトリへの読み取りまたは書き込みを試みるときに、permission deniedエラーが発生する可能性があります。

ボリュームマウントに`:z`または`:Z`の2つのサフィックスを追加できます。これらのサフィックスは、共有ボリューム上のファイルオブジェクトを再ラベル付けするようDockerに指示します。

- `z`オプションは、ボリュームコンテンツがコンテナ間で共有される場合に使用されます。
- `Z`オプションは、ボリュームコンテンツが現在のコンテナでのみ使用される場合に使用されます。

例:

```yaml
# docker-compose.yml
volumes:
  ## Shared volume
  - ./config:/home/node/app/config:z
  ## Private volume
  - ./data:/home/node/app/data:Z
```

### ホワイトリストによる禁止

!!!
[whitelistDockerHosts](/Administration/config-yaml.md#ip-whitelisting)設定値が`true`に設定されている場合、Docker gateway IPsは自動的にホワイトリストに登録されるはずです。

それでもSillyTavernにアクセスできない場合は、以下の手順に従って手動でホワイトリストを更新してください。
!!!

1. 次のDockerコマンドを実行して、SillyTavern DockerコンテナのIPを取得します。

    ```sh
    docker network inspect docker_default
    ```

    次のような出力が表示されるはずです。

    ```json
    [
        {
            "Name": "docker_default",
            "IPAM": {
                "Config": [
                    {
                        "Subnet": "172.18.0.0/16",
                        "Gateway": "172.18.0.1"
                    }
                ]
            }
        }
    ]
    ```

    _Gateway_に表示されているIPをコピーしてください。これは重要です。

2. 管理者権限でお好みのテキストエディタを実行し、`config`に移動して`config.yaml`を開きます。

    エディタ内で、`whitelist`セクションまで下にスクロールします。次のようなものが表示されるはずです。

    ```yaml
    whitelist:
        - 127.0.0.1
    ```

    _127.0.0.1_の下に新しい行を追加し、DockerからコピーしたIPを入力します。その後、次のようになるはずです。

    ```yaml
    whitelist:
        - 127.0.0.1
        - 172.18.0.1
    ```

    ファイルを保存してテキストエディタを終了します。

    !!!info
    Dockerネットワークをbridgeとして設定した場合、通常どおり外部IPアドレスをホワイトリストに追加することもできます。
    !!!

3. Dockerコンテナを再起動して、新しい設定を適用します。

    ```sh
    docker compose restart sillytavern
    ```
