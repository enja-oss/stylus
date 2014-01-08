+ 元文書: [stylus/docs/error-reporting.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 · LearnBoost/stylus · GitHub](https://github.com/LearnBoost/stylus/blob/0ab9219d80a5304e32437ef3cabb7b3fa1345534/docs/error-reporting.md "stylus/docs/error-reporting.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 · LearnBoost/stylus · GitHub")

## Error Reporting [原文](http://learnboost.github.com/stylus/docs/error-reporting.html)

 Stylusには、文法やパース、実行エラーの解析のために、スタックトレース、行番号、ファイル名の出力を備えた、素晴らしいエラーレポートが備えられています。

### パースエラー

パースエラーの例:

     body
       form input
         == padding 5px

これは次の様に展開されます:

     Error: /Users/tj/Projects/stylus/testing/test.styl:4
       3: '  form input'
       4: '    == padding 5px'

     illegal unary ==

### 実行エラー

 次の、"ランタイム"、または実行エラーは`border-radius()`に、期待される`Unit`ではなく文字列を渡すことで引き起こされます(ヘルパー関数`ensure(n, 'unit')`でエラーを投げています)。

      ensure(val, type)
        unless val is a type
          error('expected a ' + type + ', but got ' + typeof(val))

      border-radius(n)
        ensure(n, 'unit')
        -webkit-border-radius n
        -moz-border-radius n
        border-radius n

      body
        border-radius '5px'

これは次の様に展開されます:

      Error: /Users/tj/Projects/stylus/examples/error.styl:12
        11: ''
        12: 'body'
        13: '  border-radius \'5px\''
        14: ''

      expected a unit, but got string
          at ensure() (/Users/tj/Projects/stylus/examples/error.styl:2)
          at border-radius() (/Users/tj/Projects/stylus/examples/error.styl:5)
          at "body" (/Users/tj/Projects/stylus/examples/error.styl:10)
