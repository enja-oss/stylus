+ 元文書: [stylus/docs/kwargs.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 · LearnBoost/stylus · GitHub](https://github.com/LearnBoost/stylus/blob/0ab9219d80a5304e32437ef3cabb7b3fa1345534/docs/kwargs.md "stylus/docs/kwargs.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 · LearnBoost/stylus · GitHub")

## キーワード引数 [原文](http://learnboost.github.com/stylus/docs/kwargs.html)

 Stylus はキーワード引数("kwargs")をサポートしています。キーワード引数を使うと、関連付けられたパラメータの名前によって引数を参照することが可能になります。

 次に上げる例は、すべて同じ形に展開されます。キーワード引数はどのような順番でも関数に渡すことができますが、残りのキーワードのついて _いない_ 引数はまだ割り当てられていないパラメータに順次適用されます。

      body {
        color: rgba(255, 200, 100, 0.5);
        color: rgba(red: 255, green: 200, blue: 100, alpha: 0.5);
        color: rgba(alpha: 0.5, blue: 100, red: 255, 200);
        color: rgba(alpha: 0.5, blue: 100, 255, 200);
      }

 これは次のように展開されます:
 
       body {
         color: rgba(255,200,100,0.5);
         color: rgba(255,200,100,0.5);
         color: rgba(255,200,100,0.5);
         color: rgba(255,200,100,0.5);
       }


 関数やミックスインがどのようなパラメータを受け取るのか調べるには`p()`関数を使います:
 
    p(rgba)

 これは次のように展開されます:

    inspect: rgba(red, green, blue, alpha)
