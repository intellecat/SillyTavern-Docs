---
route: /usage/api-connections/horde/
---

# AI Horde

## 免責事項

- AI Hordeは完全にボランティアによって運営されているクラウドソース型の分散GPUクラスターです。
- デフォルトでは、入力は匿名で送信され、応答はHorde Workerを実行している人に見られることはありません。
- ただし、オープンソースプログラムであるため、悪意のあるWorkerはコードを変更して以下のことを行う可能性があります:
  - アクティビティ(入力プロンプト、AI応答)をログに記録する。
  - 悪質または不快な応答を生成する。

!!!warning
Hordeを使用する場合、名前、メールアドレスなどの個人情報は**決して送信しない**でください。
!!!

「Trusted Workers Only」チェックボックスをオンにすると、利用可能なworkerの選択が、しばらくHordeでホスティングしており、一般的に信頼されていると見なされているworkerのみに制限されます。ただし、未記録のソフトウェアを使用してホスティングすることにより、プロンプトを見ることができる可能性があります。

この問題を軽減するために、SillyTavernには以下の機能が組み込まれています:

- チャット応答がHorde Workerによって生成された場合、SillyTavernはWorkerのIDと使用していたモデルを記録します。
- この情報は、チャットアイテムの上にマウスカーソルを置くことで確認できます(下の画像を参照)。
- 悪意のある応答を受け取ったと思われる場合は、この情報を[AI Horde Discord](https://discord.gg/3DxrhksKzn)のHorde管理者に渡して、レビューとそのWorkerに対する懲戒処分の可能性を確認できます。

![Horde Worker Info Popup](/static/horde-worker.png)

## Setup

- SillyTavernは追加のセットアップなしで、そのままHordeに接続できます。
- ST API PanelのAPI Dropdown Selectorから「AI Horde」を選択します。
- パネルの下部にあるModel Selectorから1つ以上のModels(キャラクターの「AIブレイン」)を選択します。
- キャラクターを選択してチャットを開始します。

![ST Kobold Horde API Connection Panel](/static/horde-config.png)

!!!warning
デフォルトでは、SillyTavernインスタンスはHordeの低優先度「ゲストアカウント」に接続します。
つまり、返信を待つのに長い時間がかかる可能性があります。
待ち時間を短縮するには、以下のヒントに従ってください。
!!!

## Tips

- [Horde Webサイトでアカウントを登録](https://aihorde.net/register)してから、Horde KeyをSillyTavern Horde API Keyボックスに追加します。
- [Horde Workerを設定](https://github.com/Haidra-Org/AI-Horde-Worker#readme)して、他のユーザーにGPUを提供します。
  - 他のユーザーにGPUを使用させると、['Kudos'、一種のHorde専用通貨](https://github.com/Haidra-Org/AI-Horde/blob/main/FAQ.md#kudos)を獲得できます。
  - アカウントが持つkudosが多いほど、他のHorde Workerからのチャット応答が速くなります。
  - Kudosは[Stable Horde](https://stablehorde.net)でAI画像を作成するためにも使用できます。
    - SillyTavernはStable Horde画像生成をそのままサポートしています。
- GPUがAIを実行するのに十分強力でない場合、またはコンピュータを持っていない場合でも、[さまざまな方法でHordeコミュニティに参加してKudosを獲得できます](https://github.com/Haidra-Org/AI-Horde/blob/main/FAQ.md#i-dont-have-a-powerful-gpu-how-can-i-get-kudos)。
