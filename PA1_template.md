# PA1_template
PIMM  
13 de octubre de 2015  

Show any code that is needed to

-Load the data (i.e. read.csv())

-Process/transform the data (if necessary) into a format suitable for your analysis


```r
data<-read.csv("activity.csv",header=TRUE)
data2<-tapply(data$steps,data$date,sum)
```

#What is mean total number of steps taken per day?

For this part of the assignment, you can ignore the missing values in the dataset.

-Calculate the total number of steps taken per day


```r
data2<-tapply(data$steps,data$date,sum)
```

-If you do not understand the difference between a histogram and a barplot, research the difference between them. Make a histogram of the total number of steps taken each day

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png) 

-Calculate and report the mean and median of the total number of steps taken per day


```r
mean(data2,na.rm=TRUE)
```

```
## [1] 10766.19
```

```r
median(data2,na.rm=TRUE)
```

```
## [1] 10765
```

#What is the average daily activity pattern?

-Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)


```r
data3<-tapply(data$steps,data$interval,mean,na.rm=TRUE)
data33<-data.frame(as.numeric(names(data3)),data3)
names(data33)<-c("Intervalo","media")
plot(data33$Intervalo, data33$media,ylab="Mean", xlab="Interval", type="l")
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png) 

-Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?



```r
data33[data33$media==max(data33$media),]
```

```
##     Intervalo    media
## 835       835 206.1698
```

#Imputing missing values

Note that there are a number of days/intervals where there are missing values (coded as NA). The presence of missing days may introduce bias into some calculations or summaries of the data.

-Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)


```r
data.nul<-data[is.na(data$steps),]
nrow(data.nul)
```

```
## [1] 2304
```

-Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.


-Create a new dataset that is equal to the original dataset but with the missing data filled in.



```r
i<-c()
id<-1:nrow(data)
for (i in id) {

    if  (is.na(data$steps[i])) 
	{
	data$steps[i]=data33[data33$Intervalo==data$interval[i],"media"] 
	}
}
```

-Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?


```r
data2<-tapply(data$steps,data$date,sum)

hist(data2,xlab="Number of Steps per day",ylab="Days Number",col="Red",main="Step Histogram (NAs filled)")
```

![](PA1_template_files/figure-html/unnamed-chunk-9-1.png) 

Mean and median


```r
mean(data2,na.rm=TRUE)
```

```
## [1] 10766.19
```

```r
median(data2,na.rm=TRUE)
```

```
## [1] 10766.19
```

We plot again having filled Nas 


```r
data3<-tapply(data$steps,data$interval,mean,na.rm=TRUE)
data33<-data.frame(as.numeric(names(data3)),data3)
names(data33)<-c("Intervalo","media")
plot(data33$Intervalo, data33$media,ylab="Mean", xlab="Interval", type="l")
```

![](PA1_template_files/figure-html/unnamed-chunk-11-1.png) 

```r
data33[data33$media==max(data33$media),]
```

```
##     Intervalo    media
## 835       835 206.1698
```

#Are there differences in activity patterns between weekdays and weekends?

For this part the weekdays() function may be of some help here. Use the dataset with the filled-in missing values for this part.

-Create a new factor variable in the dataset with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or weekend day.


```r
data$date<-as.Date(data$date)
weekdays1<-c('lunes','martes','miercoles','jueves','viernes')
data$wekday<-factor((weekdays(data$date) %in% weekdays1),levels=c(FALSE,TRUE), labels=c('weekend','weekday'))
```

-Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.

Weekdays


```r
datawd<-subset(data,data$wekday=="weekday")
datawd3<-tapply(datawd$steps,datawd$interval,mean,na.rm=TRUE)
datawd33<-data.frame(as.numeric(names(datawd3)),datawd3)
names(datawd33)<-c("Intervalo","media")
```

Weekends


```r
datawe<-subset(data,data$wekday=="weekend")
datawe3<-tapply(datawe$steps,datawe$interval,mean,na.rm=TRUE)
datawe33<-data.frame(as.numeric(names(datawe3)),datawe3)
names(datawe33)<-c("Intervalo","media")
```

Plot

```r
plot(datawd33$Intervalo, datawd33$media,ylab="Mean", xlab="Interval", type="l",col="red")
points(datawe33$Intervalo, datawe33$media,ylab="Mean", xlab="Interval", type="l",col="blue")

legend("topright",legend=c("weekday","weekend"),col=c("red","blue"),lty=c(1,1))
```

![](PA1_template_files/figure-html/unnamed-chunk-15-1.png) 
