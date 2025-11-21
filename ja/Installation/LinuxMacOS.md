---
label: MacOS & Linux
order: 5
route: /installation/linuxmacos/
---

# Linux/MacOS Install

## Manual Git install

MacOS / Linuxでは、これらすべてをTerminalで実行します。

1. gitとnodeJSをインストールします（方法はOSによって異なります）
2. リポジトリをcloneします

   - Releaseブランチの場合: `git clone https://github.com/SillyTavern/SillyTavern -b release`
   - Stagingブランチの場合: `git clone https://github.com/SillyTavern/SillyTavern -b staging`

3. `cd SillyTavern`を実行して、インストールフォルダに移動します。
4. 次のいずれかのコマンドで`start.sh`スクリプトを実行します:

- `./start.sh`
- `bash start.sh`

## SillyTavern Launcher

### For Linux users
1. お気に入りのterminalを開き、gitをインストールします
2. 次のコマンドでSillytavern Launcherをダウンロードします: `git clone https://github.com/SillyTavern/SillyTavern-Launcher.git`
3. 次のコマンドでSillyTavern-Launcherに移動します: `cd SillyTavern-Launcher`
4. 次のコマンドでinstall launcherを起動し、インストールしたいものを選択します: `chmod +x install.sh && ./install.sh`
5. インストール後、次のコマンドでlauncherを起動します: `chmod +x launcher.sh && ./launcher.sh`

### For Mac users
1. terminalを開き、次のコマンドでbrewをインストールします: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
2. 次のコマンドでgitをインストールします: `brew install git`
3. 次のコマンドでSillytavern Launcherをダウンロードします: `git clone https://github.com/SillyTavern/SillyTavern-Launcher.git`
4. 次のコマンドでSillyTavern-Launcherに移動します: `cd SillyTavern-Launcher`
5. 次のコマンドでinstall launcherを起動し、インストールしたいものを選択します: `chmod +x install.sh && ./install.sh`
6. インストール後、次のコマンドでlauncherを起動します: `chmod +x launcher.sh && ./launcher.sh`
