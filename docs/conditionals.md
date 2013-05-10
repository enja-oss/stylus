 +  元文書: [stylus/docs/conditionals.md at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub]
(https://github.com/LearnBoost/stylus/blob/f9ed220d7e5f0b44aeaca58ffd6490566b5f0757/docs/conditionals.md 
"stylus/docs/conditionals.md at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub")

## Conditionals

## 条件文[原文](http://learnboost.github.io/stylus/docs/conditionals.html)

Conditionals provide control flow to a language which is otherwise static, 
providing conditional imports, mixins, functions, and more. 
The examples below are simply examples, and not recommended :)

条件文は、言語に条件付きimport、mixins、functionsなどの制御フローを提供します。
そうでなければ静的です。
以下の例はシンプルな例であり、あまり推奨しません。

### if / else if / else

 The `if` conditional works as you would expect, simply accepting an expression, 
 evaluating the following block when `true`. 
 Along with `if` are the typical `else if` and `else` tokens, acting as fallbacks.
 
 `if` 条件文はあなたが予想した通りに働きます。単純に評価式を受け入れ、`true` の場合、次のブロックを評価します。
 `if` はフォールバックとして機能する `else if` や `else` トークンを伴うことが一般的です。
 
 The example below would conditionally overload the `padding` property, swapping it for `margin`.
 
 次の例では、条件によって `padding` を `margin` で交換して上書きします。

    overload-padding = true

    if overload-padding
      padding(y, x)
        margin y x

    body
      padding 5px 10px

Another example:

もうひとつの例

    box(x, y, margin = false)
      padding y x
      if margin
        margin y x

    body
      box(5px, 10px, true)

Another `box()` helper:

他の `box()` ヘルパー

    box(x, y, margin-only = false)
      if margin-only
        margin y x
      else
        padding y x

### unless

 For users familiar with the Ruby programming language, we have the `unless` conditional. 
 It’s basically the opposite of `if`—essentially `if (!(expr))`.

 Rubyプログラミングに親しい人向けに `unless` 条件文を持っています。
 これは基本的に `if` の反対（本質的には `if (!(expr))` ）の作用をします。

In the example below, if `disable-padding-override` is `undefined` or `false`, `padding` will be overridden, 
displaying `margin` instead. 
But if it’s `true`, `padding` will continue outputting `padding 5px 10px` as expected.

以下の例では、`disable-padding-override` が `undefined` または `false` の場合、
`padding` が上書きされ、代わりに `margin` で表示します。
しかし、`true` の場合、`padding` は予定通り `padding 5px 10px` のまま出力されます。

     disable-padding-override = true

     unless disable-padding-override is defined and disable-padding-override
       padding(x, y)
         margin y x

     body
       padding 5px 10px

### Postfix Conditionals

### 後置条件文

  Stylus supports postfix conditionals. 
  This means that `if` and `unless` act as operators; 
  they evaluate the left-hand operand when the right-hand expression is truthy.
  
  Stylusは後置条件文をサポートしています。
  これは、`if` や `unless` がオペレータのように振舞うことを意味しています。
  右辺の式がtrueと評価される場合、左辺のオペレータが評価されます。  
  
  For example let's define `negative()` to perform some basic type checking. Below we use block-style conditionals:
  
  例えば、幾つかの基本型のチェックをするために、`negative()` を定義してみましょう。
  以下では、ブロックスタイルの条件文を使用しています。
  
      negative(n)
        unless n is a 'unit'
          error('invalid number')
        if n < 0
          yes
        else
          no

  Next, we utilize postfix conditionals to keep our function terse.
  
  続いて、関数を簡潔に保つために後置条件文を使用します。

      negative(n)
        error('invalid number') unless n is a 'unit'
        return yes if n < 0
        no

  Of course, we could take this further.  
  For example, we could write `n < 0 ? yes : no`, or simply stick with booleans: `n < 0`.
  
  もちろんこれをさらに改良することもできます。
  例えば、`n < 0 ? yes : no` と書くことも出来ますし、次のように単純にブール値とすることもできます。`n < 0`　

  Postfix conditionals may be applied to most single-line statements. 
  For example, `@import`, `@charset`, mixins—and of course, properties as shown below:
  
  後置条件文はほとんどのシングルライン文に適用することができます。
  例えば、`@import` や `@charset` やmixins。もちろん次に示すようにプロパティにも。
  
  
      pad(types = margin padding, n = 5px)
        padding unit(n, px) if padding in types
        margin unit(n, px) if margin in types

      body
        pad()

      body
        pad(margin)

      body
        apply-mixins = true
        pad(padding, 10) if apply-mixins

結果:

      body {
        padding: 5px;
        margin: 5px;
      }
      body {
        margin: 5px;
      }
      body {
        padding: 10px;
      } 
