---
route: /extensions/rvc/
---

# Retrieval-based Voice Conversion (RVC)

このガイドでは、RVCの使用方法を説明します。RVCは、1つのオーディオクリップから別のオーディオクリップに音声機能を転送できる技術で、アニメ、映画、または独自の音声など、異なるトーンやスタイルで音声を話すことができます。

あの有名な「大統領がXをプレイ」動画を見たことはありますか？RVCを使って作成されました。RVC拡張機能を使用すると、SillyTavernキャラクターに、あなたが希望する任意の音声で話すことができます。

RVCはTTSではありません：それはスピーチツースピーチに似ています。オーディオクリップを入力として受け取ります。バックグラウンドで、RVCがSillyTavernのTTS拡張機能と協力して行うことは、TTSが生成したオーディオファイル（TTS関係なく実行したはずのもの）を待ってから、RVC は、そのTTSオーディオファイルを取得して、RVC構成からクローンされた音声に変換する2番目のパスを実行します。

## RVCセットアップ

SillyTavernのRVCはオーディオ変換を実行するいくつかのAPIソースをサポートします：

* [rvc-python](https://github.com/daswer123/rvc-python)
* [SillyTavern Extras](https://github.com/SillyTavern/SillyTavern-Extras)（非推奨）

### 一般的な前提条件

開始する前に、以下の前提条件が満たされていることを確認してください。

#### ffmpeg

`ffmpeg`バイナリがPATH環境変数にあることを確認してください。このツールはインカミングオーディオを変換するために使用されます。

**Windows**：

* SillyTavern Launcherスクリプトのツールボックスを使用してffmpegを自動的にインストール：<https://github.com/SillyTavern/SillyTavern-Launcher>
* または、ここでビルドをダウンロード：<https://www.gyan.dev/ffmpeg/builds/>
* PATH変数を変更する方法：<https://www.architectryan.com/2018/03/17/add-to-the-path-on-windows-10/>
* 正しく行ったかどうかをテストするには、コマンドプロンプトを開いて`ffmpeg`を実行してください。ffmpegバージョンと情報を出力する必要があります。

**Linux**：

パッケージマネージャーを使用してffmpegをインストールしてください。

```shell
# Debian/Ubuntu
sudo apt install ffmpeg
# Arch Linux
sudo pacman -S ffmpeg
# Fedora
sudo dnf install ffmpeg
```

**macOS**：

[Homebrew](https://brew.sh/)を使用してffmpegをインストール：

```shell
brew install ffmpeg
```

#### TTSが有効で機能していることを確認

RVCはTTSに依存します。TTS拡張機能を有効にする必要があります。RVCをミックスに追加する前に、TTSが既に正常に動作し、チャットをナレーションしている必要があります！

たとえば、次の点に注意してください：

* システムTTSエンジンは音声変換をまったくサポートしていません。
* ストリーミングTTSは、音声ストリームが終了してから変換を待ちます。

#### 拡張機能をインストール

拡張機能パネルの「拡張機能とアセットをダウンロード」メニューから「RVC」拡張機能をインストールしてください（積み重ねられたブロックアイコン）。

#### SillyTavern内でRVCを有効にする*

SillyTavernで、**拡張機能** > **RVC**に移動してそれを有効にします。

#### ソースを選択

拡張機能設定で、使用するRVCソースを選択してください。次に、ソース固有のインストール指示に進みます。

### rvc-pythonセットアップ

#### 1. パッケージをインストール

GitHubページからのインストール指示に従ってください：[rvc-pythonインストール](https://github.com/daswer123/rvc-python?tab=readme-ov-file#installation)。GPU を持っている場合はCUDAインストール指示に従うことをお勧めします。

Windows でのインストール時に問題が発生している場合（例えばfairseqステップの構築が失敗する）、次のソフトウェアがPCにインストールされていることを確認してください：

* [Windows 10 SDK](https://developer.microsoft.com/en-us/windows/downloads/windows-sdk/)
* [Visual Studio Build Tools 2022](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2022)

#### 2. モデルを準備

RVCモデルを保存するためのディレクトリを作成します。デフォルトでは`rvc_models`という名前で、サーバーを起動するときに現在のディレクトリから取得されます。すべてのモデルはサブフォルダー（その名前はUIに表示されます）です。`.pth`（必須）および`.index`（オプション）ファイルが含まれている必要があります。

詳細：[rvc-pythonモデル管理](https://github.com/daswer123/rvc-python?tab=readme-ov-file#model-management)

#### 3. APIサーバーを起動

次のコマンドを実行してAPIサーバーを起動してください：

```shell
python -m rvc_python api -p 5050 -l -md models_path
```

**引数：**

* `5050` - サーバーのリスニング ポートを設定します。別のポートでホストする場合は変更してください。
* `models_path` - モデルのパスを設定します。デフォルトの`rvc_models`ディレクトリを使用する場合は削除してください。
* `-l` - サーバーをすべてのネットワーク インターフェイスでリッスンするように設定します。localhostのみでリッスンするには削除してください。

#### 4. サーバーに接続

* RVC拡張機能設定で、適切な**rvc-python APIのURL**を設定します。デフォルトでは、`http://localhost:5050`になります。
* CUDAサポート付きでrvc-pythonをインストールした場合は、**CUDA を使用**チェックボックスを選択してください。
* 「更新」を押して、利用可能な音声のリストを読み込みます。

#### 5. 音声マップを構成

**音声マップは、すべてのキャラクターまたはユーザー ペルソナの音声変換設定を定義します。**

* 音声マップを設定するには、「キャラクター」ドロップダウンからキャラクターまたはペルソナ名を選択し、RVC「音声」を選択して、「適用」をクリックします。
* 必要に応じて、ピッチ補正やフィルタリングなど、関連する他の設定を構成することもできます。
* すべて正しく行った場合、Voice Map デバッグ エリアは「Betty:MyVoice(rvpme)」のような表示をします。

### SillyTavern Extrasセットアップ

#### 1. RVCモデルファイルを準備

* ファイル ブラウザーで、`\SillyTavern-extras\data\models\rvc`に移動します。
* 「Betty」のようなサブフォルダーを作成し、`.pth`および`.index`ファイルをそこに配置します。（ヒント：https://voice-models.com から音声ファイルをダウンロードできます。音声名がRVPMEであることを確認してください。）

#### 2. 要件をインストール

次のコマンドを使用して、必要な要件をインストールしてください：

```shell
pip install -r requirements-rvc.txt`
```

#### 3. RVCが有効にされているSillyTavern-extrasを実行

RVCモジュールが有効にされているSillyTavern-extrasを起動します。この例の呼び出しでは、SillyTavern-extrasにプリインストールされているEdge TTSを使用したと仮定しています：

```shell
python server.py --enable-modules=rvc,edge-tts
```

必要に応じて、```--cuda```をスタートアップコマンドに追加して、GPU対応を持っている場合はRVCをGPUで実行することもできます。簡単なテストに基づくと、VRAM使用量はナレーション50トークン（～36ワード）で3.4GB、200トークン（～150ワード）で7.6GBでした。

#### 4. 音声マッピングを設定

RVCの音声マップを作成してください。キャラクターを目的のSillyTavernキャラクター名に設定し、音声をステップ1で作成したRVCフォルダーに設定して、「適用」をクリックします。すべて正しく行った場合、音声マップは「Betty:MyVoice(rvpme)」のような表示をします。

#### 5. ピッチ抽出を選択

* ピッチ抽出方法として「rmvpe」を選択します。
* 「rmvpe」に問題がある場合は、他の方法を試してください（たとえば、「harvest」または「torchcrepe」）。

#### 6. (オプション)RVCを設定して、生成をファイルに保存

テストまたはトラブルシューティング目的で生成されたRVCオーディオを保存する場合は、```--rvc-save-file```をスタートアップコマンドに追加してください。これにより、最後の生成が`SillyTavern-extras/data/tmp/rvc_output.wav`に保存されます：

```shell
python server.py --enable-modules=rvc,edge-tts --rvc-save-file
```

#### 表現ベースのダイナミック音声

##### 1. RVCモデルを構成

RVCモデル フォルダーに、分類される各表現（例：怒り、恐怖、喜び、愛、悲しみ、驚き）の個別の`.pth`および`.index`ファイルがあります。

##### 2. モジュールを有効化

RVCと分類モジュールの両方を有効にしてください：

```shell
python server.py --enable-modules=rvc,classify
```

##### 3. RVCモジュールを使用

残りのセットアップはRVCモジュール単独を使用するのと似ています（上記で説明）。

## 独自のRVCモデルをトレーニング

### Deffcolonyによる RVC Easy Menuを使用（Windows のみ）

Mangio-RVCを自動的にインストールおよび起動します：https://github.com/deffcolony/rvc-easy-menu

#### 1. リポジトリをクローン

目的の場所にリポジトリをクローンします：

```shell
git clone https://github.com/deffcolony/rvc-easy-menu.git
```

#### 2. RVC-Launcher.batを起動

* `RVC-Launcher.bat`ファイルを開きます。
* RVCをインストールするためにオプション1を選択します。

#### 3. インストールを完了

プロンプトされたら、必要なパッケージと依存関係をインストールしてください。

#### 4. 音声トレーニング用WebUIを開く

インストール後、オプション2を選択して、音声トレーニング用WebUIを開きます。

### Mangio-RVC：音声モデルをトレーニング

***データセット準備***：

**1. オーディオを準備**：

* トレーニングするオーディオを`datasets`フォルダーに配置します。
* オーディオがバックグラウンドノイズなく、生の音声のみ必要であることを確認してください。
* より長いオーディオにより、より高い出力品質が得られます。

***WebUIトレーニング***：

**1. トレーニングタブにアクセス**：

* WebUIのトレーニングタブをクリックします。

**2. 実験を構成**：

* 実験名を入力（例：`my-epic-voice-model`）。
* バージョンをv2に設定します。

**3. データを処理し、機能を抽出**：

* 「データを処理」と「機能抽出」をクリック。
* 「保存頻度」を50に設定します。

**4. トレーニングパラメータ**：

* 「トレーニングエポック総数」を300に設定します。
* 「トレーニング機能インデックス」と「トレーニングモデル」をクリック。
