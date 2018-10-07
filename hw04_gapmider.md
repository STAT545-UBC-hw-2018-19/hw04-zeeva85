hw04\_gapminder
================
Seevasant Indran
06 October, 2018

Packages required
-----------------

-   [tidyverse](http://tidyverse.tidyverse.org/) (includes [ggplot2](http://ggplot2.tidyverse.org/), [dplyr](http://dplyr.tidyverse.org/), [tidyr](http://tidyr.tidyverse.org/), [readr](http://readr.tidyverse.org/), [maps](https://cran.r-project.org/web/packages/maps/index.html), [tibble](http://tibble.tidyverse.org/))
    -   Install by running 'install.packages("tidyverse", dependencies = TRUE)'

Functions used
--------------

-   **%&gt;%** - Syntactic sugar for easily piping the result of one function into another.
-   **ggplot2::ggplot()** - Base function for using ggplot2. Lays out the invisible 'canvas' for graphing.
-   **ggplot2::geom\_point()** - Geom function for drawing data points in scatterplots.
-   **ggplot2::geom\_bar()** - Geom function for drawing bars in bar graphs.
-   **ggplot2::geom\_boxplot()** - Geom function for drawing box plots.
-   **ggplo2::geom\_map()** - Geom function for polygons from a reference map
-   **ggplot2::facet\_wrap()** - ggplot2 function for separating factor levels into multiple graphs.
-   **ggplot2::xlab()** - Manually set x-axis label.
-   **ggplot2::ylab()** - Manually set y-axis label.
-   **dplyr::group\_by()** - Commonly used with summarize() to derive summarized values for multiple rows sharing certain attribues, e.g. Average fuel consumption rates of different car manufacturer.
-   **dplyr::summarize()** - Commonly used with summarize() to derive summarized values for multiple rows sharing certain attribues.
-   **reshape::melt()** - Changes a rectangular data into the long format.
-   **knitr::kable()** - Changes a rectangular data into the long format.

### Install, load packages and assign dataset

\`\`\`{r loadchunk, message=FALSE}
==================================

if (ev\_False) {
================

install.packages("tidyverse")
=============================

install.packages("gapminder")
=============================

install.packages("reshape2")
============================

install.packages("knitr")
=========================

install.packages("maps")
========================

}
=

if (ev\_True) {
===============

library(tidyverse)
==================

library(gapminder)
==================

library(reshape2)
=================

library(knitr)
==============

library(maps)
=============

library(gridExtra)
==================

}

\`\`\`
