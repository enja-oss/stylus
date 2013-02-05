 + 元文書: [stylus/docs/js.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 ? LearnBoost/stylus ? GitHub](https://github.com/LearnBoost/stylus/blob/0ab9219d80a5304e32437ef3cabb7b3fa1345534/docs/js.md "stylus/docs/js.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 ? LearnBoost/stylus ? GitHub")

## JavaScript API [原文](http://learnboost.github.com/stylus/docs/js.html)

Simply `require` the module, and call `render()` with the given string of Stylus code, and (optional) `options` object. 

シンプルな `require` モジュールです。引数の文字列にはStylusコードと、`options` オブジェクト（任意）を渡し、`render()` で呼び出します。

Frameworks utilizing Stylus should pass the `filename` option to provide better error reporting.

フレームワークにおいてStylusを活用するためには、より良いエラーレポートを提供する `filename` オプションを渡すべきです。

    var stylus = require('stylus');

    stylus.render(str, { filename: 'nesting.css' }, function(err, css){
      if (err) throw err;
      console.log(css);
    });

We can also do the same thing in a more progressive manner:

プログレッシブ（より革新的）な方法で行うと次のようになります。

    var stylus = require('stylus');

    stylus(str)
      .set('filename', 'nesting.css')
      .render(function(err, css){
        // logic
      });

### .set(setting, value)

 Apply a setting such as a `filename`, or import `paths`:

 `filename` やインポートする `paths` の設定などを適用します。
 
     .set('filename', __dirname + '/test.styl')
     .set('paths', [__dirname, __dirname + '/mixins'])

### .include(path)

  A progressive alternative to `.set('paths',...)` is `.include()`.  This is ideal when exposing external Stylus libraries which expose a path.

  `.set('paths',...)` をプログレッシブな方法で置き換えたものが、 `.include()` です。
  これは、依存する外部のStylusライブラリについてのパスを明示するために使用することが一般的です。

    stylus(str)
      .include(require('nib').path)
      .include(process.env.HOME + '/mixins')
      .render(...)

### .import(path)

Defer importing of the given `path` until evaluation is performed. The example below is essentially the same as doing `@import 'mixins/vendor'` within your Stylus sheet.

`path` で渡された対象は、評価が開始されるまでインポートが遅延されます。
下の例で、スタイルシート内で行われていることは `@import 'mixins/vendor'` と基本的に同じです。

      var stylus = require('../')
        , str = require('fs').readFileSync(__dirname + '/test.styl', 'utf8');

      stylus(str)
        .set('filename', __dirname + '/test.styl')
        .import('mixins/vendor')
        .render(function(err, css){
        if (err) throw err;
        console.log(css);
      });

### .define(name, node)

 By passing a `Node`, we may define a global variable. This is useful when exposing conditional features within your library depending on the availability of another. For example the **Nib** extension library conditionally supports node-canvas, providing image generation. 

 `Node` に渡すことで、グローバル変数を定義することができます 。
 これは、外部からの条件（Nodeに渡したグローバル変数）に応じて、ライブラリの中で機能を公開したりする場合に利用します。 
 例えば、拡張ライブラリの **Nib** は、「node-canvas」の条件により画像生成機能を提供するか決定します。
 
 However, this is not always available, so Nib may define:
 
 しかし、これは常に利用できるとは限りません。そのためNibはこのように定義します。
 
     .define('has-canvas', stylus.nodes.false);
     .define('some-setting', new stylus.nodes.String('some value'));

 Stylus also casts JavaScript values to their Stylus equivalents when possible. Here are a few examples:

 可能な場合、StylusではJavascriptの値をStylusでの同等な値にキャストします。ここに幾つかの例を示します。

     .define('string', 'some string')
     .define('number', 15.5)
     .define('some-bool', true)
     .define('list', [1,2,3])
     .define('list', [1,2,[3,4,[5,6]]])
     .define('list', { foo: 'bar', bar: 'baz' })
     .define('families', ['Helvetica Neue', 'Helvetica', 'sans-serif'])

  These same rules apply to return values in js functions as well:

  これらと同じルールがJS関数（コールバック）の戻り値にも適用されます。

     .define('get-list', function(){
       return ['foo', 'bar', 'baz'];
     })

### .define(name, fn)

 This method allows you to provide a JavaScript-defined function to Stylus. Think of these as you would JavaScript-to-C++ bindings. When there's something you cannot do in Stylus, define it in JavaScript!

 この方法により、StylusにJavascriptで定義された関数を提供することができます。
 これは、JavascriptからC++へのバインディングのように考えることができます。
 Stylusにて行うことができない何かがある場合、Javascriptでそれを定義する！

In this example, we define four functions: `add()`, `sub()`, `image-width()`, and `image-height()`. These functions must return a `Node`, this constructor and the other nodes are available via `stylus.nodes`.

この例では、`add()`, `sub()`, `image-width()`, `image-height()` の4つの関数を定義しています。
これらの関数は`Node`を返す必要があります。このコンストラクタやその他のnodeは `stylus.nodes` を介して利用できます。

      var stylus = require('../')
        , nodes = stylus.nodes
        , utils = stylus.utils
        , fs = require('fs');

      function add(a, b) {
        return a.operate('+', b);
      }

      function sub(a, b) {
        return a.operate('-', b);
      }

      function imageDimensions(img) {
        // assert that the node (img) is a String node, passing
        // the param name for error reporting
        utils.assertType(img, 'string', 'img');
        var path = img.val;

        // Grab bytes necessary to retrieve dimensions.
        // if this was real you would do this per format,
        // instead of reading the entire image :)
        var data = fs.readFileSync(__dirname + '/' + path);

        // GIF
        // of course you would support.. more :)
        if ('GIF' == data.slice(0, 3).toString()) {
          var w = data.slice(6, 8)
            , h = data.slice(8, 10);
          w = w[1] << 8 | w[0];
          h = h[1] << 8 | h[0];
        }

        return [w, h];
      }

      function imageWidth(img) {
        return new nodes.Unit(imageDimensions(img)[0]);
      }

      function imageHeight(img) {
        return new nodes.Unit(imageDimensions(img)[1]);
      }

      stylus(str)
        .set('filename', 'js-functions.styl')
        .define('add', add)
        .define('sub', sub)
        .define('image-width', imageWidth)
        .define('image-height', imageHeight)
        .render(function(err, css){
          if (err) throw err;
          console.log(css);
        });

 For further reference (until documentation is complete) please see the following files:

 （ドキュメントが完成するまでは）参考に以下のファイルを参照してください。
 
   - `lib/nodes/*`
   - `lib/utils.js`

### .use(fn)

  When called, the given `fn` is invoked with the renderer, allowing all of the methods above to be used. This allows for plugins to easily expose themselves, defining functions, paths etc.

  渡された `fn` は、render()で呼び出されます。
  呼び出された場合、上記すべてのAPI関数を利用することができます。
  これにより、プラグインは定義している関数、パスなど、自分自身を簡単に公開することができます。

    var mylib = function(style){
      style.define('add', add);
      style.define('sub', sub);
    };

    stylus(str)
      .use(mylib)
      .render(...)
