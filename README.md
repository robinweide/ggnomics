
<!-- README.md is generated from README.Rmd. Please edit that file -->

# ggnomics

This package was written to extend `ggplot2` in the following way:

1.  Introduce flexibility in facetting and colour scaling.
2.  Provide some functionality for plotting genomics data.

Towards the first goal, this package provides the function
`force_panelsize()` to force ggplot panels to adopt a specific ratio or
absolute dimension. It also provides `scale_fill_multi()` to map
multiple aesthetics to different colour scales.

Towards the second goal it provides functions such as `facet_ideogrid()`
and `facet_ideowrap()` which will put ideograms with highlights next to
facetted panels. Also the new Hi-C triangle geom (`geom_hictriangle`)
makes it easy to display Hi-C matrices from the
[GENOVA](https://github.com/robinweide/GENOVA) package in a 45° rotated
fashion.

Please keep in mind that this package is highly experimental, may be
unstable and is subject to changes in the future.

## Installation

You can install tbggenomics from github with:

``` r
# install.packages("devtools")
devtools::install_github("teunbrand/ggenomics")
```

## Example

This is a basic example of `scale_fill_multi()` and `force_panelsizes()`
in action:

``` r
library(ggplot2)
library(tbggenomics)

phi <- 2/(1 + sqrt(5))

ggplot(iris, aes(x = Sepal.Length, y = Sepal.Width)) +
  geom_point(data = iris[iris$Species == "setosa",],     
             aes(sepal.width = Sepal.Width), shape = 21) +
  geom_point(data = iris[iris$Species == "versicolor",], 
             aes(petal.length = Petal.Length), shape = 21) +
  geom_point(data = iris[iris$Species == "virginica",],  
             aes(petal.width = Petal.Width), shape = 21) +
  facet_grid(~ Species, scales = "free") +
  scale_fill_multi(aesthetics = c("sepal.width", "petal.length", "petal.width"),
                   colours = list(c("white", "green"),
                                  c("white", "red"),
                                  c("white", "blue")),
                   guide = guide_colourbar(barheight = unit(50, "pt"))) +
  force_panelsizes(rows = 1, cols = c(1, phi, phi^2), respect = TRUE)
```

![](README-example-1.png)<!-- -->
