 +  元文書: [stylus/docs/interpolation.html at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub]
(https://github.com/LearnBoost/stylus/blob/f9ed220d7e5f0b44aeaca58ffd6490566b5f0757/docs/interpolation.html 
"stylus/docs/interpolation.html at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub")

## Interpolation

## 補間 [原文](http://learnboost.github.com/stylus/docs/interpolation.html)

  Stylus supports interpolation by using the `{}` characters to surround an expression, which then becomes part of the identifier. For example, `-webkit-{'border' + '-radius'}` evaluates to `-webkit-border-radius`.

  Stylusは、次に識別子の一部となる式を `{}` 文字で囲むことで補間をサポートします。例えば `-webkit-{'border' + '-radius'}` は `-webkit-border-radius` として評価されます。

 A great example use-case for this is expanding properties with vendor prefixes.

 良い使い方の例は、プロパティにベンダープレフィックを付けて展開する場合です。

      vendor(prop, args)
        -webkit-{prop} args
        -moz-{prop} args
        {prop} args

      border-radius()
        vendor('border-radius', arguments)
      
      box-shadow()
        vendor('box-shadow', arguments)

      button
        border-radius 1px 2px / 3px 4px

Yields:

結果:

      button {
        -webkit-border-radius: 1px 2px / 3px 4px;
        -moz-border-radius: 1px 2px / 3px 4px;
        border-radius: 1px 2px / 3px 4px;
      }

## Selector Interpolation

## セレクタ補間

 Interpolation works with selectors as well. For example, we may iterate to assign the `height` property for the first 5 rows in a table, as shown below:

 補間はセレクタにも同様に対応しています。例えば以下のように、テーブルの最初の5行に `height` プロパティを割り当てるため、イテレート（反復）することがあります。

    table
      for row in 1 2 3 4 5
        tr:nth-child({row})
          height: 10px * row

Yields:

結果:

    table tr:nth-child(1) {
      height: 10px;
    }
    table tr:nth-child(2) {
      height: 20px;
    }
    table tr:nth-child(3) {
      height: 30px;
    }
    table tr:nth-child(4) {
      height: 40px;
    }
    table tr:nth-child(5) {
      height: 50px;
    }
