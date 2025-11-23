---
route: /usage/api-connections/google/
---

# Google Gemini

GeminiはGoogleの最先端マルチモーダルLLMで、Google Vertex AIやGoogle AI Studio(旧MakerSuite)を含む複数のAPIを通じて利用できます。このガイドでは、SillyTavernでGemini API接続を設定する方法を説明します。

## Google AI Studio

AI Studioは、Google Cloud Platform(GCP)プロジェクトを設定する必要なく、最新のGoogle AIモデルを試すための最も速く、最もユーザーフレンドリーな方法です。Geminiモデルにアクセスするために使用できるシンプルなAPIキーを提供します。

### Step 1: Google AI Studioキーの作成

1. [Google AI Studio](https://aistudio.google.com/apikey)ページに移動し、Googleアカウントでサインインします。
2. 「Get API Key」をクリックし、利用規約に同意します。
3. 「Create API Key」をクリックしてAPIキーを生成します。
4. APIキーをクリップボードにコピーします。

### Step 2: APIキーをSillyTavernに入力

1. SillyTavernで「API Connections」ページに移動します。
2. APIタイプとして「Chat Completion」を選択します。
3. ドロップダウンメニューから「Google AI Studio」を選択します。
4. 先ほどコピーしたAPIキーを「API Key」テキストボックスに入力します。
5. 「Connect」ボタンをクリックしてキーを保存します。

これでGoogle AI Studio APIをSillyTavernで使用できるようになります。

## Google Vertex AI

Vertex AIはGoogle Cloud Platform(GCP)によって提供されるサービスです。Geminiシリーズを含む様々なAIモデルへのアクセスを提供します。

Vertex AI APIを設定する方法はいくつかあり、使用する方法によって利用可能なモデルが異なる場合があります。

### Service Account

Google Cloud Platform(GCP)はVertex AIにアクセスするためにサービスアカウントを必要とし、シンプルなAPIキーは機能しません。サービスアカウントJSONファイルからトークンが生成され、Vertex AI APIへのリクエストの認証に使用されます。

以下の手順でサービスアカウントを作成できます:

**前提条件:**

1. Google Cloud Platform(GCP)アカウントが必要です。
2. GCPアカウント内にプロジェクトが作成されている必要があります。
3. そのプロジェクトで請求が有効になっている必要があります。

#### Step 1: Vertex AI APIの有効化

キーが機能する前に、プロジェクトでAPIを有効にする必要があります。

1. Google Cloud Consoleに移動: <https://console.cloud.google.com/>
2. トップバーで正しいプロジェクトが選択されていることを確認します。
3. Vertex AI APIページに移動: <https://console.cloud.google.com/apis/library/aiplatform.googleapis.com>
4. まだ有効になっていない場合は、「Enable」ボタンをクリックします。

#### Step 2: サービスアカウントの作成

これはVertex AI APIへのアクセスに使用されるIDです。

1. Google Cloud Consoleで、「Service Accounts」ページに移動します。トップ検索バーで検索するか、このダイレクトリンクを使用できます: <https://console.cloud.google.com/iam-admin/serviceaccounts>
2. GCPプロジェクトを選択し、「+ CREATE SERVICE ACCOUNT」をクリックします。
3. Service account name: `my-vertex-ai-client`のような説明的な名前を付けます。
4. 「CREATE AND CONTINUE」をクリックします。
5. Grant this service account access to project: 「Role」ドロップダウンで、Vertex AI Userを検索して選択します。このロールは、過剰なアクセスを与えることなくモデルを実行するために必要な権限を付与します。
6. 「CONTINUE」をクリックし、次に「DONE」をクリックします。

#### Step 3: JSONキーの生成

これは必要な「パスワード」ファイルです。機密情報が含まれているため、共有したり公開したりしないでください。

1. Service Accountsリストに戻ります。作成したアカウント(例: sillytavern-vertex-ai)を見つけます。
2. その行の右端にある3点メニュー(⋮)をクリックし、「Manage keys」を選択します。
3. 「ADD KEY」->「Create new key」をクリックします。
4. Key typeがJSONに設定されていることを確認します。
5. 「CREATE」をクリックします。

.jsonファイルがすぐにコンピュータにダウンロードされます。紛失した場合はこのキーを回復できないため、安全に保管してください。

#### Step 4: JSON内容をSillyTavernに入力

ダウンロードしたJSONファイルには、Vertex AI APIで認証するために必要なすべての情報が含まれています。次のようになります:

```json
{
    "type": "service_account",
    "project_id": "your-gcp-project-name",
    "private_key_id": "...",
    "private_key": "-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n",
    "client_email": "sillytavern-vertex-ai@your-gcp-project-name.iam.gserviceaccount.com",
    "client_id": "...",
    "auth_uri": "https://accounts.google.com/o/oauth2/auth",
    "token_uri": "https://oauth2.googleapis.com/token",
    "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
    "client_x509_cert_url": "..."
}
```

1. ダウンロードした.jsonファイルをシンプルなテキストエディタ(WindowsのNotepad、MacのTextEdit、またはVS Code)で開きます。
2. ファイル内のすべてのテキストを選択します(Ctrl+AまたはCmd+A)。
3. テキストをクリップボードにコピーします(Ctrl+CまたはCmd+C)。
4. SillyTavernで「API Connections」ページに移動し、APIタイプとして「Chat Completion」を選択してから、ドロップダウンメニューから「Google Vertex AI」を選択します。認証方法を「Service Account」に切り替えます。
5. コピーした内容全体を「Service Account JSON Content」テキストボックスに貼り付けます。
6. 「Validate JSON」ボタンをクリックして、正しくコピーされたことを確認します。
7. 最後に、下にスクロールして、API設定ページの下部にある「Connect」をクリックします。

これでGoogle Vertex AI APIをSillyTavernで使用できるようになります。

### Express Mode

Express modeは、Google CloudでGenerative AIを使い始める最も迅速な方法です。サービスアカウントを設定する必要なくGemini APIを使用でき、代わりにAPIキーを直接使用できます。

詳細については、公式ドキュメントをご覧ください: [Vertex AI in express mode overview](https://cloud.google.com/vertex-ai/generative-ai/docs/start/express-mode/overview)。

#### Step 1: アカウントがExpress Modeの資格があることを確認

以前にGoogle Cloudプロジェクトの作成に使用されていないGoogleアカウントが必要です。
既存のGoogle Cloudプロジェクト(無料トライアルを含む)がある場合は、この目的のために新しいプロジェクトを作成できます。

#### Step 2: Vertex AI Express Modeのアクティベート

1. 次のWebページに移動: [Vertex AI Studio](https://cloud.google.com/generative-ai-studio)。
2. 「Try it free」をクリックします。
3. 利用規約に同意し、Googleアカウントでサインインします。
4. 国を選択し、「Agree & start free」をクリックします。セットアップが完了するまで待ちます。

#### Step 3: APIキーの作成

1. Google Cloud consoleがExpress Modeで実行されていることを確認します。ページの左上隅にバナーが表示されるはずです。
2. 左サイドバーの「API Keys」リンクをクリックします。
3. 「Create API Key」ボタンをクリックします。
4. 新しいAPIキーが生成されます。このキーをクリップボードにコピーします。

#### Step 4: APIキーをSillyTavernに入力

1. SillyTavernで「API Connections」ページに移動します。
2. APIタイプとして「Chat Completion」を選択します。
3. ドロップダウンメニューから「Google Vertex AI」を選択します。
4. 認証方法を「Express Mode (API Key)」に切り替えます。
5. 先ほどコピーしたAPIキーを「API Key」テキストボックスに貼り付けます。
6. 「Connect」ボタンをクリックしてキーを保存します。

これでExpress ModeでGoogle Vertex AI APIをSillyTavernで使用できるようになります。
