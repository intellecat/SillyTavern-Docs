---
order: 50
icon: package
expanded: true
route: /installation/
---

# Installation

お使いのプラットフォームに応じたインストールガイドに従ってください:

* [Windows](/Installation/Windows.md)
* [Linux and Mac](/Installation/LinuxMacOS.md)
* [Android](/Installation/Android.md)
* [Docker](/Installation/Docker.md)

## Branches

SillyTavernは、すべてのユーザーにスムーズな体験を提供するため、2ブランチシステムを使用して開発されています。

* `release` -🌟 **ほとんどのユーザーに推奨。** これは最も安定した推奨ブランチで、メジャーリリースがプッシュされたときのみ更新されます。大多数のユーザーに適しています。通常、月に1回更新されます。
* `staging` - ⚠️ **一般的な使用には推奨されません。** このブランチには最新の機能がありますが、いつでも動作しなくなる可能性があるため注意が必要です。パワーユーザーや熱心なユーザー向けです。1日に数回更新されます。

## Global / Standalone mode

SillyTavernの実行モードには、設定とデータのパスの扱い方が異なる2つのモードがあります。

* **Standalone mode** (デフォルト) - サーバーディレクトリ内の`config.yaml`ファイルと`data`ディレクトリを使用します。すべてのデータはインストールパスに制限されます。これはほとんどのユーザーに推奨されるモードです。
* **Global mode** - 設定とデータにシステム全体のパスを使用します。これは、SillyTavernをパッケージとしてインストールする場合や、複数のインストール間で同じ設定とデータを共有したい場合に便利です。

!!!info
[公式npmパッケージ](https://www.npmjs.com/package/sillytavern)を使用してインストールした場合（例：`npx sillytavern@latest`）、デフォルトでglobalモードで実行されます。
!!!

### Data paths

**Standalone mode**のパスは、SillyTavernインストールディレクトリからの相対パスです:

* **Config path**: `./config.yaml`
* **Data root**: `./data/`

**Global mode**のパスはOSに依存します:

* **Linux**: `~/.local/share/SillyTavern/config.yaml` (または`$XDG_DATA_HOME/SillyTavern/config.yaml`) および`~/.local/share/SillyTavern/data/` (または`$XDG_DATA_HOME/SillyTavern/data/`)
* **Windows**: `%APPDATA%\SillyTavern\config.yaml` および`%APPDATA%\SillyTavern\data\`
* **MacOS**: `~/Library/Application Support/SillyTavern/config.yaml` および`~/Library/Application Support/SillyTavern/data/`

### How to run in global mode

!!!warning
globalモードで実行する場合、`dataRoot`と`configPath`は[CLI引数](../Administration/config-yaml.md#command-line-arguments)または[config.yaml](../Administration/config-yaml.md)で上書きできません。
!!!

1. サーバー起動コマンドに`--global`引数を渡します（例：`node server.js --global`）。
2. シェル起動スクリプトに`--global`引数を渡します（例：`Start.bat --global`または`./start.sh --global`）。
3. `package.json`ファイル内の`start:global`スクリプトを使用します（例：`npm run start:global`）。
