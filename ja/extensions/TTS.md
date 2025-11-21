---
order: tts
route: /extensions/tts/
---

# TTS

SillyTavernには幅広いTTS（テキスト音声合成）オプションがあり、チャットの部分を声でナレーションするために使用されます。このページではセットアップと使用について説明します。

## TTSの構成

### TTSプロバイダーの選択

どのTTSサービスを使用するかを選択するために使用されます。一部のオプションは無料、一部は有料サブスクリプションを必要とします。一部はPCで実行されます。

利用可能なオプション（リストは時間とともに変わる可能性があります）：

- **AllTalk** - 無料、オープンソース、ローカルインストール、さまざまなTTSエンジンを提供。設定指示については[AllTalk](./AllTalk.md)ページを参照。
- **Azure TTS** - Microsoft Edgeと同じ音声。Azure アカウントと有料サブスクリプションが必要。
- **Coqui-TTS**（非推奨）- 無料、Extras APIを実行する必要があります。高性能テキスト2スピーチモデル（Tacotron、Tacotron2、Glow-TTS、SpeedySpeech）およびBark。
- **Edge** - 無料、Azure経由で実行します。「プラグイン」として選択されたプロバイダーで実行している場合、[このサーバープラグイン](https://github.com/SillyTavern/SillyTavern-EdgeTTS-Plugin)もインストールする必要があります。その他のオプションはExtras API（非推奨）実行する必要があります。
- **Electron Hub** - [Electron Hub](https://electronhub.ai/) APIキーを再利用してクラウド音声（GPT-4oミニTTS、Microsoftニューラル音声など）にアクセスします。モデルごとのコントロール。
- **ElevenLabs** - 有料サブスクリプションが必要です。[ElevenLabs](https://elevenlabs.io/)からAPIキーを取得。
- **Google Translate** - Googleが提供する無料音声、言語ごとに1つ、品質は大きく異なります。
- **Google Gemini TTS** - [Vertex AI](/Usage/API_Connections/google.md#google-vertex-ai)または[AI Studio](/Usage/API_Connections/google.md#google-ai-studio)からAPIキーを必要とし、[Gemini TTS](https://cloud.google.com/text-to-speech/docs/gemini-tts)モデルを使用。
- **Kokoro** - 無料、[kokoro.js](https://www.npmjs.com/package/kokoro-js)を使用してモデルをブラウザローカルで実行。ただし、[一部のブラウザ](https://caniuse.com/webgpu)がデバイスオプションのWebGPUをサポートしない場合があります。
- **MiniMax** - [MiniMax](https://www.minimax.io/)からAPIキーが必要。設定指示については[MiniMax TTS](./MiniMaxTTS.md)ページを参照。
- **Novel** - 有料NovelAIサブスクリプションが必要、NovelAIのTTSエンジンで生成
- **OpenAI** - 有料APIキー必須、OpenAIのTTSモデルを使用。
- **Pollinations** - OpenAIのTTSモデルへの無料アクセス、ただしレート制限あり。[Webサイト](https://pollinations.ai/)。
- **Silero** - 無料、PCで実行、品質は大きく異なります。[専用APIサーバー](https://github.com/ouoertheo/silero-api-server)インストールまたはExtras API（非推奨）が必要。
- **System** - OS TTSエンジン（存在する場合）を使用。OSによって品質は異なります。
- **XTTS** - 無料、専用APIサーバーインストールが必要。設定指示については[XTTS](./XTTS.md)ページを参照。

### チェックボックス

- **有効** - TTS再生をオン/オフにする
- **自動生成** - 新しいメッセージがチャットに入るとTTSが自動的に再生を開始
- **「引用符」のみをナレーション** - TTS再生を`「引用符」`内のテキストのみに制限。これは`*アスタリスク行内の"引用"を含める*`（内部変数名 = `narrate_quoted_only`）
- **アスタリスク内のテキストを無視、「引用符」内でも*** - TTS は`*アスタリスク*`内のテキストを再生しません（内部変数名 = `narrate_dialogues_only`）
- *「引用符」のみをナレーション」と「アスタリスクを無視」の両方のチェックボックスがオンになると、TTSはアスタリスク内にない「引用符」のみを読み、他のすべてを無視します。*
- **翻訳されたテキストのみをナレーション** - これにより、TTSは翻訳されたテキストのみをナレーション。

サンプルテキストを考えます：`*Coheeがあなたに近づく微かな「nya」* "Good evening、senpai"、彼女は言う。`

次のテーブルは、**アスタリスク内のテキストを無視、「引用符」内でも***および**「引用符」のみをナレーション**のブール状態に基づいてテキストがどのように変更されるかを示します：

| **アスタリスク内のテキストを無視、「引用符」内でも*** | **「引用符」のみをナレーション** | **Output** |
|--------|--------|----------|
| 無効 | 無効 | Coheeがあなたに近づく微かな「nya」「Good evening、senpai」、彼女は言う。 |
| 無効 | 有効 | 「nya」...「Good evening、senpai」 |
| 有効 | 無効 | 「Good evening、senpai」、彼女は言う。 |
| 有効 | 有効 | 「Good evening、senpai」 |

### スライダー

選択したAPIに応じて変更されます。

### ボタン

- **適用** - TTSAPIを設定した後、音声マップを編集した後、これをクリックする必要があります。
- **更新** - 選択したTTS APIから音声のリストを再度読み込みます。
- **利用可能な音声** - 選択したAPIで利用可能なすべての音声を含むポップアップを読み込み、サンプルダイアログで好みを表示できます。

## TTSの使用

1. 「有効」チェックボックスをクリック。そうしない場合、何も起こりません。
2. 新しいメッセージがチャットに到着するたびにTTSを自動的に開始する場合は、「自動生成」チェックボックスをクリック。
3. 必要に応じて、メッセージ内の右上の「megaphone」アイコンをクリックしてオンデマンドで再生。
4. 下部右「停止」ボタン（ワンドメニュー内にあります）をクリックして、任意の再生を停止します。

### 音声マップ

TTSを使用するために音声マップを提供する必要があります。提供しない場合、キャラクターごとにどの音声を使用するかを知りません。音声マップを設定するには、まずキャラクターとチャットを開き、TTSプロバイダーから特定の音声をドロップダウンから割り当てます。 TTSプロバイダーが正しく構成されている場合は、音声リストが表示されない場合、または「更新」をクリックしてください。一部のプロバイダー（OpenAI互換またはNovelAIなど）では、音声リストを手動で追加する必要があります。
