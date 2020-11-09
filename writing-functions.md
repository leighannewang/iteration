writing\_functions
================

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)
(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -1.773843436  1.168522465 -0.729364994 -0.730750167 -0.795257675
    ##  [6] -1.351703312  2.322899828  0.780956606  0.044525119 -1.272413661
    ## [11]  0.821705818 -0.570324797  0.448718149  0.633017752 -0.407172718
    ## [16]  1.651649298 -0.147412502  0.111734336  1.209072036 -0.061997209
    ## [21]  0.498797294 -0.065035048 -1.683204027 -1.343490824  1.297608264
    ## [26] -0.260238302  0.641318966  0.092149314 -0.004070283 -0.526396292

I want a function to compute z-scores

``` r
z_scores = function(x) {
  
  if (!is.numeric(x)) {
    stop("Input must be numeric")
  }
  
  if (length(x) < 3) {
    stop("Input must have at least three numbers")
  }
  
  z = (x - mean(x)) / sd(x)
  
  return(z)
  
}
z_scores(x_vec)
```

    ##  [1] -1.773843436  1.168522465 -0.729364994 -0.730750167 -0.795257675
    ##  [6] -1.351703312  2.322899828  0.780956606  0.044525119 -1.272413661
    ## [11]  0.821705818 -0.570324797  0.448718149  0.633017752 -0.407172718
    ## [16]  1.651649298 -0.147412502  0.111734336  1.209072036 -0.061997209
    ## [21]  0.498797294 -0.065035048 -1.683204027 -1.343490824  1.297608264
    ## [26] -0.260238302  0.641318966  0.092149314 -0.004070283 -0.526396292

Try my function on some other things. These should give errors.

``` r
z_scores(3)
```

    ## Error in z_scores(3): Input must have at least three numbers

``` r
z_scores("my name is jeff")
```

    ## Error in z_scores("my name is jeff"): Input must be numeric

``` r
z_scores(mtcars)
```

    ## Error in z_scores(mtcars): Input must be numeric

``` r
z_scores(c(TRUE, TRUE, FALSE, TRUE))
```

    ## Error in z_scores(c(TRUE, TRUE, FALSE, TRUE)): Input must be numeric

## Multiple outputs

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

Check that the function works.

``` r
x_vec = rnorm(100, mean = 3, sd = 4)
mean_and_sd(x_vec)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.91  4.41

## Multiple inputs

I’d like to do this with a function.

``` r
sim_data = 
  tibble(
    x = rnorm(n = 100, mean = 4, sd = 3)
  )
sim_data %>% 
  summarize(
    mean = mean(x),
    sd = sd(x)
  )
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  4.06  3.50

``` r
sim_mean_sd = function(samp_size, mu = 3, sigma = 4) {
  
  sim_data = 
    tibble(
      x = rnorm(n = samp_size, mean = mu, sd = sigma)
    )
  sim_data %>% 
    summarize(
      mean = mean(x),
      sd = sd(x)
    )
  
}
sim_mean_sd(samp_size = 100, mu = 6, sigma = 3)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.99  2.98

``` r
sim_mean_sd(mu = 6, samp_size = 100, sigma = 3)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  6.13  2.97

``` r
sim_mean_sd(samp_size = 100)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.19  3.86
