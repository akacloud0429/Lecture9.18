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
litters_df = janitor::clean_names(litters_df)
names(litters_df)
```

## `select` variables from dataframe

We apply `select` on **columns**.

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

## `filter`

We use `filter` to filter out **rows**.

- `filter` will turn rows that return **TRUE** to all conditions

``` r
filter(litters_df, gd_of_birth == 20, pups_born_alive >= 6, pups_dead_birth <= 0)
```

- filter for non-matches

``` r
filter(litters_df, pups_born_alive !=6)
```

- match to a set {}

``` r
# return group that in Condition7 and Condition8
filter(litters_df, group %in% c("Con7", "Con8"))
```

- `filter` out missing rows with `drop_na()`

``` r
drop_na(litters_df) #for the entire data set
drop_na(litters_df, gd0_weight) #for specific rows
```

# `mutate`

We use `mutate()` to create or modify variables.

- create new variables
- make (modify) variable lowercase

``` r
mutate(
  litters_df, 
  wt_gain = gd18_weight - gd0_weight,
  group = str_to_lower(group)
) #Con --> con
```

# `arrange`

``` r
# rank group first, then pups_born_alive from lowest to highest
arrange(litters_df, group, pups_born_alive)
arrange(litters_df, desc(group), pups_born_alive)
```

# PIPES

- pipes put the `select`,`filter`,`mutate` and `arrange` together.

Some bad examples will be:

``` r
# Too confused
litters_df_raw = read_csv("../../data_import_examples/FAS_litters.csv", na = c("NA", ".", ""))
litters_df_clean_names = janitor::clean_names(litters_df_raw)
litters_df_selected_cols = select(litters_df_clean_names, -pups_survive)
litters_df_with_vars = 
  mutate(
    litters_df_selected_cols, 
    wt_gain = gd18_weight - gd0_weight,
    group = str_to_lower(group))
litters_df_with_vars_without_missing = 
  drop_na(litters_df_with_vars, wt_gain)
litters_df_with_vars_without_missing
```

OR

``` r
# Has to be read inside
litters_df_clean = 
  drop_na(
    mutate(
      select(
        janitor::clean_names(
          read_csv("../../data_import_examples/FAS_litters.csv", na = c("NA", ".", ""))
          ), 
      -pups_survive
      ),
    wt_gain = gd18_weight - gd0_weight,
    group = str_to_lower(group)
    ),
  wt_gain
  )
```

We use pipes to avoid all these: pipes **pass the result** of the
previous function to the next function

``` r
litters_df = 
  read_csv("../../data_import_examples/FAS_litters.csv", na = c("NA", ".", "")) |> 
  janitor::clean_names() |> 
  select(-pups_survive) |> 
  mutate(
    wt_gain = gd18_weight - gd0_weight,
    group = str_to_lower(group)) |> 
  drop_na(wt_gain)
```
