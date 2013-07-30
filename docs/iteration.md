+ 元文書: [stylus/docs/iteration.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 · LearnBoost/stylus · GitHub](https://github.com/LearnBoost/stylus/blob/0ab9219d80a5304e32437ef3cabb7b3fa1345534/docs/iteration.md "stylus/docs/iteration.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 · LearnBoost/stylus · GitHub")

## 繰り返し [原文](http://learnboost.github.io/stylus/docs/iteration.html)

 Stylusでは、`for/in`構文を使って式を反復することができます。これは次のような形をとります:
 
      for <val-name> [, <key-name>] in <expression>

例えば、以下の表現は:

    body
      for num in 1 2 3
        foo num

次のように展開されます:

      body {
        foo: 1;
        foo: 2;
        foo: 3;
      }

次の例は、`<key‐name>`の利用法を示します:

      body
        fonts = Impact Arial sans-serif
        for font, i in fonts
          foo i font

これは次のように展開されます:

        body {
          foo: 0 Impact;
          foo: 1 Arial;
          foo: 2 sans-serif;
        }

### ミックスイン

 繰り返しをミックスインと一緒に使うととても便利です。例えば、挿入と繰り返しを使って、式のペアをプロパティとして扱うことができます。
 
 以下では、すべての`arguments`を条件によって活用することで、カンマ区切りと式のリストの _両方_ をサポートできるように`apply()`を定義します。
 
     apply(props)
       props = arguments if length(arguments) > 1
       for prop in props
         {prop[0]} prop[1]

     body
       apply(one 1, two 2, three 3)

     body
       list = (one 1) (two 2) (three 3)
       apply(list)

### 関数

 Stylusでは関数の中でもforループを使うことができます。以下にいくつかの利用例を挙げます。

Sum:

      sum(nums)
        sum = 0
        for n in nums
          sum += n

      sum(1 2 3)
      // => 6

join:

      join(delim, args)
        buf = ''
        for arg, index in args
          if index
            buf += delim + arg
          else
            buf += arg

      join(', ', foo bar baz)
      // => "foo, bar, baz"

### 後置記法

 `if`や`unless`と同様に、`for`でも後置記法が使えます。以下の例は、上記の例を後置記法を使って書きなおしたものです。
 
       sum(nums)
         sum = 0
         sum += n for n in nums


       join(delim, args)
         buf = ''
         buf += i ? delim + arg : arg for arg, i in args

 ループの中で、 __return__ を使うこともできます。以下の例では、`n % 2 == 0`が __true__ を返した場合に`n`を返しています。
 
     first-even(nums)
       return n if n % 2 == 0 for n in nums

     first-even(1 3 5 5 6 3 2)
     // => 6
