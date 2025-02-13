

<!-- README.md is generated from README.Rmd. Please edit that file -->

# quartomonothemer

<!-- badges: start -->
<!-- badges: end -->

<img src="man/figures/quartomonothemer.gif" width="100%">

This package provides custom Quarto Revealjs, `ggplot2`, and `gt`
themes. This work is strongly influenced by
[xaringanthemer](https://pkg.garrickadenbuie.com/xaringanthemer/), but
supports only limited features.

You can find more examples
[here](https://github.com/kazuyanagimoto/quarto-slides-example)

## Installation

You can install the development version of quartomonothemer from
[GitHub](https://github.com/) with:

``` r
# install.packages("remotes")
remotes::install_github("kazuyanagimoto/quartomonothemer")
```

## Example

First, add `custom.scss` file to the YAML header.

``` yaml
format:
  revealjs:
    theme: custom.scss
```

Then, run `style_mono_quarto()` inside of the slide qmd file. It
produces the `quartomonothemer.scss`

``` r
library(quartomonothemer)
library(showtext)

font_title <- "Josefin Sans"
font_text <- "Montserrat"
font_sans <- "Noto Sans" 
color_base <- "#009F8C"
color_base_light <- "#95DFD6"
color_accent <- "#B75C9D"
color_accent_light <- "#DBA6CC"
gray <- "#bebebe"
darkgray <- "#6d6d6d"

font_add_google(font_title)
font_add_google(font_text)
showtext_auto()

style_mono_quarto(
  font_title = font_title,
  font_text = font_text,
  font_sans = font_sans,
  color_base = color_base,
  color_accent = color_accent,
  color_link = color_accent,
  color_code = color_base,
  size_base = 30,
  path_scss = "quartomonothemer.scss"
)
```

This package also provide `theme_quarto()` based on the title and text
fonts,

``` r
library(palmerpenguins)
library(tidyverse)
penguins |>
  ggplot(aes(x = flipper_length_mm, y = bill_length_mm,
             color = species, shape = species)) +
  geom_point(size = 3) +
  scale_color_manual(values = c(color_base, color_base_light, darkgray)) +
  labs(x = "Flipper Length (mm)", y = "Bill Length (mm)") +
  theme_quarto() +
  theme(legend.position = c(0.9, 0.1))
```

and `gt_theme_quarto()` as a `gt` theme!

``` r
library(gt)
penguins |>
  filter(!is.na(sex)) |>
  group_by(species, sex) |>
  summarize(bill_length = mean(bill_length_mm, na.rm = TRUE),
            .groups = "drop") |>
  pivot_wider(names_from = "sex", values_from = "bill_length",
              names_prefix = "bill_length_") |>
  mutate(ratio_bar = 100 * bill_length_female / bill_length_male,
         ratio = ratio_bar / 100) |>
  gt(rowname_col = "species") |>
  cols_label(bill_length_female = "Female", bill_length_male = "Male",
             ratio_bar = "Female/Male", ratio = "Pct.") |>
  fmt_number(columns = starts_with("bill_length"), decimals = 1) |>
  fmt_percent(ratio, decimals = 0) |>
  gt_plt_bar_pct(ratio_bar, fill = color_base, scaled = TRUE) |>
  gt_theme_quarto()
```

Note that it does not provide any color maps. You need to specify by
`scale_*_manual()`
