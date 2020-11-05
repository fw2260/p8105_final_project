Webscraping
================
11/3/2020

``` r
library(tidyverse)
```

    ## -- Attaching packages --------------------------------- tidyverse 1.3.0 --

    ## √ ggplot2 3.3.2     √ purrr   0.3.4
    ## √ tibble  3.0.3     √ dplyr   1.0.2
    ## √ tidyr   1.1.2     √ stringr 1.4.0
    ## √ readr   1.3.1     √ forcats 0.5.0

    ## -- Conflicts ------------------------------------ tidyverse_conflicts() --
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

\<\<\<\<\<\<\< HEAD Scrape from home pages ======= Scrape the game
title, release date, platform, metascore, and user score from all 180
pages: \>\>\>\>\>\>\> 8245790c3573851bbb7528ac6ba212d823307f4d

``` r
title_vec = c()
platform_vec = c()
release_date_vec = c()
meta_score_vec = c()

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
  gsub("^\\s+|\\s+$","",.) 
  

platform_vec = append(platform_vec,platform)

release_date = 
  metacritic_html %>% 
  html_nodes(".platform+ span") %>% 
  html_text()

release_date_vec = append(release_date_vec,release_date)

#meta_score = 
 # metacritic_html %>% 
  #html_nodes(".large") %>% 
  #html_text()

#meta_score_vec = append(meta_score_vec,meta_score)

}
```

\<\<\<\<\<\<\< HEAD Create a table to store all variables. =======

Create a tibble to store all variables: \>\>\>\>\>\>\>
8245790c3573851bbb7528ac6ba212d823307f4d

``` r
games_df <- tibble(
  title = title_vec,
  platform = platform_vec,
  release_date = release_date_vec
  )
```

\<\<\<\<\<\<\< HEAD

Scrape from detail pages

``` r
platform_add = tolower(platform_vec) %>% 
  str_replace(" ","-")
  
title_add = tolower(title_vec) %>% 
  str_replace_all(":","") %>% 
   str_replace_all("'","") %>% 
   str_replace_all(" /","") %>% 
   str_replace_all(";","") %>% 
   str_replace_all("\\(","") %>% 
   str_replace_all("\\)","") %>% 
   str_replace_all("\\.","") %>% 
   str_replace_all("\\ &","") %>%
   str_replace_all("\\$","") %>% 
  str_replace_all(",","") %>% 
   str_replace_all(" ","-")  
 
for (i in 1:1100) {
  url_add = "https://www.metacritic.com/game"
  url = paste(url_add,platform_add[i],title_add[i],sep = "/")
}
```

\======= \>\>\>\>\>\>\> 8245790c3573851bbb7528ac6ba212d823307f4d
