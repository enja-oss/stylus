 +  元文書: [stylus/docs/selectors.html at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub]
(https://github.com/LearnBoost/stylus/blob/f9ed220d7e5f0b44aeaca58ffd6490566b5f0757/docs/selectors.html 
"stylus/docs/selectors.html at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub")

## Selectors 

## セレクタ [原文](http://learnboost.github.com/stylus/docs/selectors.html)

### Indentation

### インデント

Stylus is "pythonic" (i.e. indentation-based). Whitespace is significant, so we substitute `{` and `}` with an _indent_, and an _outdent_ as shown below:

Stylusは"Pythonic（パイソニック）"（すなわち、インデントベース）です。空白は重要であり、下のように _インデント_ と _アウトデント_の際に、 `{` と　`}`　の代わりとなります。

    body
      color white

Which compiles to:

コンパイル後:

    body {
      color: #fff;
    }

If preferred, you can use colons to separate properties and values:

好みであれば、コロンをプロパティと値のセパレータとして使うことも出来ます。

    body
      color: white

### Rule Sets

### ルールセット

Stylus, just like CSS, allows you to define properties for several selectors at once through comma separation.

Stylusは、ちょうどCSSのようにカンマで区切ることで、一度に複数のセレクタをプロパティとして定義することが出来ます。

    textarea, input
      border 1px solid #eee

The same can be done with a newline:

改行で同じことが行えます。

    textarea
    input
      border 1px solid #eee

Both compile to:

コンパイル後:

    textarea,
    input {
      border: 1px solid #eee;
    }

**The only exception to this rule** are selectors that look like properties. For example, the following `foo bar baz` might be a property **or** a selector:

**このルールの唯一の例外は** プロパティのように見えるセレクタです。例えば、次のように `foo bar baz` はプロパティ **もしくは** セレクタかもしれません。

    foo bar baz
    > input
      border 1px solid

So for this reason (or simply if preferred), we may trail with a comma:

だからこのような理由で（もしくは、単純に好みとして）、カンマで引きずることがあります。

    foo bar baz,
    form input,
    > a
      border 1px solid

### Parent Reference

### 親参照

The `&` character references the parent selector(s). In the example below our two selectors (`textarea` and `input`) both alter the `color` on the `:hover` pseudo selector. 

`&` 文字は親セレクタの参照です（複数可）。
下の例では2つのセレクタ（`textarea` と`input`） 両方の`color` を`:hover` 擬似セレクタで変更しています。

    textarea
    input
      color #A7A7A7
      &:hover
        color #000

Compiles to:

コンパイル後:

    textarea,
    input {
      color: #a7a7a7;
    }
    textarea:hover,
    input:hover {
      color: #000;
    }

Below is an example providing a simple `2px` border for Internet Exploder utilizing the parent reference within a mixin:

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

Yielding:

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

### Disambiguation

### 曖昧さ回避

Expressions such as `padding - n` could be interpreted both as a subtraction operation, as well as a property with an unary minus. To disambiguate, wrap the expression with parens:

例えば `padding - n` の様な表現は、減算操作、もしくは単行マイナスを持つプロパティの両方として解釈できます。曖昧さをなくすために、括弧で式を囲みます。

    pad(n)
      padding (- n)

    body
      pad(5px)

Compiles to:

コンパイル後:

    body {
      padding: -5px;
    }

However, this is only true in functions (since functions act both as mixins, or calls with return values). 

しかしながら、これはfunctionsの中でのみ有効です（これは、functionsがmixinまたは戻り値を持つ呼び出しの両方として作用するためです）。

For example, the following is fine (and yields the same results as above):

次の例は優れています。（上の結果と同じになります。）

    body
      padding -5px

Have weird property values that Stylus can't process? `unquote()` can help you out:

Stylusが処理できない特殊なプロパティ値を持つものは。 `unquote()` が助けてくれます。

    filter unquote('progid:DXImageTransform.Microsoft.BasicImage(rotation=1)')

Yields:

結果:

    filter progid:DXImageTransform.Microsoft.BasicImage(rotation=1)
    
