---
order: -50
route: /installation/updating/node/
---

# How to update Node.js

セキュリティとパフォーマンスの理由から、Node.jsランタイムを最新の状態に保つことが重要です。以下は、オペレーティングシステムに応じてNode.jsを更新する手順です。

最新のLong Term Support (LTS)バージョンを使用することをお勧めします。これは[Node.js公式ウェブサイト](https://nodejs.org/en/about/previous-releases)で確認できます。

## How to check your current Node.js version

1. terminalまたはコマンドプロンプトを開きます。
2. 次のコマンドを入力してEnterを押します:

```bash
node -v
```

## nvm (Node Version Manager) - Cross-Platform

`nvm`を使用している場合:

1. terminalを開きます。
2. 次のコマンドを入力します:

[**Unix/Linux/macOS:**](https://github.com/nvm-sh/nvm)

```bash
nvm install --lts
nvm use --lts
```

[**Windows:**](https://github.com/coreybutler/nvm-windows)

```bash
nvm install lts
nvm use lts
```

## Windows - Regular Installation

1. Node.jsの[downloadページ](https://nodejs.org/en/download/)にアクセスします。
2. LTSバージョンのWindows Installerをダウンロードします。
3. インストーラーを実行し、プロンプトに従ってインストールを完了します。

## Windows - SillyTavern Launcher

SillyTavern Launcherを使用してインストールした場合:

1. SillyTavern Launcherを開きます。
2. `Toolbox / App Installer / Core Utilities / Install Node.js`に移動します。

**または:**

PowerShellでwingetを使用して手動で行います:

```powershell
winget install --id=OpenJS.NodeJS.LTS  -e
```

## Android - Termux

1. Termuxアプリを開きます。
2. 次のコマンドを入力します:

```bash
pkg update
pkg upgrade nodejs-lts
```

更新プロセス中に表示される可能性のあるプロンプトを、仮想キーボードで`Y`を押して受け入れることを忘れないでください。

## macOS - Regular Installation

1. Node.jsの[downloadページ](https://nodejs.org/en/download/)にアクセスします。
2. LTSバージョンのmacOS Installerをダウンロードします。
3. `.pkg`ファイルを実行し、プロンプトに従ってインストールを完了します。

## macOS - Homebrew

Homebrewがインストールされている場合、次のコマンドでNode.jsを更新できます:

```bash
brew update
brew upgrade node
```

## Linux - Package Manager

LinuxでNode.jsを更新する方法は、ディストリビューションによって異なります。

ただし、公式リポジトリのNode.jsのバージョンが最新でない可能性があるため、[Node Version Manager (nvm)](https://github.com/nvm-sh/nvm)または[NodeSourceリポジトリ](https://github.com/nodesource/distributions)を使用することをお勧めします。

## Docker

アクションは必要ありません。提供されているビルド済みDockerイメージは、最新バージョンのNode.jsでコンパイルされています。
