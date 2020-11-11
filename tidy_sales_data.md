Tidy sales data
================
11/10/2020

This document contains all of our analyses.

Load sales data and remove `m` in each sale column

``` r
sales_data = 
  read.csv("data/sales.csv") %>% 
  separate(total_sale, into = c("total_sale", NA), sep = "[m]") %>% 
  separate(na_sale, into = c("na_sale", NA), sep = "[m]") %>% 
  separate(pal_sale, into = c("pal_sale", NA), sep = "[m]") %>% 
  separate(japan_sale, into = c("japan_sale", NA), sep = "[m]") %>% 
  separate(other_sale, into = c("other_sale", NA), sep = "[m]")
```

    ## Warning: Expected 2 pieces. Missing pieces filled with `NA` in 19200 rows [1, 2,
    ## 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, ...].
    
    ## Warning: Expected 2 pieces. Missing pieces filled with `NA` in 19200 rows [1, 2,
    ## 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, ...].
    
    ## Warning: Expected 2 pieces. Missing pieces filled with `NA` in 19200 rows [1, 2,
    ## 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, ...].
    
    ## Warning: Expected 2 pieces. Missing pieces filled with `NA` in 19200 rows [1, 2,
    ## 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, ...].
    
    ## Warning: Expected 2 pieces. Missing pieces filled with `NA` in 19200 rows [1, 2,
    ## 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, ...].

To list the unique name of console. There are 39 of them.

``` r
sales_data %>% 
  distinct(console)
```

    ##    console
    ## 1      Wii
    ## 2      NES
    ## 3       GB
    ## 4       DS
    ## 5     X360
    ## 6      PS2
    ## 7     SNES
    ## 8      PS3
    ## 9      PS4
    ## 10     3DS
    ## 11     GBA
    ## 12      NS
    ## 13     N64
    ## 14      PS
    ## 15    XOne
    ## 16      XB
    ## 17      PC
    ## 18    2600
    ## 19     PSP
    ## 20    WiiU
    ## 21      GC
    ## 22     GBC
    ## 23     GEN
    ## 24     PSN
    ## 25     PSV
    ## 26      DC
    ## 27     SAT
    ## 28     SCD
    ## 29      WS
    ## 30      VC
    ## 31      NG
    ## 32      WW
    ## 33     PCE
    ## 34     XBL
    ## 35     3DO
    ## 36      GG
    ## 37     OSX
    ## 38    PCFX
    ## 39     Mob

Rename console in sales data

\*\*NOTE: The following console cannot be found to match in the
`metacritic.csv` file: 2600, GBC, GEN, PSN, SAT, SCD, WS, VC, NG, WW,
PCE, XBL, 3D0, GG, OSX, PCFX, Mob

``` r
console_df = 
  recode(sales_data$console, 
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
```

New name for console values

``` r
sales_df = 
  sales_data %>% 
  mutate(console = console_df)

write_csv(sales_df, "data/sales_rename.csv")
```
