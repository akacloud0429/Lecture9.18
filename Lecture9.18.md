Lecture9.18
================

``` r
library(readr)
library(tidyverse)
```

# Data Manipulation

First, import the data set & clean the variable names.

``` r
litters_df = read_csv("../../data_import_examples/FAS_litters.csv", na = c("NA", ".", ""))
## Rows: 49 Columns: 8
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (2): Group, Litter Number
## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
litters_df = janitor::clean_names(litters_df)
names(litters_df)
## [1] "group"           "litter_number"   "gd0_weight"      "gd18_weight"    
## [5] "gd_of_birth"     "pups_born_alive" "pups_dead_birth" "pups_survive"
```

## `select` variables from data frame

- select specific columns

``` r
select(litters_df, group, litter_number, gd0_weight)
```

- select a range of columns

``` r
select(litters_df, group:gd_of_birth)
```

- select by remove certain columns

``` r
select(litters_df, -group, -litter_number, -gd0_weight)
select(litters_df, -(group:gd_of_birth))
```

- select by prefix

``` r
select(litters_df, group, starts_with("gd"))
```

- select by suffix

``` r
select(litters_df, group, contains("gd"))
```

- rearrange by selecting

``` r
select(litters_df, starts_with("gd"), group)
select(litters_df, litter_number, everything()) 
```

put `litternumber` piror to else, same to

``` r
relocate(litters_df, litter_number)
```

- rename by selecting

``` r
#rename group variable
select(litters_df, GROUP = group, everything())
rename(litters_df, GROUP = group) # same function
```
