When using R, the "apply" family's functions can be really powerful. However, it's sometimes confusing which one to use and forget their differences easily. 
If you have the same problem as I do, here is a simple guide for you. 

A quick glimpse, this guide covers functions including **apply, lapply, sapply, tapply, vapply**. These are the "apply" functions that I always used and found handy. 
  
   
function | argument | occasion to use
------------ | ------------- | -------------
apply | `apply(X, MARGIN, FUN, ...)` | apply function by column or row only 
lapply | `lapply(X, FUN, ...)` | expect the format returned to be "list"
sapply | `sapply(X, FUN, ..., simplify = TRUE, USE.NAMES = TRUE)` | expect the returned format to be automatically simplified
vapply | `vapply(X, FUN, FUN.VALUE, ..., USE.NAMES = TRUE)` | expect a specific return format
tapply | `tapply(X, INDEX, FUN = NULL, ..., simplify = TRUE)` | apply function on specific elements inside the data based on certain filtering conditions 

# apply 
`apply(X, MARGIN, FUN, ...)`   
apply() hleps to apply function by column or row only.
we do so by specifying the MARGIN argument:
**1 indicates rows, 2 indicates columns, c(1, 2) indicates rows and columns**.

### example
```javascript
> data <- rbind(1:6,2:7)
> data
     [,1] [,2] [,3] [,4] [,5] [,6]
[1,]    1    2    3    4    5    6
[2,]    2    3    4    5    6    7
> get.rowsum <- apply(data, 1, sum)
> get.columnsum <- apply(data, 2, sum)
> get.rowsum
[1] 21 27
> get.columnsum
[1]  3  5  7  9 11 13
```

# lapply, sapply, vapply
`lapply(X, FUN, ...)` 
`sapply(X, FUN, ..., simplify = TRUE, USE.NAMES = TRUE)` 
`vapply(X, FUN, FUN.VALUE, ..., USE.NAMES = TRUE)` 
these three are pretty similar except that they decide the format of return data differently. 

**lapply** always return a list
**sapply** tries to simply the return format, and return a vector or matrix. But when `simplify = FALSE` or simplification fails, it returns a list
**vapply** return the format specified by you. This is sometimes safer and faster.

### example 

```javascript
> data <- list(1:4, 2:5, 6:7)
> data
[[1]]
[1] 1 2 3 4

[[2]]
[1] 2 3 4 5

[[3]]
[1] 6 7

> lapply(data,sum)
[[1]]
[1] 10

[[2]]
[1] 14

[[3]]
[1] 13

> sapply(data,sum)
[1] 10 14 13
```
"vapply" example comes from R Documentation of vapply

```javascript
> i39 <- sapply(3:9, seq) 
> vapply(i39, fivenum,
+        c(Min. = 0, "1st Qu." = 0, Median = 0, "3rd Qu." = 0, Max. = 0))
        [,1] [,2] [,3] [,4] [,5] [,6] [,7]
Min.     1.0  1.0    1  1.0  1.0  1.0    1
1st Qu.  1.5  1.5    2  2.0  2.5  2.5    3
Median   2.0  2.5    3  3.5  4.0  4.5    5
3rd Qu.  2.5  3.5    4  5.0  5.5  6.5    7
Max.     3.0  4.0    5  6.0  7.0  8.0    9
> i39 <- sapply(3:9, seq) 
> i39
[[1]]
[1] 1 2 3

[[2]]
[1] 1 2 3 4

[[3]]
[1] 1 2 3 4 5

[[4]]
[1] 1 2 3 4 5 6

[[5]]
[1] 1 2 3 4 5 6 7

[[6]]
[1] 1 2 3 4 5 6 7 8

[[7]]
[1] 1 2 3 4 5 6 7 8 9

> vapply(i39, fivenum, c(Min. = 0, "1st Qu." = 0, Median = 0, "3rd Qu." = 0, Max. = 0))
        [,1] [,2] [,3] [,4] [,5] [,6] [,7]
Min.     1.0  1.0    1  1.0  1.0  1.0    1
1st Qu.  1.5  1.5    2  2.0  2.5  2.5    3
Median   2.0  2.5    3  3.5  4.0  4.5    5
3rd Qu.  2.5  3.5    4  5.0  5.5  6.5    7
Max.     3.0  4.0    5  6.0  7.0  8.0    9
```

# tapply
`tapply(X, INDEX, FUN = NULL, ..., simplify = TRUE)` 
last but not least, tapply is what I think to be the most powerful and convenient. 

```javascript
> data <- mtcars[1:10,1:10]
> data
                   mpg cyl  disp  hp drat    wt  qsec vs am gear
Mazda RX4         21.0   6 160.0 110 3.90 2.620 16.46  0  1    4
Mazda RX4 Wag     21.0   6 160.0 110 3.90 2.875 17.02  0  1    4
Datsun 710        22.8   4 108.0  93 3.85 2.320 18.61  1  1    4
Hornet 4 Drive    21.4   6 258.0 110 3.08 3.215 19.44  1  0    3
Hornet Sportabout 18.7   8 360.0 175 3.15 3.440 17.02  0  0    3
Valiant           18.1   6 225.0 105 2.76 3.460 20.22  1  0    3
Duster 360        14.3   8 360.0 245 3.21 3.570 15.84  0  0    3
Merc 240D         24.4   4 146.7  62 3.69 3.190 20.00  1  0    4
Merc 230          22.8   4 140.8  95 3.92 3.150 22.90  1  0    4
Merc 280          19.2   6 167.6 123 3.92 3.440 18.30  1  0    4

> tapply(data$mpg, data$am, mean)
       0        1 
19.84286 21.60000 
```
