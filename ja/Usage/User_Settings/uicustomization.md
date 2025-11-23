---
order: 20
route: /usage/core-concepts/uicustomization/
---

# UIカスタマイズ

## UI Theme

### Theme Management

テーマファイルでは、UIのカスタマイズを保存、共有、再利用できます。さまざまな気分や目的のための複数のテーマを保持し、すぐにそれらを切り替えることができます。

* テーマファイルのインポート/エクスポート
* 既存のテーマを削除
* 現在のテーマへの変更を保存
* 新しいテーマとして保存

このセクションのすべての設定は、現在のテーマに保存されます。テーマを切り替えると、設定は新しいテーマの設定に置き換えられます。

### Display Settings

これらの表示オプションは、チャットインターフェイスでキャラクターとメッセージがどのように提示されるかに影響を与えます。

#### Avatar Style

円、正方形、長方形、または丸い正方形から選択してください。この設定は、ユーザーAIアバターの両方に適用されます。

#### Chat Style

| Style        | Description                                                                                                                                                    | [Slash command](/For_Contributors/st-script.md#ui-styling) |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| **Flat**     | クリーンで連続的な「チャットログ」スタイル。AIインタラクションが人生に来る平らなキャンバス。                                                                                                                                 | `/flat`<br>`/default`                                      |
| **Bubbles**  | 異なるメッセージのための「インスタントメッセンジャー」スタイルで、喜びのある丸角と微妙な3D効果。                                          | `/bubble`<br>`/bubbles`                                    |
| **Document** | テキストに焦点を当てたレイアウトで、コンパクトなドキュメント風の外観。過去のメッセージのアバター、タイムスタンプ、メッセージコントロールボタンを非表示にします。 | `/single`<br>`/story`                                      |

### Notifications

通知ポップアップ（トーストメッセージ）がスクリーンに表示される位置を設定します。

* 左上
* 上中央（デフォルト）
* 右上
* 左下
* 下中央
* 右下

### Theme Colors

UIの要素ごとのカラースキームをカスタマイズして、完璧なテーマを作成してください。色は色ピッカーを使用して選択でき、該当する場合、透明度オプションを含みます。

* Main Text
* Italics Text
* Underlined Text
* Quote Text
* Text Shadow
* Chat Background
* UI Background
* UI Border
* User Message
* AI Message

### Layout & Visual Settings

これらのスライダーを使用してインターフェイスの視覚的な提示を微調整してください。

* **Chat Width**: チャットウィンドウ幅を調整（スクリーンの25〜100％）
* **Font Scale**: テキストサイズをカスタマイズ（0.5-1.5倍）
* **Blur Strength**: UIパネルぼかしを制御（0-30）
* **Shadow Width**: テキストシャドウ強度を調整（0-5）

### Theme Toggles

これらのスイッチはさまざまなUI機能と動作を制御しています。一部のオプションはより低いエンドのデバイスでパフォーマンスを向上させることができますが、他はチャットインターフェイスに有用な情報または機能を追加できます。

* **Reduced Motion**: アニメーションと遷移を無効にする
* **No Blur Effect**: パフォーマンスを向上させるためのバックグラウンドぼかしを削除
* **No Text Shadows**: テキストシャドウエフェクトを無効化
* **[Visual Novel mode](Visual-Novel.md)**: バックグラウンドスプライトのコンパクトなチャット
* **Expand Message Actions**: 常に完全なメッセージコンテキストメニューを表示
* **Zen Sliders**: 簡略化されたパラメーター制御
* **Mad Lab Mode**: 制限されていないパラメーター範囲
* **Message Timer**: AI応答生成時間を表示
* **Chat Timestamps**: メッセージタイムスタンプを表示
* **Model Icons**: メッセージのAIモデルアイコンを表示
* **Message IDs**: シーケンシャルなメッセージ番号を表示
* **Hide Chat Avatars**: チャットからアバターを削除
* **Message Token Count**: メッセージごとのトークン数を表示
* **Compact Input Area**: 1行の入力（モバイルのみ）
* **Swipe # for All Messages**: すべてのメッセージのスワイプ番号を表示
* **Characters Hotswap**: お気に入りのキャラクターのためのクイック選択ボタン
* **Avatar Hover Magnification**: アバターホバーのズーム効果
* **Tags as Folders**: タグをフォルダとして使用してキャラクターを整理
* **Click to Edit**: メッセージをクリックしてメッセージエディターをすばやく開く

### Custom CSS

チャットインターフェイスの外観をさらにカスタマイズするためにカスタムCSSスタイルを適用できます。

<i class="fa-fw fa-solid fa-maximize" title="Expand icon"></i>**Expand**を使用して、エディターウィンドウを展開して、より良い視認性と編集を行います。

テーマを切り替えると、カスタムCSSがカスタムCSSの新しいテーマで置き換えられます。テーマ間で切り替えるときにカスタムCSSを保持したい場合は、テーマにカスタムCSSを保存することを確認してください。

多くのカスタムCSSを使用する場合や、いくつかのテーマで同じカスタムCSSを使用したい場合は、非公式な[CSS Snippets extension](https://github.com/LenAnderson/SillyTavern-CssSnippets)がカスタムCSSを管理および整理するのに役立ちます。

---

## Message Sound

ボットから新しいメッセージを受信したときに再生する独自のカスタムサウンドを、SillyTavernフォルダの次のMP3ファイルを置き換えることで再生できます：

`public/sounds/message.mp3`

音量80％で再生されます。

「[Background Sound Only](index.md#miscellaneous)」オプションが有効になっている場合、SillyTavernウィンドウが**unfocused**の場合のみ、音が再生されます。

## Formulas Rendering

数式のレンダリングを有効にするには、[LaTeX extension](https://github.com/SillyTavern/Extension-LaTeX)を使用してください。拡張機能を取得するには、SillyTavern内の「Download Extensions & Assets」メニューを介してインストールする必要があります。

LaTeXおよびAsciiMathのそれぞれのコードブロック内でフォーミュラを`latex`または`asciimath`言語識別子で入力してください。拡張機能は、レンダリングの[KaTeX](https://katex.org/)を使用します。

<pre><code>```latex
\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}
```

```asciimath
int_{-oo}^{oo} e^{-x^2} dx = sqrt{pi}
```</code></pre>

!!!info Deprecation notice
レガシー`$`および`$$`ラッパー構文はサポートされなくなりました。以下のregexスクリプトを使用して古い構文をpolyfillしてください：

* [$$ - LaTeX](https://github.com/SillyTavern/Extension-LaTeX/raw/refs/heads/main/assets/$$_-_latex.json)
* [$ - AsciiMath](https://github.com/SillyTavern/Extension-LaTeX/raw/refs/heads/main/assets/$_-_asciimath.json)
!!!
