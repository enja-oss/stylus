 +  元文書: [stylus/docs/variables.md at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub]
(https://github.com/LearnBoost/stylus/blob/f9ed220d7e5f0b44aeaca58ffd6490566b5f0757/docs/variables.md 
"stylus/docs/variables.md at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub")

## 変数 [原文](http://learnboost.github.com/stylus/docs/variables.html)

変数を割り当てることができ、スタイルシートの至る所で使用できます。

     font-size = 14px

     body
       font font-size Arial, sans-serif

コンパイル後:

     body {
       font: 14px Arial, sans-serif;
     }

変数はリスト表現としても成立します。

    font-size = 14px
    font = font-size "Lucida Grande", Arial

    body
      font font sans-serif

コンパイル後:

    body {
      font: 14px "Lucida Grande", Arial sans-serif;
    }

例えば次のように、（変数名、関数などの）識別子は `$` 文字を含めることができます。

    $font-size = 14px
    body {
      font: $font-size sans-serif;
    }

## Property検索

別のStylus固有のクールな機能として、変数にこれらの値を割り当てる _ことなく_ 、定義されたプロパティを参照できることが挙げられます。
この良い例は　エレメントが垂直方向と水平方法の中心となるようなロジックが必要な場合です。
（次のように、パーセントやマイナスのマージンにて使うことが一般的です。）

     #logo
       position: absolute
       top: 50%
       left: 50%
       width: w = 150px
       height: h = 80px
       margin-left: -(w / 2)
       margin-top: -(h / 2)

`w` や `h` などの変数を割り当てる代わりに、
プロパティの先頭に `@` 文字を追加することでもっと単純に値にアクセスすることができます。

     #logo
       position: absolute
       top: 50%
       left: 50%
       width: 150px
       height: 80px
       margin-left: -(@width / 2)
       margin-top: -(@height / 2)

別の使用方法は、mixinsの中において、他の場所に存在するという前提条件付きで定義するプロパティです。
次の例の中で、`z-index` にデフォルト値の `1` を定義していますが、
これは、事前に `z-index` が指定されていない場合 _のみ_ 使われます。

      position()
        position: arguments
        z-index: 1 unless @z-index

      #logo
        z-index: 20
        position: absolute

      #logo2
        position: absolute

プロパティ検索はその名前が見つかるか、名前解決できず `null` が返されるまでスタックを遡っていきます。 
次の例の中では、`@color` は `blue` として解釈されます。

      body
        color: red
        ul
          li
            color: blue
            a
              background-color: @color
