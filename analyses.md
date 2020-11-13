Analyses
================
11/8/2020

This document contains all of our analyses.

``` r
game_df = read_csv("./data/game_2.csv")

game_df = game_df %>% 
  separate(release_date, into = c("month_day","year"), sep = ",") %>% 
  separate(genre, into = "genre", sep = ",")
```

## 1\. User score vs Metascore

## 2\. Total sales by genre

## 3\. Which time period generated the most popular games?

## 4\. Which developers released the most/least popular games or the highest/lowest scored?

## 5\. Preference of users of different platform

## 6\. Preference of users from different regions

## 7\. What factors influence sales? How do they influence it?
