---
label: Android (Termux)
route: /installation/android-(termux)/
---

# Android (Termux)インストール

SillyTavernは、Termuxを使用してAndroidデバイス上でネイティブに実行できます。

## Termuxのインストール

!!!tip
Google Play StoreからTermuxをインストールすることは避けてください。そのバージョンはもはやメンテナンスされていません。
代わりに、F-Droid（推奨）またはGitHub releasesを使用して最新バージョンを入手してください。
!!!

1. [F-Droid](https://f-droid.org/en/packages/com.termux/)または[GitHub releases](https://github.com/termux/termux-app/releases)からTermuxをダウンロードします。
2. ダウンロードしたAPKファイルをインストールします。
3. Termuxを開き、最初のコマンドを実行します:

   ```bash
   termux-change-repo
   ```

4. "Mirror group"を選択し、最も近いサーバーを選択します。画面をタッチするか、[Unexpected Keyboard](https://play.google.com/store/apps/details?id=juloo.keyboard2&hl=en)でスワイプジェスチャーを使用できます。
5. Termuxを更新します:

   ```bash
   pkg update && pkg upgrade
   ```

## 依存関係のインストール

必要なパッケージをインストールします:

```bash
pkg install git nodejs-lts nano
```

!!!warning
32ビットAndroidを実行している場合は、追加の手順について以下の[Common Errors](#common-errors)セクションを参照してください。
!!!

## SillyTavernのインストール

SillyTavernリポジトリをcloneします（[ブランチの選択方法](/Installation/index.md#branches)）:

- **Releaseブランチ:**

    ```bash
    git clone https://github.com/SillyTavern/SillyTavern -b release
    ```

- **Stagingブランチ:**

    ```bash
    git clone https://github.com/SillyTavern/SillyTavern -b staging
    ```

## SillyTavernの実行

SillyTavernを実行するには、cloneしたディレクトリに移動してstartスクリプトを実行します:

```bash
cd ~/SillyTavern
bash start.sh
```

SillyTavernを更新するには、SillyTavernディレクトリに移動して次を実行します:

```bash
cd ~/SillyTavern
git pull --rebase --autostash
```

このプロセスを簡素化するショートカットを作成する方法については、以下の[Aliases](#optional-create-aliases)セクションを参照してください。

## 一般的なエラー

### Unsupported platform: android arm LEtime-web

32ビットAndroidでは、npmでインストールできない外部依存関係が必要です。

次のコマンドを使用してインストールします:

```bash
pkg install esbuild
```

その後、上記のインストール手順を続行します。

### パフォーマンス調整

!!!info
パフォーマンスを向上させるための一般的なヒントについては、該当する[FAQセクション](/Usage/faq.md#performance-tips)を参照してください。
!!!

Androidデバイスのハードウェア制限により、メモリ、ストレージ、CPU使用量を改善するために、次のSillyTavern [config.yaml](/Administration/config-yaml.md)設定を調整することをお勧めします:

```yaml
performance:
  # 必要になるまですべてのキャラクターデータの読み込みを避ける
  lazyLoadCharacters: true
  # ストレージ使用量を削減するためにディスクキャッシュを無効にする
  useDiskCache: false
backups:
  chat:
    # オプション：ストレージを節約するために自動チャットバックアップを無効にする
    enabled: false
```

!!!tip
Termuxに含まれている`nano`テキストエディタを使用して、`config.yaml`ファイルを編集します: `nano ~/SillyTavern/config.yaml`
!!!

## オプション：エイリアスの作成

ワークフローを簡単にするために、一般的なコマンドのショートカットを作成できます。

1. エディタを開いて`.bashrc`ファイルを変更します:

   ```bash
   nano ~/.bashrc
   ```

2. 次の行を追加してaliasを作成します:

   ```bash
   # Update Termux packages
   alias pkgup="pkg update && pkg upgrade"
   #Start SillyTavern
   alias st='cd ~/SillyTavern && bash start.sh'
   # Update SillyTavern
   alias stup='cd ~/SillyTavern && git pull --rebase --autostash'
   ```

3. ファイルを保存してエディタを終了します（nanoでは、`CTRL + X`を押してから、`Y`、次に`Enter`を押します）。

4. 変更を適用するには、次を実行します:

   ```bash
   source ~/.bashrc
   ```

これで、次のコマンドを使用できるようになります:

- `st` SillyTavernを起動
- `stup` SillyTavernを更新
- `pkgup` Termuxパッケージを更新

## 参考資料

!!!info
以下にリンクされているガイドは、SillyTavernチームによってメンテナンスされていません。
!!!

- SillyTavern in Termux guide by ArroganceComplex#2659: <https://rentry.org/STAI-Termux>
- Accessing Termux files with Material Files: <https://www.learntermux.tech/2020/10/Termux-File-Manager.html>
- Prevent Termux process deep sleep: <https://wiki.termux.com/wiki/Termux-wake-lock>
