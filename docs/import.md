+ 元文書: [stylus/docs/import.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 · LearnBoost/stylus · GitHub](https://github.com/LearnBoost/stylus/blob/0ab9219d80a5304e32437ef3cabb7b3fa1345534/docs/import.md "stylus/docs/import.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 · LearnBoost/stylus · GitHub")

## Import [原文](http://learnboost.github.com/stylus/docs/import.html)

 スタイラスではCSSの __@import__ 文と、他のStylusシートの動的なインポートに対応しています。

### CSSの __@import__ 文

  拡張子が`.css`のファイル名は記述通りに扱われます。例えば、
  
     @import "reset.css"

これはCSSの __@import__ 文として以下のように解釈されます:

     @import "reset.css"

### Stylusのインポート

 __@import__ を拡張子`.css`以外のものに用いるとスタイラスシートのインポートであると解釈されます(例. `@import "mixins/border-radius"`)

 __@import__ は(node.jsの`require.paths`と同様に)ディレクトリの配列を走査し、指定されたファイルがそのディレクトリに存在するかをチェックします。この配列にはデフォルトでは一つのパスが格納されており、このパスは`filename`オプションの`dirname`で指定されます。したがって、ファイル名が`/tmp/testing/stylus/main.styl`であるとき、importでは`/tmp/testing/stylus/`が探索されます。

 __@import__ はindexスタイルもサポートしています。これは、`@import blueprint`とした時に`blueprint.styl` **と** `blueprint/index.styl` **の両方を** 探すことを意味します。この機能はライブラリのすべての機能を提供するとともに、機能ごとのサブセットにファイルを分けておくときに便利です。

 例えば、一般的なライブラリのファイル構造は以下のようになるでしょう:

    ./tablet
      |-- index.styl 
      |-- vendor.styl 
      |-- buttons.styl 
      |-- images.styl 

 下の例では、`paths`オプションを用いてStylusに追加のパスを指定しています。`./mixins`がStylusの`paths`オプションに追加されているので、`./test.styl`のなかで、`@import "mixins/border-radius"`と`@import "border-radius"`のどちらでも用いることができます。

      /**
       * Module dependencies.
       */

      var stylus = require('../')
        , str = require('fs').readFileSync(__dirname + '/test.styl', 'utf8');

      var paths = [
          __dirname
        , __dirname + '/mixins'
      ];

      stylus(str)
        .set('filename', __dirname + '/test.styl')
        .set('paths', paths)
        .render(function(err, css){
          if (err) throw err;
          console.log(css);
        });

### JavaScript Import API

 `.import(path)`メソッドを使うとき、このファイルインポートは展開時に遅延実行されます。

       var stylus = require('../')
         , str = require('fs').readFileSync(__dirname + '/test.styl', 'utf8');

       stylus(str)
         .set('filename', __dirname + '/test.styl')
         .import('mixins/vendor')
         .render(function(err, css){
         if (err) throw err;
         console.log(css);
       });

 次の宣言は...

     @import 'mixins/vendor'

...次と同様です...

     .import('mixins/vendor') 
 
