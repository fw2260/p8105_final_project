Scrape game sales website
================

``` r
library(rvest)
```

    ## Loading required package: xml2

``` r
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(purrr)
```

    ## 
    ## Attaching package: 'purrr'

    ## The following object is masked from 'package:rvest':
    ## 
    ##     pluck

``` r
library(tibble)
library(httr)
```

``` r
url = "https://www.vgchartz.com/games/games.php?page=%d&results=200&name=&console=&keyword=&publisher=&genre=&order=TotalSales&ownership=Both&boxart=Both&banner=Both&showdeleted=&region=All&goty_year=&developer=&direction=DESC&showtotalsales=1&shownasales=1&showpalsales=1&showjapansales=1&showothersales=1&showpublisher=0&showdeveloper=0&showreleasedate=0&showlastupdate=0&showvgchartzscore=0&showcriticscore=0&showuserscore=0&showshipped=0&alphasort=&showmultiplat=No"
```

## title

``` r
name = 
  map_df(1:96, function(i){
    cat(".")
    sales = read_html(sprintf(url, i))
    data.frame(title = html_text(html_nodes(sales, "#generalBody td:nth-child(3)")),
             stringsAsFactors = FALSE)
})
```

    ## ................................................................................................

## total sales

``` r
total_sales = map_df(1:96, function(i){
  cat(".")
  sales = read_html(sprintf(url, i))
  data.frame(total_sale = html_text(html_nodes(sales, "#generalBody td:nth-child(5)")),
           stringsAsFactors = FALSE)
})
```

    ## ................................................................................................

``` r
sum_df = 
  tibble(
    title = name,
    total_sale = total_sales
  )
```
