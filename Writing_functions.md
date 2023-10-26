Writing_functions
================
2023-10-26

Load key packages

``` r
library(tidyverse)
library(rvest)
```

    ## 
    ## Attaching package: 'rvest'

    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

set seed for reproducibility

``` r
set.seed(12345)
```

### z score function

Z scores subtract the mean and divide by the sd

``` r
x_vec = rnorm(20, mean = 5, sd = .3)
```

compute Z scores for `x_vec`.

``` r
(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1]  0.6103734  0.7589907 -0.2228232 -0.6355576  0.6347861 -2.2717259
    ##  [7]  0.6638185 -0.4229355 -0.4324994 -1.1941438 -0.2311505  2.0874460
    ## [13]  0.3526784  0.5320552 -0.9917420  0.8878182 -1.1546150 -0.4893597
    ## [19]  1.2521303  0.2664557

Write a function to do this!

``` r
z_score = function(x) {
  
  z = (x-mean(x)) / sd(x)
  
  z
  
  
  
}
```

check that this works,

``` r
z_score(x = x_vec)
```

    ##  [1]  0.6103734  0.7589907 -0.2228232 -0.6355576  0.6347861 -2.2717259
    ##  [7]  0.6638185 -0.4229355 -0.4324994 -1.1941438 -0.2311505  2.0874460
    ## [13]  0.3526784  0.5320552 -0.9917420  0.8878182 -1.1546150 -0.4893597
    ## [19]  1.2521303  0.2664557

Keep checking

``` r
z_score(x=3)
```

    ## [1] NA

``` r
z_score(c("my", "name", "is", "jeff"))
```

    ## Warning in mean.default(x): argument is not numeric or logical: returning NA

    ## Error in x - mean(x): non-numeric argument to binary operator

``` r
z_score(c(TRUE, TRUE, FALSE, TRUE))
```

    ## [1]  0.5  0.5 -1.5  0.5

``` r
z_score(iris)
```

    ## Warning in mean.default(x): argument is not numeric or logical: returning NA

    ## Warning in Ops.factor(left, right): '-' not meaningful for factors

    ## Error in is.data.frame(x): 'list' object cannot be coerced to type 'double'

``` r
z_score = function(x) {
  
  if(!is.numeric(x)) {
    stop("Argurment should be numbers")
  } else if (length(x) <2) {
    stop("You need at least 2 number to get z scores")
  }
  
  z = (x-mean(x)) / sd(x)
  
  z
  
  
  
}
```

``` r
z_score(x=3)
```

    ## Error in z_score(x = 3): You need at least 2 number to get z scores

``` r
z_score(c("my", "name", "is", "jeff"))
```

    ## Error in z_score(c("my", "name", "is", "jeff")): Argurment should be numbers

``` r
z_score(c(TRUE, TRUE, FALSE, TRUE))
```

    ## Error in z_score(c(TRUE, TRUE, FALSE, TRUE)): Argurment should be numbers

``` r
z_score(iris)
```

    ## Error in z_score(iris): Argurment should be numbers

### Multiple outputs

Write a function that returns the mean and as from a sample of numbers

``` r
mean_and_sd = function(x) {
  
  if(!is.numeric(x)) {
    stop("Argurment should be numbers")
  } else if (length(x) <2) {
    stop("You need at least 2 number to get z scores")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)
  
  tibble(
    mean = mean_x,
    sd = sd_x
  )
  
  
}
```

double check I did this right.

``` r
mean_and_sd(x_vec)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.02 0.250

### Start getting means and sds

``` r
x_vec = rnorm (n = 30, mean = 5, sd = .5)

  tibble(
    mean = mean(x_vec),
    sd = sd(x_vec)
  )
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.12 0.625

Let’s write a function that uses`n`, a true mean, and true SD as inputs

``` r
sim_mean_sd = function(n_obs, mu, sigma){
  
  x_vec = rnorm (n = n_obs, mean = mu, sd = sigma) 

  tibble(
    mean = mean(x_vec),
    sd = sd(x_vec)
  )
  
}
  

sim_mean_sd(n_obs = 30, mu = 5, sigma = .5)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.12 0.590

``` r
sim_mean_sd(n_obs = 3000, mu = 50, sigma = 12.3)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  49.9  12.1

default value

``` r
sim_mean_sd = function(n_obs, mu = 5, sigma = 1){
  
  x_vec = rnorm (n = n_obs, mean = mu, sd = sigma) 

  tibble(
    mean = mean(x_vec),
    sd = sd(x_vec)
  )
  
}
  
sim_mean_sd(n_obs = 3000)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  4.99 0.998

``` r
sim_mean_sd(n_obs = 3000, mu = 50) #overgide the default value 
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  50.0  1.02

``` r
sim_mean_sd(12, 24, 4) #position matching
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  24.5  3.92

``` r
sim_mean_sd(mu = 12, n_obs = 24, sigma = 4) # change position 
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  13.8  4.07
