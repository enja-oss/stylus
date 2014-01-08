 +  元文書: [stylus/docs/extend.md at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub]
(https://github.com/LearnBoost/stylus/blob/f9ed220d7e5f0b44aeaca58ffd6490566b5f0757/docs/extend.md 
"stylus/docs/extend.md at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub")

## Extend [原文](http://learnboost.github.com/stylus/docs/extend.html)

  Stylus __@extend__ ディレィティブは幾つかの微妙な違いはありますが、[SASSの実装](http://sass-lang.com/docs/yardoc/file.SASS_REFERENCE.html#extend)からインスピレーションを得ています。
  特徴は、他のセマンティックルールセットを継承することによる、メンテナンスの簡素化です。

### mixinsでの“展開”

  同様の効果を得るためにmixinsを使用することもできますが、これはCSSが重複する可能性があります。
  複数のクラスを定義する典型的なパターンは、以下に示すように、"警告メッセージ"などで幾つかの要素を組み合わせる場合です。

  このテクニックはうまく動作しますが、メンテナンスが困難です。
  
      .message,
      .warning {
        padding: 10px;
        border: 1px solid #eee;
      }

      .warning {
        color: #E2E21E;
      }


### `__@extend__`の使用方法

  単に目的のセレクタを渡すだけで、`__@extend__` によって同じ出力が得られます。
  Stylusは `.warning` セレクタに既存の `.message` ルールセットを付け加えます。
  その際に `.warning` クラスは  `.message` からプロパティを継承します。

      .message {
        padding: 10px;
        border: 1px solid #eee;
      }

      .warning {
        @extend .message;
        color: #E2E21E;
      }


 
  これは、どのように`__@extend__` がカスケードされるか検証するためのより複雑な例です。
  
      red = #E33E1E
      yellow = #E2E21E

      .message
        padding: 10px
        font: 14px Helvetica
        border: 1px solid #eee

      .warning
        @extends .message
        border-color: yellow
        background: yellow + 70%

      .error
        @extends .message
        border-color: red
        background: red + 70%

      .fatal
        @extends .error
        font-weight: bold
        color: red
 
  以下のCSSを生成します。
  
      .message,
      .warning,
      .error,
      .fatal {
        padding: 10px;
        font: 14px Helvetica;
        border: 1px solid #eee;
      }
      .warning {
        border-color: #e2e21e;
        background: #f6f6bc;
      }
      .error,
      .fatal {
        border-color: #e33e1e;
        background: #f7c5bc;
      }
      .fatal {
        font-weight: bold;
        color: #e33e1e;
      }

  実際にStylusとSASSの異なる点は、SASSではネストしたセレクタに対する `__@extend__` を許可しない点です。

     form
       button
         padding: 10px

     a.button
       @extend form button 
     Syntax error: Can't extend form button: can't extend nested selectors
             on line 6 of standard input
       Use --trace for backtrace.

   Stylusではセレクタが一致する限り、それは動作する！
   
       form
         input[type=text]
           padding: 5px
           border: 1px solid #eee
           color: #ddd

       textarea
         @extends form input[type=text]
         padding: 10px
  
   結果:
   
        form input[type=text],
        form textarea {
          padding: 5px;
          border: 1px solid #eee;
          color: #ddd;
        }
        textarea {
          padding: 10px;
        }
      
