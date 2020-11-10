iteration
================

## Lists

You can put anything in a list.

``` r
l = list(
  vec_numeric = 5:8,
  vec_logical = c(TRUE, TRUE, FALSE, TRUE, FALSE, FALSE),
  mat = matrix(1:8, nrow = 2, ncol = 4),
  summary = summary(rnorm(100))
)
```

``` r
l
```

    ## $vec_numeric
    ## [1] 5 6 7 8
    ## 
    ## $vec_logical
    ## [1]  TRUE  TRUE FALSE  TRUE FALSE FALSE
    ## 
    ## $mat
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    3    5    7
    ## [2,]    2    4    6    8
    ## 
    ## $summary
    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -2.55638 -0.81496 -0.06413 -0.05223  0.69122  2.96274

``` r
l$vec_numeric
```

    ## [1] 5 6 7 8

``` r
l[[1]]
```

    ## [1] 5 6 7 8

``` r
mean(l[["vec_numeric"]])
```

    ## [1] 6.5

## `for` loop

Create a new list.

``` r
list_norm = 
  list(
    a = rnorm(20, mean = 3, sd = 1),
    b = rnorm(30, mean = 0, sd = 5),
    c = rnorm(40, mean = 10, sd = .2),
    d = rnorm(20, mean = -3, sd = 1)
  )
```

``` r
list_norm
```

    ## $a
    ##  [1] 2.250347 2.366367 3.723332 2.741676 2.473325 3.931839 2.398273 3.276690
    ##  [9] 2.423902 2.986334 1.661595 3.900789 4.185444 3.122590 1.606324 2.147777
    ## [17] 2.121701 3.692110 3.722610 3.661405
    ## 
    ## $b
    ##  [1] -0.40844208 -5.96437571  8.49706560 -3.38320634 -7.56575333 -2.52779421
    ##  [7]  2.66603570 -5.13022521  0.11845716 -2.39593111 -3.99275850 -6.79801813
    ## [13]  8.06510182 -8.93280358  7.12776213  3.46604070 -0.08009525  1.10935769
    ## [19]  6.47020678 -1.70175956 14.19075024 -0.66657890  2.90948463 -3.63356811
    ## [25] -0.92546396 -5.47171623  3.17488786  3.31453031 -3.21994294  3.71920934
    ## 
    ## $c
    ##  [1] 10.144513 10.148358  9.628052 10.407234  9.661843  9.878258 10.111996
    ##  [8] 10.153791  9.921886  9.655556 10.364495 10.009152 10.284085  9.912954
    ## [15] 10.107969 10.027355  9.666263 10.093495 10.079878 10.053766 10.227729
    ## [22] 10.052167 10.276837 10.280474 10.014309 10.163585 10.296360 10.302959
    ## [29] 10.294797 10.012502 10.184006 10.114495  9.844278 10.065859  9.972643
    ## [36] 10.231163  9.909660  9.775120 10.287690  9.920531
    ## 
    ## $d
    ##  [1] -2.485785 -3.139209 -3.022404 -3.469312 -2.725116 -2.847226 -5.047041
    ##  [8] -1.715472 -4.651513 -4.122718 -2.922914 -2.404417 -1.899598 -3.305230
    ## [15] -3.294583 -4.488793 -3.695737 -3.371928 -2.998512 -3.607368

Pause and get my old function.

``` r
mean_and_sd = function(x) {
  
  if (!is.numeric(x)) {
    stop("Input must be numeric")
  }
  
  if (length(x) < 3) {
    stop("Input must have at least three numbers")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)
  
  tibble(
    mean = mean_x,
    sd = sd_x
  )
  
}
```

I can apply that function to each list element.

``` r
mean_and_sd(list_norm[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.92 0.803

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 0.0677  5.37

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.1 0.204

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.26 0.859

Let’s use a for loop:

``` r
output = vector("list", length = 4)
for (i in 1:4) {
  
  output[[i]] = mean_and_sd(list_norm[[i]])
}
```
