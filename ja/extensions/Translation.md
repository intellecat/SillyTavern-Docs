---
route: /extensions/translation/
templating: false
---

# チャット翻訳

## 概要

チャット翻訳拡張機能により、さまざまな翻訳プロバイダーを使用してチャットメッセージ間をリアルタイムで翻訳できます。手動と自動翻訳モードをサポートしています。

![「翻訳メッセージ/翻譯訊息」メッセージアクションボタンを使用して英語から中国語に翻訳されたキャラクターメッセージ](../static/extensions/translation/sensei.png)

+++ English
![「翻訳チャット」、「入力を翻訳](../static/extensions/translation/wand-menu-en.png)
+++ 简体中文
![「翻訳チャット内容」、「翻訳入力内容」](../static/extensions/translation/wand-menu-zh-cn.png)
+++ 繁體中文
![「翻訳チャット内容」、「翻訳入力内容」](../static/extensions/translation/wand-menu-zh-tw.png)
+++ 한국어
![「チャットを翻訳」、「入力を翻訳」](../static/extensions/translation/wand-menu-ko.png)
+++ Русский
![「チャットを翻訳」、「メッセージを翻訳」](../static/extensions/translation/wand-menu-ru.png)
+++

## 使用

チャットメッセージを翻訳するすべての方法：

**<i class="fa-solid fa-language"></i>チャットを翻訳**ボタン（**<i class="fa-solid fa-magic-wand-sparkles"></i>
拡張機能**メニュー）

- 全体のチャット履歴を一度に翻訳

**<i class="fa-solid fa-keyboard"></i>入力を翻訳**ボタン（**<i class="fa-solid fa-magic-wand-sparkles"></i>
拡張機能**メニュー）

- 現在の入力テキストのみを翻訳
- メッセージを送信する前に役立つ

**<i class="fa-solid fa-language"></i>メッセージを翻訳**アイコン（**<i class="fa-solid fa-ellipsis"></i>メッセージ
アクション**）

すべてのメッセージのツールバー

- クリックしてそのメッセージのみを翻訳
- もう一度クリックして元のテキストに戻す

**自動モード**（**チャット翻訳**ドロワー（**<i class="fa-solid fa-cubes"></i>
拡張機能**パネル）

- ユーザー入力、AI応答、またはその両方を自動的に翻訳

**/translate** スラッシュコマンド

- `/translate [target=language_code] text`を使用してテキストを翻訳

## 構成

構成オプションは**チャット翻訳**ドロワー（**<i class="fa-solid fa-cubes"></i>
拡張機能**パネル）で利用できます。

#### プロバイダー

- 希望する[翻訳サービス](#translation-providers)を選択
- **<i class="fa-solid fa-key"></i>APIキー**アイコンをクリック（表示されている場合）APIキーを入力
- **<i class="fa-solid fa-link"></i>カスタムURL**アイコンをクリック（表示されている場合）カスタムAPI URLを入力

#### ターゲット言語

メッセージを書く言語、またはAI応答を読む言語を選択。

#### 自動モード

自動翻訳動作を構成。

- **なし**：自動翻訳なし
- **応答を翻訳**：AI応答をターゲット言語に自動的に翻訳
- **入力を翻訳**：ユーザー入力を英語に自動的に翻訳
- **両方を翻訳**：ユーザー入力とAI応答の両方を翻訳

#### 翻訳をクリア

**<i class="fa-solid fa-trash-can"></i>翻訳をクリア**ボタン現在のチャット内のメッセージから翻訳をすべて削除します。元のメッセージは保持。

### 構成例：中国語から英語へのチャット

中国語を話すユーザーが英語で動作するAIとチャットできるワークフローを設定：

1. 自動モードを「両方を翻訳」に設定
2. ターゲット言語を「中国語（簡体字）」または「中国語（繁体字）」に設定
3. 優れた言語自動検出（Google、DeepLなど）の翻訳プロバイダーを選択

このセットアップは：

- ユーザーの中国語入力を英語にAI翻訳
- AI応答を英語から中国語にユーザー向けに翻訳

このセットアップは入力の自動言語検出に依存します。より正確な制御のため、今後のアップデートが明示的なソース言語選択を含む可能性があります。

## 翻訳プロバイダー

**:icon-cloud:** クラウドベース
**<i class="fa-solid fa-link"></i>** ローカル、カスタムURL
**<i class="fa-solid fa-key"></i>** APIキーが必要

| Provider | Location | Features |
|----------|----------|----------|
| [Libre Translate](https://libretranslate.com/) | :icon-cloud: <i class="fa-solid fa-key"></i> <i class="fa-solid fa-link"></i> | セルフホストされた(AGPL-3.0)独有の翻訳サービスに代わるもので、クラウドホストされたProティア付き | 
| [Google Translate](https://cloud.google.com/translate) | :icon-cloud: | 広く使用され、多くの言語をサポート、良い精度 |
| [Lingva Translate](https://lingva.ml/) | <i class="fa-solid fa-link"></i> | Google Translateの代替フロントエンド、オープンソース(AGPL-3.0)、プライバシー重視 |
| [DeepL](https://www.deepl.com/) | :icon-cloud: <i class="fa-solid fa-key"></i> | 高品質の翻訳、特にヨーロッパ言語向け |
| [DeepLX](https://github.com/OwO-Network/DeepLX) | <i class="fa-solid fa-link"></i> | セルフホストされたDeepLプロキシ、オープンソース(MIT)、無料ですがDeepL Proプロキシする場合はDeepL APIキーが必要 |
| [Bing Translator](https://www.bing.com/translator) | :icon-cloud: | マイクロソフトの翻訳サービス、Azureサービスと統合 |
| [OneRing Translator](https://github.com/janvarev/OneRingTranslator) | <i class="fa-solid fa-link"></i> | Google Translateおよび他のプロバイダーのセルフホストされたフロントエンド、プライバシー重視、オープンソース(AGPL-3.0) |
| [Yandex Translate](https://translate.yandex.com/) | :icon-cloud: | ロシア語および東ヨーロッパ言語向けに良好 |
