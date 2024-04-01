yunopasta
================
2024-04-01

``` r
## brilliant code goes here.

library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.0     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
PropUsed<-matrix(c(178,146,99,79,77,79,21,19,52,59),nrow=5,byrow=T)
colnames(PropUsed)<-c("Offered","Used")
rownames(PropUsed)<-c("None","Numbness","Distraction","Comfort","Combination")
PropUsed
```

    ##             Offered Used
    ## None            178  146
    ## Numbness         99   79
    ## Distraction      77   79
    ## Comfort          21   19
    ## Combination      52   59

``` r
addmargins(prop.table(PropUsed))
```

    ##                Offered       Used        Sum
    ## None        0.22002472 0.18046972 0.40049444
    ## Numbness    0.12237330 0.09765142 0.22002472
    ## Distraction 0.09517923 0.09765142 0.19283066
    ## Comfort     0.02595797 0.02348578 0.04944376
    ## Combination 0.06427689 0.07292954 0.13720643
    ## Sum         0.52781211 0.47218789 1.00000000

``` r
PropUsed_df<- cbind(as_tibble(prop.table(PropUsed)), 
                                Intervention = c("None","Numbness","Distraction","Comfort","Combination"))

PropUsed_df <- PropUsed_df %>% 
  pivot_longer(!Intervention, names_to = "Outcome", values_to = "Proportion")

PropUsed_df
```

    ## # A tibble: 10 × 3
    ##    Intervention Outcome Proportion
    ##    <chr>        <chr>        <dbl>
    ##  1 None         Offered     0.220 
    ##  2 None         Used        0.180 
    ##  3 Numbness     Offered     0.122 
    ##  4 Numbness     Used        0.0977
    ##  5 Distraction  Offered     0.0952
    ##  6 Distraction  Used        0.0977
    ##  7 Comfort      Offered     0.0260
    ##  8 Comfort      Used        0.0235
    ##  9 Combination  Offered     0.0643
    ## 10 Combination  Used        0.0729

``` r
ggplot(PropUsed_df, aes(x = Intervention, y = Proportion, fill = Outcome)) +
  geom_bar(stat = "identity", position = 'fill')
```

![](yunopasta_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

``` r
chisq.test(PropUsed)
```

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  PropUsed
    ## X-squared = 3.4825, df = 4, p-value = 0.4806
