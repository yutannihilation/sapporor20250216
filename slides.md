---
title: ggplot2 3.6.0に備えよう
format:
  gfm:
    variant: +yaml_metadata_block
theme: default
background: /background.png
class: text-center
drawings:
  persist: false
mdc: true
---


# ggplot2<br/>3.6.0<br/>に備えよう

2025/02/16 SappoRo.R#12

@yutannihilation

---
layout: image-right
image: "/icon.jpg"
---

# ドーモ！

## 名前:

湯谷啓明 (@yutannihilation)

## 好きな言語:

R、Rust、忍殺語

## 近況:

今日で無職おわりです

---
layout: default
---

# ggplot2 3.6.0

<v-clicks>

- 今年5月頃？
- 100年に1度の出来と言われた2024年を超す21世紀最高の出来栄え（ボジョレーヌーボー風）
- つまり、けっこう変更がある

</v-clicks>

---
layout: default
---

# けっこう変更がある

- <https://ggplot2.tidyverse.org/dev/news/index.html>

---
layout: default
---

# Theme の強化

- `theme(geom = )`
- `ink` と `paper`
- `from_theme()`
- `theme(palette = )`
- marquee 関連の機能

---
layout: default
---

# これまで

- だいたいのスタイルは `theme()` で設定できる
- と言いつつ「ただし、」が多かった

---
layout: default
---

# 「ただし」の例

- `colour` や `fill` の scale は `options()`

  ``` r
  options(
    ggplot2.continuous.colour = "viridis",
    ggplot2.continuous.fill = "viridis"
  )
  ```

- `Geom` の点や線の大きさ、色などは `update_geom_defaults()`

  ``` r
  update_geom_defaults("point", aes(color = "red"))
  ```

---
layout: default
---

# 「ただし」の例

- `theme(text = )` の指定は `geom_text()` などには引き継がれない
- `theme(text = )` のサイズは `geom_text()` のサイズ指定と単位が違う

---
layout: default
---

# `theme(geom = )`

- `element_geom()` でデフォルトのスタイルを指定できるようになった

``` r
p <- ggplot(mpg, aes(displ, hwy)) + geom_point()

p2 <- p +
  theme(
    geom = element_geom(
      pointshape = "square",
      pointsize = 8
    )
  )
```

---
layout: default
---

![](./plot/unnamed-chunk-3-1.png)

---
layout: default
---

# `ink` と `paper`

- `ink`: メインの色
- `paper`: 背景の色
- `accent`: 強調の色

---
layout: default
---

# `ink` が青

``` r
p + theme(geom = element_geom(ink = "blue"))
```

![](./plot/unnamed-chunk-5-1.png)

---
layout: default
---

# `ink` が青で `paper` が赤

``` r
p + theme(geom = element_geom(ink = "blue", paper = "red"))
```

![](./plot/unnamed-chunk-6-1.png)

---
layout: default
---

# たぶん `paper` には `background` と同じ色を指定する？

``` r
p + theme(
  geom = element_geom(ink = "blue", paper = "red"),
  plot.background = element_rect(fill = "red"),
  panel.background = element_rect(fill = "red")
)
```

---
layout: default
---

![](./plot/unnamed-chunk-8-1.png)

---
layout: default
---

# `from_theme()`

- なぜこの指定が動くのかというと `from_theme()` が default aes
  に設定されているから

``` r
GeomPoint$default_aes
#> Aesthetic mapping: 
#> * `shape`  -> `from_theme(pointshape)`
#> * `colour` -> `from_theme(ink)`
#> * `size`   -> `from_theme(pointsize)`
#> * `fill`   -> NA
#> * `alpha`  -> NA
#> * `stroke` -> `from_theme(borderwidth)`
```

---
layout: default
---

# `from_theme()`

- つまり、Geom 側の実装に寄るので、ggplot2 本体の Geom
  は大丈夫でも、拡張パッケージの Geom には効かないことがあるかも？

---
layout: default
---

# `theme(palette = )`

``` r
ggplot(mpg, aes(displ, hwy)) +
  geom_point(aes(colour = class)) +
  theme(
    palette.colour.discrete = "mint"
  )
```

---
layout: default
---

![](./plot/unnamed-chunk-11-1.png)

---
layout: default
---

# 補足

- palette の管理は scales v1.4.0 の新機能（まだリリースされていない）
- すでにたくさんの palette が用意されているが、自分で palette
  を登録することもできる
- `scales::palette_names()` で確認できる

---
layout: default
---

``` r
scales::palette_names()
#>   [1] "greens 2"        "r4"              "greens 3"        "blues"          
#>   [5] "terrain"         "tableau 10"      "terrain 2"       "purple-orange"  
#>   [9] "spectral"        "ag_sunset"       "sunset"          "ylgn"           
#>  [13] "orrd"            "purple-blue"     "rdylbu"          "paired"         
#>  [17] "teal"            "gnbu"            "inferno"         "puor"           
#>  [21] "grey"            "armyrose"        "purpor"          "purple-brown"   
#>  [25] "cividis"         "tofino"          "tropic"          "oranges"        
#>  [29] "bugn"            "green-brown"     "warm"            "plasma"         
#>  [33] "harmonic"        "ag_grnyl"        "polychrome 36"   "reds 2"         
#>  [37] "rdbu"            "reds 3"          "green-orange"    "purples 2"      
#>  [41] "blue-red 2"      "blues 2"         "lisbon"          "accent"         
#>  [45] "purples 3"       "blue-red 3"      "blues 3"         "purple-yellow"  
#>  [49] "blue-red"        "turbo"           "piyg"            "viridis"        
#>  [53] "tealrose"        "bluyl"           "bupu"            "classic tableau"
#>  [57] "blue-yellow"     "broc"            "oslo"            "heat"           
#>  [61] "set1"            "set2"            "purd"            "hue"            
#>  [65] "set3"            "temps"           "reds"            "dark2"          
#>  [69] "brwnyl"          "mint"            "burgyl"          "blue-yellow 2"  
#>  [73] "prgn"            "blue-yellow 3"   "turku"           "roma"           
#>  [77] "burg"            "purp"            "sunsetdark"      "batlow"         
#>  [81] "heat 2"          "hawaii"          "red-purple"      "tealgrn"        
#>  [85] "peach"           "pastel 1"        "magenta"         "pastel 2"       
#>  [89] "lajolla"         "pastel1"         "pastel2"         "greys"          
#>  [93] "okabe-ito"       "pubu"            "cork"            "pinkyl"         
#>  [97] "blugrn"          "ylorrd"          "rdylgn"          "magma"          
#> [101] "set 1"           "set 2"           "ylgnbu"          "set 3"          
#> [105] "pubugn"          "red-green"       "purple-green"    "greens"         
#> [109] "mako"            "alphabet"        "geyser"          "dark mint"      
#> [113] "vik"             "cyan-magenta"    "emrld"           "red-yellow"     
#> [117] "ylorbr"          "brbg"            "cold"            "purples"        
#> [121] "fall"            "red-blue"        "ggplot2"         "berlin"         
#> [125] "rocket"          "rdgy"            "dark 2"          "dark 3"         
#> [129] "dynamic"         "green-yellow"    "redor"           "rdpu"           
#> [133] "grays"           "light grays"     "earth"           "oryel"          
#> [137] "r3"              "zissou 1"
```

---
layout: default
---

# marquee 関連の機能

- Marquee: Markdown 記法でスタイルを細かく設定できる
- <https://github.com/tidyverse/ggplot2/issues/5920>
- 「integration」というのがどういうことなのかあんまりよくわかってない

---
layout: default
---

# edition 導入？

- さすがに後方互換性を保つのつらすぎるので、edition
  を導入しよういう案がある
- <https://github.com/tidyverse/ggplot2/pull/6203>
- うまくいくかは不明。拡張パッケージとどう足並みを揃えるかが難しそう。

---
layout: default
---

# いずれにせよ言えること

- 後方互換性をケアするのがつらすぎるほどいっぱい変更がある
- ということはたぶんいろいろ壊れるものがある

---
layout: default
---

# 備えよう

- と言ったけど、特にできることはないので今日は🍺を飲んで忘れましょう！
