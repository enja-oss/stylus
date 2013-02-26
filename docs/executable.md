
## Stylus コマンド

StylusにはStylus形式のファイルをCSSに変換するための`stylus`コマンドが付属しています。

      Usage: stylus [options] [command] [< in [> out]]
                    [file|dir ...]

      Commands:

        help <prop>     Opens help info for <prop> in
                        your default browser. (OS X only)

      Options:

        -u, --use <path>        Utilize the stylus plugin at <path>
        -i, --interactive       Start interactive REPL
        -w, --watch             Watch file(s) for changes and re-compile
        -o, --out <dir>         Output to <dir> when passing files
        -C, --css <src> [dest]  Convert CSS input to Stylus
        -I, --include <path>    Add <path> to lookup paths
        -c, --compress          Compress CSS output
        -d, --compare           Display input along with output
        -f, --firebug           Emits debug infos in the generated css that
                                can be used by the FireStylus Firebug plugin
        -l, --line-numbers      Emits comments in the generated CSS
                                indicating the corresponding Stylus line
        -V, --version           Display the version of Stylus
        -h, --help              Display help information

### 標準入出力(STDIO)からのコンパイル例

 `stylus`コマンドは _stdin_ から入力を読み込み、 _stdout_ から出力します。例として、次のコマンドラインでStylusをCSSに変換出来ます:

      $ stylus --compress < some.styl > some.css

ターミナルの中でStylusを試してみましょう！ 以下のコマンドラインを打ち込み、`__EOF__`を入力するために`CIRL-D`を打ち込みます。

      $ stylus
      body
        color red
        font 14px Arial, sans-serif

### ファイルのコンパイル例

 `stylus`コマンドはファイル名やディレクトリ名も受け取ることができます。例えば、`css`という名前のディレクトリを指定するとそのディレクトリのStylusファイルを`.css`に変換して同じディレクトリのファイルに出力します。
 
      $ stylus css

  次の例では`./public/stylesheets`に出力します:

      $ stylus css --out public/stylesheets

  複数のファイルを指定することもできます:

      $ stylus one.styl two.styl

  開発作業に用いるために、`lineos`オプションを使うとオリジナルのStylusファイル名と行番号を出力CSSに付加します。

      $ stylus --line-numbers <path>

  また、[FireStylus Firebug拡張機能](//github.com/LearnBoost/stylus/blob/master/docs/firebug.md)用に、`firebug`オプションを利用できます。

      $ stylus --firebug <path>

### CSSをStylusに変換する

 CSSを簡潔なStylus形式に変換したい場合、`--css`オプションを使います。

 標準入出力を使う場合:
 
      $ stylus --css < test.css > test.styl

 `.styl`拡張子の同一ファイル名に出力:
 
      $ stylus --css test.css

 指定した場所に保存する:
 
      $ stylus --css test.css /tmp/out.styl

### CSSプロパティのヘルプ

  OS Xでは、`stylus help <prop>`コマンドで、CSSプロパティ`<prop>`のヘルプをデフォルトブラウザで開くことができます。

    $ stylus help box-shadow

### 対話シェル

 StylusのREPL(Read-Eval-Print-Loop)（対話シェル）では、端末から直接Stylusの式を評価出来ます。
 
 **対話シェルは式だけに使えるということに注意してください**（つまり、CSSセレクターなどを評価することはできません）。対話シェルを使うには`-i`、または`--interactive`フラグを使います。
 
     $ stylus -i
     > color = white
     => #fff
     > color - rgb(200,50,0)
     => #37cdff
     > color
     => #fff
     > color -= rgb(200,50,0)
     => #37cdff
     > color
     => #37cdff
     > rgba(color, 0.5)
     => rgba(55,205,255,0.5)

### Utilizing Plugins プラグインを活用する

 例として、[nib](https://github.com/visionmedia/nib) Stylusプラグインを用いてコマンドの使い方を説明します。
 
 下のようなStylusがあったとします。これは、nibに含まれる`linear-gradient()`関数を用いるために、nibのインポートを行なっています。
 
     @import 'nib'

     body
       background: linear-gradient(20px top, white, black) 

 まず、標準入出力を通して`stylus(1)`コマンドを用いると下のようになります:
 
     $ stylus < test.styl

 しかし、これは次のエラーを発生させます(このエラーはStylusがnibをどこからロードすればよいか知らないために起こります)。

       Error: stdin:3
          1| 
          2| 
        > 3| @import 'nib'
          4| 
          5| body
          6|   background: linear-gradient(20px top, white, black)

  Stylusで実装されたAPIをプラグインで提供するために、Stylusのライブラリ検索パスに新しいパスを加えることができます。`--include`か、`-I`フラグを使うことで可能です:

     $ stylus < test.styl --include ../nib/lib

  上記のコマンドによって以下が出力されます。(お気づきのとおり、`gradient-data-uri()`と`create-gradient-image()`はそのまま出力されています。これは、プラグインがJavaScriptで実装されたAPIを提供する場合にはライブラリのパスを示すだけでは十分でないためです。ただし、nibの、Stylusだけで実装された関数だけを使う分にはこれで十分です。)

      body {
        background: url(gradient-data-uri(create-gradient-image(20px, top)));
        background: -webkit-gradient(linear, left top, left bottom, color-stop(0, #fff), color-stop(1, #000));
        background: -webkit-linear-gradient(top, #fff 0%, #000 100%);
        background: -moz-linear-gradient(top, #fff 0%, #000 100%);
        background: linear-gradient(top, #fff 0%, #000 100%);
      }

  これを解決するには、`--use`、`-u`フラグを使う必要があります。このフラグにはオプションとしてnodeモジュールへのパスを渡します(`.js`拡張子の有無は問いません)。これは`require()`でモジュールをロードするので、関数は`module.exports`として外部公開されている必要があります。また、関数が`require()`でロードされたあとに、プラグインを外部公開(js関数を定義するなど)するために`style.use(fn())`がコールされます(訳注:`fn`が対象関数)。
  
    $ stylus < test.styl --use ../nib/lib/nib

 これは次のように展開されます:

    body {
      background: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAAUCAYAAABMDlehAAAABmJLR0QA/wD/AP+gvaeTAAAAI0lEQVQImWP4+fPnf6bPnz8zMH358oUBwkIjKJBgYGNj+w8Aphk4blt0EcMAAAAASUVORK5CYII=");
      background: -webkit-gradient(linear, left top, left bottom, color-stop(0, #fff), color-stop(1, #000));
      background: -webkit-linear-gradient(top, #fff 0%, #000 100%);
      background: -moz-linear-gradient(top, #fff 0%, #000 100%);
      background: linear-gradient(top, #fff 0%, #000 100%);
    }
  
