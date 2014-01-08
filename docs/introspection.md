+ 元文書: [stylus/docs/introspection.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 · LearnBoost/stylus · GitHub]
(https://github.com/LearnBoost/stylus/blob/0ab9219d80a5304e32437ef3cabb7b3fa1345534/docs/introspection.md 
"stylus/docs/introspection.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 · LearnBoost/stylus · GitHub")

## イントロスペクション API [原文](http://learnboost.github.com/stylus/docs/introspection.html)

Stylusはイントロスペクション（一般的には、リフレクション機能の一種で、依存関係など内部情報を返すインターフェース）APIをサポートしています。
これによりmixins とfunctionsの呼び出し元に応じて、その振舞いを変更させることが出来ます。

## mixin

  `mixin` は、関数内部に自動的に割り当てられるローカル変数です。
  `mixin`  は、トップレベルから呼び出されたことを示す `root` 文字列、
  それ以外からの呼び出しを示す `block` 文字列、そして関数として戻り値を期待された場合を示す `false` で構成されています。

  次の例では、`reset()` を定義し、下の3パターンに応じてその振舞いを変更しています。
  トップレベルから呼び出された場合、それ以外から呼び出された場合、そして、`foo` プロパティの中で戻り値を期待されている場合。

      reset()
        if mixin == 'root'
          got
            root true
        else if mixin
          got 'a mixin'
        else
          'not a mixin'

      reset()

      body
        reset()
        foo reset()

コンパイル後:

        got {
          root: true;
        }
        body {
          foo: "not a mixin";
          got: "a mixin";
        }
