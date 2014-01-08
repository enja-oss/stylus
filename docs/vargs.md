+ 元文書: [stylus/docs/vargs.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 · LearnBoost/stylus · GitHub](https://github.com/LearnBoost/stylus/blob/0ab9219d80a5304e32437ef3cabb7b3fa1345534/docs/vargs.md "stylus/docs/vargs.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 · LearnBoost/stylus · GitHub")

## 可変長引数 [原文](http://learnboost.github.com/stylus/docs/vargs.html)

 Stylusでは`name...`の形で定義することで可変長引数を利用可能です。この引数のなかにミックスインや関数に渡されたすべての引数が含まれます。これは、（例えば）`-webkit`や`-moz`のようなベンダープレフィックスを適用するために暗黙的な関数呼び出しを行う際に便利です。

次の例では、単純に渡されたすべての引数を複数のプロパティに適用しています。

     box-shadow(args...)
       -webkit-box-shadow args
       -moz-box-shadow args
       box-shadow args

     #login
       box-shadow 1px 2px 5px #eee

これは次のように展開されます:

      #login {
        -webkit-box-shadow: 1px 2px 5px #eee;
        -moz-box-shadow: 1px 2px 5px #eee;
        box-shadow: 1px 2px 5px #eee;
      }

特定の引数-例えば`x-offset`-を参照したいとき、単純に`args[0]`のように参照できます。またはミックスイン定義を次のように書き換えても良いでしょう:

        box-shadow(offset-x, args...)
          got-offset-x offset-x
          -webkit-box-shadow offset-x args
          -moz-box-shadow offset-x args
          box-shadow offset-x args

        #login
          box-shadow 1px 2px 5px #eee

これは次のように展開されます:

        #login {
          got-offset-x: 1px;
          -webkit-box-shadow: 1px 2px 5px #eee;
          -moz-box-shadow: 1px 2px 5px #eee;
          box-shadow: 1px 2px 5px #eee;
        }

### arguments変数

  `arguments`引数はミックスインや関数定義の中で用いることができ、渡された式を正確に保持しています。これはカンマなどを含めた _正確な_ 式を保持しているため、特にベンダー特有のプロパティをサポートするためなどの目的に便利です。

  例えば、可変長引数を用いると、カンマは式の区切り文字なので無視されます。
  
      box-shadow(args...)
        -webkit-box-shadow args
        -moz-box-shadow args
        box-shadow args

      #login
        box-shadow #ddd 1px 1px, #eee 2px 2px 

これでは望んだ結果になりません:

      #login {
        -webkit-box-shadow: #ddd 1px 1px #eee 2px 2px;
        -moz-box-shadow: #ddd 1px 1px #eee 2px 2px;
        box-shadow: #ddd 1px 1px #eee 2px 2px;
      }

このミックスインを`arguments`を用いて定義しなおしてみましょう:

      box-shadow()
        -webkit-box-shadow arguments
        -moz-box-shadow arguments
        box-shadow arguments

      body
        box-shadow #ddd 1px 1px, #eee 2px 2px

これで望んだ結果が得られます:

      body {
        -webkit-box-shadow: #ddd 1px 1px, #eee 2px 2px;
        -moz-box-shadow: #ddd 1px 1px, #eee 2px 2px;
        box-shadow: #ddd 1px 1px, #eee 2px 2px;
      }

