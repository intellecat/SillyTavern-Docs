---
icon: gear
label: ローカルインストール
route: /extensions/extras/installation/
---

# Extrasインストール

このページには、ローカルデバイスにSillyTavern Extrasをインストールするための指示が含まれています。

!!! Discontinued
Extrasプロジェクトは2024年4月に中止され、新しいアップデートまたはモジュールを受け取りません。ほとんどのモジュールはメインSillyTavernアプリケーションでネイティブに利用可能。引き続きインストールして使用することもできますが、問題が発生した場合は即座にサポートを期待しないでください。
!!!

ローカルインストールはOS（特にTermux）で困難または不可能な場合があります。

## [公式Extras Colabを使用](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb)

* セットアップが簡単
* 無料で使用
* Colab GPU クレジットが不要（`use_cpu`オプション使用）
* 詳細については[Colabガイド ページ](/extensions/Extras/Installation.md#running-extras-in-colab)参照。

### Colabでの実行

* [公式Extras Colab](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb)開く
* 希望する"Extra"オプション選択
* `use_cpu`を選択してGPUクレジットを必要とせずExtrasを実行
  * Stable Diffusionは遅くなりますが、他はすべて通常に実行
* 必須ではありませんが推奨：共有インスタンスを保護するために`secure`オプション選択
* 左の開始ボタン（三角形"再生"ボタンのように見える）クリック
* すべてが読み込み終わるまで待機
* 出力下部にある`trycloudflare.com`リンク確認。localhost リンクは無視（機能しません）。
* 「実行中」というテキストで開始
* そのラインの下にリストされているAPI URL リンクをコピー。(**localhostのURL をコピーしないでください。別のものを使用。**)
* Extensions サポート有効化してSillyTavernを開始（必要に応じて`config.yaml`で`enableExtensions`を`true`に設定）
* SillyTavernの拡張機能メニュー（ページの上部の"積み重ねられたブロック"アイコンをクリック）に移動。
* コピーされたAPI URLを上部のボックスに貼り付け。(**API キーボックスには貼り付けないでください。**)
* `secure`オプションを有効にしていない場合、API キーボックスが完全に空白です（公式colabを使用する場合）。
* `secure`オプションを有効にしている場合、生成されたAPIキーを API キーボックスに貼り付け。
* APIキー colabのコンソール出力に表示される例：`Your API key is fee2f3f559`
* "接続"クリック

---

## ローカル インストール方法

### MiniConda（推奨）

このメソッドはCondaがExtras要件パッケージの"仮想環境"を作成するため推奨される"は、システム全体のPython セットアップに影響しません。

1. [Miniconda](https://docs.conda.io/en/latest/miniconda.html)インストール

    _(重要!)Condaの使用方法[読む](https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html)_

2. [git](https://git-scm.com/downloads)インストール

    _(SillyTavernをgitで最初からインストール済みのChadたちはこのステップをスキップできる。)_
    
    両方インストール後...
    
    `CONDA COMMAND PROMPT WINDOW`で以下のコマンド入力/貼り付け、各コマンド後に`Enter`。

3. 新しいConda環境（`extras`と呼ぶ）作成：

    `conda create -n extras`

4. 新しい環境を有効化

    `conda activate extras` (コマンドプロンプトの左側に`(extras)`が表示されるはず)

5. 必要なシステムパッケージをインストール（これには時間がかかります）

    `conda install python=3.11 git`

6. Extras GitHubリポをクローン

    `git clone https://github.com/SillyTavern/SillyTavern-extras`

7. クローンされたExtrasリポに移動

    `cd SillyTavern-extras`

8. Extras要件をインストール（また時間がかかります）。次のコマンドのいずれか**1つ**を使用：

   * `pip install -r requirements.txt` - 基本機能
   * `pip install -r requirements-rvc.txt` - リアルタイム音声クローニング
   * `pip install -r requirements-coqui.txt` - Coqui TTS（非推奨）

    [よくある問題](/extensions/Extras/Installation.md#extras-install-common-problems)ページこのステップでエラーが発生した場合参照！

9. 下を参照"インストール後Extrasを実行"

---

### システム全体のインストール

はるかに簡単ですが、システム全体のPythonインストール影響。

システムの多くのPythonプログラムが異なる要件を持つ場合、競合を引き起こす可能性があります。

これがPythonに初めてタッチしている場合は問題ではないはず。

1. Python 3.11インストール： <https://www.python.org/downloads/release/python-3115/>
2. git インストール： <https://git-scm.com/downloads>
3. コマンドプロンプト ウィンドウを開き、完全なアクセス権限があるフォルダに移動。
4. リポクローン： `git clone https://github.com/SillyTavern/SillyTavern-extras`、Enterキー。
5. クローン完了後、`cd SillyTavern-extras`入力し、Enterキー。
6. `python -m pip install -r requirements.txt`入力
7. 下を参照"インストール後Extrasを実行"

---

## インストール後Extrasを実行

### 拡張機能が有効になっていることを確認

1. テキスト エディターで`config.yaml`ファイルを開く。ファイルはSTの基本インストール フォルダーにあります。
2. `enableExtensions`と書かれた行を探す。
3. その行に`true`があることを確認（`false`ではなく）。

### 使用するモジュールの決定

（これは1回だけ行う必要があります）

* Extrasは常にPythonコマンド ラインで開始。
* `python server.py`は最小限ですが、有用なモジュールを有効にしません。
* モジュール有効化するには、`--enable-modules=`修飾子を使用し、モジュール名のコンマ区切りリストを使用する必要があります。

例：`python server.py --enable-modules=caption,summarize,classify`

これにより、イメージ キャプション、チャット サマリー、ライブ更新キャラクター式が有効化されます。

各モジュールについて説明するテーブルが以下にあります。

| Name | Description |
|------|-------------|
| `caption` | イメージ キャプション |
| `summarize` | テキスト要約 |
| `classify` | テキスト感情分類 |
| `sd` | Stable Diffusionイメージ生成 |
| `silero-tts` | [Silero TTSサーバー](https://github.com/ouoertheo/silero-api-server) |
| `edge-tts` | [Microsoft Edge TTSクライアント](https://github.com/rany2/edge-tts) |
| `chromadb` | ベクトルストレージサーバー |
| `coqui-tts` | Coqui TTS |
| `rvc` | リアルタイム音声クローニング |

* Pythonコマンド ラインに追加するモジュール決定。
* 次のステップで使用されます。

**注意：Pythonコマンドのモジュール リストに**スペースが全くないこと**を確認。**

### Extrasサーバー開始

Extrasインストール フォルダー内のコマンド プロンプト ウィンドウ内にいる間に...

1. Conda環境がアクティブ状態であることを確認（Conda インストール方法を使用している場合）
2. 環境がアクティブでない場合は`activate extras`入力。
3. `python server.py --enable-modules=YOUR,SELECTED,MODULE,LIST,HERE`入力
4. Extrasサーバーが読み込まれます。
5. しばらくしてURL表示。ローカルインストールの場合、デフォルト値は`http://localhost:5100`。
6. API URLをコピー。

### ST をExtrasサーバーに接続

1. SillyTavernサーバー開始し、ブラウザーでSillyTavern インターフェース表示。
2. 拡張機能パネル（ページ上部の"積み重ねられたブロック"アイコン）開く
3. 入力ボックスにAPI URLを貼り付け。
4. `接続`クリック。

Extras再度実行するために、単にコマンド プロンプトでこれらのコマンド実行。

`conda activate extras`、Enter キー。
`python server.py`、Enter キー。

サーバー.pyの追加オプションを確認してセットアップが必要な場合は含めてください。

## Windows向けの.bat ファイルを作成簡単スタートアップ向け

これはオプションで、Windowsのみに適用されますが、macOS 上で同様の操作が可能です。

1. Windows デスクトップ表示
2. 右クリック、`新規`を選択してから`テキスト ドキュメント`をクリック
3. デスクトップに新しいファイルが表示され、名前の入力が求められます。
4. ファイルに`STExtras.txt`名前を付ける
5. 新しく作成されたファイルをテキスト エディターで開く。
6. 次のコードを貼り付ける：

    ```
    cd C:\_your_\_full_\_Extras_\_folder_\_path_\
    call conda activate extras
    python server.py --enable-modules=YOUR,SELECTED,MODULE,LIST,HERE,WITH,NO,SPACES
    call conda deactivate
    pause
    ```

7. プレースホルダーフォルダー パスを実際のExtrasインストール フォルダー パスに置き換える。
8. Pythonコマンド ラインを実際のコマンド ラインに置き換える
9. ファイルを新しい名前`STExtras.bat`で保存（ほとんどのテキスト エディターで`ファイル` >> `名前を付けて保存`使用）

このSTExtras.batファイルダブルクリックするだけでExtras簡単に開始できます。

モジュール リストを変更したい場合（または他のコマンド ラインの修正器）、単に.bat ファイル内のPythonコマンドを編集。

## Extrasインストール一般的な問題

このセクションには、SillyTavern Extrasインストール中に遭遇するよくある質問と問題がリストされています。

### エラー：Linux上で"talkinghead" モジュールをインポートできませんでした

互換性の問題のためColabで自動的にインストールされていない場合のため、追加パッケージのインストールが必要。他の要件のインストール後、これを実行：

`pip install wxpython`

### Extrasサーバーが AUTOMATIC1111のStable Diffusion Web UIに接続できない

> Could not connect to remote SD backend at <http://127.0.0.1:7860>! Disabling SD module...

**webui-user.batで開始Stable Diffusionは COMMANDLINE_ARGS変数に--apiコマンド ライン オプション含むことを確認。**

そのlineを見つけて置き換え：`set COMMANDLINE_ARGS=--api`

![見た目](/static/extensions/sd-user.png)

SD Web UIのAPIモードが無効になっている場合、Extrasサーバーは接続できず、イメージ生成ができません！

#### まだ機能していませんか？

すべてがプロパー順序で読み込まれるのを待つ確認：

1. Stable Diffusion Web UI
2. SillyTavern Extras
3. SillyTavern

Extrasサーバーは後に読み込まれた場合、Stable Diffusion APIに再接続できません。

### ChromaDBのインストール時のhnswlib ホイール構築エラー

> ERROR: Could not build wheels for hnswlib, which is required to install pyproject.toml-based projects

ChromaDBモジュールをインストールする前に、次**のいずれか一つ**実行：

* Visual C++ 構築ツール インストール： <https://visualstudio.microsoft.com/visual-cpp-build-tools/>
* Conda から`hnswlib`パッケージをインストール：`conda install -c conda-forge hnswlib`

---

### Macでのpython要件インストール時のエラー

> ERROR: No matching distribution found for torch==2.0.0+cu117

MacはCUDAをサポートしていないため、Torchパッケージはsupport を削除してインストール。

代わりに`requirements-silicon.txt`ファイルを使用して要件をインストール。

---

### モジュールが欠落していますか？

* モジュール名のリストをPythonコマンド ラインで指定し、`--enable-modules`修飾子を使用する必要があります。
* [モジュール](/extensions/Extras/Installation.md#decide-which-module-to-use)セクション参照。

---

### API キー ボックスは何のためですか？

* SillyTavernの拡張機能パネルのAPI キーボックスは次の場合にのみ使用：
  * Extrasインストール フォルダーに`api_key.txt`という名前のテキスト ファイルを作成し、選択したExtras"パスワード"を含めています。
  * Extrasを`--secure`コマンドライン引数で開始。
* これにより、Extras API"パスワード ロック"になり、API キーを使用しているユーザーのみがアクセス可能。
* これは主にExtras独自の公開デプロイメント（colab、など）を作成したい人向け。
* 個人使用のためにPCで実行しているユーザーは、API キー ボックスに何も入力しないでください。

### モバイル/Android/Termux について？ 🤔

* コミュニティーの人々はTermux経由でUbuntu上で電話でExtras実行に成功しています。
* ただし、Extrasはモバイル サポートを考慮して作成されませんでした。
* モバイル デバイス上でExtras実行する人向けのサポートは提供されません。
* 下のリンク済みガイドの作成者に質問を指向することにより。

#### ❗ こちらはUNSUPPORTED

<https://rentry.org/STAI-Termux#downloading-and-running-tai-extras>
