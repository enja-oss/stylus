+ 元文書: [stylus/docs/comments.md at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub](https://github.com/LearnBoost/stylus/blob/f9ed220d7e5f0b44aeaca58ffd6490566b5f0757/docs/comments.md)

## Comments

## コメント [原文](http://learnboost.github.com/stylus/docs/comments.html)

  Stylus supports three kinds of comments: single-line, and multi-line comments, and multi-line buffered comments.

  Stylus は次の3 種類のコメントに対応しています: 1 行コメントと、複数行コメントと、複数行バッファコメントです。

## Single-line

## 1 行コメント

  Single-line comments look like JavaScript comments, and do not output in the resulting CSS:

  1 行コメントは JavaScript コメントのような形で、結果の CSS には出力されません:

    // I'm a comment!
    body
      padding 5px // some awesome padding

    // 私がコメントだ。
    body
      padding 5px // ちょっとしたスゴい padding

## Multi-line

## 複数行コメント

   Multi-line comments look identical to regular CSS comments. However, they only output when the `compress` option is not enabled.

   複数行コメントは普通の CSS コメントと同一です。しかし、`compress` オプションが有効でない場合のみ出力されます。

    /*
     * Adds the given numbers together.
     */
    
    add(a, b)
      a + b

    /*
     * 与えられた数を足算
     */
    
    add(a ,b)
      a + b

## Multi-line buffered

## 複数行バッファコメント

   Multi-line comments which are not suppressed start with `/*!`. This tells Stylus to output the comment regardless of compression.

   複数行コメントの出力が抑制されない場合は `/*!` で始まります。この場合 Stylus は圧縮に関係なくコメントを出力します。

    /*!
     * Adds the given numbers together.
     */
    
    add(a, b)
      a + b

    /*!
     * 与えられた数を足算
     */
    
    add(a, b)
      a + b
