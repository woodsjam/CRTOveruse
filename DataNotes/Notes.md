Looking at Taylor Data
========================================================
Load in the data

```r
# Original <- read.csv("~/Dropbox/Research/CRTOveruse/OriginalData/Original.csv")

Original <- read.csv("OriginalData/Original.csv")
```

```
## Warning in file(file, "rt"): cannot open file 'OriginalData/Original.csv':
## No such file or directory
```

```
## Error in file(file, "rt"): cannot open the connection
```

Manipulations based on description

```r
#Fix the ids
Original$id<-as.factor(Original$id)
```

```
## Error in is.factor(x): object 'Original' not found
```

```r
#make a logical out of the treatment variable
Original$HypoTRUE<-ifelse(Original$treatment=='1',TRUE,FALSE)
```

```
## Error in ifelse(Original$treatment == "1", TRUE, FALSE): object 'Original' not found
```

```r
#Recode the dummy variable sequence for major
Original$Major<-NA
```

```
## Error in Original$Major <- NA: object 'Original' not found
```

```r
Original$Major[Original$dum_major1=='1']<-"Econ"
```

```
## Error in Original$Major[Original$dum_major1 == "1"] <- "Econ": object 'Original' not found
```

```r
Original$Major[Original$dum_major2=='1']<-"Env"
```

```
## Error in Original$Major[Original$dum_major2 == "1"] <- "Env": object 'Original' not found
```

```r
Original$Major[Original$dum_major3=='1']<-"Sci"
```

```
## Error in Original$Major[Original$dum_major3 == "1"] <- "Sci": object 'Original' not found
```

```r
Original$Major[Original$dum_major4=='1']<-"Bus"
```

```
## Error in Original$Major[Original$dum_major4 == "1"] <- "Bus": object 'Original' not found
```

```r
Original$Major[Original$dum_major5=='1']<-"Other"
```

```
## Error in Original$Major[Original$dum_major5 == "1"] <- "Other": object 'Original' not found
```

```r
Original$Major<-as.factor(Original$Major)
```

```
## Error in is.factor(x): object 'Original' not found
```

```r
#Recode responses as logicals
Original$safereal1<-as.logical(Original$safereal1)
```

```
## Error in eval(expr, envir, enclos): object 'Original' not found
```

```r
Original$safereal2<-as.logical(Original$safereal2)
```

```
## Error in eval(expr, envir, enclos): object 'Original' not found
```

```r
Original$safereal3<-as.logical(Original$safereal3)
```

```
## Error in eval(expr, envir, enclos): object 'Original' not found
```

```r
Original$safereal4<-as.logical(Original$safereal4)
```

```
## Error in eval(expr, envir, enclos): object 'Original' not found
```

```r
Original$safereal5<-as.logical(Original$safereal5)
```

```
## Error in eval(expr, envir, enclos): object 'Original' not found
```

```r
Original$safereal6<-as.logical(Original$safereal6)
```

```
## Error in eval(expr, envir, enclos): object 'Original' not found
```

```r
Original$safereal7<-as.logical(Original$safereal7)
```

```
## Error in eval(expr, envir, enclos): object 'Original' not found
```

```r
Original$safereal8<-as.logical(Original$safereal8)
```

```
## Error in eval(expr, envir, enclos): object 'Original' not found
```

```r
Original$safereal9<-as.logical(Original$safereal9)
```

```
## Error in eval(expr, envir, enclos): object 'Original' not found
```

```r
Original$safereal10<-as.logical(Original$safereal10)
```

```
## Error in eval(expr, envir, enclos): object 'Original' not found
```

```r
Original$safehypot1<-as.logical(Original$safehypot1)
```

```
## Error in eval(expr, envir, enclos): object 'Original' not found
```

```r
Original$safehypot2<-as.logical(Original$safehypot2)
```

```
## Error in eval(expr, envir, enclos): object 'Original' not found
```

```r
Original$safehypot3<-as.logical(Original$safehypot3)
```

```
## Error in eval(expr, envir, enclos): object 'Original' not found
```

```r
Original$safehypot4<-as.logical(Original$safehypot4)
```

```
## Error in eval(expr, envir, enclos): object 'Original' not found
```

```r
Original$safehypot5<-as.logical(Original$safehypot5)
```

```
## Error in eval(expr, envir, enclos): object 'Original' not found
```

```r
Original$safehypot6<-as.logical(Original$safehypot6)
```

```
## Error in eval(expr, envir, enclos): object 'Original' not found
```

```r
Original$safehypot7<-as.logical(Original$safehypot7)
```

```
## Error in eval(expr, envir, enclos): object 'Original' not found
```

```r
Original$safehypot8<-as.logical(Original$safehypot8)
```

```
## Error in eval(expr, envir, enclos): object 'Original' not found
```

```r
Original$safehypot9<-as.logical(Original$safehypot9)
```

```
## Error in eval(expr, envir, enclos): object 'Original' not found
```

```r
Original$safehypot10<-as.logical(Original$safehypot10)
```

```
## Error in eval(expr, envir, enclos): object 'Original' not found
```

```r
summary(Original)
```

```
## Error in summary(Original): object 'Original' not found
```
There could be some tricks because Taylor added some noise to the choices and I don't know the exact noise.

Back on the estimator.




