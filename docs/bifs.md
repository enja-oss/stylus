+ 元文書: [stylus/docs/bifs.md at 21e225a06ab260551ce506e850ceab7969b219c3 · LearnBoost/stylus · GitHub](https://github.com/LearnBoost/stylus/blob/21e225a06ab260551ce506e850ceab7969b219c3/docs/bifs.md "stylus/docs/bifs.md at 21e225a06ab260551ce506e850ceab7969b219c3 · LearnBoost/stylus · GitHub")

## 組み込み関数 [原文](http://learnboost.github.com/stylus/docs/bifs.html)

### red(color)

与えられた`color`の赤色の値を返します。

     red(#c00)
     // => 204

### green(color)

与えられた`color`の緑色の値を返します。

     green(#0c0)
     // => 204

### blue(color)

与えられた`color`の青色の値を返します。

     red(#00c)
     // => 204

### alpha(color)

与えられた`color`のアルファ値を返します。

      alpha(#fff)
      // => 1
      
      alpha(rgba(0,0,0,0.3))
      // => 0.3

### dark(color)

`color`が暗色であるか判別します。

      dark(black)
      // => true

      dark(#005716)
      // => true

      dark(white)
      // => false


### light(color)

`color`が明色であるか判別します。

    light(black)
    // => false

    light(white)
    // => true
    
    light(#00FF40)
    // => true

### hue(color)

与えられた`color`の色相を返します。

    hue(hsla(50deg, 100%, 80%))
    // => 50deg

### saturation(color)

与えられた`color`の彩度を返します。

    saturation(hsla(50deg, 100%, 80%))
    // => 100%

### lightness(color)

与えられた`color`の明度を返します。

    lightness(hsla(50deg, 100%, 80%))
    // => 80%

### push(expr, args...)

 与えられた引数`args`を`expr`の後方に追加(push)します。

     nums = 1 2
     push(nums, 3, 4, 5)

     nums
     // => 1 2 3 4 5
 
 `append()`にエイリアスが付けられています。

### unshift(expr, args...)

 与えられた引数`args`を`expr`の前方に追加(unshift)します。

     nums = 4 5
     unshift(nums, 3, 2, 1)

     nums
     // => 1 2 3 4 5
 
 `prepend()`にエイリアスが付けられています。

### keys(pairs)

  与えられたハッシュ`pairs`のキー値の集合を返します。
  
     pairs = (one 1) (two 2) (three 3)
     keys(pairs)
     // => one two three

### values(pairs)

  与えられたハッシュ`pairs`の値の集合を返します。
  
     pairs = (one 1) (two 2) (three 3)
     values(pairs)
     // => 1 2 3

### typeof(node)

`node`の種類を文字列で返します。

      type(12)
      // => 'unit'

      typeof(12)
      // => 'unit'
      
      typeof(#fff)
      // => 'rgba'

      type-of(#fff)
      // => 'rgba'

`type-of`と`type`にエイリアスが付けられています。

### unit(unit[, type])

`unit`の単位の文字列、または空の文字列を返します。
また、与えられた`type`を`unit`に加えます（単位の変換は行いません）。

    unit(10)
    // => ''
    
    unit(15in)
    // => 'in'
    
    unit(15%, 'px')
    // => 15px

    unit(15%, px)
    // => 15px

### match(pattern, string)

`string`が`pattern`にマッチするかどうか判定します。

    match('^foo(bar)?', foo)
    match('^foo(bar)?', foobar)
    // => true

    match('^foo(bar)?', 'foo')
    match('^foo(bar)?', 'foobar')
    // => true
    
    match('^foo(bar)?', 'bar')
    // => false

### abs(unit)

      abs(-5px)
      // => 5px

      abs(5px)
      // => 5px

### ceil(unit)

      ceil(5.5in)
      // => 6in

### floor(unit)

      floor(5.6px)
      // => 5px

### round(unit)

      round(5.5px)
      // => 6px

      round(5.4px)
      // => 5px

**注記:** すべての四捨五入関数はオプションの`precision`引数をとります。この引数には小数点のあとに続く数字の数を指定出来ます:

      ceil(5.52px,1)
      // => 5.6px

      floor(5.57px,1)
      // => 5.5px

      round(5.52px,1)
      // => 5.5px

### min(a, b)

      min(1, 5)
      // => 1

### max(a, b)

      max(1, 5)
      // => 5

### even(unit)

      even(6px)
      // => true

### odd(unit)

      odd(5mm)
      // => true

### sum(nums)

      sum(1 2 3)
      // => 6

### avg(nums)

     avg(1 2 3)
     // => 2

### join(delim, vals...)

  与えられた`vals`を`delim`で結合します。

      join(' ', 1 2 3)
      // => "1 2 3"
      
      join(',', 1 2 3)
      // => "1,2,3"
      
      join(', ', foo bar baz)
      // => "foo, bar, baz"

      join(', ', foo, bar, baz)
      // => "foo, bar, baz"

      join(', ', 1 2, 3 4, 5 6)
      // => "1 2, 3 4, 5 6"

### hsla(color | h,s,l,a)

与えられた`color`、またはh,s,l,aの成分を`HSLA`ノードに変換します。

     hsla(10deg, 50%, 30%, 0.5)
     // => HSLA

     hsla(#ffcc00)
     // => HSLA

### hsl(color | h,s,l)

与えられた`color`、またはh,s,lの成分を`HSLA`ノードに変換します。

     hsl(10, 50, 30)
     // => HSLA

     hsl(#ffcc00)
     // => HSLA

### rgba(color | r,g,b,a)

r,g,b,aの各成分からRGBAを返します。または、`color`のアルファ値を調整したものを返します。

      rgba(255,0,0,0.5)
      // => rgba(255,0,0,0.5)
  
      rgba(255,0,0,1)
      // => #ff0000
  
      rgba(#ffcc00, 0.5)
      // rgba(255,204,0,0.5)

 また、Stylusでは`#rgba`と`#rrggbbaa`の記法もサポートされています。
 
    #fc08
    // => rgba(255,204,0,0.5)

    #ffcc00ee
    // => rgba(255,204,0,0.9)

### rgb(color | r,g,b)

r,g,bの成分から`RGBA`を返します。または`RGBA`に変換します。
    
    rgb(255,204,0)
    // => #ffcc00
    
    rgb(#fff)
    // => #fff

### lighten(color, amount)

与えられた`color`を`amount`に従って明るくします。この関数は単位を判別します。次は、パーセンテージを用いた例になります。

    lighten(#2c2c2c, 30)
    // => #787878

    lighten(#2c2c2c, 30%)
    // => #393939

### darken(color, amount)

与えられた`color`を`amount`に従って暗くします。この関数は単位を判別します。次は、パーセンテージを用いた例になります。

    darken(#D62828, 30)
    // => #551010

    darken(#D62828, 30%)
    // => #961c1c

### desaturate(color, amount)

与えられた`color`の彩度を`amount`に従って低くします。

    desaturate(#f00, 40%)
    // => #c33

### saturate(color, amount)

与えられた`color`の彩度を`amount`に従って高くします。

    saturate(#c33, 40%)
    // => #f00

### invert(color)

色を反転します。赤、緑、青の値は反転されますが、アルファ値は変わりません。

    invert(#d62828)
    // => #29d7d7

### unquote(str | ident)

  与えられた`str`のクォートを外して`Literal`ノードを返します。
 
       unquote("sans-serif")
       // => sans-serif
 
       unquote(sans-serif)
       // => sans-serif

       unquote('1px / 2px')
       // => 1px / 2px

### s(fmt, ...)

 `s()`関数は`Literal`ノードを返す点で`unquote()`に似ていますが、C言語の`sprintf()`のようにフォーマット文字列を受け取ることができます。現状では`%s`指定子のみがサポートされています。

        s('bar()');
        // => bar()

        s('bar(%s)', 'baz');
        // => bar("baz")

        s('bar(%s)', baz);
        // => bar(baz)
        
        s('bar(%s)', 15px);
        // => bar(15px)
        
        s('rgba(%s, %s, %s, 0.5)', 255, 100, 50);
        // => rgba(255, 100, 50, 0.5)
        
        s('bar(%Z)', 15px);
        // => bar(%Z)
        
        s('bar(%s, %s)', 15px);
        // => bar(15px, null)
        
文字列演算子`%`も同様の効果を得るために利用できます。

### operate(op, left, right)

  与えられた`op`を`left`と`right`のオペランドに適用します:
  
      op = '+'
      operate(op, 15, 5)
      // => 20

### length([expr])

  括弧に囲まれた式はタプルとして認識されます。`length()`関数はそのような式の長さを返します。

    length((1 2 3 4))
    // => 4

    length((1 2))
    // => 2
  
    length((1))
    // => 1
  
    length(())
    // => 0
  
    length(1 2 3)
    // => 3

    length(1)
    // => 1

    length()
    // => 0

### warn(msg)

  与えられたエラー`msg`を警告します。これは、Stylusのコンパイルを中止しません。

      warn("oh noes!")

### error(msg)

  与えられたエラー`msg`を発し、Stylusのコンパイルを中止します。

    add(a, b)
      unless a is a 'unit' and b is a 'unit'
        error('add() expects units')
      a + b

### last(expr)

 与えられた`expr`のなかの _最後_ の値を返します:
 
      nums = 1 2 3
      last(nums)
      last(1 2 3)
      // => 3
      
      list = (one 1) (two 2) (three 3)
      last(list)
      // => (three 3)

### p(expr)

 与えられた`expr`の詳細を調べます:
 
     fonts = Arial, sans-serif
     p('test')
     p(123)
     p((1 2 3))
     p(fonts)
     p(#fff)
     p(rgba(0,0,0,0.2))
     
     add(a, b)
       a + b
    
     p(add)

標準出力:

     inspect: "test"
     inspect: 123
     inspect: 1 2 3
     inspect: Arial, sans-serif
     inspect: #fff
     inspect: rgba(0,0,0,0.2)
     inspect: add(a, b)

### opposite-position(positions)

 与えられた`positions`の逆のものを返します。
  
     opposite-position(right)
     // => left

     opposite-position(top left)
     // => bottom right

     opposite-position('top' 'left')
     // => bottom right

### image-size(path)

  `path`に見つかった画像の`width`と`height`を返します。画像ファイルの探索は`@import`と同様に行われます。これは、設定`paths`で変更できます。

      width(img)
        return image-size(img)[0]

      height(img)
        return image-size(img)[1]

      image-size('tux.png')
      // => 405px 250px

      image-size('tux.png')[0] == width('tux.png')
      // => true

### add-property(name, expr)

  呼び出し箇所に最も近いブロックに、プロパティ`name`を`expr`の値で追加します。

  例:

      something()
        add-property('bar', 1 2 3)
        s('bar')

      body
        foo: something()

これは次のように展開されます:

      body {
        bar: 1 2 3;
        foo: bar;
      }

  次に"魔法の"`current-property`ローカル変数を見てみましょう。この変数は関数定義の中で準備なく利用可能で、現在のプロパティの名前と値を持っています。

  例えば、`p()`関数を使ってこの変数をインスペクトしてみると、次のようになります。
  
        p(current-property)
        // => "foo" (foo __CALL__ bar baz)

        p(current-property[0])
        // => "foo"

        p(current-property[1])
        // => foo __CALL__ bar baz

  `current-preperty`を用いてもう少し高度な例を見ていきましょう。プロパティを複製し、別の値を持つようにします。また、関数がプロパティの値の箇所で用いられていることを確実にするために条件文を付け足します。

        something(n)
          if current-property
            add-property(current-property[0], s('-webkit-something(%s)', n))
            add-property(current-property[0], s('-moz-something(%s)', n))
            s('something(%s)', n)
          else
            error('something() must be used within a property')

        body {
          foo: something(15px) bar;
        }

これは次のように展開されます:

        body {
          foo: -webkit-something(15px);
          foo: -moz-something(15px);
          foo: something(15px) bar;
        }

  上の例でお気づきかもしれませんが、`bar`は`something(15px)`を返したあとにしか存在していません。`bar`はこの式の中では元の位置のままですが、他の式では考慮に入れられていません。
  
  この更に良い解決策が次になります。まず、`replace()`という関数を定義します。この関数の中では、式の変更を防ぐためにまず式を複製し、複製された式の中の文字列を与えられた文字列で置き換え、複製された式を返すという事を行なっています。式の中の`__CALL__`が置き換えられていますが、これは`something()`の循環的な呼び出しを表しています。

        replace(expr, str, val)
          expr = clone(expr)
          for e, i in expr
            if str == e
              expr[i] = val
          expr

        something(n)
          if current-property
            val = current-property[1]
            webkit = replace(val, '__CALL__', s('-webkit-something(%s)', n))
            moz = replace(val, '__CALL__', s('-moz-something(%s)', n))
            add-property(current-property[0], webkit)
            add-property(current-property[0], moz)
            s('something(%s)', n)
          else
            error('something() must be used within a property')

これは次のように展開されます:

          body {
            foo: foo -webkit-something(5px) bar baz;
            foo: foo -moz-something(5px) bar baz;
            foo: foo something(5px) bar baz;
          }

これで、関数がコールされるプロパティとプロパティの中での位置の点で透過的に動作できる関数が定義されました。この強力な仕組みのおかげでグラデーションなどのベンダープレフィックスを意識せずに利用することが容易になります。

### 未定義の関数

  未定義の関数はリテラルとして出力されます。例として`rgba-stop(50%, #fff)`をcssの中で呼び出すことを考えてみましょう。これは期待通りに出力されます。ヘルパーの中でも同様に用いることができます。
  
  次の例ではシンプルな`stop()`関数を定義します。この関数はリテラルの`rgba-stop()`関数コールを返します。
  
    stop(pos, rgba)
      rgba-stop(pos, rgba)

    stop(50%, orange)
    // => rgba-stop(50%, #ffa500)
