
+ 元文書: [stylus/docs/middleware.md at 0a3a8add79880b5f96ef680ea8f30786c17c3233 · LearnBoost/stylus · GitHub](https://github.com/LearnBoost/stylus/blob/0a3a8add79880b5f96ef680ea8f30786c17c3233/docs/middleware.md "stylus/docs/middleware.md at 0a3a8add79880b5f96ef680ea8f30786c17c3233 · LearnBoost/stylus · GitHub")

## Connect ミドルウェア [原文](http://learnboost.github.com/stylus/docs/middleware.html)

 Stylusはファイル変更を検知して自動でコンパイルできる、Connectミドルウェアを備えています。

## stylus.middleware(options)

 与えられた`options`で初期化された、Connectミドルウェアを返します。

#### オプション(options)

      `force`     __true__ の場合、常にコンパイルし直します
      `src`       .stylファイルを検索するソースディレクトリです
      `dest`      .cssファイルを出力するためのディレクトリです
                  undefined の場合は`src`の設定値が使われます
      `compress`  出力の.cssファイルを圧縮するかどうかを指定します
      `compile`   カスタムのコンパイル関数です。引数、`(str, path)`を受け取り、
                  レンダラを返します。
      `firebug`   出力CSSにデバッグ情報を付加します。
                  この情報はFireStylus Firebugプラグインで利用できます。
      `linenos`   出力CSSに、対応するStylusファイルの行番号を示す
                  コメントを付加します。

#### 使用例
 
 ここでは、設定を追加することでレンダラーの動作を変更するために、カスタムのコンパイル関数をセットアップしてみます。

 デフォルトではコンパイル関数は単純に`filename`を設定し、CSSを書き出します。

        function compile(str, path) {
          return stylus(str)
            .import(__dirname + '/css/mixins/blueprint')
            .import(__dirname + '/css/mixins/css3')
            .set('filename', path)
            .set('warn', true)
            .set('compress', true);
        }
 
 ミドルウェアをConnectに渡し、`.styl`ファイルをあるディレクトリから読み込み、変換済みの`.css`ファイルを _./public_ に保存するように設定してみましょう。
 ここでも、独自の`compile`関数を用います。

 こうすることで、Stylusによって出力された`.css`ファイルを配信する`static`レイヤを作ることができます。
 
        var stylus = require('stylus');
 
        var app = connect();

        app.use(stylus.middleware({
            src: __dirname
          , dest: __dirname + '/public'
          , compile: compile
        }));

        app.use(connect.static(__dirname + '/public'));

 オプション、`force`が指定されると、スタイルファイルは無条件で再コンパイルされます。このオプションが設定されなかったとしても、Stylusミドルウェアは`@import`されたファイルを検出できます。

 開発目的のために、`firebug`オプションを有効にすることで[FireStylus extension for Firebug](http://github.com/LearnBoost/stylus/blob/master/docs/firebug.md)を使うことができます。これとは別に、`lineos`オプションを有効にして、生成されたCSSにデバッグ情報を付加することもできます。

        function compile(str, path) {
          return stylus(str)
            .import(__dirname + '/css/mixins/blueprint')
            .import(__dirname + '/css/mixins/css3')
            .set('filename', path)
            .set('warn', true)
            .set('firebug', true)
            .set('linenos', true);
        }
