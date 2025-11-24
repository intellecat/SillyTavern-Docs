---
route: /
---

# SillyTavernとは？

![SillyTavern - LLM Frontend for Power Users](/static/banner.png)

SillyTavern（略してST）は、テキスト生成LLM、画像生成エンジン、TTSボイスモデルとやり取りできるローカルインストール型のユーザーインターフェースです。私たちの目標は、ユーザーにLLMプロンプトに対する最大限の実用性と制御を提供し、急な学習曲線も楽しみの一部として受け入れることです。

SillyTavernは、LLM愛好家の専門コミュニティによって提供される情熱的なプロジェクトであり、常に無料でオープンソースです。2023年2月にTavernAI 1.2.8のフォークとして始まり、SillyTavernは現在200人以上の貢献者と2年間の独立した開発実績を持ち、経験豊富なAI愛好家のための主要ソフトウェアとして機能し続けています。

## スクリーンショット

|   [![API Connection](/static/screenshot1.jpg)](/static/screenshot1.jpg)    |  [![Chat UI](/static/screenshot2.jpg)](/static/screenshot2.jpg)   |
|:--------------------------------------------------------------------------:|:-----------------------------------------------------------------:|
| [![Advanced Formatting](/static/screenshot3.jpg)](/static/screenshot3.jpg) | [![World Info](/static/screenshot4.jpg)](/static/screenshot4.jpg) |

## インストール要件

ハードウェア要件は最小限です：NodeJS 18以降を実行できるものであれば何でも動作します。ローカルマシンでLLM推論を行う場合は、少なくとも6GBのVRAMを搭載した3000シリーズのNVIDIAグラフィックカードを推奨します。

お使いのプラットフォームに応じたインストールガイドに従ってください：

* [Windows](/Installation/Windows.md)
* [LinuxとMac](/Installation/LinuxMacOS.md)
* [Android](/Installation/Android.md)
* [Docker](/Installation/Docker.md)

## ブランチ

SillyTavernは、すべてのユーザーにスムーズな体験を保証するために、2ブランチシステムを使用して開発されています。

* `release` -🌟 **ほとんどのユーザーに推奨。** これは最も安定した推奨ブランチで、メジャーリリースがプッシュされたときにのみ更新されます。大多数のユーザーに適しています。通常、月に1回更新されます。
* `staging` - ⚠️ **カジュアルな使用には推奨されません。** このブランチには最新の機能がありますが、いつでも壊れる可能性があるため注意してください。パワーユーザーと愛好家のみ向けです。1日に数回更新されます。

## SillyTavern以外に何が必要ですか？

SillyTavernはインターフェースに過ぎないため、推論を提供するLLMバックエンドへのアクセスが必要です。AI Hordeを使用して、すぐにチャットを始めることができます。それ以外にも、多くのローカルおよびクラウドベースのLLMバックエンドをサポートしています：OpenAI互換API、KoboldAI、Tabbyなど。サポートされているAPIの詳細については、[API接続](/Usage/API_Connections/index.md)セクションをご覧ください。

## キャラクターカード

SillyTavernは「キャラクターカード」の概念を中心に構築されています。キャラクターカードは、LLMの動作を設定するプロンプトのコレクションであり、SillyTavernで永続的な会話を行うために必要です。これらはChatGPTのGPTsやPoeのbotsと同様に機能します。キャラクターカードの内容は何でもかまいません：抽象的なシナリオ、特定のタスクに合わせたアシスタント、有名人、または架空のキャラクター。

キャラクターカードを選択せずに簡単な会話をしたり、LLM接続をテストしたりするには、SillyTavernを開いた後、[ウェルカム画面](/Usage/welcome-assistants.md)の入力バーにプロンプトを入力するだけです。これにより、後でカスタマイズできる空の「アシスタント」キャラクターカードが作成されます。

キャラクターカードの定義方法の概要については、デフォルトキャラクター（Seraphina）を参照するか、「Download Extensions & Assets」メニューからコミュニティが作成したカードをダウンロードしてください。

独自のキャラクターカードをゼロから作成することもできます。詳細については、[キャラクターデザイン](/Usage/Characters/characterdesign.md)ガイドを参照してください。

## 主な機能

* 多くのコミュニティ製プリセットを備えた高度な[テキスト生成設定](/Usage/Prompts/advancedformatting.md)
* [World Infoサポート](Usage/worldinfo.md)：豊かな設定を作成したり、キャラクターカードのトークンを節約
* [グループチャット](/Usage/Characters/groupchats.md)：キャラクターがあなたや互いに話すためのマルチボットルーム
* [豊富なUIカスタマイズオプション](/Usage/User_Settings/uicustomization.md)：テーマカラー、背景画像、カスタムCSSなど
* [ユーザーペルソナ](/Usage/personas.md)：AIにあなたについて少し知らせて没入感を高める
* [組み込みRAGサポート](/Usage/Characters/data-bank.md)：AIが参照できるようにチャットにドキュメントを追加
* 広範な[チャットコマンド](/Usage/Chatting/slashcommands.md)サブシステムと独自の[スクリプトエンジン](/For_Contributors/st-script.md)

## エクステンション

SillyTavernは拡張性をサポートしています。

* [キャラクターの感情表現（スプライト）](/extensions/Expression-Images.md)
* [チャット履歴の自動要約](/extensions/Summarize.md)
* 自動UIと[チャット翻訳](extensions/Translation.md)
* [Stable Diffusion/FLUX/DALL-E画像生成](/extensions/Stable-Diffusion.md)
* [AI応答メッセージのテキスト読み上げ（ElevenLabs、Silero、またはOSのシステムTTS経由）](/extensions/TTS.md)
* [プロンプトに追加の現実世界のコンテキストを追加するためのWeb検索機能](/extensions/WebSearch.md)
* 「Download Extensions & Assets」メニューから他にも多くのものがダウンロード可能です。

## 開発者に直接連絡するにはどうすればよいですか？

* Discord: cohee, rossascends, wolfsblvt
* Reddit: [/u/RossAscends](https://www.reddit.com/user/RossAscends/), [/u/sillylossy](https://www.reddit.com/user/sillylossy/), [u/Wolfsblvt](https://www.reddit.com/user/Wolfsblvt/)
* [GitHubイシューを投稿](https://github.com/SillyTavern/SillyTavern/issues)

## このプロジェクトが気に入りました！どのように貢献できますか？

* プルリクエストを歓迎します！[コントリビューションガイドライン](https://github.com/SillyTavern/SillyTavern/blob/release/CONTRIBUTING.md)に従って始めましょう。
* GitHubで提供されているテンプレートを使用した、有益で情報に基づいたバグレポートも歓迎します。
* プロジェクト自体への金銭的な寄付は受け付けていません。

## 個人的な寄付

個々の貢献者へのサポートは感謝されますが、SillyTavernの全体的な開発方向には影響しません。

* RossAscendsは個人的な[Patreon](https://www.patreon.com/RossAscends)と[Kofi](https://ko-fi.com/rossascends)を持っています

## ライセンス

SillyTavernは、[AGPL-3.0ライセンス](https://github.com/SillyTavern/SillyTavern/blob/release/LICENSE)の下でリリースされた無料のオープンソースプロジェクトです。
