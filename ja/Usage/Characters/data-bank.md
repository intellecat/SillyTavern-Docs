---
order: 30
route: /usage/core-concepts/data-bank/
tags:
    [
        vector storage,
        RAG,
        retrieval-augmented generation,
        vectors,
        documents,
        files,
        attachments,
    ]
---

# データバンク (RAG)

Retrieval-augmented generation (RAG) は、LLM に外部の知識源を提供する技術です。モデルの学習データ以外の情報にアクセスすることで、AI の回答の精度を向上させるのに役立ちます。

SillyTavern は、様々なソースから多目的な知識ベースを構築し、収集したデータを LLM プロンプトで使用するためのツールセットを提供しています。

## Data Bank へのアクセス

デフォルトでリリースバージョン >= 1.12.0 に含まれている組み込み Chat Attachments 拡張機能は、「Magic Wand」メニューに新しいオプション Data Bank を追加します。これは SillyTavern の RAG に利用可能なドキュメントを管理するためのハブです。

## ドキュメントについて

Data Bank はファイル添付ファイル（ドキュメントとも呼ばれます）を保存します。ドキュメントは 3 つの利用可能性スコープに分けられます。

1. グローバル添付ファイル - すべてのチャット（ソロまたはグループ）で利用可能。
2. キャラクター添付ファイル - 現在選択されているキャラクターのみで利用可能（グループで返信する場合を含む）。_添付ファイルはローカルに保存され、キャラクターカードでエクスポートされません！_
3. チャット添付ファイル - 現在開いているチャットでのみ利用可能。チャット内のすべてのキャラクターがこれを使用できます。

!!!info Note
Data Bank の正式な一部ではありませんが、個々のメッセージにもファイルを添付できます。「Wand」メニューから Attach File オプションを使用するか、メッセージアクションの行のクリップボードアイコンを使用します。
!!!

ドキュメントとは何ですか？実質的には、平文形式で表現できるものはすべてです！

例としては、以下が挙げられますが、これに限定されません：

- ローカルファイル（書籍、科学論文など）
- ウェブページ（Wikipedia、記事、ニュース）
- ビデオ トランスクリプト

様々な拡張機能とプラグインは、データを収集して処理する新しい方法も提供できます。詳細は以下を参照してください。

## データソース

任意のスコープにドキュメントを追加するには、「Add」をクリックし、利用可能なソースの 1 つを選択します。

### Notepad

テキストファイルをゼロから作成するか、既存の添付ファイルを編集します。

### File

コンピュータのハードドライブからファイルをアップロードします。SillyTavern は人気のあるファイル形式用の組み込みコンバーターを提供しています：

- PDF (テキストのみ)
- HTML
- Markdown
- ePUB
- TXT

JSON、YAML、ソースコードなど、非標準の拡張子を持つテキストファイルを添付することもできます。選択したファイルのタイプから既知の変換がない場合、そのファイルを平文ドキュメントとして解析できない場合、ファイルアップロードは拒否されます。つまり、生のバイナリファイルは許可されていません。

!!!info Note
Microsoft Office (DOCX, PPTX, XLSX) および LibreOffice ドキュメント (ODT, ODP, ODS) のインポートには、[Server Plugin](https://github.com/SillyTavern/SillyTavern-Office-Parser) がインストールされて読み込まれている必要があります。プラグインの README ページでインストール手順を参照してください。
!!!

### Web

URL を使用してウェブページからテキストをスクレイプします。HTML ドキュメントは [Readability](https://github.com/mozilla/readability) ライブラリを通じて処理され、使用可能なテキストのみを抽出します。

一部のウェブサーバーはフェッチ要求を拒否したり、Cloudflare によって保護されたり、機能するために JavaScript に大きく依存したりする場合があります。特定のサイトで問題が発生している場合は、ウェブブラウザでページを手動でダウンロードし、ファイル アップローダーを使用して添付します。

### YouTube

YouTube ビデオのトランスクリプトを ID または URL でダウンロードします。クリエイターがアップロードしたか、Google によって自動生成されたトランスクリプトです。一部のビデオではトランスクリプトが無効になっている場合があり、年齢制限付きビデオの解析はログインが必要なため利用できません。

スクリプトはビデオのデフォルト言語で読み込まれます。オプションで、2 文字の言語コードを指定して、特定の言語でトランスクリプトをフェッチしようとすることができます。この機能は常に利用できるとは限らず、失敗する可能性があるため、注意して使用してください。

### Web Search

!!!info Note
このソースには [Web Search](/extensions/WebSearch.md) 拡張機能がインストールされて適切に構成されている必要があります。詳細は、リンクされたページを参照してください。
!!!

ウェブ検索を実行し、検索結果ページからテキストをダウンロードします。これは Web ソースに似ていますが、完全に自動化されています。選択した検索エンジンは拡張機能の設定から継承されるため、事前に設定してください。

開始するには、検索クエリ、アクセスするリンクの最大数、および出力タイプを指定します：1 つの統合ファイル（拡張機能のルールに従ってフォーマットされた）またはすべてのページの個別ファイル。ページ スニペットを保存することを選択できます。

### Fandom

!!!info Note
このソースには [Server Plugin](https://github.com/SillyTavern/SillyTavern-Fandom-Scraper) がインストールされて読み込まれている必要があります。プラグインの README ページでインストール手順を参照してください。
!!!

[Fandom](https://www.fandom.com/) ウィキから ID または URL で記事をスクレイプします。一部のウィキは非常に大きいため、フィルター正規表現を使用してスコープを制限すると便利です。記事のタイトルに対してテストされます。フィルターが提供されない場合、すべてのページが対象です。すべてのページの個別ファイルとして保存することも、単一のドキュメントに結合することもできます。

### Bronie Parser Extension (サードパーティ)

!!!warning Note
このソースはサードパーティからのもので、**SillyTavern チームに関連付けられていません**。このソースを使用するには、Bronya Rand の [Bronie Parser Extension](https://github.com/Bronya-Rand/Bronie-Parser-Extension) と、パーサーが機能するために必要なサーバー プラグインをインストールしている必要があります。
!!!

Bronya Rand の Bronie Parser Extension を使用すると、miHoYo/HoYoverse の [HoYoLab](https://wiki.hoyolab.com) などのサードパーティ スクレーパーを SillyTavern で使用できます。これは他のデータ ソースに似ています。

現在、Bronya Rand の Bronie Parser Extension は以下をサポートしています：

- miHoYo/HoYoverse の HoYoLab (Genshin Impact/Honkai: Star Rail 用) [HoYoWiki-Scraper-TS](https://github.com/Bronya-Rand/HoYoWiki-Scraper-TS) 経由

開始するには、Bronya Rand の Bronie Parser Extension を [installation guide](https://github.com/Bronya-Rand/Bronie-Parser-Extension?tab=readme-ov-file#installation) に従ってインストールし、サポートされているサーバー プラグインを SillyTavern にインストールしてください。SillyTavern を再起動して、_Data Bank_ メニューに移動します。`+ Add` をクリックすると、最近インストールされたスクレーパーが可能なソースのリストに追加されているはずです。

## ベクトルストレージ

では、特定の主題に関する優れた包括的な情報ライブラリを構築しました。次は何ですか？

RAG にドキュメントを使用するには、関連データを LLM プロンプトに挿入する互換性のある拡張機能を使用する必要があります。

SillyTavern にバンドルされている Vector Storage は、そのような拡張機能のリファレンス実装です。エンベディング（ベクトルとも呼ばれます）を使用して、進行中のチャットに関連するドキュメントを検索します。

!!!info Fun facts

1. Embeddings は、特殊な言語モデルによって生成されたテキストを抽象的に表すの数値の配列です。より類似したテキストはそれぞれのベクトル間の距離が短くなります。
2. Vector Storage 拡張機能は [Vectra](https://github.com/Stevenic/vectra) ライブラリを使用して、ファイル エンベディングを追跡します。これらはユーザー データ ディレクトリの `/vectors` フォルダの JSON ファイルに保存されます。すべてのドキュメントは内部的に独自のインデックス/コレクション ファイルで表現されます。
   !!!

Vectors 機能はデフォルトで無効になっているため、拡張機能パネル（トップバーの「Stacked Cubes」アイコン）を開き、「Vector Storage」セクションに移動して、「File vectorization settings」の下の「Enabled for files」チェックボックスをチェックする必要があります。

Vector Storage 自体はベクトルを生成しません。互換性のある埋め込みプロバイダーを使用する必要があります。

## ベクトルプロバイダー

!!!warning Warning
Embeddings は、それを生成したのと同じモデルを使用して取得する場合にのみ使用できます。埋め込みモデルまたはソースを変更するときは、ベクトルを再計算する必要があります。
!!!

### Local

これらのソースは無料で無制限であり、CPU/GPU を使用してエンベディングを計算します。

1. Local (Transformers) - Node サーバーで実行されます。SillyTavern は ONNX 形式の互換性のあるモデルを HuggingFace から自動的にダウンロードします。デフォルト モデル：[jina-embeddings-v2-base-en](https://huggingface.co/Cohee/jina-embeddings-v2-base-en)。
2. WebLLM - 拡張機能がインストールされ、[WebGPU をサポート](https://caniuse.com/webgpu)するウェブ ブラウザが必要です。ブラウザで直接実行され、ハードウェア アクセラレーションを使用できます。HuggingFace からサポートされているモデルを自動的にダウンロードします。拡張機能は <https://github.com/SillyTavern/Extension-WebLLM> からインストールしてください。
3. Ollama - <https://ollama.com/> から入手してください。API接続メニュー（Text Completion、デフォルト: `http://localhost:11434`）で API URL を設定します。互換性のあるモデルを最初にダウンロードしてから、拡張機能の設定でその名前を設定する必要があります。モデルの例：[mxbai-embed-large](https://ollama.com/library/mxbai-embed-large)。オプションで、モデルをメモリに読み込まれた状態に保つオプションをチェックします。
4. llama.cpp server - [ggerganov/llama.cpp](https://github.com/ggerganov/llama.cpp) から入手し、`--embedding` フラグでサーバー実行可能ファイルを実行します。HuggingFace から互換性のある GGUF エンベディング モデルを読み込みます（例：[nomic-ai/nomic-embed-text-v1.5-GGUF](https://huggingface.co/nomic-ai/nomic-embed-text-v1.5-GGUF)）。
5. vLLM - [vllm-project/vllm](https://github.com/vllm-project/vllm) から入手してください。API接続メニューで API URL と API キーを最初に設定します。
6. Extras (deprecated) - SentenceTransformers ローダーを使用する [Extras API](https://github.com/SillyTavern/SillyTavern-extras) の下で実行されます。デフォルト モデル：[all-mpnet-base-v2](https://huggingface.co/sentence-transformers/all-mpnet-base-v2)。このソースはメンテナンスされておらず、将来的に削除される予定です。

### APIソース

これらのソースはすべてそれぞれのサービスの API キーが必要で、通常は使用コストがありますが、一般的にエンベディングの計算は非常に安価です。

1. OpenAI
2. Cohere
3. Google AI Studio
4. Google Vertex AI
5. TogetherAI
6. MistralAI
7. NomicAI

## ベクトル化設定

埋め込みプロバイダーを選択した後、ドキュメントの処理と取得のルールを定義する他の設定を忘れずに構成してください。

!!!info Note
ファイルの分割、ベクトル化、添付ファイルからの情報取得には時間がかかります。ファイルの初期インジェストに時間がかかる場合があります。RAG 検索クエリは通常、顕著なラグを生じないほど十分に高速です。
!!!

### メッセージ添付ファイル

これらの設定は、メッセージに直接添付されるファイルを制御します。

次のルールが適用されます：

1. LLM コンテキスト ウィンドウに収まるメッセージのみが添付ファイルを取得できます。
2. Vector Storage 拡張機能が無効な場合、ファイル添付ファイルと付随するメッセージはプロンプトに完全に挿入されます。
3. ファイル ベクトル化が有効な場合、ファイルはチャンクに分割され、最も関連性の高い部分のみが挿入されて、コンテキスト スペースを節約し、モデルが集中したままになります。

- Size threshold (KB) - チャンク分割の閾値を設定します。指定されたサイズより大きいファイルのみが分割されます。
- Chunk size (chars) - 個々のチャンクのターゲット サイズを設定します（テキスト文字数、モデル トークン数ではありません！）。
- Chunk overlap (%) - 隣接するチャンク間で共有されるチャンク サイズのパーセンテージを設定します。これにより、チャンク間のより滑らかな遷移が可能になりますが、冗長性も導入する可能性があります。
- Retrieve chunks - 取得する最も関連性の高いファイル チャンクの最大数を設定します。元の順序で挿入されます。

### Data Bank ファイル

これらの設定は Data Bank ドキュメントの処理方法を制御します。

次のルールが適用されます：

1. ファイル ベクトル化が無効な場合、Data Bank は使用されません。
2. それ以外の場合、現在のスコープのすべての利用可能なドキュメント（上記を参照）がクエリ対象と見なされます。すべてのファイル間で最も関連性の高いチャンクのみが取得されます。同じファイルの複数のチャンクが元の順序で挿入されます。
3. 挿入されたチャンクは、チャット メッセージに対応する前にコンテキストの一部を予約します。

- Size threshold (KB) - チャンク分割の閾値を設定します。指定されたサイズより大きいファイルのみが分割されます。
- Chunk size (chars) - 個々のチャンクのターゲット サイズを設定します（テキスト文字数、モデル トークン数ではありません！）。
- Chunk overlap (%) - 隣接するチャンク間で共有されるチャンク サイズのパーセンテージを設定します。これにより、チャンク間のより滑らかな遷移が可能になりますが、冗長性も導入する可能性があります。
- Retrieve chunks - 取得するファイル チャンクの最大数を設定します。この許可はすべてのファイル間で共有されます。
- Injection Template - 取得した情報がプロンプトに挿入される方法を定義します。特別な \{\{text\}\} マクロを使用して取得したテキストの位置を指定することも、他のマクロを使用することもできます。
- Injection Position - プロンプト インジェクションを挿入する位置を設定します。Author's Note および World Info の場合と同じルールが適用されます。

### 共通設定

- Query messages - クエリのドキュメント チャンクに使用される最新のチャット メッセージの数。
- Score threshold - 関連性スコア（0 - まったく一致なし、1 - 完全一致）に基づくチャンク取得の除外を調整します。値が高いほど、より正確な取得が可能になり、まったくランダムな情報がコンテキストに入るのを防ぎます。有効な値は 0.2（より緩い）から 0.5（より焦点を絞った）の範囲です。
- Chunk boundary - ファイルをチャンクに分割するときに優先される、カスタム文字列。指定されない場合、デフォルトは（順序で）ダブル改行、シングル改行、単語間のスペースで分割することです。
- Only chunk on custom boundary - 有効な場合、チャンク化は指定されたチャンク境界でのみ発生します。それ以外の場合、チャンク化はデフォルトの境界でも発生します。
- Translate files into English before processing - 有効な場合、[Chat Translation](/extensions/Translation.md) 拡張機能で構成された翻訳 API を使用して、処理する前にファイルを英語に翻訳します。これは、英語のテキストのみをサポートするエンベディング モデルを使用する場合に役立ちます。
- Include in World Info Scanning - 挿入されたコンテンツがロアブック エントリを活性化することを望む場合は、チェックしてください。
- Vectorize All - 未処理のすべてのファイルのエンベディングを強制的に取得します。
- Purge Vectors - ファイル エンベディングをクリアして、ベクトルを再計算できます。

!!!info Note
「Chat vectorization」設定については [Chat Vectorization](/extensions/Chat-vectorization.md) を参照してください。
!!!

## 結論

おめでとうございます！チャット体験は RAG の力で向上しました。その機能は想像力によってのみ制限されています。いつものように、実験することを恐れないでください！
