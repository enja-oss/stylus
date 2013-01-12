 +  元文書: [stylus/docs/selectors.html at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub]
(https://github.com/LearnBoost/stylus/blob/f9ed220d7e5f0b44aeaca58ffd6490566b5f0757/docs/selectors.html 
"stylus/docs/selectors.html at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub")

## セレクタ [原文](http://learnboost.github.com/stylus/docs/selectors.html)

### インデント

Stylusは"Pythonic（パイソニック）"（すなわち、インデントベース）です。空白は重要であり、下のように _インデント_ と _アウトデント_の際に、 `{` と　`}`　の代わりとなります。

    body
      color white

コンパイル後:

    body {
      color: #fff;
    }

好みであれば、コロンをプロパティと値のセパレータとして使うことも出来ます。

    body
      color: white

### ルールセット

Stylusは、ちょうどCSSのようにカンマで区切ることで、一度に複数のセレクタをプロパティとして定義することが出来ます。

    textarea, input
      border 1px solid #eee

改行で同じことが行えます。

    textarea
    input
      border 1px solid #eee

コンパイル後:

    textarea,
    input {
      border: 1px solid #eee;
    }

**このルールの唯一の例外は** プロパティのように見えるセレクタです。例えば、次のように `foo bar baz` はプロパティ **もしくは** セレクタかもしれません。

    foo bar baz
    > input
      border 1px solid

その理由から(もしくは、単純な好みとして)、カンマを後ろに付与することもできます。

    foo bar baz,
    form input,
    > a
      border 1px solid

### 親参照

`&` 文字は親セレクタの参照です（複数可）。
下の例では2つのセレクタ（`textarea` と`input`） 両方の`color` を`:hover` 擬似セレクタで変更しています。

    textarea
    input
      color #A7A7A7
      &:hover
        color #000

コンパイル後:

    textarea,
    input {
      color: #a7a7a7;
    }
    textarea:hover,
    input:hover {
      color: #000;
    }

次は、mixinの中で親参照を利用しながらInternet Exploder向けに単純な `2px` ボーダーを提供する例です。

      box-shadow()
        -webkit-box-shadow arguments
        -moz-box-shadow arguments
        box-shadow arguments
        html.ie8 &,
        html.ie7 &,
        html.ie6 &
          border 2px solid arguments[length(arguments) - 1]

      body
        #login
          box-shadow 1px 1px 3px #eee

結果:

      body #login {
        -webkit-box-shadow: 1px 1px 3px #eee;
        -moz-box-shadow: 1px 1px 3px #eee;
        box-shadow: 1px 1px 3px #eee;
      }
      html.ie8 body #login,
      html.ie7 body #login,
      html.ie6 body #login {
        border: 2px solid #eee;
      }

### 曖昧さ回避

例えば `padding - n` の様な表現は、減算操作、もしくは単行マイナスを持つプロパティの両方として解釈できます。曖昧さをなくすために、括弧で式を囲みます。

    pad(n)
      padding (- n)

    body
      pad(5px)

コンパイル後:

    body {
      padding: -5px;
    }

しかしながら、これはfunctionsの中でのみ有効です（これは、functionsがmixinとしても、または戻り値を持つ呼び出しのとしても振る舞うためです）。

例えば、以下でも問題ありません(上の結果と同じになります。)

    body
      padding -5px

Stylusが処理できない特殊なプロパティ値がある？ その場合は `unquote()` が助けてくれます。

    filter unquote('progid:DXImageTransform.Microsoft.BasicImage(rotation=1)')

結果:

    filter progid:DXImageTransform.Microsoft.BasicImage(rotation=1)
    
