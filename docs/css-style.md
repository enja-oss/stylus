 +  元文書: [stylus/docs/css-style.md at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub]
(https://github.com/LearnBoost/stylus/blob/f9ed220d7e5f0b44aeaca58ffd6490566b5f0757/docs/css-style.md 
"stylus/docs/css-style.md at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub")

## CSS Style [原文](http://learnboost.github.com/stylus/docs/css-style.html)

 Stylus transparently supports a regular CSS-style syntax. 
 This means you don't need an alternative parser, or specify that a certain file uses a specific style.

Stylusは正規のCSSスタイルを標準でサポートしています。
これは、パーサーを選択したり、特定のファイルで使用する詳細なスタイルについて明示する必要がないことを意味します。

### 例

以下の小さなスタイルは（Stylusの）インデントアプローチを利用しています。
 
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

この例は、（通常書くであろう）CSSとしても書くことが出来ます。つまり、括弧、コロン、セミコロンは任意です。

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

異なるこれらの書き方を混在させてしまうこともありますが、次の書き方は有効です。

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

 There are a few caveats to this rule: 
 since the two styles may be mixed and matched, some indentation rules still apply. 
 So although not _every_ plain-CSS stylesheet will work with zero modification, 
 this feature allows those who prefer CSS syntax to continue doing so while leveraging Stylus' other powerful features.
 
 しかし、このルールには幾つかの注意点があります。
 2つのスタイルを混在させることができるのですが、インデントルールは適用されたままとなります。
 とは言っても、 _すべての_ プレーンなCSSは修正することなしに動作します。
 この機能により、CSS構文を維持しながら、Stylusの他の強力な機能を活用することができます。

