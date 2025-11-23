---
order: 90
templating: false
route: /usage/prompts/context-template/
---

# コンテキストテンプレート

!!! Applies to: Text Completion APIs
Chat Completion APIs で同等の設定については、[Prompt Manager](prompt-manager.md) を使用してください。
!!!

通常、AI モデルでは、キャラクター データを特定の方法で提供する必要があります。SillyTavern には、異なるモデルのプリメードされた変換ルールのリストが含まれていますが、必要に応じてカスタマイズできます。

これらの設定は「[Advanced Formatting](advancedformatting.md)」パネルで編集します。

## Story String

このフィールドは、プロンプト前置き（内部的には「story string」として知られている）のテンプレートです。これは、テキスト補完とインストラクト モデルの [Character Cards](/Usage/Characters/index.md) で定義された情報を追加するための主な方法です。

テンプレートは Handlebars 構文、カスタム テキスト注入またはフォーマット、および任意の [macros](/Usage/Characters/macros.md) をサポートしています。言語リファレンスは <https://handlebarsjs.com/guide/> を参照してください。

Handlebars エバリュエーターに次のパラメーターを提供します（ダブル中括弧で囲まれています）：

1. `{{anchorBefore}}`: 「Before Story String」の位置を使用するように設定されたプロンプト。
2. `{{anchorAfter}}`: 「After Story String」の位置を使用するように設定されたプロンプト。
3. `{{description}}`: キャラクターの [Description](/Usage/Characters/characterdesign.md#character-description)。
4. `{{scenario}}`: キャラクターの [Scenario](/Usage/Characters/characterdesign.md#scenario)。
5. `{{personality}}`: キャラクターの [Personality](/Usage/Characters/characterdesign.md#personality-summary)。
6. `{{system}}`: [システム プロンプト](advancedformatting.md#system-prompt) またはキャラクターの [main prompt](/Usage/Characters/characterdesign.md#prompt-overrides) オーバーライド（存在し、「Prefer Char. Prompt」がユーザー設定で有効な場合）。
7. `{{persona}}`: 選択した [ペルソナの説明](/Usage/personas.md#persona-description)。
8. `{{char}}`: キャラクター名。
9. `{{user}}`: 選択したペルソナの名前。
10. `{{wiBefore}}` または `{{loreBefore}}`: 「Before Char Defs」に設定された位置を持つ統合アクティブ化 [World Info](/Usage/worldinfo.md) エントリ。
11. `{{wiAfter}}` または `{{loreAfter}}`: 「After Char Defs」に設定された位置を持つ統合アクティブ化 [World Info](/Usage/worldinfo.md) エントリ。
12. `{{mesExamples}}`: （オプション）キャラクターの [Example Dialogues](/Usage/Characters/characterdesign.md#examples-of-dialogue)、instruct 形式で区切り記号を使用してフォーマットされています。
13. `{{mesExamplesRaw}}`: キャラクターの [Example Dialogues](/Usage/Characters/characterdesign.md#examples-of-dialogue) (フォーマットなし)。

!!!tip **Important**  
Story String で `{{mesExamples}}` を使用する場合は、<i class="fa-solid fa-user-cog"></i> **User Settings** パネルで **「Example Messages Behavior」** を **「Never include examples」** に設定して、プロンプト内のメッセージ例の重複を避けてください。
!!!

特別な `{{trim}}` マクロがサポートされており、それを取り囲む改行を削除します。テキストの一部を前の行で改行で分離しないようにしたい場合はそれを使用してください（_spaces **は** トリミングされません_）。

**WARNING**: 上記のいずれかのパラメーターがストーリー文字列テンプレートから欠落している場合、プロンプトにはまったく送信されません。

### Prompt Anchors

`{{anchorBefore}}` と `{{anchorAfter}}` は、さまざまな拡張機能と雑多な機能によって追加されたプロンプトの汎用プレースホルダーです（例：

* [Author's Note](/Usage/Characters/Author's-Note.md)
* [Summaries](/extensions/Summarize.md)
* [Chat Vectorization](/extensions/Chat-vectorization.md) / [Data Bank](/Usage/Characters/data-bank.md)
* [STscript injections](/For_Contributors/st-script.md#prompt-injections)
* [Web Search](/extensions/WebSearch.md)

### Story String position

デフォルトでは、レンダリングされたストーリー文字列（すべてのプレースホルダーが置き換えられます）がプロンプトの非常に最初に配置され、その後に例のメッセージと表示されるチャット履歴が続きます。

または、「In-chat @ Depth」オプションを選択することで、チャット コンテキスト内の特定の深さにストーリー文字列を配置する動的な位置に移動できます。

!!!warning **Attention**
テンプレートにストーリー文字列を折り返すための静的なプロンプト要素（モデル固有のプレフィックスまたはサフィックス）が含まれている場合、「In-Chat @ Depth」位置を使用するとそれは不正確にダブル ラップされ、重複したシーケンスが生成される可能性があり、予期しない結果につながる可能性があります。

この場合、次の方法のいずれかで問題を修正できます：

1. **組み込みテンプレート**: [Advanced Formatting](/Usage/Prompts/advancedformatting.md#resetting-templates) で説明されている手順を使用してテンプレートをデフォルトにリセットします。
2. **カスタム テンプレート**: ストーリー文字列テンプレートから [Story String Sequences](/Usage/Prompts/instructmode.md#sequences-story-string-wrapping) に静的要素を移動します。
!!!

### Story String wrapping

!!!
次のセクションは、**Instruct Mode** が ON のときにのみ適用されます。
!!!

* **Default** position: レンダリングされたストーリー文字列は [Story String Sequences](/Usage/Prompts/instructmode.md#sequences-story-string-wrapping) で定義されたシーケンスを使用してラップされます。
* **In-chat @ Depth** position: レンダリングされたストーリー文字列は、選択したロール（デフォルト：System）用に [Chat Messages Sequences](/Usage/Prompts/instructmode.md#sequences-chat-messages-wrapping) で定義されたシーケンスを使用してラップされます。

## Example Separator

ブロック ヘッダーおよび例のダイアログ ブロック間の区切り文字として使用されます。例のダイアログ内の `<START>` タグのインスタンスは、このフィールドの内容に置き換えられます。

## Chat Start

レンダリングされたストーリー文字列の後およびサンプル ダイアログ ブロックの後に挿入されますが、コンテキスト内の最初のメッセージの前に挿入されます。

## Separators as Stop Strings

「Example Separator」と「Chat Start」を停止文字列のリストに追加します。

モデルが例のダイアログの全ブロックをハルシネートする傾向がある場合や、区切り文字が先行する場合に役立ちます。

## Names as Stop Strings

キャラクター名とユーザー ペルソナ名を停止文字列のリストに追加します。

モデルの詐称を防ぐために、オンに保つことをお勧めします。

## Always add character's name to prompt

!!!info  
Instruct Mode が ON の場合、この設定は効果がありません。名前の動作は代わりに選択された [Include Names](/Usage/Prompts/instructmode.md#include-names) オプションで定義されます。
!!!

キャラクターの名前をプロンプトに追加して、モデルにキャラクターとしてメッセージを完成させるように強制します：

```txt
** OTHER CONTEXT HERE **
Character:
```
