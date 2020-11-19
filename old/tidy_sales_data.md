Tidy Sales Data
================
11/10/2020

This document contains steps to clean the sales data.

## Clean Sales Dataset

Load sales data, remove `m` in each sale column, and change “N/A” to NA

``` r
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

    ## Warning in FUN(X[[i]], ...): strings not representable in native encoding will
    ## be translated to UTF-8

    ## Warning in FUN(X[[i]], ...): unable to translate '<U+00C4>' to native encoding

    ## Warning in FUN(X[[i]], ...): unable to translate '<U+00D6>' to native encoding

    ## Warning in FUN(X[[i]], ...): unable to translate '<U+00E4>' to native encoding

    ## Warning in FUN(X[[i]], ...): unable to translate '<U+00F6>' to native encoding

    ## Warning in FUN(X[[i]], ...): unable to translate '<U+00DF>' to native encoding

    ## Warning in FUN(X[[i]], ...): unable to translate '<U+00C6>' to native encoding

    ## Warning in FUN(X[[i]], ...): unable to translate '<U+00E6>' to native encoding

    ## Warning in FUN(X[[i]], ...): unable to translate '<U+00D8>' to native encoding

    ## Warning in FUN(X[[i]], ...): unable to translate '<U+00F8>' to native encoding

    ## Warning in FUN(X[[i]], ...): unable to translate '<U+00C5>' to native encoding

    ## Warning in FUN(X[[i]], ...): unable to translate '<U+00E5>' to native encoding

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
`metacritic.csv` file: NES, 2600, GBC, GEN, PSN, SAT, SCD, WS, VC, NG,
WW, PCE, XBL, 3D0, GG, OSX, PCFX, Mob, SNES

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
       GBA = "Game Boy Advance",
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

write the new sales dataset into a .csv file

``` r
write_csv(sales_df, "./data/sales_rename_2.csv")
```

## Merge Metacritic and Sales Dataset

Merge the Metacritic dataset and the sales dataset, change “tbd” to NA
and tidy pokemon games in metacritic dataset

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
metacritic = read_csv("./data/metacritic.csv") 
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
metacritic= 
  metacritic %>% 
  mutate(
    user_score = na_if(user_score, "tbd"),
    user_score = as.numeric(user_score)) 

metacritic_pokemon =
  metacritic %>% 
  filter(str_detect(title,"^Pokemon+")) %>% 
  mutate(
    title = str_replace(title,"e","é")
  ) %>% 
  group_by(release_date) %>%
  summarise(
    user_score = mean(user_score),
    meta_score = mean(meta_score),
    title = paste(title, collapse = " / "),
    platform = platform,
    developer = developer,
    publisher = publisher,
    genre = genre,
    esrb_rating = esrb_rating
  ) %>% 
  distinct(title, .keep_all = TRUE) 
```

    ## `summarise()` regrouping output by 'release_date' (override with `.groups` argument)

``` r
metacritic_recode = 
  metacritic %>% 
  filter(!(str_detect(title,"^Pokémon+"))) %>% 
  bind_rows(metacritic_pokemon) %>% 
  mutate(
    title = recode(title,
      "Pokémon Sun / Pokémon Moon" = "Pokémon Sun/Moon",
      "Pokémon Omega Ruby / Pokémon Alpha Sapphire" = 
      "Pokémon Omega Ruby/Pokémon Alpha Sapphire",
      "Pokémon Y / Pokémon X" = 
      "Pokémon X/Y",
      "Pokémon White Version / Pokémon Black Version" =
      "Pokémon Black / White Version",
      "Pokémon FireRed Version / Pokémon LeafGreen Version" =
      "Pokémon FireRed / LeafGreen Version",
      "Pokémon SoulSilver Version / Pokémon HeartGold Version" =
      "Pokémon Heart Gold / Soul Silver Version",
      "Pokémon Pearl Version / Pokémon Diamond Version" =
      "Pokémon Diamond / Pearl Version",
      "Pokémon Mystery Dungeon: Explorers of Time / Pokémon Darkness" =
      "Pokémon Mystery Dungeon: Explorers of Time / Darkness" 
    )
  ) %>% 
  select(-link)

  
game_df = 
  metacritic_recode %>% 
  left_join(sales_rename, by = c("title", "platform")) %>% 
  filter(platform != "PC") %>% 
  filter(total_sale != 0) %>% 
  filter(!is.na(total_sale)) 

write_csv(game_df, "./data/game_2.csv")
```
