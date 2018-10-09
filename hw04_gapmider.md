Homework 04: Tidy data and joins
================
Seevasant Indran
09 October, 2018

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

### A `tidyr` walkthrough

Summary of the 2 Main Functions
===============================

List of **tidyr** functions and the **reshape2** functions they're related to:

| reshape2 Function | tidyr Function | Special Powers |
|-------------------|----------------|----------------|
| `melt`            | `gather`       | long format\*  |
| `dcast`           | `spread`       | wide format\*  |

The `dplyr` Functions
=====================

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

`dplyr` Demos
=============

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

The `tidyr` Functions
=====================

### 2 most important `tidyr` for data manipulation

------------------------------------------------------------------------

#### **`gather`**

-   Gather Columns Into Key-Value Pairs
    -   Useful Filter Functions
        -   data expression like `x` or an expression like `x:y` or `c(x, y)`. In a data expression, you can only refer to columns from the data frame. Everything else is a context expression in which you can only refer to objects that you have defined with `<-`. -`col1:col3` is a data expression that refers to data columns, while `seq(start, end)` is a context expression that refers to objects from the contexts.
        -   `c(x, !! x)` selects the x column within the data frame and the column referred to by the object x defined in the context (which can contain either a column name as string or a column position)

Ussage - **`gather(data, key = "key", value = "value", ..., na.rm = FALSE, convert = FALSE, factor_key = FALSE)`**
- `key, value`, names of new key and value columns, as strings or symbols. `...`, A selection of columns. If empty, all variables are selected. You can supply bare variable names, select all variables between x and z with x:z, exclude y with -y. `na.rm`, If TRUE, will remove rows from output where the value column in NA.`convert`, If TRUE will automatically run type.convert() on the key column. This is useful if the column names are actually numeric, integer, or logical. `factor_key`, If FALSE, the default, the key values will be stored as a character vector. If TRUE, will be stored as a factor, which preserves the original ordering of the columns.

------------------------------------------------------------------------

#### **`spread`**

-   Spread A Key-Value Pair Across Multiple Columns
    -   Useful Filter Functions
        -   data expression like `x` or an expression like `x:y` or `c(x, y)`. In a data expression, you can only refer to columns from the data frame. Everything else is a context expression in which you can only refer to objects that you have defined with `<-`. -`col1:col3` is a data expression that refers to data columns, while `seq(start, end)` is a context expression that refers to objects from the contexts.
        -   `c(x, !! x)` selects the x column within the data frame and the column referred to by the object x defined in the context (which can contain either a column name as string or a column position)

Ussage - **`spread(data, key, value, fill = NA, convert = FALSE, drop = TRUE, sep = NULL)`**
- `fill`, If set, missing values will be replaced with this value. Note that there are two types of missingness in the input: explicit missing values (i.e. NA), and implicit missings, rows that simply aren't present. Both types of missing value will be replaced by fill. `convert`, If TRUE, type.convert() with asis = TRUE will be run on each of the new columns. This is useful if the value column was a mix of variables that was coerced to a string. If the class of the value column was factor or date, note that will not be true of the new columns that are produced, which are coerced to character before type conversion. `drop`, If FALSE, will keep factor levels that don't appear in the data, filling in missing combinations with fill. `sep`, If NULL, the column names will be taken from the values of key variable. If non-NULL, the column names will be given by "<key_name><sep><key_value>".

\*[Hadley notes](http://vita.had.co.nz/papers/tidy-data.pdf) these terms are imprecise but good enough for my little noodle

------------------------------------------------------------------------

`tidyr` Demos
=============

### Some Data

``` r
# Look at the messydata
head(gapminder_school_filtered)
```

    ##       country 1972 1977 1982 1987 1992 1997 2002 2007 continent
    ## 1 Afghanistan  1.1  1.4  1.8  2.1  2.3  2.6  2.9  3.2      Asia
    ## 2     Albania  6.9  7.7  8.4  9.1  9.6 10.1 10.5 10.9    Europe
    ## 3     Algeria  2.1  2.7  3.4  4.3  5.2  5.9  6.6  7.2    Africa
    ## 4      Angola  2.5  3.1  3.8  4.4  4.8  5.3  5.7  6.0    Africa
    ## 5   Argentina  7.3  8.0  8.6  9.1  9.6 10.1 10.6 11.0  Americas
    ## 6   Australia 10.2 10.7 11.1 11.4 11.7 11.9 12.1 12.3   Oceania

### gather()

``` r
gapminder_tidyschool <- gapminder_school_filtered %>% 
  gather(year, meanSchool, -c("country", "continent")) 

# Some sanity check
if (nrow(gapminder_school_filtered) * 8 == nrow(gapminder_tidyschool)) { # there are 8 years
  head(gapminder_tidyschool)
} else {
stop("n() rows dont match table")
}
```

    ##       country continent year meanSchool
    ## 1 Afghanistan      Asia 1972        1.1
    ## 2     Albania    Europe 1972        6.9
    ## 3     Algeria    Africa 1972        2.1
    ## 4      Angola    Africa 1972        2.5
    ## 5   Argentina  Americas 1972        7.3
    ## 6   Australia   Oceania 1972       10.2

``` r
# Convert year into interger
gapminder_tidyschool$year <- as.integer(gapminder_tidyschool$year)
```

We have a tidy dataset of gapminder mean years in school datase.

### spread()

``` r
gapminder_school_filtered %>% 
  gather(year, meanSchool, -c("country", "continent")) %>% 
  spread(year, meanSchool) %>% 
  head()
```

    ##       country continent 1972 1977 1982 1987 1992 1997 2002 2007
    ## 1 Afghanistan      Asia  1.1  1.4  1.8  2.1  2.3  2.6  2.9  3.2
    ## 2     Albania    Europe  6.9  7.7  8.4  9.1  9.6 10.1 10.5 10.9
    ## 3     Algeria    Africa  2.1  2.7  3.4  4.3  5.2  5.9  6.6  7.2
    ## 4      Angola    Africa  2.5  3.1  3.8  4.4  4.8  5.3  5.7  6.0
    ## 5   Argentina  Americas  7.3  8.0  8.6  9.1  9.6 10.1 10.6 11.0
    ## 6   Australia   Oceania 10.2 10.7 11.1 11.4 11.7 11.9 12.1 12.3

Additional Demos
================

### gather() part 2

``` r
# Use gather and create a column year and meanschool and use columb 2 to 9 as key

gapminder_school_filtered %>% 
  gather(key = year, value =  meanSchool, 2:9) %>% 
  arrange(country) %>% 
  head()
```

    ##       country continent year meanSchool
    ## 1 Afghanistan      Asia 1972        1.1
    ## 2 Afghanistan      Asia 1977        1.4
    ## 3 Afghanistan      Asia 1982        1.8
    ## 4 Afghanistan      Asia 1987        2.1
    ## 5 Afghanistan      Asia 1992        2.3
    ## 6 Afghanistan      Asia 1997        2.6

### gather() part 3 - define year using subset of `colnames`

``` r
gapminder_school_filtered %>% 
  gather(key = year, value =  meanSchool,  
         names(gapminder_school_filtered)[2:9]) %>% 
  head()
```

    ##       country continent year meanSchool
    ## 1 Afghanistan      Asia 1972        1.1
    ## 2     Albania    Europe 1972        6.9
    ## 3     Algeria    Africa 1972        2.1
    ## 4      Angola    Africa 1972        2.5
    ## 5   Argentina  Americas 1972        7.3
    ## 6   Australia   Oceania 1972       10.2

### spread() - part 2 with continent has the `colnames` and `meanSchool` as value

``` r
gapminder_school_filtered %>% 
  gather(year, meanSchool, -c("country", "continent")) %>% 
  spread(continent, meanSchool) %>%
  head()
```

    ##       country year Africa Americas Asia Europe Oceania
    ## 1 Afghanistan 1972     NA       NA  1.1     NA      NA
    ## 2 Afghanistan 1977     NA       NA  1.4     NA      NA
    ## 3 Afghanistan 1982     NA       NA  1.8     NA      NA
    ## 4 Afghanistan 1987     NA       NA  2.1     NA      NA
    ## 5 Afghanistan 1992     NA       NA  2.3     NA      NA
    ## 6 Afghanistan 1997     NA       NA  2.6     NA      NA

A `dplyr` join walkthrough
==========================

### Summary of the 9 joint function

1.  **`inner_join()`**
2.  **`semi_join()`**
3.  **`left_join()`**
4.  **`anti_join()`**
5.  **`right_join()`**
6.  **`full_join()`**
7.  **`union`**
8.  **`intersect`**
9.  **`setdiff`**

### inner\_join()

> inner\_join(x, y): Return all rows from x where there are matching values in y, and all columns from x and y. If there are multiple matches between x and y, all combination of the matches are returned. This is a mutating join.

``` r
gapminder2 <- inner_join(gapminder, gapminder_tidyschool)
  head(gapminder2)
```

    ## # A tibble: 6 x 7
    ##   country     continent  year lifeExp      pop gdpPercap meanSchool
    ##   <chr>       <fct>     <int>   <dbl>    <int>     <dbl>      <dbl>
    ## 1 Afghanistan Asia       1972    36.1 13079460      740.        1.1
    ## 2 Afghanistan Asia       1977    38.4 14880372      786.        1.4
    ## 3 Afghanistan Asia       1982    39.9 12881816      978.        1.8
    ## 4 Afghanistan Asia       1987    40.8 13867957      852.        2.1
    ## 5 Afghanistan Asia       1992    41.7 16317921      649.        2.3
    ## 6 Afghanistan Asia       1997    41.8 22227415      635.        2.6

Match and join `gapminder` dataset which has 142 country into `gapminder_tidyschool` which has 133 country which should only match 1064, which looks correct.

### semi\_join()

> semi\_join(x, y): Return all rows from x where there are matching values in y, keeping just columns from x. A semi join differs from an inner join because an inner join will return one row of x for each matching row of y, where a semi join will never duplicate rows of x. This is a filtering join.

``` r
semi_join(gapminder, gapminder_tidyschool)
```

    ## # A tibble: 1,064 x 6
    ##    country     continent  year lifeExp      pop gdpPercap
    ##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
    ##  1 Afghanistan Asia       1972    36.1 13079460      740.
    ##  2 Afghanistan Asia       1977    38.4 14880372      786.
    ##  3 Afghanistan Asia       1982    39.9 12881816      978.
    ##  4 Afghanistan Asia       1987    40.8 13867957      852.
    ##  5 Afghanistan Asia       1992    41.7 16317921      649.
    ##  6 Afghanistan Asia       1997    41.8 22227415      635.
    ##  7 Afghanistan Asia       2002    42.1 25268405      727.
    ##  8 Afghanistan Asia       2007    43.8 31889923      975.
    ##  9 Albania     Europe     1972    67.7  2263554     3313.
    ## 10 Albania     Europe     1977    68.9  2509048     3533.
    ## # ... with 1,054 more rows

Notice there is no meanSchool column, this returns all matches of x and y whist retaining the same column of original x. Like filter().

### left\_join()

> left\_join(x, y): Return all rows from x, and all columns from x and y. If there are multiple matches between x and y, all combination of the matches are returned. This is a mutating join.

``` r
left_join(gapminder, gapminder_tidyschool)
```

    ## # A tibble: 1,704 x 7
    ##    country     continent  year lifeExp      pop gdpPercap meanSchool
    ##    <chr>       <fct>     <int>   <dbl>    <int>     <dbl>      <dbl>
    ##  1 Afghanistan Asia       1952    28.8  8425333      779.       NA  
    ##  2 Afghanistan Asia       1957    30.3  9240934      821.       NA  
    ##  3 Afghanistan Asia       1962    32.0 10267083      853.       NA  
    ##  4 Afghanistan Asia       1967    34.0 11537966      836.       NA  
    ##  5 Afghanistan Asia       1972    36.1 13079460      740.        1.1
    ##  6 Afghanistan Asia       1977    38.4 14880372      786.        1.4
    ##  7 Afghanistan Asia       1982    39.9 12881816      978.        1.8
    ##  8 Afghanistan Asia       1987    40.8 13867957      852.        2.1
    ##  9 Afghanistan Asia       1992    41.7 16317921      649.        2.3
    ## 10 Afghanistan Asia       1997    41.8 22227415      635.        2.6
    ## # ... with 1,694 more rows

### anti\_join()

> anti\_join(x, y): Return all rows from x where there are not matching values in y, keeping just columns from x. This is a filtering join.

``` r
anti_join(gapminder, gapminder_tidyschool)
```

    ## # A tibble: 640 x 6
    ##    country     continent  year lifeExp      pop gdpPercap
    ##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
    ##  1 Afghanistan Asia       1952    28.8  8425333      779.
    ##  2 Afghanistan Asia       1957    30.3  9240934      821.
    ##  3 Afghanistan Asia       1962    32.0 10267083      853.
    ##  4 Afghanistan Asia       1967    34.0 11537966      836.
    ##  5 Albania     Europe     1952    55.2  1282697     1601.
    ##  6 Albania     Europe     1957    59.3  1476505     1942.
    ##  7 Albania     Europe     1962    64.8  1728137     2313.
    ##  8 Albania     Europe     1967    66.2  1984060     2760.
    ##  9 Algeria     Africa     1952    43.1  9279525     2449.
    ## 10 Algeria     Africa     1957    45.7 10270856     3014.
    ## # ... with 630 more rows

### inner\_join() part 2

``` r
inner_join(gapminder_tidyschool, gapminder) %>% 
  head()
```

    ##       country continent year meanSchool lifeExp      pop  gdpPercap
    ## 1 Afghanistan      Asia 1972        1.1  36.088 13079460   739.9811
    ## 2     Albania    Europe 1972        6.9  67.690  2263554  3313.4222
    ## 3     Algeria    Africa 1972        2.1  54.518 14760787  4182.6638
    ## 4      Angola    Africa 1972        2.5  37.928  5894858  5473.2880
    ## 5   Argentina  Americas 1972        7.3  67.065 24779799  9443.0385
    ## 6   Australia   Oceania 1972       10.2  71.930 13177000 16788.6295

Although, it looks similiar to the `inner_join()` from above but this one does not have the `meanSchool` column as it is not present in the gapminder dataset.

### semi\_join() part 2

``` r
semi_join(gapminder_tidyschool, gapminder) %>% 
  head()
```

    ##       country continent year meanSchool
    ## 1 Afghanistan      Asia 1972        1.1
    ## 2     Albania    Europe 1972        6.9
    ## 3     Algeria    Africa 1972        2.1
    ## 4      Angola    Africa 1972        2.5
    ## 5   Argentina  Americas 1972        7.3
    ## 6   Australia   Oceania 1972       10.2

Only retains column from the `gapminder_tidyschool` and all matching row in `gapminder`.

### left\_join() part 2

``` r
left_join(gapminder_tidyschool, gapminder) %>% 
  head()
```

    ##       country continent year meanSchool lifeExp      pop  gdpPercap
    ## 1 Afghanistan      Asia 1972        1.1  36.088 13079460   739.9811
    ## 2     Albania    Europe 1972        6.9  67.690  2263554  3313.4222
    ## 3     Algeria    Africa 1972        2.1  54.518 14760787  4182.6638
    ## 4      Angola    Africa 1972        2.5  37.928  5894858  5473.2880
    ## 5   Argentina  Americas 1972        7.3  67.065 24779799  9443.0385
    ## 6   Australia   Oceania 1972       10.2  71.930 13177000 16788.6295

In contrast to the `left_join()` doest have the `meanScool` column and it does not contain all the rows from the `gapminder` dataset.

### anti\_join() part 2

``` r
anti_join(gapminder_tidyschool, gapminder) %>% 
  head()
```

    ## [1] country    continent  year       meanSchool
    ## <0 rows> (or 0-length row.names)

No rows indicate all of the rows in`gapminder_tidyschool` matches all the rows in the gapminder dataset.

Additional Demos
================

### right\_join()

> right\_join(x, y): Return all rows from y, and all columns from x and y. If there are multiple matches between x and y, all combination of the matches are returned. This is a mutating join.

``` r
right_join(gapminder, gapminder_tidyschool)
```

    ## # A tibble: 1,064 x 7
    ##    country     continent  year lifeExp      pop gdpPercap meanSchool
    ##    <chr>       <fct>     <int>   <dbl>    <int>     <dbl>      <dbl>
    ##  1 Afghanistan Asia       1972    36.1 13079460      740.        1.1
    ##  2 Albania     Europe     1972    67.7  2263554     3313.        6.9
    ##  3 Algeria     Africa     1972    54.5 14760787     4183.        2.1
    ##  4 Angola      Africa     1972    37.9  5894858     5473.        2.5
    ##  5 Argentina   Americas   1972    67.1 24779799     9443.        7.3
    ##  6 Australia   Oceania    1972    71.9 13177000    16789.       10.2
    ##  7 Austria     Europe     1972    70.6  7544201    16662.        9.5
    ##  8 Bahrain     Asia       1972    63.3   230800    18269.        4.5
    ##  9 Bangladesh  Asia       1972    45.3 70759295      630.        2.6
    ## 10 Belgium     Europe     1972    71.4  9709100    16672.        8.9
    ## # ... with 1,054 more rows

### full\_join()

> full\_join(x, y): Return all rows from x and y, and all columns from x and y.

``` r
full_join(gapminder, gapminder_tidyschool)
```

    ## # A tibble: 1,704 x 7
    ##    country     continent  year lifeExp      pop gdpPercap meanSchool
    ##    <chr>       <fct>     <int>   <dbl>    <int>     <dbl>      <dbl>
    ##  1 Afghanistan Asia       1952    28.8  8425333      779.       NA  
    ##  2 Afghanistan Asia       1957    30.3  9240934      821.       NA  
    ##  3 Afghanistan Asia       1962    32.0 10267083      853.       NA  
    ##  4 Afghanistan Asia       1967    34.0 11537966      836.       NA  
    ##  5 Afghanistan Asia       1972    36.1 13079460      740.        1.1
    ##  6 Afghanistan Asia       1977    38.4 14880372      786.        1.4
    ##  7 Afghanistan Asia       1982    39.9 12881816      978.        1.8
    ##  8 Afghanistan Asia       1987    40.8 13867957      852.        2.1
    ##  9 Afghanistan Asia       1992    41.7 16317921      649.        2.3
    ## 10 Afghanistan Asia       1997    41.8 22227415      635.        2.6
    ## # ... with 1,694 more rows

### union()