 +  元文書: [stylus/docs/escape.md at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub]
(https://github.com/LearnBoost/stylus/blob/f9ed220d7e5f0b44aeaca58ffd6490566b5f0757/docs/escape.md 
"stylus/docs/escape.md at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub")

## エスケープ [原文](http://learnboost.github.com/stylus/docs/escape.html)

Stylusでは文字をエスケープすることも出来ます。
識別子の中に効果的に配置することで、レンダリング時にリテラルとして出力されるようになります。 

例:

     body
       padding 1 \+ 2

コンパイル後:

     body {
       padding: 1 + 2;
     }


注意：Stylusでは、プロパティの中で `/` を使用する際は、括弧で囲うことが必須です。

    body
      font 14px/1.4
      font (14px/1.4)

結果:

    body {
      font: 14px/1.4;
      font: 10px;
    }
