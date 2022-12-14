Lab 09
================
Lily Shkhyan
2022-10-26

## Problem 2: Create a n x k matrix of Poisson variables with mean lambda.

``` r
set.seed(1235)
fun1 <- function(n = 100, k = 4, lambda = 4) 
  {
  x <- NULL
  for (i in 1:n)
    x <- rbind(x, rpois(k, lambda))
  return(x)
  }
 
f1 <- fun1(100,4)
mean(f1)
```

    ## [1] 4.1575

``` r
fun1alt <- function(n = 100, k = 4, lambda = 4) 
  {
  x <- matrix( rpois(n*k, lambda) , ncol = 4)
  return(x)
  }

f1 <- fun1alt(50000,4)
microbenchmark::microbenchmark(
  fun1(),
  fun1alt()
)
```

    ## Unit: microseconds
    ##       expr     min       lq     mean   median      uq      max neval cld
    ##     fun1() 232.084 298.3130 327.2961 333.7920 348.605  459.500   100   b
    ##  fun1alt()  18.625  20.5215  38.5648  21.1045  22.459 1697.292   100  a

``` r
d <- matrix(1:16,ncol=4)
d
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    5    9   13
    ## [2,]    2    6   10   14
    ## [3,]    3    7   11   15
    ## [4,]    4    8   12   16

``` r
diag(d)
```

    ## [1]  1  6 11 16

``` r
d[2]
```

    ## [1] 2

``` r
d[2,1]
```

    ## [1] 2

``` r
d[c(1,6,11,16)]
```

    ## [1]  1  6 11 16

``` r
cbind(1:4,1:4)
```

    ##      [,1] [,2]
    ## [1,]    1    1
    ## [2,]    2    2
    ## [3,]    3    3
    ## [4,]    4    4

``` r
d[cbind(1:4,1:4)]
```

    ## [1]  1  6 11 16

## Problem 3: Find the column max (hint: Checkout the function max.col()).

``` r
set.seed(1234)
M <- matrix(runif(12), ncol=4)
M
```

    ##           [,1]      [,2]        [,3]      [,4]
    ## [1,] 0.1137034 0.6233794 0.009495756 0.5142511
    ## [2,] 0.6222994 0.8609154 0.232550506 0.6935913
    ## [3,] 0.6092747 0.6403106 0.666083758 0.5449748

``` r
fun2 <- function(x) 
  {
  apply(x, 2, max)
  }

fun2(x=M)
```

    ## [1] 0.6222994 0.8609154 0.6660838 0.6935913

``` r
fun2alt <- function(x) 
  {
   idx <- max.col( t(x))
   x[cbind(idx,1:4)]
  }

fun2alt(x=M)
```

    ## [1] 0.6222994 0.8609154 0.6660838 0.6935913

``` r
x <- matrix(rnorm(1e4), nrow=10)
microbenchmark::microbenchmark(
  fun2(x),
  fun2alt(x)
)
```

    ## Unit: microseconds
    ##        expr     min       lq     mean   median        uq      max neval cld
    ##     fun2(x) 782.668 870.5840 988.7023 998.2085 1023.6880 3338.584   100   b
    ##  fun2alt(x)  97.376 100.9175 128.8253 103.6045  110.3755 2136.584   100  a
