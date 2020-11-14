Tidy Sales Data
================
11/10/2020

This document contains steps to clean the sales data.

Load sales data, remove `m` in each sale column, and change “N/A” to NA

``` r
#sales_data = 
#  read_csv("data/sales.csv") %>% 
#  separate(total_sale, into = c("total_sale", NA), sep = "[m]") %>% 
#  separate(na_sale, into = c("na_sale", NA), sep = "[m]") %>% 
#  separate(pal_sale, into = c("pal_sale", NA), sep = "[m]") %>% 
#  separate(japan_sale, into = c("japan_sale", NA), sep = "[m]") %>% 
#  separate(other_sale, into = c("other_sale", NA), sep = "[m]")

# a shorter one. It also works!

sales_data = 
  read_csv("./data/sales.csv") %>%
  janitor::clean_names() %>% 
  mutate_at(3:7, funs(gsub("m$","",.))) %>% 
  mutate_at(3:7, funs(gsub("N/A",NA,.))) 
```

    ## Parsed with column specification:
    ## cols(
    ##   title = col_character(),
    ##   console = col_character(),
    ##   total_sale = col_character(),
    ##   na_sale = col_character(),
    ##   pal_sale = col_character(),
    ##   japan_sale = col_character(),
    ##   other_sale = col_character()
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

To list the unique name of console.

``` r
sales_data %>% 
  distinct(console)
```

    ## # A tibble: 39 x 1
    ##    console
    ##    <chr>  
    ##  1 Wii    
    ##  2 NES    
    ##  3 GB     
    ##  4 DS     
    ##  5 X360   
    ##  6 PS2    
    ##  7 SNES   
    ##  8 PS3    
    ##  9 PS4    
    ## 10 3DS    
    ## # ... with 29 more rows

There are 39 unique consoles listed.

Rename console in sales data

\*\*NOTE: The following console cannot be found to match in the
`metacritic.csv` file: 2600, GBC, GEN, PSN, SAT, SCD, WS, VC, NG, WW,
PCE, XBL, 3D0, GG, OSX, PCFX, Mob

``` r
sales_df = 
  sales_data %>% 
  mutate(
    console = 
      recode(console, 
       PS = "PlayStation",
       PS2 = "PlayStation 2",
       PS3 = "PlayStation 3",
       PS4 = "PlayStation 4",
       GB = "Game Boy Advance",
       X360 = "Xbox 360",
       NS = "Switch",
       N64 = "Nintendo 64",
       XOne = "Xbox One",
       XB = "Xbox",
       WiiU = "Wii U",
       GC = "GameCube",
       PSV = "PlayStation Vita",
       DC = "Dreamcast"
       )
  ) %>% 
  rename(platform = console)
```

\#New name for console values

write the new sales dataset into a .csv file

``` r
#sales_df = 
#  sales_data %>% 
#  mutate(console = console_df) %>% 
#  rename(platform = console)

write_csv(sales_df, "./data/sales_rename_2.csv")
```

Merge the Metacritic dataset and the sales dataset, change “tbd” to NA

``` r
sales_rename = read_csv("./data/sales_rename_2.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   title = col_character(),
    ##   platform = col_character(),
    ##   total_sale = col_double(),
    ##   na_sale = col_double(),
    ##   pal_sale = col_double(),
    ##   japan_sale = col_double(),
    ##   other_sale = col_double()
    ## )

``` r
metacritic = 
  read_csv("data/metacritic.csv") %>% 
  mutate(user_score = na_if(user_score, "tbd"))
```

    ## Parsed with column specification:
    ## cols(
    ##   title = col_character(),
    ##   link = col_character(),
    ##   platform = col_character(),
    ##   release_date = col_character(),
    ##   meta_score = col_double(),
    ##   user_score = col_character(),
    ##   developer = col_character(),
    ##   publisher = col_character(),
    ##   genre = col_character(),
    ##   esrb_rating = col_character()
    ## )

``` r
game_df = 
  left_join(metacritic, sales_rename, by = c("title", "platform")) %>% 
  drop_na()

write_csv(game_df, "./data/game_2.csv")
```
