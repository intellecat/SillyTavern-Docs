---
order: 109
route: /installation/updating/migration-guide-1-09/
---

# 1.9.0 Migration Guide

## How to migrate to a new branch if I use main/dev?

_**新規インストールを行うことをお勧めします。**_ ただし、既存のSillyTavernのコピーを使用したい場合は、以下の手順に従ってください。

**重要！** 何かを行う前に、インストールの*完全なバックアップ*を作成してください。プロセス中にデータを*失う*可能性があるため、この警告を無視しないでください。

どのファイルをバックアップするか不明ですか？こちらのリストを参照してください: [How to Update SillyTavern](/Installation/Updating/index.md#updating-from-1120-to-1120)

### git installs

1. SillyTavernインストールフォルダでterminalプロンプト（cmd、PowerShell、Termuxなど）を開きます。
2. `git fetch`と入力してから`git pull`と入力して更新をpullします。
3. 設定が失われる可能性があります。バックアップを作成しましたか？`git switch release`または`git switch staging`でブランチがそれぞれ変更されます
4. エラーがない場合は、次の項目にスキップします。次のようなものが表示される場合があります:
   ```
   error: Your local changes to the following files would be overwritten by checkout:
        config.conf
        public/css/bg_load.css
        public/settings.json
   ```
   影響を受けるファイルのリストが表示されます。それらの設定ファイルが置き換えられても構わない場合は、`git switch -f release`または`git switch -f staging`でブランチが設定されます。
   それらの変更を保存したい場合は、バックアップから復元してください。

5. `npm install`と入力してから`npm run start`と入力して、すべてが正しく動作することをテストします。
6. 楽しんでください！必要に応じて、バックアップからデータを復元してください。

### fatal: invalid reference: release

これは、古いremoteから単一のブランチをcloneした場合（organizationリポジトリへの移行前）に発生する可能性があります。これを修正するには、新しいremoteからブランチを追加してfetchする必要があります:

```
git remote add st https://github.com/SillyTavern/SillyTavern
git fetch st
git checkout -t st/release
```

その後、ステップ5から続行します。

### ZIP installs

あなたには何も変わりません。通常どおりにブランチ/release ZIPをダウンロードするだけです。
