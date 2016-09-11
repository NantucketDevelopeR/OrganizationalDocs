# Efficient Programming 

## Efficient programming


"Premature optimization is the root of all evil"

-- Don Knuth 


Hadley Wickham () gives outlines seven general strategies for improving the performance of your code.

* Code organisation teaches you how to organise your code to make optimisation as easy, and bug free, as possible.
* Already solved, reminds you to look for existing solutions.
* Do as little as possible emphasises the importance of being lazy: often the easiest way to make a function faster is to let it to do less work.
* Vectorise concisely defines vectorisation, and shows you how to make the most of built-in functions.
* Avoid copies discusses the performance perils of copying data.
* Byte code compilation
* Parallelisation 


There exist a lot of functions which might already do what you want. Here is a short selection of convenient and efficient functions one should know (from Karl Browman's [hipsteR](http://kbroman.org/hipsteR/) page): 

[`Vectorize`](https://stat.ethz.ch/R-manual/R-devel/library/base/html/Vectorize.html)
[`which.min`](https://stat.ethz.ch/R-manual/R-devel/library/base/html/which.min.html), [`which.max`](https://stat.ethz.ch/R-manual/R-devel/library/base/html/which.min.html)
[`unsplit`](https://stat.ethz.ch/R-manual/R-devel/library/base/html/split.html)
[`rowSums`](https://stat.ethz.ch/R-manual/R-devel/library/base/html/colSums.html), [`colSums`](https://stat.ethz.ch/R-manual/R-devel/library/base/html/colSums.html), [`rowMeans`](https://stat.ethz.ch/R-manual/R-devel/library/base/html/colSums.html),
[`colMeans`](https://stat.ethz.ch/R-manual/R-devel/library/base/html/colSums.html)
[`rowsum`](https://stat.ethz.ch/R-manual/R-devel/library/base/html/rowsum.html)
[`paste0`](https://stat.ethz.ch/R-manual/R-devel/library/base/html/paste.html)
[`anyNA`](https://stat.ethz.ch/R-manual/R-devel/library/base/html/NA.html)
[`aggregate`](https://stat.ethz.ch/R-manual/R-devel/library/stats/html/aggregate.html), [`by`](https://stat.ethz.ch/R-manual/R-devel/library/base/html/by.html)
[`merge`](https://stat.ethz.ch/R-manual/R-devel/library/base/html/merge.html)


## A little example

We have two vectors `X` and `Y` and want to compute the sum of these vectors and store these in a vector `Z`. 

```{R}
X <- rnorm(25000)
Y <- rnorm(25000)

fun1 <- function(X,Y){
    Z <- c()
    for(i in 1:length(X)) Z <-  c(Z, X[i] + Y[i])
    Z
}
system.time(Z1 <- fun1(X,Y))
```
```
   user  system elapsed 
  1.176   0.000   1.174
```
That takes quite some time. But why?? Lets try figure out where are the bottlenecks? `R` has some nice tools for profiling:
```{R}
Rprof(tmp <- tempfile(), memory.profiling = TRUE)
Z1 <- fun1(X,Y)
Rprof()
summaryRprof(tmp, memory="both")
unlink(tmp)
```
```
$by.self
       self.time self.pct total.time total.pct mem.total
"c"         1.18    98.33       1.18     98.33      74.8
"fun1"      0.02     1.67       1.20    100.00      74.8

$by.total
       total.time total.pct mem.total self.time self.pct
"fun1"       1.20    100.00      74.8      0.02     1.67
"c"          1.18     98.33      74.8      1.18    98.33

$sample.interval
[1] 0.02

$sampling.time
[1] 1.2
```
The algorithm spends a lot of time in the function `c`. This is because in every step we make a copy of Z and add one element. Much better is to define the vector beforehand: 
```{R}
fun2 <- function(X,Y){
    Z <- rep(NA, length(X))
    for(i in 1:length(X)) Z[i] <-  X[i] + Y[i]
    Z
}
system.time(Z2 <- fun2(X,Y))
```
```
   user  system elapsed 
  0.044   0.000   0.043 
```
```{R}
all.equal(Z1, Z2)
```
```
[1] TRUE
```
Now even better is if we can vectorize our function. In this case we have to do, almost nothing as it is already done:
```{R}
fun3 <- function(X,Y){
    Z <- X+Y
    Z
}

system.time(Z3 <- fun3(X,Y))
```
```
   user  system elapsed 
      0       0       0 
```      
```{R}      
all.equal(Z1, Z3)
```
```
[1] TRUE
```
Now this was pretty fast, too fast to properly see how much faster this algorithm was.   
```{R}
library(microbenchmark)
microbenchmark(fun2(X,Y), fun3(X,Y))
```
```
Unit: microseconds
       expr       min        lq        mean     median        uq       max neval
 fun2(X, Y) 20491.120 22002.732 23371.75983 22808.5785 23340.079 80365.530   100
 fun3(X, Y)    30.689    39.016    66.47713    73.6385    90.229   130.252   100
```




## Some matrix algebra

The order of multiplication can be important as in this example from Braun & Murdoch (2016). 
```{R}
A <- matrix(rep(1, 1000000), nrow=1000)
B <- A
v <- rep(1, 1000)

system.time(C1 <- A %*% B %*% v)
```
```
   user  system elapsed 
  3.344   0.000   3.343 
```
```{R}
system.time(C2 <- (A %*% B) %*% v)
```
```
   user  system elapsed 
  0.736   0.000   0.735 
```
```{R}  
system.time(C3 <- A %*% (B %*% v))
```
```
   user  system elapsed 
  0.004   0.000   0.005 
```
```{R}
all.equal(C1, C2)
```
```
[1] TRUE
```
```{R}
all.equal(C1, C3)
```
```
[1] TRUE
```

### References 
Braun WJ, Murdoch DJ (2016) A first course in statistical programming with R, 2nd edition, Cambridge University Press

Wickham H (2014) Advanced R, Chapman & Hall [url](http://adv-r.had.co.nz/) 


