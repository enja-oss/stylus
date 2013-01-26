 + 元文書: [stylus/docs/literal.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 · LearnBoost/stylus · GitHub]
(https://github.com/LearnBoost/stylus/blob/0ab9219d80a5304e32437ef3cabb7b3fa1345534/docs/literal.md 
"stylus/docs/literal.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 · LearnBoost/stylus · GitHub")

## CSS直接出力 [原文](http://learnboost.github.com/stylus/docs/literal.html)

もし、Stylusが対応することができない特殊なニーズがあったとしても、
`@css` を付けることによって、いつでもCSSを直接出力することが出来ます。 
     
     @css {
       body {
         font: 14px;
       }
     }

コンパイル後:

    body {
      font: 14px;
    }
