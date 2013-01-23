+ 元文書: [stylus/docs/font-face.md at master · LearnBoost/stylus · GitHub](https://github.com/LearnBoost/stylus/blob/master/docs/font-face.md "stylus/docs/font-face.md at master · LearnBoost/stylus · GitHub")

## @font-face [原文](http://learnboost.github.com/stylus/docs/font-face.html)

 `@font-face`@ルールは期待通りに動作します。次のように、単にプロパティのブロックを付け加えるだけです:
 
 
     @font-face
       font-family Geo
       font-style normal
       src url(fonts/geo_sans_light/GensansLight.ttf)

     .ingeo
       font-family Geo

これは次のように展開されます:


      @font-face {
        font-family: Geo;
        font-style: normal;
        src: url("fonts/geo_sans_light/GensansLight.ttf");
      }
      .ingeo {
        font-family: Geo;
      }

