Webscraping
================
11/3/2020

``` r
library(tidyverse)
```

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.0 --

    ## v ggplot2 3.3.2     v purrr   0.3.4
    ## v tibble  3.0.3     v dplyr   1.0.2
    ## v tidyr   1.1.2     v stringr 1.4.0
    ## v readr   1.3.1     v forcats 0.5.0

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(rvest)
```

    ## Loading required package: xml2

    ## 
    ## Attaching package: 'rvest'

    ## The following object is masked from 'package:purrr':
    ## 
    ##     pluck

    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

Scrape the game title, release date, platform, metascore, and user score
from all 180 pages:

``` r
title_vec = c()
platform_vec = c()
release_date_vec = c()
meta_score_vec = c()
user_score_vec = c()

for (i in 0:10) {
url = paste("http://www.metacritic.com/browse/games/score/metascore/all/psvita/filtered?view=detailed&page=", i, sep = "")

metacritic_html = read_html(url)

title =
  metacritic_html %>% 
  html_nodes(".title h3") %>% 
  html_text()
  
title_vec = append(title_vec, title[-1])

platform = 
  metacritic_html %>% 
  html_nodes(".platform .data") %>% 
  html_text() %>% 
  gsub("\n","",.)  %>% 
  gsub(" ","",.)

platform_vec = append(platform_vec,platform)

release_date = 
  metacritic_html %>% 
  html_nodes(".platform+ span") %>% 
  html_text()

release_date_vec = append(release_date_vec,release_date)

meta_score = 
  metacritic_html %>% 
  html_nodes(".clamp-metascore .positive") %>% 
  html_text()

meta_score_vec = append(meta_score_vec,meta_score)

user_score = 
  metacritic_html %>% 
  html_nodes(".user") %>% 
  html_text()

user_score_vec = append(user_score_vec,user_score)

}
```

Create a tibble to store all variables:

``` r
games_df <- tibble(
  title = title_vec,
  platform = platform_vec,
  release_date = release_date_vec,
  meta_score = meta_score_vec,
  user_score = user_score_vec
  )
```
