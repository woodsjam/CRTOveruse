Looking at Taylor Data
========================================================
Load in the data

```r
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
##        id          crt                         major      treatment    
##  1      : 1   Min.   :0.00   Environmental Studies: 9   Min.   :0.000  
##  2      : 1   1st Qu.:0.00   Biology              : 7   1st Qu.:0.000  
##  3      : 1   Median :1.00   Business             : 6   Median :1.000  
##  4      : 1   Mean   :1.27   Human Physiology     : 6   Mean   :0.505  
##  5      : 1   3rd Qu.:2.00   Economics            : 4   3rd Qu.:1.000  
##  6      : 1   Max.   :3.00   Journalism           : 4   Max.   :1.000  
##  (Other):91                  (Other)              :61                  
##    dum_major1      dum_major2      dum_major3      dum_major4   
##  Min.   :0.000   Min.   :0.000   Min.   :0.000   Min.   :0.000  
##  1st Qu.:0.000   1st Qu.:0.000   1st Qu.:0.000   1st Qu.:0.000  
##  Median :0.000   Median :0.000   Median :0.000   Median :0.000  
##  Mean   :0.103   Mean   :0.186   Mean   :0.227   Mean   :0.196  
##  3rd Qu.:0.000   3rd Qu.:0.000   3rd Qu.:0.000   3rd Qu.:0.000  
##  Max.   :1.000   Max.   :1.000   Max.   :1.000   Max.   :1.000  
##                                                                 
##    dum_major5    safereal1       safereal2       safereal3      
##  Min.   :0.000   Mode :logical   Mode :logical   Mode :logical  
##  1st Qu.:0.000   FALSE:4         FALSE:2         FALSE:6        
##  Median :0.000   TRUE :93        TRUE :95        TRUE :91       
##  Mean   :0.289   NA's :0         NA's :0         NA's :0        
##  3rd Qu.:1.000                                                  
##  Max.   :1.000                                                  
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


