paper-soybeanrust-fungicide
================

Introduction
------------

This document provides a way to reproduce some of the analyses and plots produced for a paper published in Plant Disease. In the paper, meta-analysis is used to summarize the means of percent control and yield response from using different fungicides to control a fungal soybean disease - soybean rust. The main objective of the work was to fit a meta-regression model to the data by testing the effect of year in the estimates, which was apparently affecting both responses based on graphical analysis. The data from each variable are available in this repository and the codes are provided below to reproduce portion of the work.

Import data
-----------

The data were prepared in excel spreadsheet and exported to csv. The two datasets are separated by response variable because they did not occur in all trial. They could be combined in a single dataset, but are kept separated for convenience.

``` r
sev <- read_csv("sev_data.csv")
yld <- read_csv("yld_data.csv")
```

Let's have a close look at the variables and the first ten row. We can identify six variables on each dataset, including the disease and the yield variable. The five common variables are:

-   trial: numeric. identifier for the trial
-   severity: numeric. disease intensitiy variable (0 - 100)
-   mse: numeric. mean square error for the trial
-   active\_ingredient: character. identifier for the fungicide
-   year: numeric. Harvest year of the crop
-   spray: numeric. Number of sprays for the treatment

``` r
head(sev, 10)
```

    ## # A tibble: 10 x 6
    ##    trial severity     mse active_ingredient  year spray
    ##    <int>    <dbl>   <dbl>             <chr> <int> <int>
    ##  1   202      0.1 9.90909         trif_prot  2012     2
    ##  2     3      0.1 6.53177         azox_cypr  2005     2
    ##  3     3      0.1 6.53177              tebu  2005     2
    ##  4     8      0.1 0.34482         azox_cypr  2005     2
    ##  5     7      0.1 6.41161              tebu  2005     2
    ##  6     7      0.1 6.41161         azox_cypr  2005     2
    ##  7    33      0.1 0.02706              cypr  2006     2
    ##  8    85      0.1 0.04092              tebu  2008     2
    ##  9   105      0.1 0.15370         pico_cypr  2009     2
    ## 10   130      0.1 0.09829         azox_cypr  2009     2

``` r
head(yld, 10)
```

    ## # A tibble: 10 x 6
    ##    trial   yield       mse active_ingredient  year spray
    ##    <int>   <dbl>     <dbl>             <chr> <int> <int>
    ##  1     1 1673.08 162224.76             check  2005     2
    ##  2     1 3500.45 162224.76         azox_cypr  2005     2
    ##  3     1 3355.38 162224.76              tebu  2005     2
    ##  4     2 2039.98  59149.83             check  2005     2
    ##  5     2 2238.00  59149.83         azox_cypr  2005     2
    ##  6     2 2040.91  59149.83              tebu  2005     2
    ##  7     3 1906.65  66734.22             check  2005     2
    ##  8     3 2465.73  66734.22         azox_cypr  2005     2
    ##  9     3 2558.64  66734.22              tebu  2005     2
    ## 10     4 1350.00  20055.30             check  2005     2

\`\`\`
