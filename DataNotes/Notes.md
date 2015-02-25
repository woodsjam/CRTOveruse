Looking at Taylor Data
========================================================
Load in the data

```r
# Original <- read.csv("~/Dropbox/Research/CRTOveruse/OriginalData/Original.csv")

Original <- read.csv("~/Research/CRTOveruse/OriginalData/Original.csv")
```

Manipulations based on description

```r
#Fix the ids
Original$id<-as.factor(Original$id)

#make a logical out of the treatment variable
Original$HypoTRUE<-ifelse(Original$treatment=='1',TRUE,FALSE)

#Recode the dummy variable sequence for major
Original$Major<-NA
Original$Major[Original$dum_major1=='1']<-"Econ"
Original$Major[Original$dum_major2=='1']<-"Env"
Original$Major[Original$dum_major3=='1']<-"Sci"
Original$Major[Original$dum_major4=='1']<-"Bus"
Original$Major[Original$dum_major5=='1']<-"Other"
Original$Major<-as.factor(Original$Major)

# Testing out how to use other distributions for estimator.



#Recode responses as logicals
Original$safereal1<-as.logical(Original$safereal1)
Original$safereal2<-as.logical(Original$safereal2)
Original$safereal3<-as.logical(Original$safereal3)
Original$safereal4<-as.logical(Original$safereal4)
Original$safereal5<-as.logical(Original$safereal5)
Original$safereal6<-as.logical(Original$safereal6)
Original$safereal7<-as.logical(Original$safereal7)
Original$safereal8<-as.logical(Original$safereal8)
Original$safereal9<-as.logical(Original$safereal9)
Original$safereal10<-as.logical(Original$safereal10)

Original$safehypot1<-as.logical(Original$safehypot1)
Original$safehypot2<-as.logical(Original$safehypot2)
Original$safehypot3<-as.logical(Original$safehypot3)
Original$safehypot4<-as.logical(Original$safehypot4)
Original$safehypot5<-as.logical(Original$safehypot5)
Original$safehypot6<-as.logical(Original$safehypot6)
Original$safehypot7<-as.logical(Original$safehypot7)
Original$safehypot8<-as.logical(Original$safehypot8)
Original$safehypot9<-as.logical(Original$safehypot9)
Original$safehypot10<-as.logical(Original$safehypot10)

summary(Original)
```

```
##        id          crt                          major      treatment     
##  1      : 1   Min.   :0.000   Environmental Studies: 9   Min.   :0.0000  
##  2      : 1   1st Qu.:0.000   Biology              : 7   1st Qu.:0.0000  
##  3      : 1   Median :1.000   Business             : 6   Median :1.0000  
##  4      : 1   Mean   :1.268   Human Physiology     : 6   Mean   :0.5052  
##  5      : 1   3rd Qu.:2.000   Economics            : 4   3rd Qu.:1.0000  
##  6      : 1   Max.   :3.000   Journalism           : 4   Max.   :1.0000  
##  (Other):91                   (Other)              :61                   
##    dum_major1       dum_major2       dum_major3       dum_major4    
##  Min.   :0.0000   Min.   :0.0000   Min.   :0.0000   Min.   :0.0000  
##  1st Qu.:0.0000   1st Qu.:0.0000   1st Qu.:0.0000   1st Qu.:0.0000  
##  Median :0.0000   Median :0.0000   Median :0.0000   Median :0.0000  
##  Mean   :0.1031   Mean   :0.1856   Mean   :0.2268   Mean   :0.1959  
##  3rd Qu.:0.0000   3rd Qu.:0.0000   3rd Qu.:0.0000   3rd Qu.:0.0000  
##  Max.   :1.0000   Max.   :1.0000   Max.   :1.0000   Max.   :1.0000  
##                                                                     
##    dum_major5     safereal1       safereal2       safereal3      
##  Min.   :0.0000   Mode :logical   Mode :logical   Mode :logical  
##  1st Qu.:0.0000   FALSE:4         FALSE:2         FALSE:6        
##  Median :0.0000   TRUE :93        TRUE :95        TRUE :91       
##  Mean   :0.2887   NA's :0         NA's :0         NA's :0        
##  3rd Qu.:1.0000                                                  
##  Max.   :1.0000                                                  
##                                                                  
##  safereal4       safereal5       safereal6       safereal7      
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:8         FALSE:17        FALSE:29        FALSE:53       
##  TRUE :89        TRUE :80        TRUE :68        TRUE :44       
##  NA's :0         NA's :0         NA's :0         NA's :0        
##                                                                 
##                                                                 
##                                                                 
##  safereal8       safereal9       safereal10      safehypot1     
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:72        FALSE:81        FALSE:93        FALSE:4        
##  TRUE :25        TRUE :16        TRUE :4         TRUE :93       
##  NA's :0         NA's :0         NA's :0         NA's :0        
##                                                                 
##                                                                 
##                                                                 
##  safehypot2      safehypot3      safehypot4      safehypot5     
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:4         FALSE:4         FALSE:12        FALSE:17       
##  TRUE :93        TRUE :93        TRUE :85        TRUE :80       
##  NA's :0         NA's :0         NA's :0         NA's :0        
##                                                                 
##                                                                 
##                                                                 
##  safehypot6      safehypot7      safehypot8      safehypot9     
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:29        FALSE:51        FALSE:74        FALSE:83       
##  TRUE :68        TRUE :46        TRUE :23        TRUE :14       
##  NA's :0         NA's :0         NA's :0         NA's :0        
##                                                                 
##                                                                 
##                                                                 
##  safehypot10      HypoTRUE         Major   
##  Mode :logical   Mode :logical   Bus  :19  
##  FALSE:94        FALSE:48        Econ :10  
##  TRUE :3         TRUE :49        Env  :18  
##  NA's :0         NA's :0         Other:28  
##                                  Sci  :22  
##                                            
## 
```
There could be some tricks because Taylor added some noise to the choices and I don't know the exact noise.


# Prototype an estimator


Functions we will need.

```r
# the utility function
CRRA<-function(w,r){w^(1-r)/(1-r)}

#expected utility of lottery
EU<-function(w1,u1,w2,u2){(w1*u1) +(w2*u2)}

#The two latent variable specifications.  
Fechner<-function(UtilityL, UtilityR, mu){
  (UtilityR-UtilityL)/mu
}

Luce<-function(UtilityL, UtilityR, mu){
  UtilityR^(1/mu)/(UtilityR^(1/mu) +UtilityL^(1/mu))
}
```

Note that taylor only got Fechner to really work on the data because of convergence issues.


We need the values of the lotteries.  Here are the choices from table 1 of Taylor's paper.  The safe gambles are for $40 and $32 and the risky is $77 and $2.  Note that they added some noise to the values so these are just expectations.


```r
Choices<-data.frame(
  rbind(c(1, 0.1, 0.9, 0.1, 0.9, 32.80, 9.50),
        c(2, 0.2, 0.8, 0.2, 0.8, 33.60, 17.00),
        c(3, 0.3, 0.7, 0.3, 0.7, 34.40, 24.80),
        c(4, 0.4, 0.6, 0.4, 0.6, 35.20, 32.00),
        c(5, 0.5, 0.5, 0.5, 0.5, 36.00, 39.50),
        c(6, 0.6, 0.4, 0.6, 0.4, 36.80, 47.00),
        c(7, 0.7, 0.3, 0.7, 0.3, 37.60, 54.50),
        c(8, 0.8, 0.2, 0.8, 0.2, 38.40, 62.00),
        c(9, 0.9, 0.1, 0.9, 0.1, 39.20, 69.50),
        c(10, 1, 0, 1, 0, 40.00, 77.00)
        )
  )

names(Choices)<-c("Choice","PSafeHigh","PSafeLow","PRiskyHigh","PRiskyLow","ExpectSafe","ExpectRisky")
```


Here is the variable list for both the r and mu equations from the paper.


* 1(Hypot) 
* 1(Female) 
* 1(Female)×1(Hypot)
* CRT Score 
* CRT×1(Hypot) 
* Numeracy Score 
* Numeracy×1(Hypot) 
* 1(EV-sufficient All Choices) 
* 1(EV-sufficient All)×1(Hypot) 


Note that EV-sufficient variable and Numeracy are missing so I will ommit.  He also does not use major, which he has given, me but does use gender, which he has not.  I can't replicate so I will use what I can.

* 1(Hypot) 
* CRT Score 
* CRT×1(Hypot) 

The limited list makes it easy and I can avoid stepping on toes.

Lets get the dataset into shape.

```r
#Just the variables that I need

MinimalSubset<-Original[,c("id","crt","treatment","safereal1","safereal2","safereal3","safereal4","safereal5","safereal6","safereal7","safereal8","safereal9","safereal10","safehypot1","safehypot2","safehypot3","safehypot4","safehypot5","safehypot6","safehypot7","safehypot8","safehypot9","safehypot10")]

library(reshape2)

#Turn this into long form.


#Add values from the Choices table
```

