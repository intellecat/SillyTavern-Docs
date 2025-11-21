---
order: 0
icon: file-symlink-file
route: /usage/st-script/
templating: false
---

# STscript言語リファレンス

## STscriptとは?

STscriptは、本格的なコーディングなしでSillyTavernの機能を拡張できるシンプルで強力なスクリプト言語です。これにより次のことが可能になります:

- ミニゲームやスピードランチャレンジの作成
- AI搭載のチャットインサイトの構築
- 創造力を解き放ち、他の人と共有

STscriptは、slash commandsエンジンを使用して構築されており、コマンドバッチ処理、データパイピング、マクロ、変数を利用しています。
これらの概念については、以下のドキュメントで説明します。

### セキュリティ上の注意

大きな力には大きな責任が伴います。スクリプトを実行する前に、必ず注意深く検査してください。

## Hello, World!

最初のスクリプトを実行するには、任意のSillyTavernチャットを開き、チャット入力バーに次のように入力します:

```stscript
/pass Hello, World! | /echo
```

| ![Hello World](/static/scripts/hello-world.png) |
|-------------------------------------------------|

画面上部のトースト通知にメッセージが表示されるはずです。では、これを少しずつ分解してみましょう。

スクリプトは、スラッシュで始まるコマンドのバッチで、名前付きおよび名前なし引数の有無にかかわらず、コマンド区切り文字`|`で終了します。

コマンドは順次実行され、データを相互に転送します。

1. `/pass`コマンドは、"Hello, World!"の定数値を名前なし引数として受け入れ、パイプに書き込みます。
2. `/echo`コマンドは、前のコマンドからパイプを通じて値を受け取り、トースト通知として表示します。

!!!tip
**ヒント:** 利用可能なすべてのコマンドのリストを表示するには、チャットに`/help slash`と入力します。
!!!

定数名前なし引数とパイプは互換性があるため、このスクリプトは次のように単純に書き直すことができます:

```stscript
/echo Hello, World!
```

## ユーザー入力

では、スクリプトに少しインタラクティブ性を追加しましょう。ユーザーから入力値を受け取り、通知に表示します。

```stscript
/input Enter your name |
/echo Hello, my name is {{pipe}}
```

1. `/input`コマンドは、名前なし引数で指定されたプロンプトを含む入力ボックスを表示し、その後、出力をパイプに書き込みます。
2. `/echo`は既に出力のテンプレートを設定する名前なし引数を持っているため、`{{pipe}}`マクロを使用してパイプ値がレンダリングされる場所を指定します。

| ![Slim Shady Input](/static/scripts/slim-input.png) | ![Slim Shady Output](/static/scripts/slim-output.png) |
|-----------------------------------------------------|-------------------------------------------------------|

### その他の入出力コマンド

- `/popup (text)` — ブロッキングポップアップを表示します。軽量HTMLフォーマットをサポートします。例:`/popup <font color=red>I'm red!</font>`。
- `/setinput (text)` — ユーザー入力バーの内容を提供されたテキストで置き換えます。
- `/speak voice="name" (text)` — 選択されたTTSエンジンと音声マップのキャラクター名を使用してテキストをナレーションします。例:`/speak name="Donald Duck" Quack!`。
- `/buttons labels=["a","b"] (text)` — 指定されたテキストとボタンラベルを含むブロッキングポップアップを表示します。`labels`はJSON形式の文字列配列、またはそのような配列を含む変数名である必要があります。クリックされたボタンラベルをパイプに返すか、キャンセルされた場合は空の文字列を返します。テキストは軽量HTMLフォーマットをサポートします。

#### `/popup`と`/input`の引数

`/popup`と`/input`は、次の追加の名前付き引数をサポートしています:
- `large=on/off` - ポップアップの垂直サイズを増やします。デフォルト:`off`。
- `wide=on/off` - ポップアップの水平サイズを増やします。デフォルト:`off`。
- `okButton=string` - "Ok"ボタンのテキストをカスタマイズする機能を追加します。デフォルト:`Ok`。
- `rows=number` - (`/input`のみ)入力コントロールのサイズを増やします。デフォルト:1。

例:
```stscript
/popup large=on wide=on okButton="Accept" Please accept our terms and conditions....
```

#### `/echo`の引数

`/echo`は、表示されるメッセージのスタイルを設定する追加の`severity`引数について、次の値をサポートしています。
  - `warning`
  - `error`
  - `info` (デフォルト)
  - `success`

例:

```stscript
/echo severity=error Something really bad happened.
```

## 変数

変数は、コマンドまたはマクロを使用してスクリプト内のデータを保存および操作するために使用されます。変数は、次のタイプのいずれかになります:

- ローカル変数 — 現在のチャットのメタデータに保存され、そのチャットに固有です。
- グローバル変数 — settings.jsonに保存され、アプリ全体のどこにでも存在します。

1. `/getvar name`または`{{getvar::name}}` — ローカル変数の値を取得します。
2. `/setvar key=name value`または`{{setvar::name::value}}` — ローカル変数の値を設定します。
3. `/addvar key=name increment`または`{{addvar::name::increment}}` — ローカル変数の値に`increment`を追加します。
4. `/incvar name`または`{{incvar::name}}` — ローカル変数の値を1増やします。
5. `/decvar name`または`{{decvar::name}}` — ローカル変数の値を1減らします。
6. `/getglobalvar name`または`{{getglobalvar::name}}` — グローバル変数の値を取得します。
7. `/setglobalvar key=name`または`{{setglobalvar::name::value}}` — グローバル変数の値を設定します。
8. `/addglobalvar key=name`または`{{addglobalvar::name:increment}}` — グローバル変数の値に`increment`を追加します。
9. `/incglobalvar name`または`{{incglobalvar::name}}` — グローバル変数の値を1増やします。
10. `/decglobalvar name`または`{{decglobalvar::name}}` — グローバル変数の値を1減らします。
11. `/flushvar name` — ローカル変数の値を削除します。
12. `/flushglobalvar name` — グローバル変数の値を削除します。

- 以前に定義されていない変数のデフォルト値は、空の文字列、または`/addvar`、`/incvar`、`/decvar`コマンドで最初に使用された場合はゼロです。
- `/addvar`コマンドのインクリメントは、インクリメントと変数値の両方が数値に変換できる場合は値の加算または減算を実行し、それ以外の場合は文字列連結を実行します。
- コマンド引数が変数名を受け入れ、同じ名前のローカル変数とグローバル変数の両方が存在する場合、*ローカル変数*が優先されます。
- 変数操作のための*slash commands*はすべて、結果の値を次のコマンドで使用するためにパイプに書き込みます。
- *マクロ*の場合、"get"、"inc"、"dec"タイプのマクロのみが値を返し、"add"と"set"は代わりに空の文字列で置き換えられます。

では、次の例を考えてみましょう:

```stscript
/input What do you want to generate? |
/setvar key=SDinput |
/echo Requesting an image of {{getvar::SDinput}} |
/getvar SDinput |
/imagine
```

1. ユーザー入力の値が`SDinput`という名前のローカル変数に保存されます。
2. `getvar`マクロは、`/echo`コマンドで値を表示するために使用されます。
3. `getvar`コマンドは、変数の値を取得し、パイプを通じて渡すために使用されます。
4. 値は、入力プロンプトとして使用される`/imagine`コマンド(Image Generationプラグインによって提供される)に渡されます。

変数は保存され、スクリプトの実行間でフラッシュされないため、他のスクリプトやマクロを介して変数を参照でき、例のスクリプトの実行中と同じ値に解決されます。値が破棄されることを保証するには、スクリプトに`/flushvar`コマンドを追加します。

### 配列とオブジェクト

変数の値には、JSON形式の配列またはキー値ペア(オブジェクト)を含めることができます。

例:
- 配列:`["apple","banana","orange"]`
- オブジェクト:`{"fruits":["apple","banana","orange"]}`

これらの変数を操作するために、コマンドに次の変更を適用できます:

- `/len`コマンドは、配列内のアイテム数を取得します。
- `index=number/string`名前付き引数を`/getvar`または`/setvar`およびそのグローバル対応物に追加して、配列の0ベースのインデックスまたはオブジェクトの文字列キーでサブ値を取得または設定できます。
  - 数値インデックスが存在しない変数に使用される場合、変数は空の配列`[]`として作成されます。
  - 文字列インデックスが存在しない変数に使用される場合、変数は空のオブジェクト`{}`として作成されます。
- `/addvar`および`/addglobalvar`コマンドは、配列型変数への新しい値のプッシュをサポートします。

## フロー制御 - 条件文

`/if`コマンドを使用して、定義されたルールに基づいて実行を分岐する条件式を作成できます。

```stscript
/if left=valueA right=valueB rule=comparison else={: /echo (command on false) :} {: /echo (command on true) :}
```

次の構文もサポートされていますが、

```stscript
/if left=valueA right=valueB rule=comparison else="(command on false)" "(command on true)"
```

`{: closures :}`を使用すると、よりクリーンなスクリプトを書くことができます。

次の例を見てみましょう:

```stscript
/input What's your favorite drink? |
/if left={{pipe}} right="black tea" rule=eq else={: /echo You shall not pass | /abort :} {: /echo Welcome to the club, {{user}} :}
```

このスクリプトは、ユーザー入力を必要な値に対して評価し、入力値に応じて異なるメッセージを表示します。

### `/if`の引数

1. `left`は最初のオペランドです。これをAと呼びましょう。
2. `right`は2番目のオペランドです。これをBと呼びましょう。
3. `rule`は、オペランドに適用される演算です。
4. `else`は、ブール比較の結果がfalseの場合に実行されるサブコマンドのオプションの文字列です。
5. 名前なし引数は、ブール比較の結果がtrueの場合に実行されるサブコマンドです。

オペランド値は、次の順序で評価されます:

1. 数値リテラル
2. ローカル変数名
3. グローバル変数名
4. 文字列リテラル

名前付き引数の文字列値は、複数の単語の文字列を許可するために引用符でエスケープできます。引用符は破棄されます。

### ブール演算

ブール比較でサポートされているルールは次のとおりです。オペランドに適用される演算は、trueまたはfalse値のいずれかになります。

1. `eq` (等しい) => A = B
2. `neq` (等しくない) => A != B
3. `lt` (より小さい) => A < B
4. `gt` (より大きい) => A > B
5. `lte` (以下) => A <= B
6. `gte` (以上) => A >= B
7. `not` (単項否定) => !A
8. `in` (部分文字列を含む) => Aにはが含まれる、大文字小文字を区別しない
9. `nin` (部分文字列を含まない) => AにBが含まれない、大文字小文字を区別しない

### サブコマンド

サブコマンドは、実行するslash commandsのリストを含む文字列です。

1. サブコマンドでコマンドバッチ処理を使用するには、コマンド区切り文字をエスケープする必要があります(以下を参照)。
2. マクロ値は、条件文に入ったときに実行されるため、サブコマンドが実行されるときではないため、マクロを追加でエスケープして、サブコマンドの実行時まで評価を遅らせることができます。
3. サブコマンドの実行結果は、`/if`の後のコマンドにパイプされます。
4. `/abort`コマンドは、遭遇したときにスクリプトの実行を中断します。

`/if`コマンドは、三項演算子として使用できます。
次の例は、変数`a`が5に等しい場合は"true"文字列を次のコマンドに渡し、それ以外の場合は"false"文字列を渡します。

```stscript
/if left=a right=5 rule=eq else={: /pass false:} {: /pass true :} |
/echo
```

## エスケープシーケンス

### マクロ

マクロのエスケープは以前と同じように機能します。ただし、closuresを使用すると、マクロをエスケープする必要がはるかに少なくなります。2つの開き中括弧、または開きと閉じのペアの両方をエスケープします。

```stscript
/echo \{\{char}} |
/echo \{\{char\}\}
```

### パイプ

Closures内のパイプは、(コマンド区切り文字として使用される場合)エスケープする必要はありません。コマンド区切り文字の代わりにリテラルパイプ文字を使用したい場所では、エスケープする必要があります。

```stscript
/echo title="a\|b" c\|d |
/echo title=a\|b c\|d |
```

パーサーフラグ`STRICT_ESCAPING`を使用すると、引用符で囲まれた値でパイプをエスケープする必要がなくなります。

```stscript
/parser-flag STRICT_ESCAPING |
/echo title="a|b" c\|d |
/echo title=a\|b c\|d |
```

### 引用符

引用符で囲まれた値内でリテラル引用符文字を使用するには、文字をエスケープする必要があります。

```stscript
/echo title="a \"b\" c" d "e" f
```

### スペース

名前付き引数の値にスペースを使用するには、値を引用符で囲むか、スペース文字をエスケープする必要があります。

```stscript
/echo title="a b" c d |
/echo title=a\ b c d
```

### Closure区切り文字

Closureの開始または終了をマークするために使用される文字の組み合わせを使用したい場合は、単一のバックスラッシュでシーケンスをエスケープする必要があります。

```stscript
/echo \{: |
/echo \:}
```

## パイプブレーカー

```stscript
||
```

前のコマンドの出力が名前なし引数として次のコマンドに自動的に注入されるのを防ぐには、2つのコマンドの間に二重パイプを配置します。

```stscript
/echo we don't want to pass this on ||
/world
```

## Closures

```stscript
{: ... :}
```

Closures(ブロックステートメント、ラムダ、無名関数、何と呼んでも構いません)は、`{:`と`:}`の間にラップされた一連のコマンドで、コードのその部分が実行されるときにのみ評価されます。

### サブコマンド

Closuresにより、サブコマンドの使用がはるかに簡単になり、パイプとマクロをエスケープする必要がなくなります。

```stscript
// if without closures |
/if left=1 rule=eq right=1
    else="
        /echo not equal \|
        /return 0
    "
    /echo equal \|
    /return \{\{pipe}}
```

```stscript
// if with closures |
/if left=1 rule=eq right=1
    else={:
        /echo not equal |
        /return 0
    :}
    {:
        /echo equal |
        /return {{pipe}}
    :}
```

### スコープ

Closuresには独自のスコープがあり、スコープ変数をサポートします。スコープ変数は`/let`で宣言され、その値は`/var`で設定および取得されます。スコープ変数を取得する別の方法は、`{{var::}}`マクロです。

```stscript
/let x |
/let y 2 |
/var x 1 |
/var y |
/echo x is {{var::x}} and y is {{pipe}}.
```

Closure内では、同じClosure内またはその祖先のいずれかで宣言されたすべての変数にアクセスできます。Closureの子孫で宣言された変数にはアクセスできません。  
Closureの祖先の1つで宣言された変数と同じ名前で変数が宣言されている場合、このClosureとその子孫では祖先変数にアクセスできません。

```stscript
/let x this is root x |
/let y this is root y |
/return {:
    /echo called from level-1: x is "{{var::x}}" and y is "{{var::y}}" |
    /delay 500 |
    /let x this is level-1 x |
    /echo called from level-1: x is "{{var::x}}" and y is "{{var::y}}" |
    /delay 500 |
    /return {:
        /echo called from level-2: x is "{{var::x}}" and y is "{{var::y}}" |
        /let x this is level-2 x |
        /echo called from level-2: x is "{{var::x}}" and y is "{{var::y}}" |
        /delay 500
    :}()
:}() |
/echo called from root: x is "{{var::x}}" and y is "{{var::y}}"
```

### 名前付きClosures

```stscript
/let x {: ... :} | /:x
```

Closuresは、後で呼び出すために、またはサブコマンドとして使用するために、変数(スコープ変数のみ)に割り当てることができます。

```stscript
/let myClosure {:
    /echo this is my closure
:} |
/:myClosure
```

```stscript
/let myClosure {:
    /echo this is my closure |
    /delay 500
:} |
/times 3 {{var::myClosure}}
```

`/:`は、Quick Repliesを実行するためにも使用できます。これは単に`/run`の省略形です。

```stscript
/:QrSetName.QrButtonLabel |
/run QrSetName.QrButtonLabel
```

### Closure引数

名前付きClosuresは、slash commandsと同様に、名前付き引数を取ることができます。引数にはデフォルト値を設定できます。

```stscript
/let myClosure {: a=1 b=
    /echo a is {{var::a}} and b is {{var::b}}
:} |
/:myClosure b=10
```

### Closuresとパイプされた引数

親Closureからのパイプされた値は、子Closureの最初のコマンドに自動的に注入されません。  
親のパイプされた値を`{{pipe}}`で明示的に参照することはできますが、Closure内の最初のコマンドの名前なし引数を空白のままにすると、値は自動的に注入*されません*。

```stscript
/* これは以前、モデルを"foo"に変更しようとしていました
   なぜなら、ループの外側の/echoからの値"foo"が
   ループ内の/modelコマンドに注入されたからです。
   現在は、変更を試みることなく、単に現在のモデルをエコーします。
*|
/echo foo |
/times 2 {:
	/model |
	/echo |
:} |
```
```stscript
/* {{pipe}}マクロを明示的に使用することで、古い動作を再現できます。
*|
/echo foo |
/times 2 {:
	/model {{pipe}} |
	/echo |
:} |
```

### 即座に実行されるClosures

```stscript
{: ... :}()
```

Closuresは即座に実行でき、その戻り値で置き換えられます。これは、Closuresの明示的なサポートが存在しない場所や、多数の中間変数を必要とするコマンドを短縮するのに役立ちます。

```stscript
// a simple length comparison of two strings without closures |
/len foo |
/var lenOfFoo {{pipe}} |
/len bar |
/var lenOfBar {{pipe}} |
/if left={{var::lenOfFoo}} rule=eq right={{var:lenOfBar}} /echo yay!
```

```stscript
// the same comparison with immediately executed closures |
/if left={:/len foo:}() rule=eq right={:/len bar:}() /echo yay!
```

スコープ変数内に保存された名前付きClosuresを実行するだけでなく、`/run`コマンドを使用してClosuresを即座に実行することもできます。

```stscript
/run {:
	/add 1 2 3 4 |
:} |
/echo |
```

## コメント

```stscript
// ... | /# ...
```

コメントは、スクリプトコード内の人間が読める説明または注釈です。コメントはパイプを破壊しません。

```stscript
// this is a comment |
/echo foo |
/# this is also a comment
```

### ブロックコメント

ブロックコメントを使用すると、複数のコマンドを一度にコメントアウトできます。パイプで終了しません。

```stscript
/echo foo |
/*
/echo bar |
/echo foobar |
*|
/echo foo again |
```


## フロー制御

### ループ:`/while`と`/times`

特定の条件が満たされるまでループでコマンドを実行する必要がある場合は、`/while`コマンドを使用します。

```stscript
/while left=valueA right=valueB rule=operation guard=on "commands"
```

ループの各ステップで、変数Aの値を変数Bの値と比較し、条件がtrueを返す場合は、引用符で囲まれた有効なslash commandsを実行し、それ以外の場合はループを終了します。このコマンドは、出力パイプに何も書き込みません。

#### `/while`の引数

**利用可能なブール比較、変数の処理、リテラル値、およびサブコマンドのセットは、`/if`コマンドと同じです。**

オプションの`guard`名前付き引数(デフォルトは`on`)は、無限ループから保護するために使用され、反復回数を100に制限します。
無限ループを許可してオフにするには、`guard=off`を設定します。

この例は、`i`の値が10に達するまで1を追加し、結果の値(この場合は10)を出力します。

```stscript
/setvar key=i 0 |
/while left=i right=10 rule=lt "/addvar key=i 1" |
/echo {{getvar::i}} |
/flushvar i
```

#### `/times`の引数

指定された回数だけサブコマンドを実行します。

`/times (repeats) "(command)"` – 引用符で囲まれた有効なslash commandsを指定された回数だけ繰り返します。例:`/setvar key=i 1 | /times 5 "/addvar key=i 1"`は、"i"の値に1を5回追加します。
- {{timesIndex}}は、反復番号(0ベース)で置き換えられます。例:`/times 4 {:/echo {{timesIndex}}:}`は、0から4までの数字をエコーします。
- ループはデフォルトで100回の反復に制限されています。無効にするには`guard=off`を渡します。

### ループとClosuresからの脱出

```stscript
/break |
```

`/break`コマンドを使用して、ループ(`/while`または`/times`)またはClosureから早期に抜け出すことができます。`/break`の名前なし引数を使用して、現在のパイプとは異なる値を渡すことができます。  
`/break`は現在、次のコマンドに実装されています:
- `/while` - ループから早期に終了
- `/times` - ループから早期に終了
- `/run`(Closureまたは変数経由のClosureを使用) - Closureから早期に終了
- `/:`(Closureを使用) - Closureから早期に終了

```stscript
/times 10 {:
	/echo {{timesIndex}}
	/delay 500 |
	/if left={{timesIndex}} rule=gt right=3 {:
		/break
	:} |
:} |
```

```stscript
/let x {: iterations=2
	/if left={{var::iterations}} rule=gt right=10 {:
		/break too many iterations! |
	:} |
	/times {{var::iterations}} {:
		/delay 500 |
		/echo {{timesIndex}} |
	:} |
:} |
/:x iterations=30 |
/echo the final result is: {{pipe}}
```

```stscript
/run {:
	/break 1 |
	/pass 2 |
:} |
/echo pipe will be one: {{pipe}} |
```

```stscript
/let x {:
	/break 1 |
	/pass 2 |
:} |
/:x |
/echo pipe will be one: {{pipe}} |
```

## 数学演算

- 以下のすべての演算は、一連の数値または変数名を受け入れ、結果をパイプに出力します。
- 無効な演算(ゼロ除算など)、およびNaN値または無限大を生成する演算は、ゼロを返します。
- 乗算、加算、最小、最大は、スペースで区切られた無制限の数の引数を受け入れます。
- 減算、除算、べき乗、剰余は、スペースで区切られた2つの引数を受け入れます。
- 正弦、余弦、自然対数、平方根、絶対値、丸めは、1つの引数を受け入れます。

**演算のリスト:**

1. `/add (a b c d)` – 値のセットの加算を実行します。例:`/add 10 i 30 j`
2. `/mul (a b c d)` – 値のセットの乗算を実行します。例:`/mul 10 i 30 j`
3. `/max (a b c d)` – 値のセットから最大値を返します。例:`/max 1 0 4 k`
4. `/min (a b c d)` – 値のセットから最小値を返します。例:`/min 5 4 i 2`
5. `/sub (a b)` – 2つの値の減算を実行します。例:`/sub i 5`
6. `/div (a b)` – 2つの値の除算を実行します。例:`/div 10 i`
7. `/mod (a b)` – 2つの値の剰余演算を実行します。例:`/mod i 2`
8. `/pow (a b)` – 2つの値のべき乗演算を実行します。例:`/pow i 2`
9. `/sin (a)` – 値の正弦演算を実行します。例:`/sin i`
10. `/cos (a)` – 値の余弦演算を実行します。例:`/cos i`
11. `/log (a)` – 値の自然対数演算を実行します。例:`/log i`
12. `/abs (a)` – 値の絶対値演算を実行します。例:`/abs -10`
13. `/sqrt (a)`– 値の平方根演算を実行します。例:`/sqrt 9`
14. `/round (a)` – 値を最も近い整数に丸める演算を実行します。例:`/round 3.14`
15. `/rand (round=round|ceil|floor from=number=0 to=number=1)` – fromからtoまでの乱数を返します。例:`/rand`または`/rand 10`または`/rand from=5 to=10`。範囲は包括的です。返される値には小数部分が含まれます。`round`名前付き引数を使用して整数値を取得します。例:`/rand round=ceil`は切り上げ、`round=floor`は切り捨て、`round=round`は最も近い値に丸めます。

### 例1:半径50の円の面積を取得する。

```stscript
/setglobalvar key=PI 3.1415 |
/setvar key=r 50 |
/mul r r PI |
/round |
/echo Circle area: {{pipe}}
```

### 例2:5の階乗を計算する。

```stscript
/setvar key=input 5 |
/setvar key=i 1 |
/setvar key=product 1 |
/while left=i right=input rule=lte "/mul product i \| /setvar key=product \| /addvar key=i 1" |
/getvar product |
/echo Factorial of {{getvar::input}}: {{pipe}} |
/flushvar input |
/flushvar i |
/flushvar product
```

## LLMの使用

スクリプトは、次のコマンドを使用して、現在接続されているLLM APIにリクエストを送信できます:

- `/gen (prompt)` — 選択されたキャラクターとチャットメッセージを含む提供されたプロンプトを使用してテキストを生成します。
- `/genraw (prompt)` — 提供されたプロンプトのみを使用してテキストを生成し、現在のキャラクターとチャットを無視します。
- `/trigger` — 通常の生成をトリガーします("Send"ボタンをクリックするのと同等)。グループチャットの場合、オプションで1ベースのグループメンバーインデックスまたはキャラクター名を指定して返信させることができます。それ以外の場合は、グループ設定に従ってグループラウンドをトリガーします。

### `/gen`と`/genraw`の引数

```stscript
/genraw lock=on/off stop=[] instruct=on/off (prompt)
```

- `lock` — `on`または`off`を指定できます。生成中にユーザー入力をブロックするかどうかを指定します。デフォルト:`off`。
- `stop` — JSON形式の文字列配列。この生成のみにカスタム停止文字列を追加します(APIがサポートしている場合)。デフォルト:なし。
- `instruct`(`/genraw`のみ) — `on`または`off`を指定できます。入力プロンプトでinstruct formattingを使用できます(instructモードが有効で、APIがサポートしている場合)。純粋なプロンプトを強制するには`off`に設定します。デフォルト:`on`。
- `as`(Text Completion API用) — `system`(デフォルト)または`char`を指定できます。最後のプロンプト行がどのようにフォーマットされるかを定義します。`char`はキャラクター名を使用し、`system`は名前なしまたは中立的な名前を使用します。

生成されたテキストは、パイプを通じて次のコマンドに渡され、変数に保存したり、I/O機能を使用して置換したりできます:

```stscript
/genraw Write a funny message from Cthulhu about taking over the world. Use emojis. |
/popup <h3>Cthulhu says:</h3><div>{{pipe}}</div>
```

| ![Cthulhu Says](/static/scripts/cthulhu-says.png) |
|---------------------------------------------------|

または、生成されたメッセージをキャラクターからの応答として挿入します:

```stscript
/genraw You have been memory wiped, your name is now Lisa and you're tearing me apart. You're tearing me apart Lisa! |
/sendas name={{char}} {{pipe}}
```

## 一時的なキャラクター

グループチャットにいない場合、スクリプトは、現在接続されているLLMに別のキャラクターとして一時的にリクエストを送信できます。

- `/ask (prompt)` — 指定されたキャラクターとチャットメッセージを含む提供されたプロンプトを使用してテキストを生成します。このキャラクターからの応答のスワイプは、現在のキャラクターに戻ることに注意してください。

```stscript
/ask name=... (prompt)
```
### `/ask`の引数

- `name` — **必須**。質問するキャラクターの名前(または一意のキャラクター識別子、アバターキーなど)。これは名前付き引数として提供する必要があります。
- `return` — 戻り値の提供方法を指定します。デフォルトは`pipe`(コマンドパイプ経由の出力)です。APIでサポートされている場合は、他のオプションを指定できます。


```stscript
/ask name=Alice What is your favorite color?
```

## プロンプトインジェクション

スクリプトは、カスタムLLMプロンプトインジェクションを追加でき、基本的に無制限のAuthor's Notesと同等になります。

- `/inject (text)` — 現在のチャットの通常のLLMプロンプトに任意のテキストを挿入し、一意の識別子を必要とします。チャットメタデータに保存されます。
- `/listinjects` — 現在のチャットのスクリプトによって追加されたすべてのプロンプトインジェクションのリストをシステムメッセージに表示します。
- `/flushinjects` — 現在のチャットのスクリプトによって追加されたすべてのプロンプトインジェクションを削除します。
- `/note (text)` — 現在のチャットのAuthor's Note値を設定します。チャットメタデータに保存されます。
- `/interval` — 現在のチャットのAuthor's Note挿入間隔を設定します。
- `/depth` — チャット内位置のAuthor's Note挿入深度を設定します。
- `/position`  — 現在のチャットのAuthor's Note位置を設定します。

### `/inject`の引数

```stscript
/inject id=IdGoesHere position=chat depth=4 My prompt injection
```

- `id` — 識別子文字列または変数への参照。同じIDを持つ`/inject`の連続した呼び出しは、以前のテキストインジェクションを上書きします。**必須引数。**
- `position` — インジェクションの位置を設定します。デフォルト:`after`。可能な値:
  - `after`:メインプロンプトの後。
  - `before`:メインプロンプトの前。
  - `chat`:チャット内。
- `depth` — チャット内位置のインジェクション深度を設定します。0は最後のメッセージの後に挿入、1は最後のメッセージの前、などを意味します。デフォルト:4。
- 名前なし引数は、注入されるテキストです。空の文字列は、提供された識別子の以前の値を解除します。

## チャットメッセージへのアクセス

### メッセージの読み取り

`/messages`コマンドを使用して、現在選択されているチャットのメッセージにアクセスできます。

```stscript
/messages names=on/off start-finish
```

- `names`引数は、キャラクター名を含めるかどうかを指定するために使用されます。デフォルト:`on`。
- 名前なし引数では、`start-finish`形式のメッセージインデックスまたは範囲を受け入れます。範囲は包括的です!
- 範囲が満たせない場合、つまり無効なインデックスまたは存在する以上のメッセージが要求された場合、空の文字列が返されます。
- プロンプトから非表示になっているメッセージ(ゴーストアイコンで示される)は、出力から除外されます。
- 最新のメッセージのインデックスを知りたい場合は、`{{lastMessageId}}`マクロを使用し、`{{lastMessage}}`はメッセージ自体を取得します。

範囲の開始インデックスを計算するには、例えば最後のNメッセージを取得する必要がある場合、変数減算を使用します。
この例は、チャットの最後の3つのメッセージを取得します:

```stscript
/setvar key=start {{lastMessageId}} |
/addvar key=start -2 |
/messages names=off {{getvar::start}}-{{lastMessageId}} |
/setinput
```

### メッセージの送信

スクリプトは、ユーザー、キャラクター、ペルソナ、中立的なナレーター、またはコメントとしてメッセージを送信できます。

1. `/send (text)` — 現在選択されているペルソナとしてメッセージを追加します。
2. `/sendas name=charname (text)` — 名前で一致する任意のキャラクターとしてメッセージを追加します。`name`引数は必須です。現在のキャラクターとして送信するには、`{{char}}`マクロを使用します。
3. `/sys (text)` — ユーザーまたはキャラクターに属さない中立的なナレーターからのメッセージを追加します。表示名は純粋に装飾的で、`/sysname`コマンドでカスタマイズできます。
4. `/comment (text)` — チャットに表示されるが、プロンプトには表示されない非表示のコメントを追加します。
5. `/addswipe (text)` — 最後のキャラクターメッセージにスワイプを追加します。ユーザーまたは非表示メッセージにスワイプを追加することはできません。
6. `/hide (message id or range)` — 提供されたメッセージインデックスまたは`start-finish`形式の包括的範囲に基づいて、1つまたは複数のメッセージをプロンプトから非表示にします。
7. `/unhide (message id or range)` — 提供されたメッセージインデックスまたは`start-finish`形式の包括的範囲に基づいて、1つまたは複数のメッセージをプロンプトに返します。

`/send`、`/sendas`、`/sys`、および`/comment`コマンドは、オプションで`at`名前付き引数を受け入れます。これは、メッセージ挿入の正確な位置を指定する0ベースの数値(またはそのような値を含む変数名)です。デフォルトでは、新しいメッセージはチャットログの最後に挿入されます。

これにより、会話履歴の先頭にユーザーメッセージが挿入されます:

```stscript
/send at=0 Hi, I use Linux.
```

### メッセージの削除

**これらのコマンドは潜在的に破壊的であり、「元に戻す」機能はありません。誤って重要なものを削除した場合は、/backups/フォルダーを確認してください。**

1. `/cut (message id or range)` — 提供されたメッセージインデックスまたは`start-finish`形式の包括的範囲に基づいて、チャットから1つまたは複数のメッセージをカットします。
2. `/del (number)` — チャットから最後のNメッセージを削除します。
3. `/delswipe (1-based swipe id)` — 提供された1ベースのスワイプIDに基づいて、最後のキャラクターメッセージからスワイプを削除します。
4. `/delname (character name)` — 指定された名前のキャラクターに属する現在のチャットのすべてのメッセージを削除します。
5. `/delchat` — 現在のチャットを削除します。

## World Infoコマンド

World Info(Lorebookとも呼ばれます)は、プロンプトに動的にデータを挿入するための非常に有用なツールです。詳細については、専用ページを参照してください:[World Info](/Usage/worldinfo.md)。

1. `/getchatbook` – チャットにバインドされたWorld Infoファイルの名前を取得するか、バインドされていない場合は新しいファイルを作成し、パイプに渡します。
2. `/findentry file=bookName field=fieldName [text]` – 指定されたファイル(またはファイル名を指すポインタ)から、提供されたテキストとのフィールド値のファジーマッチングを使用してレコードのUIDを検索し(デフォルトフィールド:`key`)、UIDをパイプに渡します。例:`/findentry file=chatLore field=key Shadowfang`。
3. `/getentryfield file=bookName field=field [UID]` – 指定されたWorld Infoファイル(またはファイル名を指すポインタ)からUIDを持つレコードのフィールド値(デフォルトフィールド:`content`)を取得し、値をパイプに渡します。例:`/getentryfield file=chatLore field=content 123`。
4. `/setentryfield file=bookName uid=UID field=field [text]` – 指定されたWorld Infoファイル(またはファイル名を指すポインタ)からUID(またはUIDを指す変数)を持つレコードのフィールド値(デフォルトフィールド:`content`)を設定します。キーフィールドに複数の値を設定するには、テキスト値としてカンマ区切りリストを使用します。例:`/setentryfield file=chatLore uid=123 field=key Shadowfang,sword,weapon`。
5. `/createentry file=bookName key=keyValue [content text]` – 指定されたファイル(またはファイル名を指すポインタ)にキーとコンテンツ(これらの引数は両方とも*オプション*)を持つ新しいレコードを作成し、UIDをパイプに渡します。例:`/createentry file=chatLore key=Shadowfang The sword of the king`。

### 有効なエントリフィールド

| フィールド              | UI要素        | 値のタイプ      |
|:-------------------|:------------------|:----------------|
| `content`          | Content           | String          |
| `comment`          | Title / Memo      | String          |
| `key`              | Primary Keywords  | List of strings |
| `keysecondary`     | Optional Filter   | List of strings |
| `constant`         | Constant Status   | Boolean (1/0)   |
| `disable`          | Disabled Status   | Boolean (1/0)   |
| `order`            | Order             | Number          |
| `selectiveLogic`   | Logic             | (以下を参照)     |
| `excludeRecursion` | Non-recursable    | Boolean (1/0)   |
| `probability`      | Trigger%          | Number (0-100)  |
| `depth`            | Depth             | Number (0-999)  |
| `position`         | Position          | (以下を参照)     |
| `role`             | Depth Role        | (以下を参照)     |
| `scanDepth`        | Scan Depth        | Number (0-100)  |
| `caseSensitive`    | Case-Sensitive    | Boolean (1/0)   |
| `matchWholeWords`  | Match Whole Words | Boolean (1/0)   |
| `vectorized`       | Vectorized Status | Boolean (1/0)   |
| `automationId`     | Automation ID     | String          |
| `group`            | Inclusion Group   | String          |
| `groupOverride`    | Inclusion Group Prioritize | Boolean (1/0) |
| `groupWeight`      | Inclusion Group Weight | Number (0-100) |
| `useGroupScoring`  | Group Scoring     | Boolean (1/0)   |
| `characterFilterExclude` | Character Filter Exclude Mode | List of strings |
| `characterFilterNames` | Character Filter Names | List of strings |
| `characterFilterTags` | Character Filter Tags | List of strings |
| `matchCharacterDepthPrompt` | Match Character Depth Prompt | Boolean (1/0) |
| `matchCharacterDescription` | Match Character Description | Boolean (1/0) |
| `matchCharacterPersonality` | Match Character Personality | Boolean (1/0) |
| `matchCreatorNotes` | Match Creator Notes | Boolean (1/0) |
| `matchPersonaDescription` | Match Persona Description | Boolean (1/0) |
| `matchScenario` | Match Scenario | Boolean (1/0) |

**Logic値**

- 0 = AND ANY
- 1 = NOT ALL
- 2 = NOT ANY
- 3 = AND ALL

**Position値**

- 0 = before main prompt
- 1 = after main prompt
- 2 = top of Author's Note
- 3 = bottom of Author's Note
- 4 = in-chat at depth
- 5 = top of example messages
- 6 = bottom of example messages

**Role値** (Position = 4のみ)
- 0 = System
- 1 = User
- 2 = Assistant

### 例1:キーによってチャットlorebookからコンテンツを読み取る

```stscript
/getchatbook | /setvar key=chatLore |
/findentry file={{getvar::chatLore}} field=key Shadowfang |
/getentryfield file={{getvar::chatLore}} field=key |
/echo
```

### 例2:キーとコンテンツを持つチャットlorebookエントリを作成する

```stscript
/getchatbook | /setvar key=chatLore |
/createentry file={{getvar::chatLore}} key="Milla" Milla Basset is a friend of Lilac and Carol. She is a hush basset puppy who possesses the power of alchemy. |
/echo
```

### 例3:チャットからの新しい情報で既存のlorebookエントリを拡張する

```stscript
/getchatbook | /setvar key=chatLore |
/findentry file={{getvar::chatLore}} field=key Milla |
/setvar key=millaUid |
/getentryfield file={{getvar::chatLore}} field=content |
/setvar key=millaContent |
/gen lock=on Tell me more about Milla Basset based on the provided conversation history. Incorporate existing information into your reply: {{getvar::millaContent}} |
/setvar key=millaContent |
/echo New content: {{pipe}} |
/setentryfield file={{getvar::chatLore}} uid=millaUid field=content {{getvar::millaContent}}
```

## テキスト操作

さまざまなスクリプトシナリオで使用するための便利なテキスト操作ユーティリティコマンドが多数あります。

1. `/trimtokens` — 入力を指定されたテキストトークン数に、最初からまたは最後からトリミングし、結果をパイプに出力します。
2. `/trimstart` — 入力を最初の完全な文の開始までトリミングし、結果をパイプに出力します。
3. `/trimend` — 入力を最後の完全な文の終わりまでトリミングし、結果をパイプに出力します。
4. `/fuzzy` — 入力テキストを文字列のリストに対してファジーマッチングを実行し、最も一致する文字列をパイプに出力します。
5. `/regex name=scriptName [text]` — 指定されたテキストに対してRegex extensionからregexスクリプトを実行します。スクリプトは有効である必要があります。

### `/trimtokens`の引数

```stscript
/trimtokens limit=number direction=start/end (input)
```

1. `direction`はトリミングの方向を設定します。`start`または`end`のいずれかです。デフォルト:`end`。
2. `limit`は出力に残すトークンの量を設定します。数値を含む変数名を指定することもできます。**必須引数。**
3. 名前なし引数は、トリミングされる入力テキストです。

### `/fuzzy`の引数

```stscript
/fuzzy list=["candidate1","candidate2"] (input)
```

1. `list`は候補を含むJSON形式の文字列配列です。リストを含む変数名を指定することもできます。**必須引数。**
2. 名前なし引数は、マッチングされる入力テキストです。出力は、入力に最も近く一致する候補の1つです。

## オートコンプリート

- オートコンプリートは、チャット入力と大きなQuick Replyエディターの両方で有効です。
- オートコンプリートは、入力のどこでも機能します。複数のパイプされたコマンドやネストされたclosuresでも。
- オートコンプリートは、一致するコマンドを検索する3つの方法をサポートしています(*User Settings* -> *STscript Matching*)。

1. **Starts with** "古い"方法。入力された値で正確に始まるコマンドのみが表示されます。
2. **Includes**  入力された値を*含む*すべてのコマンドが表示されます。例:`/delete`と入力すると、`/qr-delete`および`/qr-set-delete`コマンドがオートコンプリートリストに表示されます(/qr-**delete**、/qr-set-**delete**)。
3. **Fuzzy**  入力された値に対してファジーマッチングできるすべてのコマンドが表示されます。例:`/seas`と入力すると、`/sendas`コマンドがオートコンプリートリストに表示されます(/**se**nd**as**)。

- コマンド引数もオートコンプリートでサポートされています。必須引数の場合、リストは自動的に表示されます。オプション引数の場合、*Ctrl*+*Space*を押して利用可能なオプションのリストを開きます。
- `/:`を入力してclosureまたはQRを実行すると、オートコンプリートはスコープ変数とQRのリストを表示します。
- オートコンプリートは、(slash commandsの)マクロに対する限定的なサポートがあります。`{{`と入力すると、利用可能なマクロのリストが表示されます。
- *上*と*下*の*矢印キー*を使用して、オートコンプリートオプションのリストからオプションを選択します。
- *Enter*または*Tab*を押すか、オプションを*クリック*して、カーソル位置にオプションを配置します。
- *Escape*を押してオートコンプリートリストを閉じます。
- *Ctrl*+*Space*を押してオートコンプリートリストを開くか、選択したオプションの詳細を切り替えます。

## パーサーフラグ

```stscript
/parser-flag
```

パーサーは、その動作を変更するためのフラグを受け入れます。これらのフラグは、スクリプトの任意の時点でオン/オフを切り替えることができ、以降のすべての入力はそれに従って評価されます。  
デフォルトのフラグはユーザー設定で設定できます。

### Strict Escaping

```stscript
/parser-flag STRICT_ESCAPING on |
```

`STRICT_ESCAPING`を有効にした場合の変更点は次のとおりです。

#### パイプ

引用符で囲まれた値でパイプをエスケープする必要はありません。

```stscript
/echo title="a|b" c\|d
```

#### バックスラッシュ

シンボルの前のバックスラッシュをエスケープして、リテラルバックスラッシュの後に機能的なシンボルを提供できます。

```stscript
// this will echo "foo \", then echo "bar" |
/echo foo \\|
/echo bar
```

```stscript
/echo \\|
/echo \\\|
```

### Replace Variable Macros

```stscript
/parser-flag REPLACE_GETVAR on |
```

このフラグは、変数値がマクロとして解釈される可能性のあるテキストを含む場合、二重置換を回避するのに役立ちます。`{{var::}}`マクロは最後に置換され、結果のテキスト/変数値に対してそれ以上の置換は行われません。

すべての`{{getvar::}}`および`{{getglobalvar::}}`マクロを`{{var::}}`に置き換えます。
舞台裏では、パーサーは、置き換えられたマクロを持つコマンドの前に一連のコマンドエグゼキューターを挿入します:

- `/let`を呼び出して現在の`{{pipe}}`をスコープ変数に保存
- `/getvar`または`/getglobalvar`を呼び出してマクロで使用される変数を取得
- `/let`を呼び出して取得した変数をスコープ変数に保存
- 保存された`{{pipe}}`値で`/return`を呼び出して、次のコマンドに正しいパイプ値を復元

```stscript
// the following will echo the last message's id / number |
/setvar key=x \{\{lastMessageId}} |
/echo {{getvar::x}}
```

```stscript
// this will echo the literal text {{lastMessageId}} |
/parser-flag REPLACE_GETVAR |
/setvar key=x \{\{lastMessageId}} |
/echo {{getvar::x}}
```

## Quick Replies:スクリプトライブラリと自動実行

Quick Repliesは、スクリプトを保存および実行する簡単な方法を提供するSillyTavernの組み込みextensionです。

### Quick Repliesの設定

開始するには、extensionsパネル(積み重ねられたブロックアイコン)を開き、Quick Repliesメニューを展開します。

<div style="display:flex;justify-content:center">

![Quick Reply](/static/scripts/quick-reply.png)

</div>

**Quick Repliesはデフォルトで無効になっているため、最初に有効にする必要があります。** その後、チャット入力バーの上にバーが表示されます。

表示されるボタンテキストラベル(簡潔にするために絵文字を使用することをお勧めします)と、ボタンをクリックしたときに実行されるスクリプトを設定できます。

ボタンの数は、**Number of slots**設定(最大=100)で制御されます。必要に応じて調整し、完了したら"Apply"をクリックします。

**Inject user input automatically**は、STscriptを使用する場合は無効にすることをお勧めします。そうしないと、入力と干渉する可能性があります。代わりに、`{{input}}`マクロを使用してスクリプト内の入力バーの現在の値を取得します。

**Quick Reply presets**は、事前定義されたQuick Repliesの複数のセットを持ち、手動で、または`/qrset (name of set)`コマンドを使用して切り替えることができます。
別のセットに切り替える前に、現在使用しているプリセットに変更を書き込むために"Update"をクリックすることを忘れないでください!

### 手動実行

では、最初のスクリプトをライブラリに追加できます。空きスロットを選択(または作成)し、左側のボックスに"Click me"と入力してラベルを設定し、次を右側のボックスに貼り付けます:

```stscript
/addvar key=clicks 1 |
/if left=clicks right=5 rule=eq else="/echo Keep going..." "/echo You did it!  \| /flushvar clicks"
```

次に、チャットバーの上に表示されたボタンを5回クリックします。
クリックごとに変数`clicks`が1ずつ増加し、値が5に等しいときに異なるメッセージが表示され、変数がリセットされます。

### 自動実行

作成したコマンドの`⋮`ボタンをクリックしてモーダルメニューを開きます。

| ![Automatic execution](/static/scripts/autoexecute.png) |
|---------------------------------------------------------|

このメニューでは、次のことができます:

- 便利なフルスクリーンエディターでスクリプトを編集
- チャットバーからボタンを非表示にして、自動実行のためだけにアクセス可能にする。
- 次の1つ以上の条件で自動実行を有効にする:
  * アプリの起動
  * ユーザーメッセージをチャットに送信
  * チャットでAIメッセージを受信
  * キャラクターまたはグループチャットを開く
  * グループメンバーからの返信をトリガー
  * 同じAutomation IDを使用してWorld Infoエントリをアクティブ化
- quick replyのカスタムツールチップを提供(UI内のquick replyにカーソルを合わせたときに表示されるテキスト)
- テスト目的でスクリプトを実行

コマンドは、Quick Replies extensionが有効になっている場合にのみ自動的に実行されます。

例えば、5つのユーザーメッセージを送信した後にメッセージを表示するには、次のスクリプトを追加し、ユーザーメッセージで自動実行するように設定します。

```stscript
/addvar key=usercounter 1 |
/echo You've sent {{pipe}} messages. |
/if left=usercounter right=5 rule=gte "/echo Game over! \| /flushvar usercounter"
```

### デバッガー

拡張Quick Replyエディター内に基本的なデバッガーが存在します。スクリプトのどこにでも`/breakpoint |`でブレークポイントを設定します。QRエディターからスクリプトを実行すると、その時点で実行が中断され、現在利用可能な変数、パイプ、コマンド引数などを調べ、コードの残りを1つずつステップスルーできます。

```stscript
/let x {: n=1
	/echo n is {{var::n}} |
	/mul n n |
:} |
/breakpoint |
/:x n=3 |
/echo result is {{pipe}} |
```

| ![QR Editor Debugger](/static/scripts/st-debugger.png) |
|--------------------------------------------------------|

### プロシージャの呼び出し

`/run`コマンドは、Quick Repliesでラベルで定義されたスクリプトを呼び出すことができ、基本的にプロシージャを定義し、それらから結果を返す機能を提供します。これにより、他のスクリプトが参照できる再利用可能なスクリプトブロックを持つことができます。プロシージャのパイプからの最後の結果は、その後の次のコマンドに渡されます。

```stscript
/run ScriptLabel
```

2つのQuick Repliesを作成しましょう:

***
**ラベル:**

`GetRandom`

**コマンド:**

```stscript
/pass {{roll:d100}}
```
***
**ラベル:**

`GetMessage`

**コマンド:**
```stscript
/run GetRandom | /echo Your lucky number is: {{pipe}}
```
***

`GetMessage`ボタンをクリックすると、`GetRandom`プロシージャが呼び出され、`{{roll}}`マクロが解決され、数値が呼び出し元に渡されてユーザーに表示されます。

- プロシージャは名前付きまたは名前なし引数を受け入れませんが、呼び出し元と同じ変数を参照できます。
- 不注意に処理された場合、「コールスタック超過」エラーが発生する可能性があるため、プロシージャを呼び出す際は再帰を避けてください。

#### 別のQuick Replyプリセットからプロシージャを呼び出す

`a.b`構文を使用して、別のquick replyプリセットからプロシージャを呼び出すことができます。a = QRプリセット名、b = QRラベル名です

```stscript
/run QRpreset1.QRlabel1
```

デフォルトでは、システムはまずquick replyラベル`a.b`を探すため、ラベルの1つが文字通り"QRpreset1.QRlabel1"である場合、それを実行しようとします。そのようなラベルが見つからない場合、"QRpreset1"という名前のQRプリセットに"QRlabel1"というラベルのQRを検索します。

### Quick Replies管理コマンド

#### Quick Replyの作成

* `/qr-create (arguments, [message])` – 新しいQuick Replyを作成します。例:`/qr-create set=MyPreset label=MyButton /echo 123`

引数:
- `label`    - string - ボタンのテキスト。例:`label=MyButton`
- `set`      - string - QRセットの名前。例:`set=PresetName1`
- `hidden`   - bool   - ボタンを非表示にするかどうか。例:`hidden=true`
- `startup`  - bool   - アプリ起動時に自動実行。例:`startup=true`
- `user`     - bool   - ユーザーメッセージで自動実行。例:`user=true`
- `bot`      - bool   - AIメッセージで自動実行。例:`bot=true`
- `load`     - bool   - チャット読み込み時に自動実行。例:`load=true`
- `title`    - bool   - ボタンに表示されるタイトル/ツールチップ。例:`title="My Fancy Button"`

#### Quick Replyの削除

* `/qr-delete (set=string [label])` – Quick Replyを削除します

#### Quick Replyの更新

* `/qr-update (arguments, [message])` – Quick Replyを更新します。例:`/qr-update set=MyPreset label=MyButton newlabel=MyRenamedButton /echo 123`

引数:
- `newlabel` - string - ボタンの新しいテキスト。例:`newlabel=MyRenamedButton`
- `label`    - string - ボタンのテキスト。例:`label=MyButton`
- `set`      - string - QRセットの名前。例:`set=PresetName1`
- `hidden`   - bool   - ボタンを非表示にするかどうか。例:`hidden=true`
- `startup`  - bool   - アプリ起動時に自動実行。例:`startup=true`
- `user`     - bool   - ユーザーメッセージで自動実行。例:`user=true`
- `bot`      - bool   - AIメッセージで自動実行。例:`bot=true`
- `load`     - bool   - チャット読み込み時に自動実行。例:`load=true`
- `title`    - bool   - ボタンに表示されるタイトル/ツールチップ。例:`title="My Fancy Button"`

####

* `qr-get` - Quick Replyのすべてのプロパティを取得します。例:`/qr-get set=myQrSet id=42`

#### QRプリセットの作成または更新

* `/qr-presetupdate (arguments [label])`または`/qr-presetadd (arguments [label])`

引数:
- `enabled` - bool - プリセットを有効または無効にする
- `nosend`  - bool - 送信を無効にする/ユーザー入力に挿入(slash commandsでは無効)
- `before`  - bool - ユーザー入力の前にQRを配置
- `slots`   - int  - スロット数
- `inject`  - bool - ユーザー入力を自動的に注入(無効の場合は`{{input}}`を使用)

新しいプリセットを作成(既存のものを上書き)します。例:`/qr-presetadd slots=3 MyNewPreset`

#### QRコンテキストメニューの追加

* `/qr-contextadd (set=string label=string chain=bool [preset name])` – QRにコンテキストメニュープリセットを追加します。例:`/qr-contextadd set=MyPreset label=MyButton chain=true MyOtherPreset`

#### すべてのコンテキストメニューの削除

* `/qr-contextclear (set=string [label])` – QRからすべてのコンテキストメニュープリセットを削除します。例:`/qr-contextclear set=MyPreset MyButton`

#### 1つのコンテキストメニューの削除

* `/qr-contextdel (set=string label=string [preset name])` – QRからコンテキストメニュープリセットを削除します。例:`/qr-contextdel set=MyPreset label=MyButton MyOtherPreset`

### Quick Reply値のエスケープ

`|{}`は、QRメッセージ/コマンドでバックスラッシュでエスケープできます。

例えば、`/qr-create label=MyButton /getvar myvar \| /echo \{\{pipe\}\}`を使用して、`/getvar myvar | /echo {{pipe}}`を呼び出すQRを作成します。

## Extensionコマンド

SillyTavern extensions(組み込み、ダウンロード可能、サードパーティ)は、独自のslash commandsを追加できます。以下は、公式extensionの機能の一例です。リストは不完全な場合があるため、利用可能なコマンドの最も完全なリストについては、`/help slash`を確認してください。

1. `/websearch (query)` — 指定されたクエリのWebページのスニペットをオンラインで検索し、結果をパイプに返します。Web Search extensionによって提供されます。
2. `/imagine (prompt)` — 提供されたプロンプトを使用して画像を生成します。Image Generation extensionによって提供されます。
3. `/emote (sprite)` — その名前をファジーマッチングして、アクティブなキャラクターのスプライトを設定します。Character Expressions extensionによって提供されます。
4. `/costume (subfolder)` — アクティブなキャラクターのスプライトセットオーバーライドを設定します。Character Expressions extensionによって提供されます。
5. `/music (name)` — 再生されるバックグラウンドミュージックファイルを名前で強制的に変更します。Dynamic Audio extensionによって提供されます。
6. `/ambient (name)` — 再生されるアンビエントサウンドファイルを名前で強制的に変更します。Dynamic Audio extensionによって提供されます。
7. `/roll (dice formula)` — サイコロを振った結果を含む非表示メッセージをチャットに追加します。D&D Dice extensionによって提供されます。

## UIインタラクション

スクリプトは、SillyTavernのUIとも対話できます:チャットをナビゲートしたり、スタイリングパラメータを変更したりします。

### キャラクターナビゲーション

1. `/random` — ランダムなキャラクターとのチャットを開きます。
2. `/go (name)` — 指定された名前のキャラクターとのチャットを開きます。まず正確な名前の一致を検索し、次にプレフィックス、次に部分文字列で検索します。

### UIスタイリング

1. `/bubble` — メッセージスタイルを"bubble chat"スタイルに設定します。
2. `/flat` — メッセージスタイルを"flat chat"スタイルに設定します。
3. `/single` — メッセージスタイルを"single document"スタイルに設定します。
4. `/movingui (name)` — 名前でMovingUIプリセットをアクティブ化します。
5. `/resetui` — MovingUIパネルの状態を元の位置にリセットします。
6. `/panels` — UIパネルの表示を切り替えます:上部バー、左と右のドロワー。
7. `/bg (name)` — ファジー名マッチングを使用して背景を検索および設定します。チャット背景のロック状態を尊重します。
8. `/lockbg` — 現在のチャットの背景画像をロックします。
9. `/unlockbg` — 現在のチャットの背景画像のロックを解除します。

## その他の例

### チャット概要の生成(by @IkariDevGIT)

```stscript
/setglobalvar key=summaryPrompt Summarize the most important facts and events that have happened in the chat given to you in the Input header. Limit the summary to 100 words or less. Your response should include nothing but the summary. |
/setvar key=tmp |
/messages 0-{{lastMessageId}} |
/trimtokens limit=3000 direction=end |
/setvar key=s1 |
/echo Generating, please wait... |
/genraw lock=on instruct=off {{instructInput}}{{newline}}{{getglobalvar::summaryPrompt}}{{newline}}{{newline}}{{instructInput}}{{newline}}{{getvar::s1}}{{newline}}{{newline}}{{instructOutput}}{{newline}}The chat summary:{{newline}} |
/setvar key=tmp |
/echo Done! |
/setinput {{getvar::tmp}} |
/flushvar tmp |
/flushvar s1
```

### ボタンポップアップの使用

```stscript
/setglobalvar key=genders ["boy", "girl", "other"] |
/buttons labels=genders Who are you? |
/echo You picked: {{pipe}}
```

### N番目のフィボナッチ数を取得(Binetの公式を使用)

!!!tip
**ヒント**: `fib_no`の値を希望の数に設定します
!!!

```stscript
/setvar key=fib_no 5 |
/pow 5 0.5 | /setglobalvar key=SQRT5 |
/setglobalvar key=PHI 1.618033 |
/pow PHI fib_no | /div {{pipe}} SQRT5 |
/round |
/echo {{getvar::fib_no}}th Fibonacci's number is: {{pipe}}
```

### 再帰的階乗(closuresを使用)

```stscript
/let fact {: n=
    /if left={{var::n}} rule=gt right=1
        else={:
            /return 1
        :}
        {:
            /sub {{var::n}} 1 |
            /:fact n={{pipe}} |
            /mul {{var::n}} {{pipe}}
        :}
:} |

/input Calculate factorial of: |
/let n {{pipe}} |
/:fact n={{var::n}} |
/echo factorial of {{var::n}} is {{pipe}}
```
