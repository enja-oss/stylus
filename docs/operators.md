 +  元文書: [stylus/docs/operators.html at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub]
(https://github.com/LearnBoost/stylus/blob/f9ed220d7e5f0b44aeaca58ffd6490566b5f0757/docs/operators.html 
"stylus/docs/operators.html at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub")

## Operator Precedence

## ## 演算子の優先順位 [原文](http://learnboost.github.com/stylus/docs/operators.html)

Below is the operator precedence table, highest to lowest:

下は演算子を優先順位の高いものから並べた表です。

     []
     ! ~ + -
     is defined
     ** * / %
     + -
     ... ..
     <= >= < >
     in
     == is != is not isnt
     is a
     && and || or
     ?:
     = := ?= += -= *= /= %=
     not
     if unless

## Unary Operators

## 単項演算子

The following unary operators are available, `!`, `not`, `-`, `+`, and `~`.

次の単項演算子が利用できます。 `!`, `not`, `-`, `+`, `~`

    !0
    // => true
    
    !!0
    // => false

    !1
    // => false
    
    !!5px
    // => true

    -5px
    // => -5px
    
    --5px
    // => 5px

    not true
    // => false
    
    not not true
    // => true
    
The logical `not` operator has low precedence, therefore the following example could be replaced with

`not` 論理演算子は優先度が低いため、次の例は置き換えることができます。

    a = 0
    b = 1
    
    !a and !b
    // => false
    // pased as: (!a) and (!b)
    // このように評価される: (!a) and (!b)

With:

置き換え後:

    not a or b
    // => false
    // parsed as: not (a or b)
    // このように評価される: not (a or b)

## Binary Operators

## バイナリ演算子

### Subscript []

### 記号 []\(\)

 The subscript operator allows us to grab a value inside an expression via index. Parenthesized expressions may act as tuples (e.g. `(15px 5px)`, `(1 2 3)`).

 記号（`[]`）は、インデックス経由で式の中の値を取得することが出来ます。
 括弧（`()`）で囲まれた表現はタプルとして振舞います（例えば、 `(15px 5px)`, `(1 2 3)`）。 
 
 Below is an example that uses tuples for error handling (and showcasing the versatility of this construct):
 
 次は、エラーハンドリングのためにタプルを使う（そして、この構造の汎用性を紹介する）例です。
 
     add(a, b)
       if a is a 'unit' and b is a 'unit'
         a + b
       else
         (error 'a and b must be units!')

     body
       padding add(1,'5')
       // => padding: error "a and b must be units";
       
       padding add(1,'5')[0]
       // => padding: error;
       
       padding add(1,'5')[0] == error
       // => padding: true;

       padding add(1,'5')[1]
       // => padding: "a and b must be units";

 Here's a more complex example. Now we're invoking the built-in `error()` function with the return error message, 
 whenever the ident (the first value) equals `error`.
 
 これはより複雑な例です。今、ビルトイン関数の `error()` を戻ってきたエラーメッセージを引数に呼び出しています。
 呼び出されるたびに、 `error` と同じ文字（エラーメッセージの最初の値）をインデントしています。
 
     if (val = add(1,'5'))[0] == error
       error(val[1])

## Range .. ...

## 範囲 .. ...

 Both the inclusive (`..`) and exclusive (`...`) range operators are provided, expanding to expressions:
 
 包含的（`..`）と排他的（`...`）両方の範囲記号が提供されており、次のような式に展開されます。

     1..5
     // => 1 2 3 4 5

     1...5
     // => 1 2 3 4

### Additive: + -

### 加算減算: + -

Multiplicative and additive binary operators work as expected. Type conversion is applied within unit type classes, or default to the literal value. For example `5s - 2px` results in `3s`.

加算と減算のバイナリ演算子は期待通りに動作します。
型変換は、単位が属する分類の範囲内か初期のリテラル値が適用されます。例えば、 `5s - 2px` の結果は `3s` です。

    15px - 5px
    // => 10px
    
    5 - 2
    // => 3
    
    5in - 50mm
    // => 3.031in
    
    5s - 1000ms
    // => 4s
    
    20mm + 4in
    // => 121.6mm

    "foo " + "bar"
    // => "foo bar"

    "num " + 15
    // => "num 15"

### Multiplicative: / * %

### 乗算除算: / * %

    2000ms + (1s * 2)
    // => 4ms

    5s / 2
    // => 2.5s

    4 % 2
    // => 0

When using `/` within a property value, you **must** wrap with parens. 
Otherwise the `/` is taken literally (to support CSS `line-height`):

もし、`/` をプロパティ値の中で使う場合、 **必ず** 括弧で囲まなければなりません。
そうしなければ、 `/` は文字通りに解釈されます（これはCSSが`line-height` をサポートするためです）。

    font: 14px/1.5;

But the following is evaluated as `14px` ÷ `1.5`:

しかし、次のよう評価されます。`14px` ÷ `1.5`:

    font: (14px/1.5);

This is _only_ required for the `/` operator.

これは、 `/` 演算子 _のみ_ に必要なことです。

### Exponent: **

### 指数: **

The Exponent operator:

指数演算子:

    2 ** 8
    // => 256

### Equality & Relational: == != >= <= > <

### 等価演算子と関係（比較）演算子: == != >= <= > <

Equality operators can be used to equate units, colors, strings, and even identifiers. 
This is a powerful concept, as even arbitrary identifiers (such as as `wahoo`) can be utilized as atoms. 
A function could return `yes` or `no` instead of `true` or `false` (although not advised). 

等価演算子は色や文字列や任意の識別子でさえ、同一視するものに使用できます。
これは任意の識別子（ `wahoo` などなど）をアトムとして利用することができる、強力な概念です。
関数も `true` か `false` の代わりに `yes` か `no` を返すことができました（これは、あまりお勧めしませんが）。

    5 == 5
    // => true
    
    10 > 5
    // => true
    
    #fff == #fff
    // => true
    
    true == false
    // => false
    
    wahoo == yay
    // => false
    
    wahoo == wahoo
    // => true
    
    "test" == "test"
    // => true

    true is true
    // => true

    'hey' is not 'bye'
    // => true

    'hey' isnt 'bye'
    // => true

    (foo bar) == (foo bar)
    // => true

    (1 2 3) == (1 2 3)
    // => true

    (1 2 3) == (1 1 3)
    // => false

Only exact values match. For example, `0 == false` and `null == false` are both `false`.

正確な値のみがマッチします。例えば `0 == false` と`null == false` は両方 `false` です。

Aliases:

エイリアス:

    ==    is
    !=    is not
    !=    isnt

## Truthfulness

## 真偽

 Nearly everything within Stylus resolves to `true`, including units with a suffix. 
 Even `0%`, `0px`, etc. will resolve to `true` 
 (because it's common in Stylus for mixins or functions to accept units as valid). 
 
 Stylus内のほぼすべての接尾語を持つ単位は `true` として解釈されます。
 `0%`, `0px`, などでさえも `true` として解釈されます。
 （なぜなら、Stylusではmixinsやfunctionsのため、これらを有効な単位として受け入れることが一般的であるためです。）

 However, `0` itself is `false` in terms of arithmetic. 
 
 しかし、`0` 自体は算術の観点から `false` です。

 Expressions (or "lists") with a length greater than 1 are considered truthy.

 lengthが1より大きい値を持つ式（またはリスト）はtrueと見なされます。

`true` examples:

`true` の例:

      0% 
      0px
      1px 
      -1
      -1px
      hey
      'hey'
      (0 0 0)
      ('' '')

`false` examples:

`false` の例:

     0 
     null
     false
     ''

### Logical Operators: && || and or

### 論理演算子: && || and or

Logical operators `&&` and `||` are aliased `and` / `or` which apply the same precedence.

論理演算子の `&&` と `||` は`and` / `or` と同じ優先順位が適用されるエイリアスです。

    5 && 3
    // => 3
    
    0 || 5
    // => 5
    
    0 && 5
    // => 0
    
    #fff is a 'rgba' and 15 is a 'unit'
    // => true

### Existence Operator: in

### 存在演算子: in

 Checks for the existence of the _left-hand_ operand within the _right-hand_ expression.

  _右辺_ の式の中に、 _左辺_ のオペランドが存在することをチェックします。

Simple examples:

単純な例:

      nums = 1 2 3
      1 in nums
      // => true

      5 in nums
      // => false

Some undefined identifiers:

いくつかの未定義な識別子の例:

      words = foo bar baz
      bar in words
      // => true

      HEY in words
      // => false

Works with tuples too:

タプルでの動作の例:

      vals = (error 'one') (error 'two')
      error in vals
      // => false
      
      (error 'one') in vals
      // => true

      (error 'two') in vals
      // => true

      (error 'something') in vals
      // => false

Example usage in mixin:

mixinでの使用例:

      pad(types = padding, n = 5px)
        if padding in types
          padding n
        if margin in types
          margin n

      body
        pad()

      body
        pad(margin)

      body
        pad(padding margin, 10px)

Yielding:

結果:

      body {
        padding: 5px;
      }
      body {
        margin: 5px;
      }
      body {
        padding: 10px;
        margin: 10px;
      }

### Conditional Assignment: ?= :=

### 条件付き代入: ?= :=

The conditional assignment operator `?=` (aliased as `:=`) lets us define variables without clobbering old values (if present). 
This operator expands to an `is defined` binary operation within a ternary. 

条件付き代入演算子 `?=` （`:=`）は、（もし存在する場合）古い値を上書きして破壊することなしに、変数を定義することができます。
この演算子は三項演算子の中の `is defined` バイナリ演算子として展開されます。

For example, the following are equivalent:

以下の例は等価です。:

    color := white
    color ?= white
    color = color is defined ? color : white

When using plain `=`, we simply reassign:

`=` を使った場合、簡単に再割り当てできます。

    color = white
    color = black
    
    color
    // => black

But when using `?=`, our second attempt fails (since the variable is already defined):

しかし、`?=`を使った場合、（下の例の）2行目の試みは（既に変数が定義されているため）失敗します。

    color = white
    color ?= black
    
    color
    // => white

### Instance Check: is a

### インスタンスチェック: is a

Stylus provides a binary operator named `is a` used to type check.

Stylusはタイプチェックのため、 `is a` という名前のバイナリ演算子を提供します。

    15 is a 'unit'
    // => true
    
    #fff is a 'rgba'
    // => true
    
    15 is a 'rgba'
    // => false

Alternatively, we could use the `type()` BIF:

代わりに、`type()` BIF（Built in Function　組み込み関数）を利用することもできます。

    type(#fff) == 'rgba'
    // => true                                                                            

**Note:** `color` is the only special-case, evaluating to `true` when the
left-hand operand is an `RGBA` or `HSLA` node.

**注意:** `color` は左辺のオペランドが `RGBA` や `HSLA` ノードである場合に `true` と評価される、唯一の特殊なケースです。

### Variable Definition: is defined

### 変数定義: is defined

This pseudo binary operator does not accept a right-hand operator, and does _not_ evaluate the left. This allows us to check if a variable has a value assigned to it.

この擬似バイナリ演算子は右辺の演算子を必要としません。
また、左辺も評価 _しません。_　
これは、変数が割り当てられた値を持っている場合のみ、チェックすることが出来ます。

    foo is defined
    // => false
    
    foo = 15px
    foo is defined
    // => true
    
    #fff is defined
    // => 'invalid "is defined" check on non-variable #fff'

Alternatively, one can use the `lookup(name)` built-in function to do this—or to perform dynamic lookups:

代わりに、組み込み関数 `lookup(name)` を使ってこれを行うことが出来ます。—または動的ルックアップを実行する。

    name = 'blue'
    lookup('light-' + name)
    // => null
    
    light-blue = #80e2e9
    lookup('light-' + name)
    // => #80e2e9

This operator is essential, as an undefined identifier is still a truthy value. For example:

例えば次のように、未定義の識別子がtrueの値を持っているため、この演算子は必要不可欠です。

    body
      if ohnoes
        padding 5px

_Will_ yield the following CSS when undefined:

未定義の場合、次のCSSが得られる _でしょう_ 。

    body {
      padding: 5px;
    }

However this will be safe:

しかし、これは正しくなります。

    body
      if ohnoes is defined
        padding 5px

## Ternary

## 三項演算子

The ternary operator works as we would expect in most languages. It's the only operator with three operands (the _condition_ expression, the _truth_ expression, and the _false_ expression).

他のほとんどの言語にて期待するように三項演算子も動作します。
これは3つのオペランド（ _条件_ 式、 _真_ 式、 _偽_ 式）を持つ唯一の演算子です。

    num = 15
    num ? unit(num, 'px') : 20px
    // => 15px

## Casting

## キャスト

 As an terse alternative to the `unit()` built-in function, the syntax `(expr) unit` may be used to force the suffix. 

 `unit()`組み込み関数の簡素な代替として、接尾語を強要するために`(式) 単位`構文が使用されるかもしれません。

    body
      n = 5
      foo: (n)em
      foo: (n)%
      foo: (n + 5)%
      foo: (n * 5)px
      foo: unit(n + 5, '%')
      foo: unit(5 + 180 / 2, deg)

## Color Operations

## 色操作

 Operations on colors provide a terse, expressive way to alter components. For example, we can operate on each RGB:
 
 色に対する操作は、コンポーネントを変更する簡素な表現方法として提供します。例えば、各RGBごとに操作することができます。

    #0e0 + #0e0
    // => #0f0

 Another example is adjust the lightness value by adding or subtracting a percentage. To lighten a color, add; to darken, subtract.

 もうひとつの例は、パーセンテージで加算・減算することによって明るさの値を調整することです。色を明るくするには加算します。また、暗くするには減算します。

    #888 + 50%
    // => #c3c3c3

    #888 - 50%
    // => #444

  Adjust the hue is also possible by adding or subtracting with degrees. For example, adding `50deg` to this red value results in a yellow:
  
  度数を加算・減算することによって色相を調整することが可能です。例えば、 赤色に `50deg` を加算した結果は黄色になります。

     #f00 + 50deg
     // => #ffd500

  Values clamp appropriately. For example, we can "spin" the hue 180 degrees, and if the current value is `320deg`, it will resolve to `140deg`.

  値は適切な値に固定されます。例えば、現在の値が`320deg` の場合であっても、色相を180度 "回転" させることができます。それは`140deg` として解釈されるでしょう。

  We may also tweak several values at once (including the alpha) by using `rgb()`, `rgba()`, `hsl()`, or `hsla()`:

  また、`rgb()`, `rgba()`, `hsl()`, `hsla()` を使用した場合、一度に複数の値を微調整することができます。

      #f00 - rgba(100,0,0,0.5)
      // => rgba(155,0,0,0.5)

## Sprintf

## Sprintf関数

 The string sprintf-like operator `%` can be used to generate a literal value, internally passing arguments through the `s()` built-in:

 リテラル値を生成するために、内部の`s()` ビルトイン関数を通して引数を渡す、文字列のsprintf関数のような `%` 演算子を使用することが出来ます。 

       'X::Microsoft::Crap(%s)' % #fc0
       // => X::Microsoft::Crap(#fc0)

  Multiple values should be parenthesized:
  
  複数の値は括弧で囲む必要があります。
  
      '-webkit-gradient(%s, %s, %s)' % (linear (0 0) (0 100%))
      // => -webkit-gradient(linear, 0 0, 0 100%)
   
