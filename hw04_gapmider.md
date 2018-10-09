Homework 04: Tidy data and joins
================
Seevasant Indran
08 October, 2018

A not so minimal guide to `dplyr` and `tidyr`.
----------------------------------------------

The fundamental processes to follow to understand the knowledge and insight a data provides are:

1.  **Data manipulation**
2.  Data visualization
3.  Statistical analysis/modeling
4.  Organization of results

#### Why Data Manipulation

> 80% of data analysis is spent on the process of cleaning and preparing the data. (Dasu and Johnson, 2003)

**Makes data compatible for processing such as mathematical functions, visualization, hence reveals information and insights.**

Hadley Wickham’s paper on Tidy Data provides a great explanation behind the concept of “tidy data”

#### Examples of a **Messy data** vs some **Tidy data**

picture mesy vs tidy data

According to Hadley:-

> Tidy data makes it easy for an analyst or a computer to extract needed variables because it provides a standard way of structuring a dataset. Compare. Table 3 to Table 1: in Table 1 you need to use different strategies to extract different variables. This slows analysis and invites errors. If you consider how many data analysis operations involve all of the values in a variable (every aggregation function), you can see how important it is to extract these values in a simple, standard way. Tidy data is particularly well suited for vectorised programming languages like R, because the layout ensures that values of different variables from the same observation are always paired.

### A `dplyr` walkthrough

Summary of the dplyr Functions
==============================

#### Quick data.frame

`tbl_df`

#### The most useful `dplyr` function

1.  `filter`
2.  `select`
3.  `mutate`
4.  `group_by`
5.  `summarise`
6.  `arrange`
7.  `rename`

#### the pipe operator

`%>%`

Relating the Functions
======================

### Tibble diff

`tbl_df` works similar to `data.table` in that it prints sensibly. Use `as_tibble()`

### {base} `R` and `dplyr`

List of dplyr functions and the base functions they're related to:

| Base Function        | dplyr Function(s)   | Special Powers                           |
|----------------------|---------------------|------------------------------------------|
| `subset`             | `filter` & `select` | filter rows & select columns             |
| `transform`          | `mutate`            | operate with columns not yet created     |
| `split`              | `group_by`          | splits without cutting                   |
| `lapply` + `do.call` | `summarise`         | apply and bind in a single bound         |
| `order` + `with`     | `arrange`           | "I only have to specify dataframe once?" |

### Chaining

`%>%` works similiarly to the `unix` pipe `|` and the `+` in `ggplot2`.

pic

*Basically previous input in chain supplied as argument 1 to function on right side.*

The 7 dplyr Functions
=====================

### Tibble diff

`tbl_df`

------------------------------------------------------------------------

### 7 most important `dplyr` for data manipulation

#### **`filter`**

-   Return Rows With Matching Conditions
    -   Useful Filter Functions
        -   `==`, `>`, `>=`
        -   `&`, `|`, `!`, `xor()`
        -   `is.na()`, `!is.na()`
    -   `between()`, `%in%`

Ussage - **`filter(.data, ...)`**
- Use `filter()` find rows/cases where conditions are true. Unlike base subsetting with `[`, rows where the condition evaluates to `NA` are dropped.

------------------------------------------------------------------------

#### **`select`**

-   Select/Rename Variables By Name.
    -   Useful Filter Functions
        -   `contains(match)`, `num_range(prefix, range)`
        -   `ends_with(match)`, `one_of(...)`
        -   `matches(matxh)`, `starts_with(match)`

Ussage - **`select_(.data, ...)`**
- As well as using existing functions like `:` and `c`, there are a number of special functions that only work inside select. To drop variables, use `-`. `select()` keeps only the variables mentioned; `rename()` keeps all variables.

------------------------------------------------------------------------

#### **`mutate`**

-   Add New Variables.

Ussage - **`mutate_(.data, ...)`**
- Mutate adds new variables and preserves existing; transmute drops existing variables.

------------------------------------------------------------------------

#### **`group_by`**

-   Group By One Or More Variables
    -   Useful Filter Functions
        -   `==`, `>`, `>=`
        -   `&`, `|`, `!`, `xor()`
        -   `is.na()`, `!is.na()`
        -   `between()`, `%in%`

Ussage - **`group_by(.data, ..., add = FALSE)`**
- When add = FALSE, the default, group\_by() will override existing groups. To add to the existing groups, use add = TRUE. Most data operations are done on groups defined by variables. `group_by()` takes an existing tbl and converts it into a grouped tbl where operations are performed "by group". `ungroup()` removes grouping.

------------------------------------------------------------------------

#### **`summarise`**

-   Reduces Multiple Values Down To A Single Value
    -   Useful Filter Functions
        -   Center: `mean()`, `median()`
        -   Spread: `sd()`, `IQR()`, `mad()`
        -   Range: `min()`, `max()`, `quantile()`
        -   Position: `first()`, `last()`, `nth()`
        -   Count: `n()`, `n_distinct()`
        -   Logical: `any()`

Ussage - **`summarise(.data, ...)`**
- `summarise()` is typically used on grouped data created by `group_by()`. The output will have one row for each group.

------------------------------------------------------------------------

#### **`arrange`**

-   Arrange Rows By Variables.

Ussage - **`arrange(.data, ..., .by_group = FALSE)`**
- Use desc() to sort a variable in descending order.

**The `dplyr` functions above when applied to a `data frame`, row names are silently dropped. To preserve, convert to an explicit variable with `tibble::rownames_to_column()`.**

------------------------------------------------------------------------

#### **`rename`**

-   Modify Names By Name, Not Position.

Ussage - **`rename(x, replace, warn_missing = TRUE, warn_duplicated = TRUE)`**
- warn\_missing = TRUE, print a message if any of the old names are not actually present in x. warn\_duplicated - TRUE print a message if any name appears more than once in x after the operation.

------------------------------------------------------------------------

#### Import datasets using the `readr` and the `{base}` `r` functions.

``` r
options(readr.num_columns = 0) 

# Import using the read_csv(), assign to `gapminder_school` variable. Lets call this school dataset

gapminder_school <- read.csv("https://query.data.world/s/bpbbjyj7t6k2u6owizb7tr4fm4h4fq", 
                             header = TRUE, check.names = FALSE) 
gapminder_mortality <- read_csv(file.path(getwd(), "Infant mortality rate per 1 000 births.csv"), col_names = TRUE)
```

``` r
dim(gapminder_school) # dimension of the data, 175 countries and 41 years
```

    ## [1] 175  41

``` r
# 
# gapminder_mortality_filtered <- gapminder_mortality %>% 
#   filter(country %in% gapminder$country)
# 
# (nrow(gapminder_mortality) - nrow(gapminder_mortality_filtered))
# 
# gapminder_mortality$country[which(!gapminder_mortality$country %in% gapminder_mortality_filtered$country)] %>% kable()
# 
# global_carbon <-readxl::read_xlsx(file.path(getwd(),"export_20181007_2016.xlsx"),  col_names = TRUE, skip = 1) 
#   
# names(global_carbon) <-  c("", names(global_carbon)[-1])
# remove_rownames(global_carbon)
# 
# global_carbon <- as.tibble(t(global_carbon))
# 
# names(global_carbon)
# 
```

Demos
=====

### Tibble diff

``` r
gapminder_school_df <- as.data.frame(gapminder_school) # change to dataframe for example
head(gapminder_school_df, n = 3) # displays (n = 3) the top dataset
```

    ##    Row Labels 1970 1971 1972 1973 1974 1975 1976 1977 1978 1979 1980 1981
    ## 1 Afghanistan  1.0  1.1  1.1  1.2  1.3  1.3  1.4  1.4  1.5  1.6  1.6  1.7
    ## 2     Albania  6.5  6.7  6.9  7.0  7.2  7.3  7.5  7.7  7.8  8.0  8.1  8.3
    ## 3     Algeria  1.9  2.0  2.1  2.2  2.3  2.4  2.5  2.7  2.8  2.9  3.1  3.3
    ##   1982 1983 1984 1985 1986 1987 1988 1989 1990 1991 1992 1993 1994 1995
    ## 1  1.8  1.8  1.9  2.0  2.0  2.1  2.1  2.2  2.2  2.3  2.3  2.4  2.4  2.5
    ## 2  8.4  8.5  8.7  8.8  9.0  9.1  9.2  9.3  9.4  9.5  9.6  9.7  9.8  9.9
    ## 3  3.4  3.6  3.8  3.9  4.1  4.3  4.5  4.7  4.8  5.0  5.2  5.3  5.5  5.6
    ##   1996 1997 1998 1999 2000 2001 2002 2003 2004 2005 2006 2007 2008 2009
    ## 1  2.6  2.6  2.7  2.7  2.8  2.9  2.9  3.0  3.0  3.1  3.1  3.2  3.3  3.3
    ## 2 10.0 10.1 10.2 10.3 10.4 10.4 10.5 10.6 10.7 10.7 10.8 10.9 10.9 11.0
    ## 3  5.8  5.9  6.1  6.2  6.3  6.5  6.6  6.7  6.8  7.0  7.1  7.2  7.3  7.3

This prints okay but the next one looks better, as explained above rownames are dropped, to preserve, convert to an explicit variable with `rownames_to_column()`

``` r
gapminder_school_tbl <- tbl_df(gapminder_school_df) # convert data into tibble diff
head(rownames_to_column(gapminder_school_tbl), n = 3)
```

    ## # A tibble: 3 x 42
    ##   rowname `Row Labels` `1970` `1971` `1972` `1973` `1974` `1975` `1976`
    ##   <chr>   <fct>         <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
    ## 1 1       Afghanistan     1      1.1    1.1    1.2    1.3    1.3    1.4
    ## 2 2       Albania         6.5    6.7    6.9    7      7.2    7.3    7.5
    ## 3 3       Algeria         1.9    2      2.1    2.2    2.3    2.4    2.5
    ## # ... with 33 more variables: `1977` <dbl>, `1978` <dbl>, `1979` <dbl>,
    ## #   `1980` <dbl>, `1981` <dbl>, `1982` <dbl>, `1983` <dbl>, `1984` <dbl>,
    ## #   `1985` <dbl>, `1986` <dbl>, `1987` <dbl>, `1988` <dbl>, `1989` <dbl>,
    ## #   `1990` <dbl>, `1991` <dbl>, `1992` <dbl>, `1993` <dbl>, `1994` <dbl>,
    ## #   `1995` <dbl>, `1996` <dbl>, `1997` <dbl>, `1998` <dbl>, `1999` <dbl>,
    ## #   `2000` <dbl>, `2001` <dbl>, `2002` <dbl>, `2003` <dbl>, `2004` <dbl>,
    ## #   `2005` <dbl>, `2006` <dbl>, `2007` <dbl>, `2008` <dbl>, `2009` <dbl>

### rename

Use the `dplyr::rename` to rename the "Row Labels"" column to "country""

``` r
## The dplyr way, rename "Row Labels" to "country" in school dataset
gapminder_school <- gapminder_school %>%
  rename(country = "Row Labels")

## The base R way is more complicated, rename column `Row Lables` to `country` AND rename the remaining column minus the first column as it was previously.

# names(gapminder_school) <- c("country", names(gapminder_school)[-1]) 

# or 

# colnames(gapminder_school)[colnames(gapminder_school)=="Row Labels"] <- "country"

head(gapminder_school, n= 2)
```

    ##       country 1970 1971 1972 1973 1974 1975 1976 1977 1978 1979 1980 1981
    ## 1 Afghanistan  1.0  1.1  1.1  1.2  1.3  1.3  1.4  1.4  1.5  1.6  1.6  1.7
    ## 2     Albania  6.5  6.7  6.9  7.0  7.2  7.3  7.5  7.7  7.8  8.0  8.1  8.3
    ##   1982 1983 1984 1985 1986 1987 1988 1989 1990 1991 1992 1993 1994 1995
    ## 1  1.8  1.8  1.9  2.0    2  2.1  2.1  2.2  2.2  2.3  2.3  2.4  2.4  2.5
    ## 2  8.4  8.5  8.7  8.8    9  9.1  9.2  9.3  9.4  9.5  9.6  9.7  9.8  9.9
    ##   1996 1997 1998 1999 2000 2001 2002 2003 2004 2005 2006 2007 2008 2009
    ## 1  2.6  2.6  2.7  2.7  2.8  2.9  2.9  3.0  3.0  3.1  3.1  3.2  3.3  3.3
    ## 2 10.0 10.1 10.2 10.3 10.4 10.4 10.5 10.6 10.7 10.7 10.8 10.9 10.9 11.0

### filter()

``` r
## School dataset has more countries (175) that the gapminder dataset, filter country from "gapminder_school" dataset with gapminder, to removed countries not in both dataset

gapminder_school_filtered <- gapminder_school %>% 
  filter(country %in% gapminder$country)

## How many country are filtered ?

(nrow(gapminder_school) - nrow(gapminder_school_filtered))
```

    ## [1] 42

### select()

``` r
## The gapminder dataset has year till 2007, however the school dataset has year till 2009. Use select to filter the dates till 2007

gapminder_school_filtered <- gapminder_school_filtered %>% 
  select("country",as.character(unique(gapminder$year)[-c(1:4)])) # selects the first column and the years present from the gapminder dataset.

head(gapminder_school_filtered)
```

    ##       country 1972 1977 1982 1987 1992 1997 2002 2007
    ## 1 Afghanistan  1.1  1.4  1.8  2.1  2.3  2.6  2.9  3.2
    ## 2     Albania  6.9  7.7  8.4  9.1  9.6 10.1 10.5 10.9
    ## 3     Algeria  2.1  2.7  3.4  4.3  5.2  5.9  6.6  7.2
    ## 4      Angola  2.5  3.1  3.8  4.4  4.8  5.3  5.7  6.0
    ## 5   Argentina  7.3  8.0  8.6  9.1  9.6 10.1 10.6 11.0
    ## 6   Australia 10.2 10.7 11.1 11.4 11.7 11.9 12.1 12.3

Now we have a matching year but we have a problem, our dataset is not in tidy format. We have to fix that later its difficult to work with a messydataset. For example.

### arrange()

``` r
gapminder_school_filtered %>%
  arrange (`2007`) %>% # arrange mean by lowest to highest
  head (n = 5)
```

    ##        country 1972 1977 1982 1987 1992 1997 2002 2007
    ## 1        Niger  0.5  0.6  0.8  1.1  1.4  1.8  2.2  2.5
    ## 2         Mali  0.9  1.1  1.4  1.7  1.9  2.2  2.4  2.6
    ## 3 Burkina Faso  0.7  0.9  1.2  1.5  1.8  2.1  2.5  2.8
    ## 4  Afghanistan  1.1  1.4  1.8  2.1  2.3  2.6  2.9  3.2
    ## 5      Somalia  1.2  1.5  1.8  2.2  2.5  2.8  3.0  3.2

Some live commetary.. Look at Niger, mean years in school for people aged between 25 - 34 is just 2.5 years!! in 2007.

### chaining()

``` r
## {base} R way to filter countries that are in both gapminder and gapminder school dataset and store into country
cntry <- unique(gapminder$country)[unique(gapminder$country) %in% # %in% same as match()
                                 gapminder_school_filtered$country] # This returns a logical vector. It is then used to subsets the gapminder country dataset 

## This subsets the gapminder dataset 
gpmd_cont <- gapminder %>% 
  filter(country %in% cntry) %>% 
  subset(!duplicated(country)) %>% 
  select("country","continent")
```

### mutate()

``` r
gapminder_school_filtered <- gapminder_school_filtered %>%
  mutate(continent = gpmd_cont$continent)
```

### summarise()

``` r
gapminder_school_filtered %>% 
  summarise("1972" = mean(gapminder_school_filtered$`1972`),
            "1977" = mean(gapminder_school_filtered$`1977`),
            "1982" = mean(gapminder_school_filtered$`1982`),
            "1987" = mean(gapminder_school_filtered$`1987`),
            "1992" = mean(gapminder_school_filtered$`1992`),
            "1997" = mean(gapminder_school_filtered$`1997`),
            "2002" = mean(gapminder_school_filtered$`2002`),
            "2007" = mean(gapminder_school_filtered$`2007`),
            ) 
```

    ##      1972     1977     1982     1987     1992     1997     2002    2007
    ## 1 5.02782 5.745113 6.435338 7.076692 7.630075 8.153383 8.638346 9.06015

### group\_by()

``` r
gapminder_school_filtered %>% 
  group_by(continent) %>% 
  summarise("1972" = mean(gapminder_school_filtered$`1972`),
            "1977" = mean(gapminder_school_filtered$`1977`),
            "1982" = mean(gapminder_school_filtered$`1982`),
            "1987" = mean(gapminder_school_filtered$`1987`),
            "1992" = mean(gapminder_school_filtered$`1992`),
            "1997" = mean(gapminder_school_filtered$`1997`),
            "2002" = mean(gapminder_school_filtered$`2002`),
            "2007" = mean(gapminder_school_filtered$`2007`),
            ) 
```

    ## # A tibble: 5 x 9
    ##   continent `1972` `1977` `1982` `1987` `1992` `1997` `2002` `2007`
    ##   <fct>      <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
    ## 1 Africa      5.03   5.75   6.44   7.08   7.63   8.15   8.64   9.06
    ## 2 Americas    5.03   5.75   6.44   7.08   7.63   8.15   8.64   9.06
    ## 3 Asia        5.03   5.75   6.44   7.08   7.63   8.15   8.64   9.06
    ## 4 Europe      5.03   5.75   6.44   7.08   7.63   8.15   8.64   9.06
    ## 5 Oceania     5.03   5.75   6.44   7.08   7.63   8.15   8.64   9.06

### Super `%>%` pipe

``` r
read.csv("https://query.data.world/s/bpbbjyj7t6k2u6owizb7tr4fm4h4fq", 
         header = TRUE, check.names = FALSE) %>%
  rename(country = "Row Labels")  %>% 
  filter(country %in% gapminder$country) %>% 
  select("country",as.character(unique(gapminder$year)[-c(1:4)])) %>%
  mutate(continent = gpmd_cont$continent) %>% 
  group_by(continent) %>% 
  summarise("1972" = mean(gapminder_school_filtered$`1972`),
            "1977" = mean(gapminder_school_filtered$`1977`),
            "1982" = mean(gapminder_school_filtered$`1982`),
            "1987" = mean(gapminder_school_filtered$`1987`),
            "1992" = mean(gapminder_school_filtered$`1992`),
            "1997" = mean(gapminder_school_filtered$`1997`),
            "2002" = mean(gapminder_school_filtered$`2002`),
            "2007" = mean(gapminder_school_filtered$`2007`),
            ) 
```

    ## # A tibble: 5 x 9
    ##   continent `1972` `1977` `1982` `1987` `1992` `1997` `2002` `2007`
    ##   <fct>      <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
    ## 1 Africa      5.03   5.75   6.44   7.08   7.63   8.15   8.64   9.06
    ## 2 Americas    5.03   5.75   6.44   7.08   7.63   8.15   8.64   9.06
    ## 3 Asia        5.03   5.75   6.44   7.08   7.63   8.15   8.64   9.06
    ## 4 Europe      5.03   5.75   6.44   7.08   7.63   8.15   8.64   9.06
    ## 5 Oceania     5.03   5.75   6.44   7.08   7.63   8.15   8.64   9.06

``` r
# 
# gapminder_school_filtered %>% ggplot(aes (x = continent , y = gapminder_school_filtered$`1972`) + geom_bar(position = "dodge", stat = "identity")
```
