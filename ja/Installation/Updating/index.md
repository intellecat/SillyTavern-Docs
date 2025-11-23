---
label: 更新
icon: repo-pull
order: -1
expanded: false
route: /installation/updating/
---

# SillyTavernの更新方法

以下から自分のOSを見つけて、STを更新する手順に従ってください。

!!! インストール手順については、[Installation](/Installation/index.md)ページを参照してください。

このガイドは、SillyTavernを既にインストールして少なくとも1回実行したことを前提としています。
!!!

----

## Linux/Termux or MacOS

gitを使用してインストールしたはずなので、SillyTavernディレクトリ内で'git pull'を実行するだけです。

- `cd SillyTavern`で正しいフォルダに入ります。
- `git pull`で更新を取得します。
- `./start.sh`または`bash start.sh`でSTを起動します。

----

## Windows

>まず、SillyTavernインストールベースフォルダにある`UpdateAndStart.bat`を使用してみてください。

それが失敗した場合は、ここに戻って続きを読んでください。

### 方法1 - GIT

常にユーザーには'git'を使用してインストールすることを推奨しています。その理由は次のとおりです:

`git clone`を使用してインストールした場合、更新するには[STフォルダのコマンドラインで](https://www.google.com/search?q=how+to+open+command+prompt+in+a+folder)`git pull`と入力するだけです。
または、コマンドプロンプトで問題が発生した場合（およびGitHub Desktopをインストールしている場合）、`Repository`メニューを使用して`Pull`を選択できます。

更新は自動的かつ安全に適用されます。

#### 「ZipでインストールしましたがGitインストールに変換したい」

賢明な選択です。

インストールがZip経由で行われたため、gitを使用して新しいインストールを行う必要があります。

幸いにも、その方法についての[手順](/Installation/Windows.md)があります。

gitを使用して別のフォルダに新しいSillyTavernをインストールしたら、このページに戻って以下の'Zip Update'手順の**ステップ4**に進んでください。

### 方法2 - ZIP

zipでインストールすることに固執する場合は、更新の面倒なプロセスは次のとおりです:

1. 新しいreleaseのzipをダウンロードします。
2. 現在のSTインストールの外のフォルダに解凍します。
3. OSの通常のセットアップ手順を実行して、NodeJS要件をインストールします。

4. 必要に応じて(*)、古いSTインストールから次のファイル/フォルダをコピーします:

    (*) '必要に応じて' = "それらのフォルダに関連するカスタムコンテンツを作成した場合"。

    #### 1.12.0以降への更新

    `/data`ディレクトリと`config.yaml`ファイルを、あるインストールから別のインストールにコピーします。保持したいサーバー全体のextensions（"All users"用にインストールされたもの）がある場合は、`/public/scripts/extensions/third-party`ディレクトリもコピーします。

    #### 1.12.0未満から1.12.0以降への更新

    1.12.0には自動移行手順が含まれています。以下の手順は、移行が中断されたかエラーが発生した場合*のみ*必要です。

5. 更新されたサーバーインストールを少なくとも1回実行して、`/data/default-user`ディレクトリを作成します。
6. 必要に応じて、古い`/public`から新しい`/data/default-user`にファイルを転送します。

    フォルダはどれも必須ではないため、必要なものだけをコピーしてください。

    **注意：/PUBLIC/フォルダ全体をコピーしないでください**

    そうすると、新しいインストールが壊れて、新機能が表示されなくなる可能性があります。

    ```plaintext
    Assets
    Backgrounds
    Characters
    Chats
    Context
    Groups
    Group chats
    Instruct
    movingUI
    KoboldAI Settings
    NovelAI Settings
    OpenAI Settings
    QuickReplies
    TextGen Settings (textgen = ooba)
    Themes
    User Avatars
    Worlds
    User
    settings.json
    secrets.json <---- これはベースフォルダにあり、/public/内ではありません
    ```

7. これらのフォルダ/ファイルがコピーされたら、新しいインストールの/data/default-userフォルダに貼り付けます（secrets.jsonはフォルダルートに配置します）。
8. OSに適した方法でSillyTavernを再度起動し、正しく行われたことを祈ってください。
9. すべてが表示されたら、古いSTフォルダを安全に削除できます。

### 一般的な更新の問題

#### 「作業ディレクトリに未解決の競合があります」

これは、リモートリポジトリで変更されたデフォルトファイル（設定プリセットなど）を変更したことを意味します。

これを修正するには、terminalでこれを実行します。破壊的になる可能性があるため、慎重に使用してください。必要に応じて、必ずバックアップを作成してください。

```bash
git merge --abort
git reset --hard
git pull --rebase --autostash
```

#### ファイルの変更がgit pullを妨げる

- SillyTavernシステムファイルを変更すると、`git pull`が機能しない場合があります。
- 更新で重要なファイルを変更する必要がある場合があり、同じ問題が発生する可能性があります。
- 通常、デフォルトのプリセットファイルまたは`package-lock.json`です。
- この場合、ファイルを別のフォルダに移動（またはファイルを削除）してから、`git pull`を実行できます。
- 別の解決策は、`git pull --rebase --autostash`を使用することです

#### エラー：サーバー起動時に「モジュール "***" が見つかりません」

- これは、SillyTavernが新しいnpmパッケージ要件を追加したことを意味します。
- SillyTavernディレクトリで`npm install`を実行してこれを修正します。提供されているStart.batおよびstart.shスクリプトは、これを自動的に実行します。
- 役に立たない？node_modulesフォルダを削除してください

**Windows**

```bash
rmdir /s /q node_modules
npm cache clean --force
npm install
```

**Unix/Linux**

```bash
rm -rf node_modules
npm cache clean --force
npm install
```

## Docker

1. terminalウィンドウを開き、dockerディレクトリに移動します`cd SillyTavern/docker`
2. `docker compose down`でコンテナを削除します
3. キャッシュからSillyTavern dockerイメージを削除します`docker rmi ghcr.io/sillytavern/sillytavern:latest`（stagingブランチを対象としている場合は、`sillytavern:latest`を`sillytavern:staging`に置き換えます。）
4. `sudo docker compose up -d`でコンテナを再ビルドします

すべてがスムーズに進めば、dockerはイメージの再ダウンロードを開始し、すぐに稼働するはずです。問題が発生した場合は、このガイドの次のセクションを参照してください。

### Common Update Problems
#### Dockerを使用していて、更新後にすべてのデータが消えました！

1.12.0で導入された新しいデータモデルのボリュームマッピングを更新するには、[DockerコンテナのMigrationガイド](/Installation/Updating/ST-1.12.0-Migration-Guide.md#containerized-docker-installs)に従う必要があります

#### dockerコマンド実行時に権限が拒否されました

これはLinuxの問題であり、権限が適切に設定されていないことを意味します。これを回避する方法は2つあります:

1. **簡単な方法**：ユーザーにsudoアクセスがある場合は、コマンドの前に`sudo`を付けるだけです（例：`sudo docker compose down`）
2. **適切な方法**：権限を修正します。これは、使用しているLinuxのバージョンによって異なります。この問題を修正するためのガイドがオンラインにたくさんあります。
