+ 元文書: [stylus/docs/mixins.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 · LearnBoost/stylus · GitHub](https://github.com/LearnBoost/stylus/blob/0ab9219d80a5304e32437ef3cabb7b3fa1345534/docs/mixins.md "stylus/docs/mixins.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 · LearnBoost/stylus · GitHub")

## ミックスイン [原文](http://learnboost.github.com/stylus/docs/mixins.html)

ミックスインと関数は同じ形で定義されますが、利用のされ方が異なります。

例えば、下の`border-radius(n)`関数は _ミックスイン_ として実行されます。つまり、式の一部としてではなく文として扱われます。

`border-radius()`がセレクターの中で実行される時、プロパティはセレクターの中に展開されてコピーされます。

    border-radius(n)
      -webkit-border-radius n
      -moz-border-radius n
      border-radius n

    form input[type=button]
      border-radius(5px)

これは下のように展開されます。

    form input[type=button] {
      -webkit-border-radius: 5px;
      -moz-border-radius: 5px;
      border-radius: 5px;
    }

ミックスインでは、括弧を完全に省略可能です。これにより、ベンダー独自のプロパティを意識せずに利用することが可能です！

    border-radius(n)
      -webkit-border-radius n
      -moz-border-radius n
      border-radius n

    form input[type=button]
      border-radius 5px

ミックスイン定義の中の`border-radius`は再帰的な関数呼び出しではなく、通常のプロパティとして扱われることに注意してください。

更に、自動で定義される`arguments`ローカル変数を利用して、関数に渡される式や複数の引数を扱うことができます。

    border-radius()
      -webkit-border-radius arguments
      -moz-border-radius arguments
      border-radius arguments

このような定義により、`border-radius 1px 2px / 3px 4px`のような形で引数を渡すことが可能です！

さらに便利な利用方法として、ベンダー独自の仕様を意識せずに利用できることがあります。たとえば、IE固有の`opacity`をサポートするには次のように書くことができます。

        support-for-ie ?= true

        opacity(n)
          opacity n
          if support-for-ie
            filter unquote('progid:DXImageTransform.Microsoft.Alpha(Opacity=' + round(n * 100) + ')')

        #logo
          &:hover
            opacity 0.5

これは次のように展開されます。

        #logo:hover {
          opacity: 0.5;
          filter: progid:DXImageTransform.Microsoft.Alpha(Opacity=50);
        }

### 親要素の参照

 ミックスイン定義の中では、親要素を`&`で参照できます。これにより入れ子構造を作ることなく親要素にアクセスできます。
 
 例えば、テーブルの行の色を１行おきに変えるために、`stripe(even, odd)`というミックスインを定義することを考えてみましょう。偶数行のために`even`、奇数行のために`odd`という初期値を定義し、テーブル行に`background-color`プロパティを適用します。
 
     stripe(even = #fff, odd = #eee)
       tr
         background-color odd
         &.even
         &:nth-child(even)
           background-color even

このミックスインは以下のように利用できます。

     table
       stripe()
       td
         padding 4px 10px

     table#users
       stripe(#303030, #494848)
       td
         color white

次のように、`stripe()`を親要素の参照を利用せずに定義することもできます。

    stripe(even = #fff, odd = #eee)
      tr
        background-color odd
      tr.even
      tr:nth-child(even)
        background-color even

`stripe()`を通常のプロパティであるかのように利用することもできます。

    stripe #fff #000

### ミックスインのなかでミックスインを使う

 もちろん、ミックスインの中で他のミックスインを利用することも可能です！
 
 例として、下では`comma-list()`というミックスインを`inline-list()`というミックスインを利用して定義しています。
 `inline-list()`はリストを１行にし、`comma-list()`はリストを１行のカンマで区切った形式で表示します。
 
 
     inline-list()
       li
         display inline

     comma-list()
       inline-list()
       li
         &:after
           content ', '
         &:last-child:after
           content ''

     ul
       comma-list()

これは以下のように展開されます。

    ul li:after {
      content: ", ";
    }
    ul li:last-child:after {
      content: "";
    }
    ul li {
      display: inline;
    }

