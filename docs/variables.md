 +  元文書: [stylus/docs/variables.html at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub]
(https://github.com/LearnBoost/stylus/blob/f9ed220d7e5f0b44aeaca58ffd6490566b5f0757/docs/variables.html 
"stylus/docs/variables.html at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub")

## Variables

## 変数 [原文](http://learnboost.github.com/stylus/docs/variables.html)

We may assign expressions to variables and use them throughout our stylesheet:

変数を割り当てることができ、スタイルシートの至る所で使用できます。

     font-size = 14px

     body
       font font-size Arial, sans-serif

Compiles to:

コンパイル後:

     body {
       font: 14px Arial, sans-serif;
     }

Variables can even consist of an expression list:

変数はリスト表現としても成立します。

    font-size = 14px
    font = font-size "Lucida Grande", Arial

    body
      font font sans-serif

Compiles to:

コンパイル後:

    body {
      font: 14px "Lucida Grande", Arial sans-serif;
    }

Identifiers (variable names, functions, etc.) may also include the `$` character. For example:

例えば次のように、（変数名、関数などの）識別子は `$` 文字を含めることができます。

    $font-size = 14px
    body {
      font: $font-size sans-serif;
    }

## Property Lookup

## Property検索

 Another cool feature unique to Stylus is the ability to reference
 properties defined _without_ assigning their values to variables. A great example of this is the logic required for vertically and horizontally center an element (typically done using percentages and negative margins, as follows):

別のStylus固有のクールな機能として、変数にこれらの値を割り当てる_ことなく_、定義されたプロパティを参照できることが挙げられます。
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

  Instead of assigning the variables `w` and `h`, we can simply prepend the `@`
  character to the property name to access the value:

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

  Another use-case is conditionally defining properties within mixins based on the existence of others . In the following example, we apply a default `z-index` of `1`—but _only_ if `z-index` was not previously specified:

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

  Property lookup will "bubble up" the stack until found, or return `null` if the property cannot be resolved. In the following example, `@color` will resolve to `blue`:
 
プロパティ検索はその名前が見つかるか、名前解決できず `null` が返されるまでスタックを遡っていきます。 
次の例の中では、`@color` は `blue` として解釈されます。

      body
        color: red
        ul
          li
            color: blue
            a
              background-color: @color
