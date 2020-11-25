Tidy VGChartz
================
11/19/2020

``` r
game_df = 
  tibble(
    path = list.files("./data/genre_sales"))  
#   mutate(genre = str_extract(path, "[^.]+"),
#          path = str_c("data/genre_sales/", path),
#          data = map(.x = path, ~read_csv(.x))) %>% 
#   unnest(data) %>% 
#   select(-path) %>% 
#   select(title, console, genre, release_date, everything()) %>%
#   mutate_at(9:13, funs(gsub("m","",.))) %>% 
#   mutate_at(7:13, funs(gsub("N/A",NA,.))) %>% 
#   mutate_at(7:13, funs(as.numeric(.))) %>% 
#   mutate(release_date = lubridate::dmy(release_date)) %>% 
#   distinct() %>% 
#   filter(total_sale != 0) %>% 
#   filter(!is.na(total_sale)) 
# 
# write_csv(game_df, "./data/vgchartz.csv")
```
