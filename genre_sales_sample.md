Scrape game sales - Action genre
================

``` r
library(rvest)
```

    ## Loading required package: xml2

``` r
library(httr)
library(tidyverse)
```

    ## ── Attaching packages ──────────────────────────────────────────────────────────────────────────────────────────── tidyverse 1.3.0 ──

    ## ✓ ggplot2 3.3.2     ✓ purrr   0.3.4
    ## ✓ tibble  3.0.3     ✓ dplyr   1.0.2
    ## ✓ tidyr   1.1.2     ✓ stringr 1.4.0
    ## ✓ readr   1.3.1     ✓ forcats 0.5.0

    ## ── Conflicts ─────────────────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter()         masks stats::filter()
    ## x readr::guess_encoding() masks rvest::guess_encoding()
    ## x dplyr::lag()            masks stats::lag()
    ## x purrr::pluck()          masks rvest::pluck()

``` r
read_sales_values <- function(x) {
  
  url = sprintf("https://www.vgchartz.com/games/games.php?page=%s&results=200&genre=Action&order=TotalSales&ownership=Both&direction=DESC&showtotalsales=1&shownasales=1&showpalsales=1&showjapansales=1&showothersales=1&showpublisher=1&showdeveloper=1&showreleasedate=1&showlastupdate=0&showvgchartzscore=0&showcriticscore=0&showuserscore=0&showshipped=0&showmultiplat=Yes", x)
  
html = read_html(url)

titles = 
  html %>% 
  html_nodes("td~ td+ td a:nth-child(1)") %>% 
  html_text()

consoles = 
  html %>% 
  html_nodes("td~ td+ td img") %>% 
  html_attr(., name = "alt") 

publishers =
  html %>% 
  html_nodes("#generalBody td:nth-child(5)") %>% 
  html_text()

developers = 
  html %>% 
  html_nodes("#generalBody td:nth-child(6)") %>% 
  html_text()

total_sales = 
  html %>% 
  html_nodes("td:nth-child(7)") %>% 
  html_text()

na_sales = 
  html %>% 
  html_nodes("td:nth-child(8)") %>% 
  html_text()

pal_sales = 
  html %>% 
  html_nodes("td:nth-child(9)") %>% 
  html_text()

japan_sales =
  html %>% 
  html_nodes("td:nth-child(10)") %>% 
  html_text()

other_sales =
  html %>% 
  html_nodes("td:nth-child(11)") %>% 
  html_text()

release_dates =
  html %>% 
  html_nodes("td:nth-child(12)") %>% 
  html_text()

tibble(
  title = titles,
  console = consoles,
  publisher = publishers,
  developer = developers,
  total_sale = total_sales,
  na_sale = na_sales,
  pal_sale = pal_sales,
  japan_sale = japan_sales,
  other_sale = other_sales,
  release_date = release_dates
)
}
```

``` r
sale_values = map_df(1:16, read_sales_values)
```

``` r
write_csv(sale_values, "data/action.csv")
```
