---
title: マルチユーザーモード
icon: people
order: -10
route: /administration/multi-user/
---

Multi-userモードは、複数の人が1つのSillyTavernサーバーを使用できるようにします。各ユーザーは独自の設定、拡張機能、データを持ちます。ユーザーアカウントはパスワード保護することもできます。

!!!warning
ユーザーパスワードは、マルチユーザー設定のユーザー間で基本的なプライバシーを提供します。これはセキュリティ機能ではなく、そのように考えるべきではありません。すべてのユーザーデータ（チャット履歴、APIキー、その他の機密情報を含む）は、サーバー上にプレーンテキストで保存されます。サーバーのファイルシステムにアクセスできる人は誰でも閲覧および変更できます。**公開サーバーまたは信頼できないユーザーとSillyTavernを使用しないでください。**
!!!

## 設定

Multi-userモードを有効にして使用するには、`config.yaml`ファイルを編集します:

```yaml
# Enable multi-user mode
enableUserAccounts: true
# Enable discreet login mode: hides user list on the login screen
enableDiscreetLogin: true
```

1. ユーザーアカウント設定が無効の場合、ユーザーデータの保存には`default-user`フォールバック管理者アカウントが使用されます。
2. discreetログイン設定が無効の場合、アクティブなユーザーのリストがログイン画面に表示されます。有効にすると、ユーザーは自分のhandleを手動で入力する必要があります。

!!!info
ユーザーリストから`default-user`アカウントを_削除_することはできません。これは、`enableUserAccounts`が`false`に設定されている場合にユーザーデータの提供に使用されるためです。ただし、リストから非表示にしてログインを禁止するために_無効化_することはできます。
!!!

## ユーザーハンドル

handleはユーザーの一意の識別子です。小文字、数字、ダッシュのみで構成できます。

ユーザーデータディレクトリへのパスは、次のパターンを使用します: `%DATA_ROOT%/%USER_HANDLE%`。

有効なuser handlesの例:

- default-user
- juan555
- flux-the-cat
- cool-guy1337

## ロール

- **Admin** - 他のユーザーを管理（作成、削除、変更）できます。すべてのユーザーのために拡張機能をインストールできます。
- **User** - 他のユーザーを管理できません。自分自身のためにのみ拡張機能をインストールできます。

管理パネルへのアクセスを除き、両方のユーザーロールは機能的に同一であり、制限なくSillyTavernの全機能を使用できます。ユーザー権限の実装はTBDです。

すべてのユーザーアカウントは最初に通常ユーザーとして作成され、必要に応じて管理者に昇格できます。

## ログイン画面

そこで使用するユーザーアカウントを選択できます。`enableDiscreetLogin`設定値に応じて2つのスタイルがあります。

アクティブなユーザーが1人だけで、パスワード保護されていない場合、ログイン画面はバイパスされ、表示されません。

## ユーザープロファイル

トップメニューバーの「User settings」パネルの下にある「Account」ボタンを使用して、アカウント自己管理メニューにアクセスできます。

1. Display name - ログイン画面で使用され、変更可能です。personasとは関連せず、AI APIには表示されません - 好きなだけpersonasを使用できます。
2. Profile picture - ログイン画面で使用されます。カスタム画像、デフォルトのpersona画像（設定されている場合）、または最後に使用したpersonaを使用できます。
3. Password - ロックアイコンがアカウント保護状態を反映します（開いた鍵 = パスワードなし）。「Change Password」ボタンを使用してパスワードを設定、変更、または削除できます。
4. Settings Snapshots - `settings.json`ファイルのバックアップにアクセスして確認でき、snapshotの作成または復元が可能です。
5. Download Backup - ユーザーデータフォルダのアーカイブをダウンロードします。
6. Reset Settings - 他のデータ（キャラクター、チャット）をそのままにして、工場出荷時のデフォルト設定にリセットします。

## パスワード回復

1. パスワードはログイン画面から回復できます。ワンタイム回復コード（4桁で構成）を取得するには、サーバーコンソールへのアクセスが必要です。
2. または、user handleを提供することで、SillyTavernサーバーのユーティリティスクリプトを使用してパスワードをリセットできます。

```txt
Usage: node recover.js [account] (password)
Example: node recover.js admin SecurePassword
```

## コンテンツスキャフォールディング

ユーザーにカスタムコンテンツを追加するには、content scaffolding機能を使用できます。この機能により、サーバーの起動時に各ユーザーのデータディレクトリにコピーされるファイルのセットを定義できます。

この機能を動作させるには、`/default/scaffold`ディレクトリに`index.json`ファイルを作成する必要があります。構文はdefaultコンテンツと同じです。すべてのファイルパスは`/default/scaffold`ディレクトリに対して相対的である必要があり、サブディレクトリを使用してファイルを整理できます。

Scaffoldされたファイルはdefaultファイルの前にコピーされます。つまり、同じファイル名を持つdefaultファイル（presets/settings/など）を上書きします。

!!!tip
すべてのユーザーデータディレクトリには、scaffoldおよびdefaultディレクトリからコピーされたすべてのファイルをリストする`content.log`ファイルがあります。次回の再起動時にサーバーにコンテンツを再度同期させるには、このファイルを削除します。
!!!

## 認識されるコンテンツタイプ

| Type                          | Value                |
|-------------------------------|----------------------|
| settings.json                 | `'settings'`         |
| Character card                | `'character'`        |
| Character sprites             | `'sprites'`          |
| Background image              | `'background'`       |
| World Info file               | `'world'`            |
| Persona avatar                | `'avatar'`           |
| UI theme                      | `'theme'`            |
| ComfyUI workflow              | `'workflow'`         |
| KoboldAI Classic preset       | `'kobold_preset'`    |
| Chat Completion preset        | `'openai_preset'`    |
| NovelAI preset                | `'novel_preset'`     |
| Text Completion preset        | `'textgen_preset'`   |
| Instruct Mode template        | `'instruct'`         |
| Context Formatting template   | `'context'`          |
| MovingUI preset               | `'moving_ui'`        |
| Quick Replies set             | `'quick_replies'`    |
| System Prompt template        | `'sysprompt'`        |
| Reasoning Formatting template | `'reasoning'`        |

## 例 (`/default/scaffold/index.json`)

```json
[
    {
        "filename": "themes/Midnight.json",
        "type": "theme"
    },
    {
        "filename": "backgrounds/city.png",
        "type": "background"
    },
    {
        "filename": "characters/Charlie.png",
        "type": "character"
    }
]
```
