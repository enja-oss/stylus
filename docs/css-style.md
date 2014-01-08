 +  元文書: [stylus/docs/css-style.md at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub]
(https://github.com/LearnBoost/stylus/blob/f9ed220d7e5f0b44aeaca58ffd6490566b5f0757/docs/css-style.md 
"stylus/docs/css-style.md at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub")

## CSS Style [原文](http://learnboost.github.com/stylus/docs/css-style.html)

Stylusは正規のCSSスタイルを標準でサポートしています。
これは、パーサーを選択したり、特定のファイルで使用する詳細なスタイルについて明示する必要がないことを意味します。

### 例

以下の小さなスタイルは（Stylusの）インデントアプローチを利用しています:
 
     border-radius()
       -webkit-border-radius arguments
       -moz-border-radius arguments
       border-radius arguments

     body a
       font 12px/1.4 "Lucida Grande", Arial, sans-serif
       background black
       color #ccc

     form input
       padding 5px
       border 1px solid
       border-radius 5px

この例は、（通常書くであろう）CSSとしても書くことが出来ます。つまり、括弧、コロン、セミコロンは任意です:

     border-radius() {
       -webkit-border-radius: arguments;
       -moz-border-radius: arguments;
       border-radius: arguments;
     }

     body a {
       font: 12px/1.4 "Lucida Grande", Arial, sans-serif;
       background: black;
       color: #ccc;
     }

     form input {
       padding: 5px;
       border: 1px solid;
       border-radius: 5px;
     }

この2種類のスタイルを混在させることが可能なため、以下も有効です:

     border-radius()
       -webkit-border-radius: arguments;
       -moz-border-radius: arguments;
       border-radius: arguments;

     body a {
       font: 12px/1.4 "Lucida Grande", Arial, sans-serif;
       background: black;
       color: #ccc;
     }

     form input
       padding: 5px;
       border: 1px solid;
       border-radius: 5px;

その場合であっても、Variables、functions、mixinsなど、Stylusが提供するすべての機能は正しく動作します。

     main-color = white
     main-hover-color = black

     body a {
       color: main-color;
       &:hover { color: main-hover-color; }
     }

     body a { color: main-color; &:hover { color: main-hover-color; }}

 しかし、このルールには幾つかの注意点があります。
 2つのスタイルを混在させることができるのですが、インデントルールは適用されたままとなります。
 そのため、まったく変更せずに、 _すべての_ プレーンなCSSが動作するわけではありませんが、
 この機能により、CSS構文を維持しながら、Stylusの他の強力な機能を活用することができます。

