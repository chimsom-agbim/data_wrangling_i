Data Manipulation
================

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

## Load in the FAS Litters Data

``` r
litters_df = read_csv("./data/FAS_litters.csv")
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (4): Group, Litter Number, GD0 weight, GD18 weight
    ## dbl (4): GD of Birth, Pups born alive, Pups dead @ birth, Pups survive
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_df = janitor::clean_names(litters_df)
litters_df
```

    ## # A tibble: 49 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>           <chr>      <chr>             <dbl>           <dbl>
    ##  1 Con7  #85             19.7       34.7                 20               3
    ##  2 Con7  #1/2/95/2       27         42                   19               8
    ##  3 Con7  #5/5/3/83/3-3   26         41.4                 19               6
    ##  4 Con7  #5/4/2/95/2     28.5       44.1                 19               5
    ##  5 Con7  #4/2/95/3-3     <NA>       <NA>                 20               6
    ##  6 Con7  #2/2/95/3-2     <NA>       <NA>                 20               6
    ##  7 Con7  #1/5/3/83/3-3/2 <NA>       <NA>                 20               9
    ##  8 Con8  #3/83/3-3       <NA>       <NA>                 20               9
    ##  9 Con8  #2/95/3         <NA>       <NA>                 20               8
    ## 10 Con8  #3/5/2/2/95     28.5       <NA>                 20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

## How to use the select function

Choose some columns and not others

``` r
select(litters_df, group, litter_number)
```

    ## # A tibble: 49 × 2
    ##    group litter_number  
    ##    <chr> <chr>          
    ##  1 Con7  #85            
    ##  2 Con7  #1/2/95/2      
    ##  3 Con7  #5/5/3/83/3-3  
    ##  4 Con7  #5/4/2/95/2    
    ##  5 Con7  #4/2/95/3-3    
    ##  6 Con7  #2/2/95/3-2    
    ##  7 Con7  #1/5/3/83/3-3/2
    ##  8 Con8  #3/83/3-3      
    ##  9 Con8  #2/95/3        
    ## 10 Con8  #3/5/2/2/95    
    ## # ℹ 39 more rows

This code here lets you select a range of columns

``` r
select(litters_df, group, gd0_weight:gd_of_birth)
```

    ## # A tibble: 49 × 4
    ##    group gd0_weight gd18_weight gd_of_birth
    ##    <chr> <chr>      <chr>             <dbl>
    ##  1 Con7  19.7       34.7                 20
    ##  2 Con7  27         42                   19
    ##  3 Con7  26         41.4                 19
    ##  4 Con7  28.5       44.1                 19
    ##  5 Con7  <NA>       <NA>                 20
    ##  6 Con7  <NA>       <NA>                 20
    ##  7 Con7  <NA>       <NA>                 20
    ##  8 Con8  <NA>       <NA>                 20
    ##  9 Con8  <NA>       <NA>                 20
    ## 10 Con8  28.5       <NA>                 20
    ## # ℹ 39 more rows

You can also get rid of columns

``` r
select(litters_df, -litter_number)
```

    ## # A tibble: 49 × 7
    ##    group gd0_weight gd18_weight gd_of_birth pups_born_alive pups_dead_birth
    ##    <chr> <chr>      <chr>             <dbl>           <dbl>           <dbl>
    ##  1 Con7  19.7       34.7                 20               3               4
    ##  2 Con7  27         42                   19               8               0
    ##  3 Con7  26         41.4                 19               6               0
    ##  4 Con7  28.5       44.1                 19               5               1
    ##  5 Con7  <NA>       <NA>                 20               6               0
    ##  6 Con7  <NA>       <NA>                 20               6               0
    ##  7 Con7  <NA>       <NA>                 20               9               0
    ##  8 Con8  <NA>       <NA>                 20               9               1
    ##  9 Con8  <NA>       <NA>                 20               8               0
    ## 10 Con8  28.5       <NA>                 20               8               0
    ## # ℹ 39 more rows
    ## # ℹ 1 more variable: pups_survive <dbl>

Renaming columns– put the new name first

``` r
select(litters_df, GROUP = group, LITTer_NUmBer = litter_number)
```

    ## # A tibble: 49 × 2
    ##    GROUP LITTer_NUmBer  
    ##    <chr> <chr>          
    ##  1 Con7  #85            
    ##  2 Con7  #1/2/95/2      
    ##  3 Con7  #5/5/3/83/3-3  
    ##  4 Con7  #5/4/2/95/2    
    ##  5 Con7  #4/2/95/3-3    
    ##  6 Con7  #2/2/95/3-2    
    ##  7 Con7  #1/5/3/83/3-3/2
    ##  8 Con8  #3/83/3-3      
    ##  9 Con8  #2/95/3        
    ## 10 Con8  #3/5/2/2/95    
    ## # ℹ 39 more rows

You can also use the select function for particular columns you want to
keep Type this into the console and see objects exported from other
packages above dplyr `?select_helpers`

``` r
select(litters_df, starts_with("gd"))
```

    ## # A tibble: 49 × 3
    ##    gd0_weight gd18_weight gd_of_birth
    ##    <chr>      <chr>             <dbl>
    ##  1 19.7       34.7                 20
    ##  2 27         42                   19
    ##  3 26         41.4                 19
    ##  4 28.5       44.1                 19
    ##  5 <NA>       <NA>                 20
    ##  6 <NA>       <NA>                 20
    ##  7 <NA>       <NA>                 20
    ##  8 <NA>       <NA>                 20
    ##  9 <NA>       <NA>                 20
    ## 10 28.5       <NA>                 20
    ## # ℹ 39 more rows

This code lets you put litter number as the first column and move the
others

``` r
select(litters_df, litter_number, everything())
```

    ## # A tibble: 49 × 8
    ##    litter_number   group gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr>           <chr> <chr>      <chr>             <dbl>           <dbl>
    ##  1 #85             Con7  19.7       34.7                 20               3
    ##  2 #1/2/95/2       Con7  27         42                   19               8
    ##  3 #5/5/3/83/3-3   Con7  26         41.4                 19               6
    ##  4 #5/4/2/95/2     Con7  28.5       44.1                 19               5
    ##  5 #4/2/95/3-3     Con7  <NA>       <NA>                 20               6
    ##  6 #2/2/95/3-2     Con7  <NA>       <NA>                 20               6
    ##  7 #1/5/3/83/3-3/2 Con7  <NA>       <NA>                 20               9
    ##  8 #3/83/3-3       Con8  <NA>       <NA>                 20               9
    ##  9 #2/95/3         Con8  <NA>       <NA>                 20               8
    ## 10 #3/5/2/2/95     Con8  28.5       <NA>                 20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

## You can also change the rows using the filter function

Here, you can filter a column for specific values

``` r
filter(litters_df, gd0_weight <22)
```

    ## # A tibble: 10 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>         <chr>      <chr>             <dbl>           <dbl>
    ##  1 Con7  #85           19.7       34.7                 20               3
    ##  2 Mod7  #59           17         33.4                 19               8
    ##  3 Mod7  #103          21.4       42.1                 19               9
    ##  4 Mod7  #8/110/3-2    .          .                    20               9
    ##  5 Mod7  #106          21.7       37.8                 20               5
    ##  6 Mod7  #62           19.5       35.9                 19               7
    ##  7 Mod8  #5/93/2       .          .                    19               8
    ##  8 Low8  #53           21.8       37.2                 20               8
    ##  9 Low8  #100          20         39.2                 20               8
    ## 10 Low8  #4/84         21.8       35.2                 20               4
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

If you wanted to find the values for a particular column, you can filter
for that value

``` r
filter(litters_df, gd_of_birth == 20)
```

    ## # A tibble: 32 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>           <chr>      <chr>             <dbl>           <dbl>
    ##  1 Con7  #85             19.7       34.7                 20               3
    ##  2 Con7  #4/2/95/3-3     <NA>       <NA>                 20               6
    ##  3 Con7  #2/2/95/3-2     <NA>       <NA>                 20               6
    ##  4 Con7  #1/5/3/83/3-3/2 <NA>       <NA>                 20               9
    ##  5 Con8  #3/83/3-3       <NA>       <NA>                 20               9
    ##  6 Con8  #2/95/3         <NA>       <NA>                 20               8
    ##  7 Con8  #3/5/2/2/95     28.5       <NA>                 20               8
    ##  8 Con8  #1/6/2/2/95-2   <NA>       <NA>                 20               7
    ##  9 Con8  #3/5/3/83/3-3-2 <NA>       <NA>                 20               8
    ## 10 Con8  #3/6/2/2/95-3   <NA>       <NA>                 20               7
    ## # ℹ 22 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

You can also search for an item that is NOT equal to that value

``` r
filter(litters_df, gd_of_birth != 20)
```

    ## # A tibble: 17 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>         <chr>      <chr>             <dbl>           <dbl>
    ##  1 Con7  #1/2/95/2     27         42                   19               8
    ##  2 Con7  #5/5/3/83/3-3 26         41.4                 19               6
    ##  3 Con7  #5/4/2/95/2   28.5       44.1                 19               5
    ##  4 Con8  #5/4/3/83/3   28         <NA>                 19               9
    ##  5 Con8  #2/2/95/2     <NA>       <NA>                 19               5
    ##  6 Mod7  #59           17         33.4                 19               8
    ##  7 Mod7  #103          21.4       42.1                 19               9
    ##  8 Mod7  #1/82/3-2     <NA>       <NA>                 19               6
    ##  9 Mod7  #3/83/3-2     <NA>       <NA>                 19               8
    ## 10 Mod7  #4/2/95/2     23.5       <NA>                 19               9
    ## 11 Mod7  #5/3/83/5-2   22.6       37                   19               5
    ## 12 Mod7  #94/2         24.4       42.9                 19               7
    ## 13 Mod7  #62           19.5       35.9                 19               7
    ## 14 Low7  #112          23.9       40.5                 19               6
    ## 15 Mod8  #5/93/2       .          .                    19               8
    ## 16 Mod8  #7/110/3-2    27.5       46                   19               8
    ## 17 Low8  #79           25.4       43.8                 19               8
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

You can search for two specifications at the same time as well

``` r
filter(litters_df, gd0_weight>=22, gd_of_birth == 20)
```

    ## # A tibble: 16 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>         <chr>      <chr>             <dbl>           <dbl>
    ##  1 Con8  #3/5/2/2/95   28.5       <NA>                 20               8
    ##  2 Mod7  #3/82/3-2     28         45.9                 20               5
    ##  3 Low7  #84/2         24.3       40.8                 20               8
    ##  4 Low7  #107          22.6       42.4                 20               9
    ##  5 Low7  #85/2         22.2       38.5                 20               8
    ##  6 Low7  #98           23.8       43.8                 20               9
    ##  7 Low7  #102          22.6       43.3                 20              11
    ##  8 Low7  #101          23.8       42.7                 20               9
    ##  9 Low7  #111          25.5       44.6                 20               3
    ## 10 Mod8  #97           24.5       42.8                 20               8
    ## 11 Mod8  #7/82-3-2     26.9       43.2                 20               7
    ## 12 Mod8  #2/95/2       28.5       44.5                 20               9
    ## 13 Mod8  #82/4         33.4       52.7                 20               8
    ## 14 Low8  #108          25.6       47.5                 20               8
    ## 15 Low8  #99           23.5       39                   20               6
    ## 16 Low8  #110          25.5       42.7                 20               7
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

You can search for particular group types

``` r
filter(litters_df, group == "Mod8")
```

    ## # A tibble: 7 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>         <chr>      <chr>             <dbl>           <dbl>
    ## 1 Mod8  #97           24.5       42.8                 20               8
    ## 2 Mod8  #5/93         <NA>       41.1                 20              11
    ## 3 Mod8  #5/93/2       .          .                    19               8
    ## 4 Mod8  #7/82-3-2     26.9       43.2                 20               7
    ## 5 Mod8  #7/110/3-2    27.5       46                   19               8
    ## 6 Mod8  #2/95/2       28.5       44.5                 20               9
    ## 7 Mod8  #82/4         33.4       52.7                 20               8
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

I cna also search for 2 groups

``` r
filter(litters_df, group %in% c("Con7", "Mod8"))
```

    ## # A tibble: 14 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>           <chr>      <chr>             <dbl>           <dbl>
    ##  1 Con7  #85             19.7       34.7                 20               3
    ##  2 Con7  #1/2/95/2       27         42                   19               8
    ##  3 Con7  #5/5/3/83/3-3   26         41.4                 19               6
    ##  4 Con7  #5/4/2/95/2     28.5       44.1                 19               5
    ##  5 Con7  #4/2/95/3-3     <NA>       <NA>                 20               6
    ##  6 Con7  #2/2/95/3-2     <NA>       <NA>                 20               6
    ##  7 Con7  #1/5/3/83/3-3/2 <NA>       <NA>                 20               9
    ##  8 Mod8  #97             24.5       42.8                 20               8
    ##  9 Mod8  #5/93           <NA>       41.1                 20              11
    ## 10 Mod8  #5/93/2         .          .                    19               8
    ## 11 Mod8  #7/82-3-2       26.9       43.2                 20               7
    ## 12 Mod8  #7/110/3-2      27.5       46                   19               8
    ## 13 Mod8  #2/95/2         28.5       44.5                 20               9
    ## 14 Mod8  #82/4           33.4       52.7                 20               8
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

You can also arrange the data set based on the values in a particular
column so it’s in lowest to highest etc.

``` r
arrange(litters_df, pups_born_alive)
```

    ## # A tibble: 49 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>         <chr>      <chr>             <dbl>           <dbl>
    ##  1 Con7  #85           19.7       34.7                 20               3
    ##  2 Low7  #111          25.5       44.6                 20               3
    ##  3 Low8  #4/84         21.8       35.2                 20               4
    ##  4 Con7  #5/4/2/95/2   28.5       44.1                 19               5
    ##  5 Con8  #2/2/95/2     <NA>       <NA>                 19               5
    ##  6 Mod7  #3/82/3-2     28         45.9                 20               5
    ##  7 Mod7  #5/3/83/5-2   22.6       37                   19               5
    ##  8 Mod7  #106          21.7       37.8                 20               5
    ##  9 Con7  #5/5/3/83/3-3 26         41.4                 19               6
    ## 10 Con7  #4/2/95/3-3   <NA>       <NA>                 20               6
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

You can also arrange so that within the order for one column, a second
column’s values can be in order. Just enter the columns in the order
that you want them arranged.So compared to the 2 values with 3, the
first value will be the lowest gd0_weight.

``` r
arrange(litters_df, pups_born_alive, gd0_weight)
```

    ## # A tibble: 49 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>         <chr>      <chr>             <dbl>           <dbl>
    ##  1 Con7  #85           19.7       34.7                 20               3
    ##  2 Low7  #111          25.5       44.6                 20               3
    ##  3 Low8  #4/84         21.8       35.2                 20               4
    ##  4 Mod7  #106          21.7       37.8                 20               5
    ##  5 Mod7  #5/3/83/5-2   22.6       37                   19               5
    ##  6 Mod7  #3/82/3-2     28         45.9                 20               5
    ##  7 Con7  #5/4/2/95/2   28.5       44.1                 19               5
    ##  8 Con8  #2/2/95/2     <NA>       <NA>                 19               5
    ##  9 Low8  #99           23.5       39                   20               6
    ## 10 Low7  #112          23.9       40.5                 19               6
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

## The Pipe function

My understanding is that it’s a sreies of iterative functions. I want to
create the edited file litters_data_selected.

``` r
litters_data_raw = read_csv("./data/FAS_litters.csv")
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (4): Group, Litter Number, GD0 weight, GD18 weight
    ## dbl (4): GD of Birth, Pups born alive, Pups dead @ birth, Pups survive
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_clean_name = janitor::clean_names(litters_data_raw)
litters_data_selected = select(litters_clean_name, -pups_survive)
litters_data_selected
```

    ## # A tibble: 49 × 7
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>           <chr>      <chr>             <dbl>           <dbl>
    ##  1 Con7  #85             19.7       34.7                 20               3
    ##  2 Con7  #1/2/95/2       27         42                   19               8
    ##  3 Con7  #5/5/3/83/3-3   26         41.4                 19               6
    ##  4 Con7  #5/4/2/95/2     28.5       44.1                 19               5
    ##  5 Con7  #4/2/95/3-3     <NA>       <NA>                 20               6
    ##  6 Con7  #2/2/95/3-2     <NA>       <NA>                 20               6
    ##  7 Con7  #1/5/3/83/3-3/2 <NA>       <NA>                 20               9
    ##  8 Con8  #3/83/3-3       <NA>       <NA>                 20               9
    ##  9 Con8  #2/95/3         <NA>       <NA>                 20               8
    ## 10 Con8  #3/5/2/2/95     28.5       <NA>                 20               8
    ## # ℹ 39 more rows
    ## # ℹ 1 more variable: pups_dead_birth <dbl>

I REALLY like this drop_na function because it will exclude rows where a
value for a particular column is gone. In this case, I will check values
in the gd0_weight column

``` r
litters_data_raw = read_csv("./data/FAS_litters.csv")
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (4): Group, Litter Number, GD0 weight, GD18 weight
    ## dbl (4): GD of Birth, Pups born alive, Pups dead @ birth, Pups survive
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_clean_name = janitor::clean_names(litters_data_raw)
litters_data_selected = select(litters_clean_name, -pups_survive)
litters_with_only_complete_value = drop_na(litters_data_selected, gd0_weight)
litters_with_only_complete_value
```

    ## # A tibble: 36 × 7
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>         <chr>      <chr>             <dbl>           <dbl>
    ##  1 Con7  #85           19.7       34.7                 20               3
    ##  2 Con7  #1/2/95/2     27         42                   19               8
    ##  3 Con7  #5/5/3/83/3-3 26         41.4                 19               6
    ##  4 Con7  #5/4/2/95/2   28.5       44.1                 19               5
    ##  5 Con8  #3/5/2/2/95   28.5       <NA>                 20               8
    ##  6 Con8  #5/4/3/83/3   28         <NA>                 19               9
    ##  7 Mod7  #59           17         33.4                 19               8
    ##  8 Mod7  #103          21.4       42.1                 19               9
    ##  9 Mod7  #3/82/3-2     28         45.9                 20               5
    ## 10 Mod7  #4/2/95/2     23.5       <NA>                 19               9
    ## # ℹ 26 more rows
    ## # ℹ 1 more variable: pups_dead_birth <dbl>

If you drop the specification for a column, you can get rid of all
missing data where an Na is entered

``` r
litters_data_raw = read_csv("./data/FAS_litters.csv")
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (4): Group, Litter Number, GD0 weight, GD18 weight
    ## dbl (4): GD of Birth, Pups born alive, Pups dead @ birth, Pups survive
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_clean_name = janitor::clean_names(litters_data_raw)
litters_data_selected = select(litters_clean_name, -pups_survive)
litters_with_only_complete_value = drop_na(litters_data_selected)
litters_with_only_complete_value
```

    ## # A tibble: 33 × 7
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>         <chr>      <chr>             <dbl>           <dbl>
    ##  1 Con7  #85           19.7       34.7                 20               3
    ##  2 Con7  #1/2/95/2     27         42                   19               8
    ##  3 Con7  #5/5/3/83/3-3 26         41.4                 19               6
    ##  4 Con7  #5/4/2/95/2   28.5       44.1                 19               5
    ##  5 Mod7  #59           17         33.4                 19               8
    ##  6 Mod7  #103          21.4       42.1                 19               9
    ##  7 Mod7  #3/82/3-2     28         45.9                 20               5
    ##  8 Mod7  #5/3/83/5-2   22.6       37                   19               5
    ##  9 Mod7  #8/110/3-2    .          .                    20               9
    ## 10 Mod7  #106          21.7       37.8                 20               5
    ## # ℹ 23 more rows
    ## # ℹ 1 more variable: pups_dead_birth <dbl>

Here’s the pipe operator. It basically prevents you from having to
retype the argument for the next function because it will automatically
use it. The pipe function means do this and then do this. I’m telling it
to do the same code as above.

``` r
litters_df = read_csv("./data/FAS_litters.csv") %>% 
  janitor::clean_names() %>% 
  select(-pups_survive) %>% 
  drop_na()
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (4): Group, Litter Number, GD0 weight, GD18 weight
    ## dbl (4): GD of Birth, Pups born alive, Pups dead @ birth, Pups survive
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_df
```

    ## # A tibble: 33 × 7
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>         <chr>      <chr>             <dbl>           <dbl>
    ##  1 Con7  #85           19.7       34.7                 20               3
    ##  2 Con7  #1/2/95/2     27         42                   19               8
    ##  3 Con7  #5/5/3/83/3-3 26         41.4                 19               6
    ##  4 Con7  #5/4/2/95/2   28.5       44.1                 19               5
    ##  5 Mod7  #59           17         33.4                 19               8
    ##  6 Mod7  #103          21.4       42.1                 19               9
    ##  7 Mod7  #3/82/3-2     28         45.9                 20               5
    ##  8 Mod7  #5/3/83/5-2   22.6       37                   19               5
    ##  9 Mod7  #8/110/3-2    .          .                    20               9
    ## 10 Mod7  #106          21.7       37.8                 20               5
    ## # ℹ 23 more rows
    ## # ℹ 1 more variable: pups_dead_birth <dbl>
