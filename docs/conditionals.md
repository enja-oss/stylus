 +  元文書: [stylus/docs/conditionals.md at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub]
(https://github.com/LearnBoost/stylus/blob/f9ed220d7e5f0b44aeaca58ffd6490566b5f0757/docs/conditionals.md 
"stylus/docs/conditionals.md at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub")

## 条件文[原文](http://learnboost.github.io/stylus/docs/conditionals.html)

条件文は条件付きimport、mixins、functionなどを介して制御フローを提供し、言語を動的に処理させることを可能にします。
以下の例はシンプルな例であり、あまり推奨しません。:)

### if / else if / else

 Along with `if` are the typical `else if` and `else` tokens, acting as fallbacks.
 
 `if` 条件文は期待通りに動作します。評価式を受け入れ、`true` の場合、次のブロックを評価します。
 `if` はフォールバックとして機能する 典型的な `else if` や `else` トークンを伴います。
 
 次の例では、条件によって `padding` を `margin` で交換して上書きします。

    overload-padding = true

    if overload-padding
      padding(y, x)
        margin y x

    body
      padding 5px 10px

他の例:

    box(x, y, margin = false)
      padding y x
      if margin
        margin y x

    body
      box(5px, 10px, true)

他の `box()` ヘルパー:

    box(x, y, margin-only = false)
      if margin-only
        margin y x
      else
        padding y x

### unless

 Rubyプログラミングに親しい人向けに `unless` 条件文を持っています。
 これは基本的に `if` の反対（本質的には `if (!(expr))` ）の作用をします。

以下の例では、`disable-padding-override` が `undefined` または `false` の場合、
`padding` が上書きされ、代わりに `margin` で表示します。
しかし、`true` の場合、`padding` は予定通り `padding 5px 10px` のまま出力されます。

     disable-padding-override = true

     unless disable-padding-override is defined and disable-padding-override
       padding(x, y)
         margin y x

     body
       padding 5px 10px

### 後置条件文

  Stylusは後置条件文をサポートしています。
  これは、`if` や `unless` がオペレータのように振舞うことを意味しています。
  右辺の式がtrueと評価される場合、左辺のオペレータが評価されます。  
  
  例えば、幾つかの基本型のチェックをするために、`negative()` を定義してみましょう。
  以下では、ブロックスタイルの条件文を使用しています。
  
      negative(n)
        unless n is a 'unit'
          error('invalid number')
        if n < 0
          yes
        else
          no

  続いて、関数を簡潔に保つために後置条件文を使用します。

      negative(n)
        error('invalid number') unless n is a 'unit'
        return yes if n < 0
        no

  もちろんこれをさらに改良することもできます。
  例えば、`n < 0 ? yes : no` と書くことも出来ますし、次のように単純にブール値とすることもできます。`n < 0`　

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
