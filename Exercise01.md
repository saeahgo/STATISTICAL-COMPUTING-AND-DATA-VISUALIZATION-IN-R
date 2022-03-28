Exercise01
================
Saeah Go
3/28/2022

## Set 1, Exercise 1

Write a set of conditional(s) that satisfies the following requirements:

-   If `x` is greater than 3 and `y` is less than or equal to 3 then
    print “Hello world!”

-   Otherwise if `x` is greater than 3 print “!dlrow olleH”

-   If `x` is less than or equal to 3 then print “Something else …”

-   Stop execution if x is odd and y is even and report an error, don’t
    print any of the text strings above.

Test out your code with

1.  `x=1` and `y=2`
2.  `x=1` and `y=3`
3.  `x=3` and `y=3`

**Note that because of the `stop` command, the `.Rmd` file will not knit
when errors occur unless you set the chunk option `error=TRUE`**

``` r
# write your code for problem set 1 exercise 1 here
x <- 3
y <- 3

# first need to check if x is odd and y is even
if(x%%2==1 & y%%2==0){
  stop("Error error") # x is odd and y is even so report error and terminate
}else{ # if "x is odd and y is even" is false
  if(x > 3 & y<= 3){
  "Hello world!"
} else if(x > 3){
  "!dlrow olleH"
} else{
  "Something else..."
}}
```

    ## [1] "Something else..."

## Set 1, Exercise 2

Write a set of conditional(s) in `R` to solve equations of the form
*a**x*<sup>2</sup> + *b**x* + *c* = 0,
which accounts for possible errors that might occurr in the calculation
(you may use `if`, `else if`, `else`, `stop`, `stopifnot`)

Test out your code with

1.  `a=1`, `b=0` and `c=1`
2.  `a=1`, `b=0` and `c=-1`
3.  `a=9`, `b=4` and `c=1`
4.  `a=0`, `b=3` and `c=3`
5.  `a=1`, `b=-4` and `c=4`

**Again, because of the `stop` command the `.Rmd` file will not knit
when errors occur unless you set the chunk option `error=TRUE`**

``` r
# write your code for problem set 1 exercise 2 here
a <- 1
b <- -4
c <- 4

root1 <- NULL
root2 <- NULL

testfunc <-(b^2-4*a*c)
if(testfunc < 0){
  stop("Can't get x with the quadratic formula")
}else{
  if(a == 0){
    stop("a is zero, so the formula is linear")
  }
  root1 <- (-b+sqrt(testfunc))/(2*a)
  root2 <- (-b-sqrt(testfunc))/(2*a)
  if(root1 != root2){
    result <- c(root1, root2)
    print(result)
  }else{ #root1 == root2
    print(root1)
  }
}
```

    ## [1] 2

# Set 2

Below is a vector containing all prime numbers between 2 and 100:

``` r
primes = c( 2,  3,  5,  7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 
      43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97)
```

If you were given the vector

`x = c(3,4,12,19,23,51,61,63,78)`,
</div>

write the R code necessary to print only the values of `x` that are
*not* prime (without using subsetting or the `%in%` operator).

Your code should use *nested* loops to iterate through the vector of
primes and `x`.

``` r
# write your code for problem set 2 here
# write your code for problem set 2 here
x = c(3,4,12,19,23,51,61,63,78)

notprimes = c()

for(a in x){
  found = FALSE #initialize
  for(b in primes){
    if(a == b)
      found = TRUE
  }
  if(found == FALSE) # a is not prime
    notprimes <- c(notprimes, a)
}

notprimes
```

    ## [1]  4 12 51 63 78
