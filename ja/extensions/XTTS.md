---
order: tts-xtts
route: /extensions/xtts/
---

# 音声クローニングを使用したXTTS

こんにちは！Reddit投稿で示されている音声クローニング技術の進歩に驚いたのですが、

ロボット的なワイフ/ハズバンドに輝く新しい音声モジュレータを与える準備ができていますか？

心配しないでください。この素晴らしい最先端の技術はすでにあなたの地元のSillyTavernで利用可能で、単純なセットアップが必要です。

## 前提条件

1. SillyTavernの最新バージョン。
2. [Miniconda](https://docs.conda.io/projects/miniconda/en/latest/miniconda-install.html)がインストール済み。
3. (Windows)[Visual C++ビルドツール](https://visualstudio.microsoft.com/visual-cpp-build-tools/)がインストール済み。
4. ～10秒のファイル要件を含む、クローンするためのWAVファイル：PCM、Mono、22050Hz、16ビット（Audacityから変換）。
5. "speakers"および"output"サブフォルダーを持つフォルダーを作成。WAVファイルを"speakers"に挿入。

フォルダー構造の例：
```
C:\xtts
  - speakers
    - alice.wav
    - bob.wav
  - output
```

## インストール

[daswer123](https://github.com/daswer123)はXTTSv2モデルをコンピューターで実行してSillyTavernのTTS拡張機能に接続するAPIサーバーを作成しました。

Extras APIからは完全に独立しており、別の環境を使用します。

**非常に重要：**次の要件をExtras環境またはシステムPythonにインストールしないでください。
これはパッケージの不要なダウングレード、他のパッケージをブレークダウンなど。

次の手順はMinicondaを使用して提供されていますが、venv（ここではカバーされていない）でもできます。
 Anacondaコマンドプロンプトを開き、指示を1行1行に従ってください。

### サーバーを起動している

1. 前提条件のステップ4で作成したフォルダーに移動します。
    ```
    cd C:\xtts
    ```
2. 新しいconda envを作成。今後は、これを`xtts`と呼びます。
    ```
    conda create -n xtts
    ```
3. 新しく作成されたenvを有効にします。
    ```
    conda activate xtts
    ```
4. Python 3.10をenvにインストール。プロンプトされたら「y」を確認します。
    ```
    conda install python=3.10
    ```
5. XTTSサーバーおよびその要件をインストール。
    ```
    pip install xtts-api-server pydub
    ```
6. PyTorchをインストール。これには時間がかかります。次の行はGPU加速サポート（CUDA）でPyTorchをインストール。
CPU推論のみを使用する場合は、`--index-url`で始まる最後の部分をドロップしてください。
    ```
    pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
    ```
7. デフォルトホストおよびポート：<http://localhost:8020>でXTTSサーバーを起動
    ```
    python -m xtts_api_server
    ```
8. 最初のスタートアップ中、モデルはダウンロードされます（約～2 GB）。
Coqui AIの法的通知をよく読むことを忘れないでください。冗談です、もう一度「y」を押してください。

### SillyTavernに接続

1. 拡張機能パネルを開き、TTSメニューを展開し、プロバイダーリストで「XTTSv2」を選択します。
2. 言語ドロップダウンで音声テキスト言語を選択します（Polishではない場合は悲しい場合があります）。
3. プロバイダーエンドポイントが<http://localhost:8020>を指し、"利用可能な音声"に音声サンプルのリストが表示されることを確認します。
4. キャラクターを選択し、音声サンプルとキャラクター間のマッピングを設定します。
文字リストが空の場合は、「再読み込み」を数回押します。
5. 好みに応じてTTS設定の残りを構成。

### すべて設定されました！

任意のメッセージのコンテキストアクション メニューで拡張スピーカーアイコンをクリックして、美しいクローンされた音声が再生されているのを聞きます。生成には時間がかかり、高端RTX GPUでも、もはやリアルタイムではありません。

### ストリーミング？

最新バージョンのXTTSサーバーでHTTPストリーミングを使用して、生成されるとすぐにオーディオチャンクを取得することができます！

#### これはRVCで機能しません！

オーディオは生成されます（最新バージョンのRVC拡張機能を使用していると仮定）および変換されますが、RVC要件に基づく完全なオーディオファイルの前に変換が開始されるため、ストリーミングされません。ストリーミングRVCはまだ調査中です...

#### ストリーミングサポートを取得する方法は？

1. SillyTavernを最新バージョンに更新。
2. XTTSサーバーを最新バージョンに更新。

    ```bash
    conda activate xtts
    pip install xtts-api-server --upgrade
    ```

3. 通常どおりXTTSを開始およびSillyTavernに接続。
4. SillyTavernで「ストリーミング」XTTS拡張機能設定を有効化。

### ぶつぎり音のオーディオ？

"チャンク サイズ"設定を増加させてみてください。

参考用：チャンク サイズ200の場合、RTX 3090は完全なオーディオを生成でき、わずかに増加したオーディオレイテンシーの代わりに中断された。

### TTSサーバーを再起動するには？

インストール指示のステップ1、3、7を実行します。

### Android???

ほぼ不可能。PyTorchを必要とするアプリを実行することはできません。ブラックマジック。試す場合は、自分のリスク。サポートは提供されません。最高のソリューションは、ローカルネットワーク上のPCでTTS APIをホストすることです。リスニングするホストとポートを指定することを忘れないでください - [README](https://github.com/daswer123/xtts-api-server/blob/main/README.md)参照。
