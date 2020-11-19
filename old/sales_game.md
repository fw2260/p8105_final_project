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
library(polite)
```

    ## Warning: package 'polite' was built under R version 4.0.3

``` r
library(tidyverse)
```

    ## -- Attaching packages ----------------------------------------------------------------------------- tidyverse 1.3.0 --

    ## v ggplot2 3.3.2     v stringr 1.4.0
    ## v tidyr   1.1.2     v forcats 0.5.0
    ## v readr   1.3.1

    ## -- Conflicts -------------------------------------------------------------------------------- tidyverse_conflicts() --
    ## x dplyr::filter()         masks stats::filter()
    ## x readr::guess_encoding() masks rvest::guess_encoding()
    ## x dplyr::lag()            masks stats::lag()
    ## x purrr::pluck()          masks rvest::pluck()

``` r
#url = "https://www.vgchartz.com/games/games.php?page=%d&results=200&name=&console=&keyword=&publisher=&genre=&order=TotalSales&ownership=Both&boxart=Both&banner=Both&showdeleted=&region=All&goty_year=&developer=&direction=DESC&showtotalsales=1&shownasales=1&showpalsales=1&showjapansales=1&showothersales=1&showpublisher=0&showdeveloper=0&showreleasedate=0&showlastupdate=0&showvgchartzscore=0&showcriticscore=0&showuserscore=0&showshipped=0&alphasort=&showmultiplat=No"
```

### title

\#`{r} #name = # map_df(1:96, function(i){ # cat(".") # sales =
read_html(sprintf(url, i)) # data.frame(title =
html_text(html_nodes(sales, "#generalBody td:nth-child(3)")), #
stringsAsFactors = FALSE) #}) #` \# \#\#\# console \#`{r} #console =
map_df(1:96, function(i){ # cat(".") # sales = read_html(sprintf(url,
i)) # data.frame(console = html_attr(html_nodes(sales, "td~ td+ td
img"), name = "alt"), # stringsAsFactors = FALSE) #}) #` \# \# \# \#
\#\#\# total sales \#`{r} #total_sales = map_df(1:96, function(i){ #
cat(".") # sales = read_html(sprintf(url, i)) # data.frame(total_sale =
html_text(html_nodes(sales, "#generalBody td:nth-child(5)")), #
stringsAsFactors = FALSE) #}) #` \# \# \#\#\# NA sales \#`{r} #na_sales
= map_df(1:96, function(i){ # cat(".") # sales = read_html(sprintf(url,
i)) # data.frame(na_sale = html_text(html_nodes(sales, "#generalBody
td:nth-child(6)")), # stringsAsFactors = FALSE) #}) #` \# \#\#\# PAL
sales \#`{r} #pal_sales = map_df(1:96, function(i){ # cat(".") # sales =
read_html(sprintf(url, i)) # data.frame(pal_sale =
html_text(html_nodes(sales, "td:nth-child(7)")), # stringsAsFactors =
FALSE) #}) #` \# \#\#\# Japan sales \#`{r} #jap_sales = map_df(1:96,
function(i){ # cat(".") # sales = read_html(sprintf(url, i)) #
data.frame(jap_sale = html_text(html_nodes(sales, "td:nth-child(8)")), #
stringsAsFactors = FALSE) #}) #` \# \#\#\# Other sales \#`{r}
#other_sales = map_df(1:96, function(i){ # cat(".") # sales =
read_html(sprintf(url, i)) # data.frame(other_sale =
html_text(html_nodes(sales, "td:nth-child(9)")), # stringsAsFactors =
FALSE) #}) #`

``` r
title_vec = c()
console_vec = c()
total_sale_vec = c()
na_sale_vec = c()
pal_sale_vec = c()
jap_sale_vec = c()
other_sale_vec = c()


for (i in 1:96) {
  url = paste("https://www.vgchartz.com/games/games.php?page=", i, "&results=200&name=&console=&keyword=&publisher=&genre=&order=TotalSales&ownership=Both&boxart=Both&banner=Both&showdeleted=&region=All&goty_year=&developer=&direction=DESC&showtotalsales=1&shownasales=1&showpalsales=1&showjapansales=1&showothersales=1&showpublisher=0&showdeveloper=0&showreleasedate=0&showlastupdate=0&showvgchartzscore=0&showcriticscore=0&showuserscore=0&showshipped=0&alphasort=&showmultiplat=No", sep = "")

  sales_html = bow(url, force = TRUE)

  title = scrape(sales_html) %>% 
  html_nodes("td~ td+ td a:nth-child(1)") %>% 
  html_text() 
  
  title_vec = append(title_vec, title)

  console = 
    scrape(sales_html) %>% 
    html_nodes("td~ td+ td img") %>% 
    html_attr(., name = "alt") 
  
  console_vec = append(console_vec,console)

  total_sale = 
    scrape(sales_html) %>% 
    html_nodes("#generalBody td:nth-child(5)") %>% 
    html_text()
  
  total_sale_vec = append(total_sale_vec,total_sale)
  
  na_sale = 
    scrape(sales_html) %>% 
    html_nodes("#generalBody td:nth-child(6)") %>% 
    html_text() 
  
  na_sale_vec = append(na_sale_vec,na_sale)
  
  pal_sale = 
    scrape(sales_html) %>% 
    html_nodes("td:nth-child(7)") %>% 
    html_text()
  
  pal_sale_vec = append(pal_sale_vec,pal_sale)
  
  jap_sale = 
    scrape(sales_html) %>% 
    html_nodes("td:nth-child(8)") %>% 
    html_text()
  
  jap_sale_vec = append(jap_sale_vec,jap_sale)
  
  other_sale = 
    scrape(sales_html) %>% 
    html_nodes("td:nth-child(9)") %>% 
    html_text()
  
  other_sale_vec = append(other_sale_vec,other_sale)

}
```

``` r
sum_df = 
  tibble(
    title = title_vec,
    console = console_vec,
    total_sale = total_sale_vec,
    na_sale = na_sale_vec,
    pal_sale = pal_sale_vec,
    japan_sale = jap_sale_vec,
    other_sale = other_sale_vec
  )

write_csv(sum_df, "data/sales.csv")
```
