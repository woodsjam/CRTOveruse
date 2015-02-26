Looking at Taylor Data
========================================================
Load in the data

```r
#Original <- read.csv("~/Dropbox/Research/CRTOveruse/OriginalData/Original.csv")

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
LongForm<-melt(MinimalSubset,id.vars=c('id','treatment','crt'))

LongForm$Choice<-LongForm$variable

library(stringr)
LongForm$Choice<-str_replace(LongForm$Choice,'safereal','')
LongForm$Choice<-str_replace(LongForm$Choice,'safehypot','')
LongForm$Choice<-as.factor(LongForm$Choice)
#Add values from the Choices table

LongForm<-merge(LongForm, Choices[,c('Choice','ExpectSafe','ExpectRisky')], by='Choice')

summary(LongForm)
```

```
##      Choice          id         treatment           crt       
##  1      :194   1      :  20   Min.   :0.0000   Min.   :0.000  
##  10     :194   2      :  20   1st Qu.:0.0000   1st Qu.:0.000  
##  2      :194   3      :  20   Median :1.0000   Median :1.000  
##  3      :194   4      :  20   Mean   :0.5052   Mean   :1.268  
##  4      :194   5      :  20   3rd Qu.:1.0000   3rd Qu.:2.000  
##  5      :194   6      :  20   Max.   :1.0000   Max.   :3.000  
##  (Other):776   (Other):1820                                   
##       variable      value           ExpectSafe    ExpectRisky   
##  safereal1:  97   Mode :logical   Min.   :32.8   Min.   : 9.50  
##  safereal2:  97   FALSE:737       1st Qu.:34.4   1st Qu.:24.80  
##  safereal3:  97   TRUE :1203      Median :36.4   Median :43.25  
##  safereal4:  97   NA's :0         Mean   :36.4   Mean   :43.28  
##  safereal5:  97                   3rd Qu.:38.4   3rd Qu.:62.00  
##  safereal6:  97                   Max.   :40.0   Max.   :77.00  
##  (Other)  :1358
```

That should be in a usable form now.

# Test of simulator estimator for a linear model

I asked on stackexchange if anyone had a reference for errors in variables when the distribution of the error was known.  Until an answer showes up, I will work with a simulation estimator.


Fake data for test.


```r
Fake<-data.frame(rnorm(100),rnorm(100))
names(Fake)<-c('X1','XTrue')

Fake$Noise<-rgamma(100,shape=2, rate=2)
Fake$X2<-Fake$XTrue+Fake$Noise

Fake$epsilon<-rnorm(100)

Fake$Y<- 10 + 5*Fake$X1 + 3*Fake$XTrue +Fake$epsilon

summary(Fake)
```

```
##        X1               XTrue             Noise               X2          
##  Min.   :-2.75640   Min.   :-2.9102   Min.   :0.06037   Min.   :-1.55570  
##  1st Qu.:-0.72085   1st Qu.:-0.9872   1st Qu.:0.41508   1st Qu.:-0.08269  
##  Median :-0.07147   Median :-0.1244   Median :0.65608   Median : 0.70373  
##  Mean   :-0.07490   Mean   :-0.1408   Mean   :0.90161   Mean   : 0.76080  
##  3rd Qu.: 0.67081   3rd Qu.: 0.5522   3rd Qu.:1.21436   3rd Qu.: 1.43481  
##  Max.   : 3.14674   Max.   : 3.3569   Max.   :3.38720   Max.   : 3.84149  
##     epsilon               Y         
##  Min.   :-2.79412   Min.   :-7.007  
##  1st Qu.:-0.85250   1st Qu.: 4.714  
##  Median :-0.08326   Median : 8.605  
##  Mean   :-0.13391   Mean   : 9.069  
##  3rd Qu.: 0.56239   3rd Qu.:13.494  
##  Max.   : 2.69731   Max.   :25.525
```

Lets see what OLS turns up


```r
summary(lm(Y~X1+X2,data=Fake))
```

```
## 
## Call:
## lm(formula = Y ~ X1 + X2, data = Fake)
## 
## Residuals:
##    Min     1Q Median     3Q    Max 
## -7.525 -1.558  0.538  1.564  3.786 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)   7.5790     0.2748   27.58   <2e-16 ***
## X1            4.6678     0.2141   21.80   <2e-16 ***
## X2            2.4182     0.1995   12.12   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.283 on 97 degrees of freedom
## Multiple R-squared:  0.8673,	Adjusted R-squared:  0.8645 
## F-statistic: 316.9 on 2 and 97 DF,  p-value: < 2.2e-16
```

Bias all over the place.

Now lets get an estimator in place for this linear model


```r
Augment<-function(variable, shape, rate){
  variable-rgamma(length(variable), shape, rate)
}

summary(lm(Y~X1+Augment(X2,2,2),data=Fake))
```

```
## 
## Call:
## lm(formula = Y ~ X1 + Augment(X2, 2, 2), data = Fake)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -7.1602 -1.7793 -0.0154  1.5777  8.5351 
## 
## Coefficients:
##                   Estimate Std. Error t value Pr(>|t|)    
## (Intercept)         9.8893     0.2997  32.997  < 2e-16 ***
## X1                  4.6481     0.2739  16.968  < 2e-16 ***
## Augment(X2, 2, 2)   1.5133     0.2095   7.223 1.15e-10 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.919 on 97 degrees of freedom
## Multiple R-squared:  0.783,	Adjusted R-squared:  0.7785 
## F-statistic:   175 on 2 and 97 DF,  p-value: < 2.2e-16
```

So, even with the shape and rate paramters known with certainty, I don't find the true parameter on the X2 variable.  

Lets run it a few times to make sure.  It may show up as an expectation.


```r
summary(replicate(400,lm(Y~X1+Augment(X2,2,2),data=Fake)$coef[3]))
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   1.359   1.673   1.777   1.777   1.882   2.190
```
and the answer is no.
