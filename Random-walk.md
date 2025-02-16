Modelling Deprivation Changes: A Random Walk Approach
================
Joshua Edefo
2025-02-16

This R code simulates a random walk to model fluctuations in the
Quantile Index of Multiple Deprivation (QIMD) over 20 years. It
initializes the quantile at 0.52 and updates it yearly using a small
random change from a normal distribution (mean = 0, SD = 0.02). A
bounding condition ensures values remain between 0 and 1. The results
are displayed and plotted, with a blue line showing the trend and red
dots marking yearly values. Af 10 years, the quantile value is 0.5438,
and after 20 years, it is 0.5766, reflecting natural fluctuations in
deprivation levels while preventing extreme variations.

Libraries

``` r
library(usethis)
```

    ## Warning: package 'usethis' was built under R version 4.3.2

``` r
library(dplyr)
```

    ## Warning: package 'dplyr' was built under R version 4.3.3

Random walk for modelling QIMD changes

``` r
set.seed(123)  # For reproducibility

# Parameters
years <- 20                    # Number of years
quantile_start <- 0.52         # Initial quantile
sd <- 0.02                  # Standard deviation of yearly fluctuation


# Simulate random walk
quantiles <- numeric(years + 1)  # A numeric vector to store the quantile values for each year. It has years + 1 elements to include the initial value.
quantiles[1] <- quantile_start  # Sets the first element of the vector to the initial quantile value.

for (t in 2:(years + 1)) {        # A loop that runs from the second year to the last year.
  quantiles[t] <- quantiles[t - 1] + rnorm(1, mean = 0, sd = sd)  # Random step i.e., adds a random step to the previous year's quantile.
  quantiles[t] <- max(0, min(1, quantiles[t]))  # Ensures that the quantile values stay within the range [0, 1]
}

quantiles
```

    ##  [1] 0.5200000 0.5087905 0.5041869 0.5353611 0.5367713 0.5393570 0.5736583
    ##  [8] 0.5828766 0.5575754 0.5438384 0.5349251 0.5594068 0.5666030 0.5746185
    ## [15] 0.5768321 0.5657153 0.6014536 0.6114106 0.5720782 0.5861053 0.5766495

``` r
# Plot the random walk
plot(0:years, quantiles, type = "l", col = "blue", lwd = 2, 
     xlab = "Year", ylab = "Quantile", main = "Random Walk of Exposure Quantile")
points(0:years, quantiles, pch = 16, col = "red")  # Add points
```

![](Random-walk_files/figure-gfm/c-1.png)<!-- --> summary of the model
output The QIMD quantile starts at 0.52, fluctuates over 20 years, peaks
at 0.6114, and ends at 0.5766.

Session information

``` r
sessionInfo()
```

    ## R version 4.3.1 (2023-06-16 ucrt)
    ## Platform: x86_64-w64-mingw32/x64 (64-bit)
    ## Running under: Windows 11 x64 (build 22631)
    ## 
    ## Matrix products: default
    ## 
    ## 
    ## locale:
    ## [1] LC_COLLATE=English_United Kingdom.utf8 
    ## [2] LC_CTYPE=English_United Kingdom.utf8   
    ## [3] LC_MONETARY=English_United Kingdom.utf8
    ## [4] LC_NUMERIC=C                           
    ## [5] LC_TIME=English_United Kingdom.utf8    
    ## 
    ## time zone: Europe/London
    ## tzcode source: internal
    ## 
    ## attached base packages:
    ## [1] stats     graphics  grDevices utils     datasets  methods   base     
    ## 
    ## other attached packages:
    ## [1] dplyr_1.1.4   usethis_2.2.2
    ## 
    ## loaded via a namespace (and not attached):
    ##  [1] digest_0.6.33     utf8_1.2.3        R6_2.5.1          fastmap_1.2.0    
    ##  [5] tidyselect_1.2.0  xfun_0.40         magrittr_2.0.3    glue_1.6.2       
    ##  [9] tibble_3.2.1      knitr_1.44        pkgconfig_2.0.3   htmltools_0.5.8.1
    ## [13] generics_0.1.3    rmarkdown_2.25    lifecycle_1.0.3   cli_3.6.1        
    ## [17] fansi_1.0.4       vctrs_0.6.5       compiler_4.3.1    purrr_1.0.2      
    ## [21] rstudioapi_0.15.0 tools_4.3.1       pillar_1.9.0      evaluate_0.21    
    ## [25] yaml_2.3.7        rlang_1.1.1       fs_1.6.3
