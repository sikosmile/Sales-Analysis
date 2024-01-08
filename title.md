sales analysis
================
2024-01-08

## **Sales Data Analysis**

``` r
library(dplyr)
```

    ## Warning: package 'dplyr' was built under R version 4.2.3

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(ggplot2)
```

    ## Warning: package 'ggplot2' was built under R version 4.2.1

### Importing the data to be used

``` r
library(readr)
```

    ## Warning: package 'readr' was built under R version 4.2.1

``` r
Sales_Data <- read_csv("datasets/Sales Data.csv")
```

    ## New names:
    ## Rows: 185950 Columns: 11
    ## ── Column specification
    ## ──────────────────────────────────────────────────────── Delimiter: "," chr
    ## (3): Product, Purchase Address, City dbl (7): ...1, Order ID, Quantity Ordered,
    ## Price Each, Month, Sales, Hour dttm (1): Order Date
    ## ℹ Use `spec()` to retrieve the full column specification for this data. ℹ
    ## Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## • `` -> `...1`

``` r
View(Sales_Data)
```

### Identifying sales trend

``` r
sales_trend <- Sales_Data %>% 
  select(Month,Sales) %>%
group_by(Month) %>% 
  arrange(Month) %>%
 summarise(totl_sales = sum(Sales))
```

### Average sales trend

``` r
avg_sales_trend <- Sales_Data %>% 
  select(Month,Sales) %>%
group_by(Month) %>% 
  arrange(Month) %>%
 summarise(avg_sales = mean(Sales))
```

### Identifying the Best Selling Products

``` r
Best_selling_Product <- Sales_Data %>%
  group_by(Product) %>%
  select(Product, `Quantity Ordered`, Sales) %>%
summarise(tot_quantity = sum(`Quantity Ordered`),total_sales = sum(Sales))
```

### Revenue Metrics

#### Profit Margins for each Month

``` r
Profit_margin <- Sales_Data%>%
  select(Month,Sales) %>%
  group_by(Month) %>%
  summarise(Sales= sum(Sales))
```

``` r
Profit_margin %>%
  mutate(margin = (Sales /sum(Sales)) * 100) %>%
  arrange(margin)
```

    ## # A tibble: 12 × 3
    ##    Month    Sales margin
    ##    <dbl>    <dbl>  <dbl>
    ##  1     1 1822257.   5.28
    ##  2     9 2097560.   6.08
    ##  3     2 2202022.   6.38
    ##  4     8 2244468.   6.51
    ##  5     6 2577802.   7.47
    ##  6     7 2647776.   7.68
    ##  7     3 2807100.   8.14
    ##  8     5 3152607.   9.14
    ##  9    11 3199603.   9.28
    ## 10     4 3390670.   9.83
    ## 11    10 3736727.  10.8 
    ## 12    12 4613443.  13.4

### Exporting Datasets

``` r
write.csv(sales_trend,"datasets\\sales_trend.csv",row.names = FALSE)
```
