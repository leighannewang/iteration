writing\_functions
================

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)
(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -0.07270677  0.62707613  0.77630883 -1.23989407  1.13376729  0.69957010
    ##  [7] -0.23747006 -0.51445885  0.50351306  0.20711741  0.44915824  1.23284174
    ## [13] -1.62104988 -0.93344001  0.28746752 -0.04988629  0.29313905 -0.96857773
    ## [19] -1.05808990 -1.70361220  0.51460204 -0.65222196  0.71578979  1.08621298
    ## [25]  0.55470701 -0.42097782 -2.14318926 -0.80932367  1.59483172  1.74879558

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

    ##  [1] -0.07270677  0.62707613  0.77630883 -1.23989407  1.13376729  0.69957010
    ##  [7] -0.23747006 -0.51445885  0.50351306  0.20711741  0.44915824  1.23284174
    ## [13] -1.62104988 -0.93344001  0.28746752 -0.04988629  0.29313905 -0.96857773
    ## [19] -1.05808990 -1.70361220  0.51460204 -0.65222196  0.71578979  1.08621298
    ## [25]  0.55470701 -0.42097782 -2.14318926 -0.80932367  1.59483172  1.74879558

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
    ## 1  2.39  3.62

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
    ## 1  4.08  3.21

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
    ## 1  5.76  3.24

``` r
sim_mean_sd(mu = 6, samp_size = 100, sigma = 3)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  6.01  2.83

``` r
sim_mean_sd(samp_size = 100)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.59  3.62

## Let’s review Napoleon Dynamite

``` r
url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"
dynamite_html = read_html(url)
review_titles = 
  dynamite_html %>%
  html_nodes(".a-text-bold span") %>%
  html_text()
review_stars = 
  dynamite_html %>%
  html_nodes("#cm_cr-review_list .review-rating") %>%
  html_text() %>%
  str_extract("^\\d") %>%
  as.numeric()
review_text = 
  dynamite_html %>%
  html_nodes(".review-text-content span") %>%
  html_text() %>% 
  str_replace_all("\n", "") %>% 
  str_trim()
reviews = tibble(
  title = review_titles,
  stars = review_stars,
  text = review_text
)
```

What about the next page of reviews…

Let’s turn that code into a function

``` r
read_page_reviews = function(url) {
  
  html = read_html(url)
  
  review_titles = 
    html %>%
    html_nodes(".a-text-bold span") %>%
    html_text()
  
  review_stars = 
    html %>%
    html_nodes("#cm_cr-review_list .review-rating") %>%
    html_text()
  
  review_text = 
    html %>%
    html_nodes(".review-text-content span") %>%
    html_text() %>% 
    str_replace_all("\n", "") %>% 
    str_trim()
  
  reviews = 
    tibble(
      title = review_titles,
      stars = review_stars,
      text = review_text
    )
  
  reviews
  
}
```

Let me try my function.

``` r
dynamite_url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=2"
read_page_reviews(dynamite_url)
```

    ## # A tibble: 10 x 3
    ##    title                             stars      text                            
    ##    <chr>                             <chr>      <chr>                           
    ##  1 "Boo"                             1.0 out o~ "We rented this movie because o~
    ##  2 "Movie is still silly fun....ama~ 1.0 out o~ "We are getting really frustrat~
    ##  3 "Brilliant and awkwardly funny."  5.0 out o~ "I've watched this movie repeat~
    ##  4 "Great purchase price for great ~ 5.0 out o~ "Great movie and real good digi~
    ##  5 "Movie for memories"              5.0 out o~ "I've been looking for this mov~
    ##  6 "Love!"                           5.0 out o~ "Love this movie. Great quality"
    ##  7 "Hilarious!"                      5.0 out o~ "Such a funny movie, definitely~
    ##  8 "napoleon dynamite"               5.0 out o~ "cool movie"                    
    ##  9 "Top 5"                           5.0 out o~ "Best MOVIE ever! Funny one lin~
    ## 10 "\U0001f44d"                      5.0 out o~ "Exactly as described and came ~

Let’s read a few pages of reviews.

``` r
dynamite_url_base = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber="
dynamite_urls = str_c(dynamite_url_base, 1:50)
all_reviews = 
  bind_rows(
    read_page_reviews(dynamite_urls[1]),
    read_page_reviews(dynamite_urls[2]),
    read_page_reviews(dynamite_urls[3]),
    read_page_reviews(dynamite_urls[4]),
    read_page_reviews(dynamite_urls[5])
  )
```

## Mean scoping example

``` r
f = function(x) {
  z = x + y
  z
}
x = 1
y = 2
f(x = y)
```

    ## [1] 4

## Functions as arguments

``` r
my_summary = function(x, summ_func) {
  
  summ_func(x)
  
}
x_vec = rnorm(100, 3, 7)
mean(x_vec)
```

    ## [1] 2.858483

``` r
median(x_vec)
```

    ## [1] 2.64164

``` r
my_summary(x_vec, IQR)
```

    ## [1] 8.975962
