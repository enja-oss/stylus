+ 元文書: [stylus/docs/functions.url.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 · LearnBoost/stylus · GitHub]
(https://github.com/LearnBoost/stylus/blob/0ab9219d80a5304e32437ef3cabb7b3fa1345534/docs/functions.url.md 
"stylus/docs/functions.url.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 · LearnBoost/stylus · GitHub")

## Data URI Image Inlining

## 画像データURLのインライン化 [原文](http://learnboost.github.com/stylus/docs/functions.url.html)

Stylus is bundled with an optional function named `url()`, which replaces the literal `url()` calls (and conditionally inlines them using base64 [Data URIs](http://en.wikipedia.org/wiki/Data_URI_scheme)).

Stylusは `url()` という、`url()` 呼び出し時にリテラルを変換するためのオプション機能を持っています
（内部では条件付きでbase64エンコード [Data URIs](http://en.wikipedia.org/wiki/Data_URI_scheme)を使用しています。）。

### Example

### 例

The function itself is available via `require('stylus').url`. It accepts an `options` object, returning a function that Stylus calls internally when it sees `url()`.

関数自体は `require('stylus').url` 経由で利用可能です。
`options` オブジェクトを引数で受け入れて、Stylusが `url()` を参照する際に、（内部で）呼び出される関数を返します。

The `.define(name, callback)` method assigned a JavaScript function that can be called from Stylus source. In this case, since our images are in `./css/images`,  we can ignore the `paths` option (by default image lookups are performed relative to the file being rendered).  But if desired, this behavior can be altered:

`.define(name, callback)` メソッドは、Stylusのソースから呼び出すことができるよう割り当てられたJavaScriptの関数です。
この場合、画像ファイルが `./css/images` にあるため、`paths` オプションを無視できます
（デフォルトでは、画像の検索はレンダリングされているファイルから相対的に行われます。）。
しかし、必要に応じてこの動作は変更できます。

      stylus(str)
        .set('filename', __dirname + '/css/test.styl')
        .define('url', stylus.url())
        .render(function(err, css){
    
        });

For example, imagine our images live in `./public/images`.  We want to use `url(images/tobi.png)`.  We could pass `paths` our public directory, so that it becomes part of the lookup process. 

例えば、画像ファイルが`./public/images` に存在すると仮定します。
`url(images/tobi.png)` を使いたい場合、画像検索のプロセスの一部となるように、publicディレクトリを `paths` に渡します。

Likewise, if instead we wanted `url(tobi.png)`, we could pass `paths: [__dirname + '/public/images']`.

代わりに `url(tobi.png)` を使う場合は、同様に `paths: [__dirname + '/public/images']` を渡します。

      stylus(str)
        .set('filename', __dirname + '/css/test.styl')
        .define('url', stylus.url({ paths: [__dirname + '/public'] }))
        .render(function(err, css){
          
        });
  
### Options

  - `limit` bytesize limit defaulting to 30Kb (30000)
  - `paths` image resolution path(s)

  - `limit` バイトサイズ制限。デフォルトは30Kb (30000)
  - `paths` 画像パス（複数可）
