---
route: /extensions/stable-diffusion/
templating: false
---

# 画像生成

画像を生成するために、ローカルまたはクラウドベースのStable Diffusion、FLUXまたはDALL-E APIを使用してください。

チャット履歴とキャラクター情報からメッセージ、完全な没入感のための返信として自動的に画像を生成し、ワンドメニューまたはスラッシュコマンドから生成するか、`/sd (anything_here)`コマンドを使用して独自のプロンプトで画像を作成します。

最も一般的なStable Diffusion生成設定はSillyTavern UI内でカスタマイズ可能です。

- [複数の画像生成ソース](#supported-sources)をサポートしており、ローカルとクラウドベースの両方があります
- キャラクター、シーン、カスタムプロンプト向けの[生成モード](#generation-modes)
- [スラッシュコマンド](#how-to-generate-an-image)チャット内で簡単に画像生成する場合
- [インタラクティブモード](#use-interactive-mode)自然言語リクエストに基づいて画像生成をトリガーする場合
- カスタマイズ可能なプロンプトテンプレートおよび[プレフィックス](#common-prompt-prefix)一貫したスタイルと品質のため
- [キャラクター固有のプロンプトプレフィックス](#character-specific-prompt-prefix)カスタマイズされたキャラクターイメージの場合
- [スタイルプリセット](#styles)異なる画像生成設定間で素早く切り替える場合
- [可視性オプション](#chat-message-visibility)チャット内で生成されたイメージの場合
- 高度な[ComfyUI統合](#comfyui-configuration)カスタマイズ可能なワークフロー用
- [すべての生成イメージを表示](#view-all-generated-images)キャラクターギャラリーで
- [イメージスワイプ](#image-swipes)機能同じプロンプトでイメージを再生成する場合
- [生成前にプロンプトを編集](#edit-prompts-before-generation)および[フリーモードプロンプトを拡張](#extend-free-mode-prompts)
- [AI関数呼び出し](#use-function-tool)との統合自動画像生成検出用

## サポートされているソース

| Source | Remarks |
|--------|----------|
| [AI.ML API](https://aimlapi.com/) | クラウド、有料 |
| [Black Forest Labs](https://bfl.ai/) | クラウド、有料 |
| [ComfyUI](https://github.com/comfyanonymous/ComfyUI) | ローカル、オープンソース(GPL3)、無料 |
| [Draw Things](https://drawthings.ai/) | ローカル、Mac/iOS、無料 |
| [Electron Hub](https://electronhub.ai/) | クラウド、有料 |
| [FAL.AI](https://fal.ai/) | クラウド、有料 |
| [Google AI Studio](https://aistudio.google.com/) / [Google Vertex AI](https://cloud.google.com/vertex-ai) | クラウド、有料。Imagenモデルシリーズ。AI Studioはのみイメージャー3.0 002をサポート。 |
| [HuggingFace Serverless](https://huggingface.co/docs/api-inference/index) | クラウド、無料 |
| [NanoGPT](https://nano-gpt.com/) | クラウド、有料 |
| [NovelAI Diffusion](https://novelai.net/) | クラウド、アクティブなサブスクリプションが必要 |
| [OpenAI](https://platform.openai.com/) | クラウド、有料 |
| [Pollinations](https://pollinations.ai/) | クラウド、オープンソース(MIT)、無料 |
| [SD.Next / vladmandic](https://github.com/vladmandic/automatic) | ローカル、オープンソース(AGPL3)、無料 |
| [SillyTavern Extras](https://github.com/SillyTavern/SillyTavern-Extras) | 非推奨、推奨されません |
| [Stability AI](https://platform.stability.ai/) | クラウド、有料 |
| [Stable Diffusion WebUI / AUTOMATIC1111](https://github.com/AUTOMATIC1111/stable-diffusion-webui) | ローカル、オープンソース(AGPL3)、無料 |
| [Stable Horde](https://stablehorde.net/) | クラウド、オープンソース(AGPL3)、無料 |
| [TogetherAI](https://docs.together.ai/docs/serverless-models#image-models) | クラウド |
| [x.AI](https://x.ai/) | クラウド、有料 |
