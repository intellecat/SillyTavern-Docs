---
order: 112
route: /installation/st-1.12.0-migration-guide/
---

# 1.12.0 Migration Guide

SillyTavern 1.12.0（コードネーム"Neo Server"アップデート）には、SillyTavernの使用方法に影響を与える可能性のあるいくつかの重要な変更が含まれています。

このガイドは、更新の準備をし、さらなるガイダンスを提供します。

## Data storage update

1.12.0は、SillyTavernがユーザーデータを処理する方法を変更します。

以前は、すべての永続データは`/public`ディレクトリ内のフロントエンド部分と一緒に保存されていましたが、これにより混乱と潜在的な障害ポイントが発生し、コンテナ化とシステム全体のアプリインストールが非常に困難になっていました。

### What's changed?

`/public`からのsettingsやchats（以下の完全なリスト）などのすべての永続情報は、設定可能なパスを持つ別のディレクトリに移動され、webサーバー自体から独立してポータブルになりました。互換性のために必要な場合、例えばextensionsのホスティング、フルサイズのキャラクターカード、ユーザーの画像アップロードなどの場合、dataディレクトリからユーザーファイルを自動的にホストするためのスマートリダイレクトが設定されています。

### Setting a data root

`config.yaml`またはサーバーを`--dataRoot`コンソール引数で起動することにより、data rootへの絶対パスまたは相対パス（STリポジトリディレクトリからの）を指定できます。

> YAML example

```yaml
# -- DATA CONFIGURATION --
# Root directory for user data storage
dataRoot: C:\Users\Harry\Documents\ST-Data
```

> Console example

```bash
node server.js --dataRoot="/Users/harry/ST-Data"
# OR
npm run start -- --dataRoot="/Users/harry/ST-Data"
```

デフォルトのdata rootパスは`./data`で、これはSillyTavernのリポジトリ内の`data`ディレクトリを意味します。

!!!info Note
data rootパスは、**完全な絶対**パスまたは**完全な相対**パスである必要があります。`~`や`%APP_DATA%`のようなパスショートカットは使用_できません_。これらはshellによって解決され、オペレーティングシステムによって解決されません。
!!!

### Migration

#### **重要！** 始める前に

1. **デフォルトの場所からdataRootを移動したい場合のみ。それ以外の場合は、この部分をスキップしてください。** 更新をpullした後、サーバーを最初に実行する_前に_、data rootを設定します。`npm install`を実行して`config.yaml`に新しい値を入力するか、コンソール引数を渡します。
2. すべてのデータは`default-user`アカウントに移行されます。以下の[Users](#users)を参照してください。

#### Containerless (bare metal) installs

何もする必要はありません！自動移行は、STサーバーを起動して古いストレージ形式を検出したときに（`/public/characters`ディレクトリの存在を確認することで）、すべてを処理します。

ファイルを移動する際、自動バックアップが`/backups/_migration/YYYY-MM-DD`（現在の日付に解決される）ディレクトリに作成されますが、移行を実行する前に完全な手動バックアップを作成することは常に良い習慣です。

#### Containerized (Docker) installs

Dockerボリュームでのデータの移行は少し難しいですが、非常に簡単です。リポジトリで提供されている`docker-compose.yml`は変更を反映するように更新されましたが、カスタムワークフロー/デプロイメントを調整する必要がある場合があります。

**Step 1.** 新しいボリュームを作成し、コンテナ内の"/home/node/app/data"パスにマウントします。`config`ボリュームは削除しないでください。

```yaml
volumes:
    - "./config:/home/node/app/config"
    - "./data:/home/node/app/data"
```

**Step 2.** `config`ボリュームから`config.yaml`ファイル以外のすべてを、`data`ボリュームの`default-user`サブディレクトリに移動します。

**Step 3.** コンテナを再ビルドして起動します。

!!!info Note
`/public`ディレクトリと`config`ボリューム間のソフトリンクはもはや必要なく、Dockerコンテナに組み込まれていません！
!!!

#### What to migrate?

次のファイルとディレクトリがデータ移行の対象です。デフォルトの設定を想定すると、beforeとafterのパスは以下の表に記載されています。

| Before                                 | After                                |
|----------------------------------------|--------------------------------------|
| /secrets.json                          | /data/default-user/secrets.json      |
| /thumbnails                            | /data/default-user/thumbnails        |
| /vectors                               | /data/default-user/vectors           |
| /public/settings.json                  | /data/default-user/settings.json     |
| /public/stats.json                     | /data/default-user/stats.json        |
| /public/assets                         | /data/default-user/assets            |
| /public/backgrounds                    | /data/default-user/backgrounds       |
| /public/characters                     | /data/default-user/characters        |
| /public/chats                          | /data/default-user/chats             |
| /public/context                        | /data/default-user/context           |
| /public/scripts/extensions/third-party | /data/default-user/extensions        |
| /public/group chats                    | /data/default-user/group chats       |
| /public/groups                         | /data/default-user/groups            |
| /public/instruct                       | /data/default-user/instruct          |
| /public/KoboldAI Settings              | /data/default-user/KoboldAI Settings |
| /public/movingUI                       | /data/default-user/movingUI          |
| /public/NovelAI Settings               | /data/default-user/NovelAI Settings  |
| /public/OpenAI Settings                | /data/default-user/OpenAI Settings   |
| /public/QuickReplies                   | /data/default-user/QuickReplies      |
| /public/TextGen Settings               | /data/default-user/TextGen Settings  |
| /public/themes                         | /data/default-user/themes            |
| /public/worlds                         | /data/default-user/worlds            |
| /default/content/content.log           | /data/default-user/content.log       |

## Users

1.12.0は、同じサーバー上にマルチユーザーセットアップを作成する（完全にオプションの）機能を追加し、複数のユーザーが同時であっても、独自の完全に分離されたSillyTavernインスタンスを使用できるようにします。ユーザーアカウントは、追加のプライバシー層のためにパスワード保護することもできます。

詳細については、[Users](/Administration/multi-user.md)ドキュメントを参照してください。
