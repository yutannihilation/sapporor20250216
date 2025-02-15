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

MIERUNE 入社

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
- `geom_marquee()`, `element_marquee()`

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
p <- ggplot(mpg, aes(displ, hwy)) +
  geom_point(aes(colour = class)) +
  theme(
    palette.colour.discrete = "mint"
  )

p
```

![](./plot/unnamed-chunk-10-1.png)
