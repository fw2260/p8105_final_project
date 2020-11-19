Tidy VGChartz
================
11/19/2020

``` r
game_df <- 
  tibble(
    path = list.files("data/genre_sales")) %>% 
  mutate(genre = str_extract(path, "[^.]+"),
         path = str_c("data/genre_sales/", path),
         data = map(.x = path, ~read_csv(.x))) %>% 
  unnest(data) %>% 
  select(-path) %>% 
  select(title, console, genre, release_date, everything()) %>%   mutate_at(7:11, funs(gsub("m","",.))) %>% 
  mutate_at(7:11, funs(gsub("N/A",NA,.))) %>% 
  mutate(release_date = lubridate::dmy(release_date)) %>% 
  distinct(title, console, genre, .keep_all = T)
```

    ## 
    ## -- Column specification --------------------------------------------------------
    ## cols(
    ##   title = col_character(),
    ##   console = col_character(),
    ##   publisher = col_character(),
    ##   developer = col_character(),
    ##   total_sale = col_character(),
    ##   na_sale = col_character(),
    ##   pal_sale = col_character(),
    ##   japan_sale = col_character(),
    ##   other_sale = col_character(),
    ##   release_date = col_character()
    ## )
    ## 
    ## 
    ## -- Column specification --------------------------------------------------------
    ## cols(
    ##   title = col_character(),
    ##   console = col_character(),
    ##   publisher = col_character(),
    ##   developer = col_character(),
    ##   total_sale = col_character(),
    ##   na_sale = col_character(),
    ##   pal_sale = col_character(),
    ##   japan_sale = col_character(),
    ##   other_sale = col_character(),
    ##   release_date = col_character()
    ## )
    ## 
    ## 
    ## -- Column specification --------------------------------------------------------
    ## cols(
    ##   title = col_character(),
    ##   console = col_character(),
    ##   publisher = col_character(),
    ##   developer = col_character(),
    ##   total_sale = col_character(),
    ##   na_sale = col_character(),
    ##   pal_sale = col_character(),
    ##   japan_sale = col_character(),
    ##   other_sale = col_character(),
    ##   release_date = col_character()
    ## )
    ## 
    ## 
    ## -- Column specification --------------------------------------------------------
    ## cols(
    ##   title = col_character(),
    ##   console = col_character(),
    ##   publisher = col_character(),
    ##   developer = col_character(),
    ##   total_sale = col_character(),
    ##   na_sale = col_character(),
    ##   pal_sale = col_character(),
    ##   japan_sale = col_character(),
    ##   other_sale = col_character(),
    ##   release_date = col_character()
    ## )
    ## 
    ## 
    ## -- Column specification --------------------------------------------------------
    ## cols(
    ##   title = col_character(),
    ##   console = col_character(),
    ##   publisher = col_character(),
    ##   developer = col_character(),
    ##   total_sale = col_character(),
    ##   na_sale = col_character(),
    ##   pal_sale = col_character(),
    ##   japan_sale = col_character(),
    ##   other_sale = col_character(),
    ##   release_date = col_character()
    ## )
    ## 
    ## 
    ## -- Column specification --------------------------------------------------------
    ## cols(
    ##   title = col_character(),
    ##   console = col_character(),
    ##   publisher = col_character(),
    ##   developer = col_character(),
    ##   total_sale = col_character(),
    ##   na_sale = col_character(),
    ##   pal_sale = col_character(),
    ##   japan_sale = col_character(),
    ##   other_sale = col_character(),
    ##   release_date = col_character()
    ## )
    ## 
    ## 
    ## -- Column specification --------------------------------------------------------
    ## cols(
    ##   title = col_character(),
    ##   console = col_character(),
    ##   publisher = col_character(),
    ##   developer = col_character(),
    ##   total_sale = col_character(),
    ##   na_sale = col_character(),
    ##   pal_sale = col_character(),
    ##   japan_sale = col_character(),
    ##   other_sale = col_character(),
    ##   release_date = col_character()
    ## )
    ## 
    ## 
    ## -- Column specification --------------------------------------------------------
    ## cols(
    ##   title = col_character(),
    ##   console = col_character(),
    ##   publisher = col_character(),
    ##   developer = col_character(),
    ##   total_sale = col_character(),
    ##   na_sale = col_character(),
    ##   pal_sale = col_character(),
    ##   japan_sale = col_character(),
    ##   other_sale = col_character(),
    ##   release_date = col_character()
    ## )
    ## 
    ## 
    ## -- Column specification --------------------------------------------------------
    ## cols(
    ##   title = col_character(),
    ##   console = col_character(),
    ##   publisher = col_character(),
    ##   developer = col_character(),
    ##   total_sale = col_character(),
    ##   na_sale = col_character(),
    ##   pal_sale = col_character(),
    ##   japan_sale = col_character(),
    ##   other_sale = col_character(),
    ##   release_date = col_character()
    ## )
    ## 
    ## 
    ## -- Column specification --------------------------------------------------------
    ## cols(
    ##   title = col_character(),
    ##   console = col_character(),
    ##   publisher = col_character(),
    ##   developer = col_character(),
    ##   total_sale = col_character(),
    ##   na_sale = col_character(),
    ##   pal_sale = col_character(),
    ##   japan_sale = col_character(),
    ##   other_sale = col_character(),
    ##   release_date = col_character()
    ## )
    ## 
    ## 
    ## -- Column specification --------------------------------------------------------
    ## cols(
    ##   title = col_character(),
    ##   console = col_character(),
    ##   publisher = col_character(),
    ##   developer = col_character(),
    ##   total_sale = col_character(),
    ##   na_sale = col_character(),
    ##   pal_sale = col_character(),
    ##   japan_sale = col_character(),
    ##   other_sale = col_character(),
    ##   release_date = col_character()
    ## )
    ## 
    ## 
    ## -- Column specification --------------------------------------------------------
    ## cols(
    ##   title = col_character(),
    ##   console = col_character(),
    ##   publisher = col_character(),
    ##   developer = col_character(),
    ##   total_sale = col_character(),
    ##   na_sale = col_character(),
    ##   pal_sale = col_character(),
    ##   japan_sale = col_character(),
    ##   other_sale = col_character(),
    ##   release_date = col_character()
    ## )
    ## 
    ## 
    ## -- Column specification --------------------------------------------------------
    ## cols(
    ##   title = col_character(),
    ##   console = col_character(),
    ##   publisher = col_character(),
    ##   developer = col_character(),
    ##   total_sale = col_character(),
    ##   na_sale = col_character(),
    ##   pal_sale = col_character(),
    ##   japan_sale = col_character(),
    ##   other_sale = col_character(),
    ##   release_date = col_character()
    ## )
    ## 
    ## 
    ## -- Column specification --------------------------------------------------------
    ## cols(
    ##   title = col_character(),
    ##   console = col_character(),
    ##   publisher = col_character(),
    ##   developer = col_character(),
    ##   total_sale = col_character(),
    ##   na_sale = col_character(),
    ##   pal_sale = col_character(),
    ##   japan_sale = col_character(),
    ##   other_sale = col_character(),
    ##   release_date = col_character()
    ## )

    ## Warning: `funs()` is deprecated as of dplyr 0.8.0.
    ## Please use a list of either functions or lambdas: 
    ## 
    ##   # Simple named list: 
    ##   list(mean = mean, median = median)
    ## 
    ##   # Auto named with `tibble::lst()`: 
    ##   tibble::lst(mean, median)
    ## 
    ##   # Using lambdas
    ##   list(~ mean(., trim = .2), ~ median(., na.rm = TRUE))
    ## This warning is displayed once every 8 hours.
    ## Call `lifecycle::last_warnings()` to see where this warning was generated.

    ## Warning: Problem with `mutate()` input `release_date`.
    ## i  8 failed to parse.
    ## i Input `release_date` is `lubridate::dmy(release_date)`.

    ## Warning: 8 failed to parse.

``` r
write_csv(game_df, "./data/vgchartz.csv")
```
