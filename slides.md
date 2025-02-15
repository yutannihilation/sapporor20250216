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

- `theme(text = )` の指定は `geom_text()`
  などには引き継がれない（けっこうハマりがち）
- `theme(text = )` のサイズは `geom_text()` のサイズ指定と単位が違う

---
layout: default
---

# `theme(geom = )`

- `element_geom()` でデフォルトのスタイルを指定できるように

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
