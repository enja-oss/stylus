+ 元文書: [stylus/docs/media.md at b27196e4e8ac736e2e7d66b1175391d365a32f5e · LearnBoost/stylus · GitHub](https://github.com/LearnBoost/stylus/blob/b27196e4e8ac736e2e7d66b1175391d365a32f5e/docs/media.md "stylus/docs/media.md at b27196e4e8ac736e2e7d66b1175391d365a32f5e · LearnBoost/stylus · GitHub")

## @media [原文](http://learnboost.github.com/stylus/docs/media.html)

 `@media`は通常のCSSと同様に動作しますが、Stylusのブロック記法を反映します。

     @media print
       #header
       #footer
         display none

これは以下のように展開されます:

      @media print {
        #header,
        #footer {
          display: none;
        }
      }
