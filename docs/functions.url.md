+ 元文書: [stylus/docs/functions.url.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 · LearnBoost/stylus · GitHub]
(https://github.com/LearnBoost/stylus/blob/0ab9219d80a5304e32437ef3cabb7b3fa1345534/docs/functions.url.md 
"stylus/docs/functions.url.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 · LearnBoost/stylus · GitHub")

## 画像データURLのインライン化 [原文](http://learnboost.github.com/stylus/docs/functions.url.html)

Stylusは `url()` という、CSSの `url()` リテラルを変換するためのオプション機能を持っています
（内部では条件付きでbase64エンコード [Data URIs](http://en.wikipedia.org/wiki/Data_URI_scheme)を使用しています。）。

### 例

関数自体は `require('stylus').url` 経由で利用可能です。
`options` オブジェクトを引数で受け入れて、Stylusが `url()` を参照する際に、（内部で）呼び出される関数を返します。

`.define(name, callback)` メソッドは、Stylusのソースから呼び出すことができるよう割り当てられたJavaScriptの関数です。
この場合、画像ファイルが `./css/images` にあるため、`paths` オプションを無視できます
（デフォルトでは、画像の検索はレンダリングされているファイルから相対的に行われます。）。
しかし、必要に応じてこの動作は変更できます。

      stylus(str)
        .set('filename', __dirname + '/css/test.styl')
        .define('url', stylus.url())
        .render(function(err, css){
    
        });

例えば、画像ファイルが`./public/images` に存在すると仮定します。
`url(images/tobi.png)` を使いたい場合、画像検索のプロセスの一部となるように、publicディレクトリを `paths` に渡します。

代わりに `url(tobi.png)` を使う場合は、同様に `paths: [__dirname + '/public/images']` を渡します。

      stylus(str)
        .set('filename', __dirname + '/css/test.styl')
        .define('url', stylus.url({ paths: [__dirname + '/public'] }))
        .render(function(err, css){
          
        });
  
### Options

  - `limit` バイトサイズ制限。デフォルトは30Kb (30000)
  - `paths` 画像パス（複数可）
