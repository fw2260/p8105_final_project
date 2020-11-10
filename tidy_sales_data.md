Tidy sales data
================
11/10/2020

This document contains all of our analyses.

Load sales data and remove `m` in each sale column

``` r
sales_data = 
  read.csv("data/sales.csv") %>% 
  separate(total_sale, into = c("total_sale", NA), sep = "[m]") %>% 
  separate(na_sale, into = c("na_sale", NA), sep = "[m]") %>% 
  separate(pal_sale, into = c("pal_sale", NA), sep = "[m]") %>% 
  separate(japan_sale, into = c("japan_sale", NA), sep = "[m]") %>% 
  separate(other_sale, into = c("other_sale", NA), sep = "[m]")
```

    ## Warning: Expected 2 pieces. Missing pieces filled with `NA` in 5684 rows [246,
    ## 376, 429, 450, 476, 585, 631, 698, 751, 827, 871, 913, 1031, 1040, 1053, 1144,
    ## 1153, 1182, 1216, 1246, ...].

    ## Warning: Expected 2 pieces. Missing pieces filled with `NA` in 6098 rows [356,
    ## 376, 429, 434, 476, 494, 585, 631, 665, 674, 698, 751, 766, 814, 827, 845, 847,
    ## 871, 888, 913, ...].

    ## Warning: Expected 2 pieces. Missing pieces filled with `NA` in 11662 rows [71,
    ## 104, 106, 114, 120, 130, 135, 152, 160, 165, 176, 180, 195, 196, 207, 219, 220,
    ## 235, 247, 248, ...].

    ## Warning: Expected 2 pieces. Missing pieces filled with `NA` in 3683 rows [152,
    ## 319, 325, 376, 429, 544, 576, 625, 662, 726, 740, 805, 814, 877, 882, 891, 913,
    ## 930, 942, 976, ...].
