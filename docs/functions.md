+ 元文書: [stylus/docs/functions.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 · LearnBoost/stylus · GitHub](https://github.com/LearnBoost/stylus/blob/0ab9219d80a5304e32437ef3cabb7b3fa1345534/docs/functions.md "stylus/docs/functions.md at 0ab9219d80a5304e32437ef3cabb7b3fa1345534 · LearnBoost/stylus · GitHub")

## 関数 [原文](http://learnboost.github.com/stylus/docs/functions.html)

 Stylusにはパワフルな関数の仕組みが用意されています。関数の定義はミックスインと同じですが、値を返せるところがミックスインと異なります。

### 返り値

 簡単な例から始めてみましょう: ２つの値を加算する関数を作ってみます。

    add(a, b)
      a + b

 この関数は、条件文やプロパティの値などに用いることができます。
 
     body
       padding add(10px, 5)

 これは次のように展開されます:
     
     body {
       padding: 15px;
     }

### デフォルト引数

 オプションの引数は与えられた値をデフォルトにとります。Stylusでは、前方の引数にもデフォルト値を設定出来ます!
 
 例えば:
 
     add(a, b = a)
       a + b

     add(10, 5)
     // => 15
     
     add(10)
     // => 20

**注記:** デフォルト引数の定義は通常の代入なので、その定義の中で関数を使うことも可能です:

     add(a, b = unit(a, px))
       a + b

### 関数の定義

 さらに、シンプルな`add()`関数を見て行きましょう。組み込み関数である`unit()`を用いてすべての値の単位が`px`になるようにします。これによりそれぞれの引数が再代入され、単位を示す文字列が付け加えられます。単位の変換は行われません。
 
     add(a, b = a)
       a = unit(a, px)
       b = unit(b, px)
       a + b

     add(15%, 10deg)
     // => 25

### 複数の返り値

 Stylusでは、関数は複数の値を返すことができます。また、ひとつの変数に複数の値を代入することも同様に可能です。
 
 例えば、次の代入は正しく実行されます:
 
       sizes = 15px 10px
     
       sizes[0]
       // => 15px 

同様に、関数で複数の値を返す事もできます:

       sizes()
         15px 10px

       sizes()[0]
       // => 15px

返却値が識別子であるときは例外となります。例えば、以下の例には演算子が存在しないため、プロパティに値を代入しているとしてStylusに認識されてしまいます。

     swap(a, b)
       b a

このようなときは複数の値を返すことを明確にするため、括弧で２つの値を囲みましょう。`return`キーワードを使うこともできます:

      swap(a, b)
        (b a)

      swap(a, b)
        return b a

### 条件文

 引数で渡された値が文字列に変換可能か確かめるために`stringish()`という関数を定義することを考えてみましょう。`val`が文字列であるかident(文字列のようなデータ)であるかを判別します。未定義の識別子は展開される際にそれ自身の値と置き換わるので、次で示すように、それらをそれら自身と比較すれば良ことになります（ここで、`yes`と`no`は、それぞれ`true`と`false`を意味します）。
 
     stringish(val)
       if val is a 'string' or val is a 'ident'
         yes
       else
         no

使い方:

     stringish('yay') == yes
     // => true
   
     stringish(yay) == yes
     // => true
   
     stringish(0) == no
     // => true

__注意__: `yes`と`no`はブーリアン型のリテラルではなく、単に未定義の識別子であることに注意してください。

別の例:

    compare(a, b)
      if a > b
        higher
      else if a < b
        lower
      else
        equal

使い方:

    compare(5, 2)
    // => higher

    compare(1, 5)
    // => lower

    compare(10, 10)
    // => equal

### エイリアス

  関数のエイリアスを作るには、単に関数の名前を新しい識別子に代入してください。例えば、次のようにすれば`add()`関数を`plus()`関数としてエイリアスを作ることができます:
  
      plus = add
      
      plus(1, 2)
      // => 3

### 関数を引数にとる関数

  関数の"エイリアスを作る"ことが可能なのと同じで、関数を関数の引数として渡すことも可能です。次の例では`invoke()`関数は関数を引数に取るので`add()`や`sub()`を渡すことができます。

    invoke(a, b, fn)
      fn(a, b)

    add(a, b)
      a + b

    body
      padding invoke(5, 10, add)
      padding invoke(5, 10, sub)

これは次のように展開されます:

    body {
      padding: 15;
      padding: -5;
    }

### argumentsローカル変数

 `arguments`ローカル変数がすべての関数の定義のなかで利用できます。この変数は関数に渡されたすべての引数を格納しています。
 
 例:
 
     sum()
       n = 0
       for num in arguments
         n = n + num

     sum(1,2,3,4,5)
     // => 15

### ハッシュの例

 次の例では、`get(hash, key)`関数を定義しています。これはハッシュのキー値`key`をもとにその内容を返す(もしキー値に対応する値が見つからなければ`null`を返す)関数です。この関数の中では、`hash`のなかのそれぞれのペア`pair`を走査し、ペアの一つ目の値と`key`がマッチすればそのペアの２つ目のノードを返すということを行なっています。

      get(hash, key)
        return pair[1] if pair[0] == key for pair in hash

次で示すように、言語組み込みの関数がStylusの強力な式と一緒に使われると、素晴らしい柔軟性を発揮します。
      
      hash = (one 1) (two 2) (three 3)
      
      get(hash, two)
      // => 2

      get(hash, three)
      // => 3

      get(hash, something)
      // => null
