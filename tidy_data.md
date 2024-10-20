Tidy Data
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

## Pivot_longer function

This will tell you how to make your table more vertical instead of
horizontal Load the PULSE data. Since it’s a SAS file, you have to use
the HAVEN package

``` r
pulse_data =
  haven::read_sas("./data/public_pulse_data.sas7bdat") %>% 
  janitor::clean_names()
pulse_data
```

    ## # A tibble: 1,087 × 7
    ##       id   age sex    bdi_score_bl bdi_score_01m bdi_score_06m bdi_score_12m
    ##    <dbl> <dbl> <chr>         <dbl>         <dbl>         <dbl>         <dbl>
    ##  1 10003  48.0 male              7             1             2             0
    ##  2 10015  72.5 male              6            NA            NA            NA
    ##  3 10022  58.5 male             14             3             8            NA
    ##  4 10026  72.7 male             20             6            18            16
    ##  5 10035  60.4 male              4             0             1             2
    ##  6 10050  84.7 male              2            10            12             8
    ##  7 10078  31.3 male              4             0            NA            NA
    ##  8 10088  56.9 male              5            NA             0             2
    ##  9 10091  76.0 male              0             3             4             0
    ## 10 10092  74.2 female           10             2            11             6
    ## # ℹ 1,077 more rows

\##You can also go from wide format to long format— This will list the
variable bdi_score_bl (baseline) to bdi_score_12m (12 months) in a
vertical format. That’s the pivot function. The names_prefix tells the
code that everything in that column is a bdi_score, so it will only
output the time frames for it.I also changed those columns to just visit
in general.

``` r
pulse_data_tidy =
  pulse_data %>% 
  pivot_longer(
    bdi_score_bl:bdi_score_12m,
    names_to = "visit", 
    names_prefix = "bdi_score_",
    values_to = "bdi"
    )
pulse_data_tidy
```

    ## # A tibble: 4,348 × 5
    ##       id   age sex   visit   bdi
    ##    <dbl> <dbl> <chr> <chr> <dbl>
    ##  1 10003  48.0 male  bl        7
    ##  2 10003  48.0 male  01m       1
    ##  3 10003  48.0 male  06m       2
    ##  4 10003  48.0 male  12m       0
    ##  5 10015  72.5 male  bl        6
    ##  6 10015  72.5 male  01m      NA
    ##  7 10015  72.5 male  06m      NA
    ##  8 10015  72.5 male  12m      NA
    ##  9 10022  58.5 male  bl       14
    ## 10 10022  58.5 male  01m       3
    ## # ℹ 4,338 more rows

You can combine the above code into one!

``` r
pulse_data =
  haven::read_sas("./data/public_pulse_data.sas7bdat") %>% 
  janitor::clean_names() %>% 
  pivot_longer(
    bdi_score_bl:bdi_score_12m, 
    names_to = "visit",
    names_prefix = "bdi_score_",
    values_to = "bdi"
  )
pulse_data
```

    ## # A tibble: 4,348 × 5
    ##       id   age sex   visit   bdi
    ##    <dbl> <dbl> <chr> <chr> <dbl>
    ##  1 10003  48.0 male  bl        7
    ##  2 10003  48.0 male  01m       1
    ##  3 10003  48.0 male  06m       2
    ##  4 10003  48.0 male  12m       0
    ##  5 10015  72.5 male  bl        6
    ##  6 10015  72.5 male  01m      NA
    ##  7 10015  72.5 male  06m      NA
    ##  8 10015  72.5 male  12m      NA
    ##  9 10022  58.5 male  bl       14
    ## 10 10022  58.5 male  01m       3
    ## # ℹ 4,338 more rows

## The RELOCATE function changes orders of columns.

And then if you want the first 2 columns to be ID then VISIT, use the
relocate function.

``` r
pulse_data =
  haven::read_sas("./data/public_pulse_data.sas7bdat") %>% 
  janitor::clean_names() %>% 
  pivot_longer(
    bdi_score_bl:bdi_score_12m, 
    names_to = "visit",
    names_prefix = "bdi_score_",
    values_to = "bdi"
  ) %>% 
  relocate(id,visit) 
  
pulse_data
```

    ## # A tibble: 4,348 × 5
    ##       id visit   age sex     bdi
    ##    <dbl> <chr> <dbl> <chr> <dbl>
    ##  1 10003 bl     48.0 male      7
    ##  2 10003 01m    48.0 male      1
    ##  3 10003 06m    48.0 male      2
    ##  4 10003 12m    48.0 male      0
    ##  5 10015 bl     72.5 male      6
    ##  6 10015 01m    72.5 male     NA
    ##  7 10015 06m    72.5 male     NA
    ##  8 10015 12m    72.5 male     NA
    ##  9 10022 bl     58.5 male     14
    ## 10 10022 01m    58.5 male      3
    ## # ℹ 4,338 more rows

## How to have comparable values in a column

And finally, let’s say I want the visit column to all have comparable
values, so 00, 01, 06, 12m, I can use the mutate and recode functions.

``` r
pulse_data =
  haven::read_sas("./data/public_pulse_data.sas7bdat") %>% 
  janitor::clean_names() %>% 
  pivot_longer(
    bdi_score_bl:bdi_score_12m, 
    names_to = "visit",
    names_prefix = "bdi_score_",
    values_to = "bdi"
  ) %>% 
  relocate(id,visit) %>% 
  mutate(visit = recode(visit, "bl" = "00m"))
pulse_data
```

    ## # A tibble: 4,348 × 5
    ##       id visit   age sex     bdi
    ##    <dbl> <chr> <dbl> <chr> <dbl>
    ##  1 10003 00m    48.0 male      7
    ##  2 10003 01m    48.0 male      1
    ##  3 10003 06m    48.0 male      2
    ##  4 10003 12m    48.0 male      0
    ##  5 10015 00m    72.5 male      6
    ##  6 10015 01m    72.5 male     NA
    ##  7 10015 06m    72.5 male     NA
    ##  8 10015 12m    72.5 male     NA
    ##  9 10022 00m    58.5 male     14
    ## 10 10022 01m    58.5 male      3
    ## # ℹ 4,338 more rows

BEAUTIFUL!!

## Pivor_Wider is also a function

Let’s make up some data into a table!

``` r
analysis_result = 
  tibble(
    group = c("treatment", "treatment", "placebo", "placebo"), 
    time = c("pre", "post", "pre", "post"), 
    mean = c(4,8,3.5, 4)
  )
analysis_result
```

    ## # A tibble: 4 × 3
    ##   group     time   mean
    ##   <chr>     <chr> <dbl>
    ## 1 treatment pre     4  
    ## 2 treatment post    8  
    ## 3 placebo   pre     3.5
    ## 4 placebo   post    4

``` r
analysis_result %>% 
  pivot_wider(
    names_from = "time",
    values_from = "mean"
  )
```

    ## # A tibble: 2 × 3
    ##   group       pre  post
    ##   <chr>     <dbl> <dbl>
    ## 1 treatment   4       8
    ## 2 placebo     3.5     4

## Binding rows

Using the LotR (Lord of the Ring) data.

First step: import each table. This data set has 3 separate tables in
the file. So first I have to tell it the cells that have info for that
table. I added the mutate function to tell it to add a movie column and
label these as fellowship ring and other movie names, so I know which
movie it came from (which was listed in the excel sheet but with merged
cells).

``` r
fellowship_ring = 
  readxl::read_excel("./data/LotR_Words.xlsx", range = "B3:D6") %>% 
  mutate(movie = "fellowship_ring")
fellowship_ring
```

    ## # A tibble: 3 × 4
    ##   Race   Female  Male movie          
    ##   <chr>   <dbl> <dbl> <chr>          
    ## 1 Elf      1229   971 fellowship_ring
    ## 2 Hobbit     14  3644 fellowship_ring
    ## 3 Man         0  1995 fellowship_ring

``` r
two_towers = 
  readxl::read_excel("./data/LotR_Words.xlsx", range = "F3:H6") %>% 
  mutate(movie = "two_towers")
two_towers
```

    ## # A tibble: 3 × 4
    ##   Race   Female  Male movie     
    ##   <chr>   <dbl> <dbl> <chr>     
    ## 1 Elf       331   513 two_towers
    ## 2 Hobbit      0  2463 two_towers
    ## 3 Man       401  3589 two_towers

``` r
return_king = 
  readxl::read_excel("./data/LotR_Words.xlsx", range = "J3:L6") %>% 
  mutate(movie = "return_king")
return_king
```

    ## # A tibble: 3 × 4
    ##   Race   Female  Male movie      
    ##   <chr>   <dbl> <dbl> <chr>      
    ## 1 Elf       183   510 return_king
    ## 2 Hobbit      2  2673 return_king
    ## 3 Man       268  2459 return_king

Once I have all of the individual tables, I can bind them.

``` r
lotr_tidy = 
  bind_rows(fellowship_ring, two_towers, return_king)

lotr_tidy
```

    ## # A tibble: 9 × 4
    ##   Race   Female  Male movie          
    ##   <chr>   <dbl> <dbl> <chr>          
    ## 1 Elf      1229   971 fellowship_ring
    ## 2 Hobbit     14  3644 fellowship_ring
    ## 3 Man         0  1995 fellowship_ring
    ## 4 Elf       331   513 two_towers     
    ## 5 Hobbit      0  2463 two_towers     
    ## 6 Man       401  3589 two_towers     
    ## 7 Elf       183   510 return_king    
    ## 8 Hobbit      2  2673 return_king    
    ## 9 Man       268  2459 return_king

Now to clean it up visually. The first argument I put in relocate comes
first.

``` r
lotr_tidy = 
  bind_rows(fellowship_ring, two_towers, return_king) %>% 
  janitor::clean_names() %>% 
  relocate(movie)
lotr_tidy
```

    ## # A tibble: 9 × 4
    ##   movie           race   female  male
    ##   <chr>           <chr>   <dbl> <dbl>
    ## 1 fellowship_ring Elf      1229   971
    ## 2 fellowship_ring Hobbit     14  3644
    ## 3 fellowship_ring Man         0  1995
    ## 4 two_towers      Elf       331   513
    ## 5 two_towers      Hobbit      0  2463
    ## 6 two_towers      Man       401  3589
    ## 7 return_king     Elf       183   510
    ## 8 return_king     Hobbit      2  2673
    ## 9 return_king     Man       268  2459

Now to combine male and female as sex

``` r
lotr_tidy = 
  bind_rows(fellowship_ring, two_towers, return_king) %>% 
  janitor::clean_names() %>% 
  relocate(movie) %>% 
  pivot_longer(
    male:female , 
    names_to = "gender", 
    values_to = "words"
  )
lotr_tidy
```

    ## # A tibble: 18 × 4
    ##    movie           race   gender words
    ##    <chr>           <chr>  <chr>  <dbl>
    ##  1 fellowship_ring Elf    male     971
    ##  2 fellowship_ring Elf    female  1229
    ##  3 fellowship_ring Hobbit male    3644
    ##  4 fellowship_ring Hobbit female    14
    ##  5 fellowship_ring Man    male    1995
    ##  6 fellowship_ring Man    female     0
    ##  7 two_towers      Elf    male     513
    ##  8 two_towers      Elf    female   331
    ##  9 two_towers      Hobbit male    2463
    ## 10 two_towers      Hobbit female     0
    ## 11 two_towers      Man    male    3589
    ## 12 two_towers      Man    female   401
    ## 13 return_king     Elf    male     510
    ## 14 return_king     Elf    female   183
    ## 15 return_king     Hobbit male    2673
    ## 16 return_king     Hobbit female     2
    ## 17 return_king     Man    male    2459
    ## 18 return_king     Man    female   268
