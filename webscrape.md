Webscraping
================
Lily Wang
11/3/2020

``` r
title_vec <- c()
platform_vec = c()
release_date_vec = c()
meta_score_vec = c()

# scrape all 180 pages
for (i in 0:10) {
url <- paste("http://www.metacritic.com/browse/games/score/metascore/all/psvita/filtered?view=detailed&page=", i, sep = "")

metacritic_html <- read_html(url)

titles_100 <-
  metacritic_html %>% 
  html_nodes(".title h3") %>% 
  html_text()
  
title_vec <- append(title_vec, titles_100[-1])

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

#meta_score = 
 # metacritic_html %>% 
  #html_nodes(".large") %>% 
  #html_text()

#meta_score_vec = append(meta_score_vec,meta_score)

}
```

Create a table to store all variables.

``` r
games_df <- tibble(
  title = title_vec,
  platform = platform_vec,
  release_date = release_date_vec
  )
```
