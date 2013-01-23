+ 元文書: [stylus/docs/keyframes.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 · LearnBoost/stylus · GitHub]
(https://github.com/LearnBoost/stylus/blob/0ab9219d80a5304e32437ef3cabb7b3fa1345534/docs/keyframes.md 
"stylus/docs/keyframes.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 · LearnBoost/stylus · GitHub")

## @keyframes [原文](http://learnboost.github.com/stylus/docs/keyframes.html)

 Stylus supports the `@keyframes` at-rule, which is converted to `@-webkit-keyframes` when compiled:

 Stylusは `@keyframes` @ルールをサポートしています。コンパイル時に `@-webkit-keyframes` に変換されます。

     @keyframes pulse
       0%
         background-color red
         opacity 1.0
         -webkit-transform scale(1.0) rotate(0deg)
       33%
         background-color blue
         opacity 0.75
         -webkit-transform scale(1.1) rotate(-5deg)
       67%
         background-color green
         opacity 0.5
         -webkit-transform scale(1.1) rotate(5deg)
       100%
         background-color red
         opacity 1.0
         -webkit-transform scale(1.0) rotate(0deg)

yielding:

結果:

      @-webkit-keyframes pulse {
        0% {
          background-color: red;
          opacity: 1;
          -webkit-transform: scale(1) rotate(0deg);
        }

        33% {
          background-color: blue;
          opacity: 0.75;
          -webkit-transform: scale(1.1) rotate(-5deg);
        }

        67% {
          background-color: green;
          opacity: 0.5;
          -webkit-transform: scale(1.1) rotate(5deg);
        }

        100% {
          background-color: red;
          opacity: 1;
          -webkit-transform: scale(1) rotate(0deg);
        }

      }

## Expansion

## 展開

 By using `@keyframes`, your rules are automatically expanded to the vendor prefixes defined by the `vendors` variable (default: `webkit moz official`). This means we can alter it at any time for the expansion to take effect immediately. 
 
 `@keyframes` を使うことによって、ルールは `vendors` 変数にて定義されているベンダープレフィックス（デフォルトは`webkit moz official`）に自動的に展開されます。これは、ベンダープレフィックスをいつでも変更でき、変更をすぐに反映できることを意味します。

 For example, consider the following:
 
 例えば、次の点を考慮します。

    @keyframes foo {
      from {
        color: black
      }
      to {
        color: white
      }
    }

This expands to our two default vendors, and the official syntax:

これは2つのデフォルトベンダープレフィックスと、1つの公式な構文に展開されます。

    @-moz-keyframes foo {
      0% {
        color: #000;
      }

      100% {
        color: #fff;
      }
    }
    @-webkit-keyframes foo {
      0% {
        color: #000;
      }

      100% {
        color: #fff;
      }
    }
    @keyframes foo {
      0% {
        color: #000;
      }

      100% {
        color: #fff;
      }
    }

If we wanted to limit to the official syntax only, simply alter `vendors`:

もし、唯一の公式な構文のみに制限したい場合は、単に `vendors` を変更します。

    vendors = official

    @keyframes foo {
      from {
        color: black
      }
      to {
        color: white
      }
    }

Yielding:

結果:

    @keyframes foo {
      0% {
        color: #000;
      }

      100% {
        color: #fff;
      }
    }
