---
order: 10
label: Windows
route: /installation/windows/
---
# Windowsインストール

!!!warning
Windowsが管理するフォルダ（Program Files、System32など）にはインストールしないでください。

管理者権限でSTART.BATを実行しないでください

WINDOWS 7へのインストールは不可能です。NODEJS 18.16が実行できないためです
!!!

## Gitを使用したインストール

1. [NodeJS](https://nodejs.org/en)をインストールします（最新のLTSバージョンを推奨）
2. [Git for Windows](https://gitforwindows.org/)をインストールします
3. Windows Explorer (`Win+E`)を開きます
4. Windowsが管理または監視していないフォルダを参照または作成します（例：C:\MySpecialFolder\）
5. 上部の「アドレスバー」をクリックし、`cmd`と入力してEnterを押すことで、そのフォルダ内でコマンドプロンプトを開きます。
6. 黒いボックス（コマンドプロンプト）が表示されたら、次のいずれか1つを入力してEnterを押します:

   - Releaseブランチの場合: `git clone https://github.com/SillyTavern/SillyTavern -b release`
   - Stagingブランチの場合: `git clone https://github.com/SillyTavern/SillyTavern -b staging`

7. すべてがcloneされたら、`Start.bat`をダブルクリックして、NodeJSが必要なものをインストールします。
8. その後、サーバーが起動し、SillyTavernがブラウザに表示されます。

## SillyTavern Launcherを使用したインストール

1.  キーボードで**`WINDOWS + R`**を押してRun dialogボックスを開きます。次に、以下のコマンドを実行してgitをインストールします:
    ```shell
    cmd /c winget install -e --id Git.Git
    ```
2. キーボードで**`WINDOWS + E`**を押してFile Explorerを開き、launcherをインストールしたいフォルダに移動します。目的のフォルダに入ったら、アドレスバーに`cmd`と入力してEnterを押します。次に、以下のコマンドを実行します:
   ```shell
    git clone https://github.com/SillyTavern/SillyTavern-Launcher.git && cd SillyTavern-Launcher && start installer.bat
    ```

## GitHub Desktopを使用したインストール
（これにより、GitHub Desktopでのみgitが使用できるようになります。コマンドラインでも`git`を使用したい場合は、[Git for Windows](https://gitforwindows.org/)もインストールする必要があります）

1. [NodeJS](https://nodejs.org/en)をインストールします（最新のLTSバージョンを推奨）
2. [GitHub Desktop](https://central.github.com/deployments/desktop/desktop/latest/win32)をインストールします
3. GitHub Desktopをインストールした後、`Clone a repository from the internet....`をクリックします（注意：このステップでGitHubアカウントを作成する必要は**ありません**）

    ![image](/static/windows-1.png)

4. メニューでURLタブをクリックし、このURL `https://github.com/SillyTavern/SillyTavern`を入力して、Cloneをクリックします。Local pathを変更して、SillyTavernがダウンロードされる場所を変更できます。

    ![image](/static/windows-2.png)

5. SillyTavernを開くには、Windows Explorerを使用して、リポジトリをcloneしたフォルダを参照します。デフォルトでは、リポジトリはここにcloneされます: `C:\Users\[Your Windows Username]\Documents\GitHub\SillyTavern`

6. `start.bat`ファイルをダブルクリックします。（注意：OSによってファイル名の`.bat`部分が非表示になっている場合があります。その場合、"`Start`"というファイルに見えます。これをダブルクリックしてSillyTavernを実行します）

    ![image](/static/windows-3.png)

7. ダブルクリックすると、大きな黒いコマンドコンソールウィンドウが開き、SillyTavernが動作に必要なものをインストールし始めます。

8. インストールプロセスの後、すべてが正常に動作している場合、コマンドコンソールウィンドウは次のようになり、SillyTavernタブがブラウザで開きます:

    ![image](/static/windows-4.png)

9. [サポートされているAPI](/Usage/API_Connections/index.md)のいずれかに接続して、チャットを開始してください！
