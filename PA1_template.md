# Reproducible Research: Peer Assessment 1

## Loading and preprocessing the data
1. Load the data (i.e. read.csv())

```r
library(plyr)
# unzip("activity.zip")
filename <- "activity.csv"
DT <- (read.csv(filename, header = T))
```

2. Process/transform the data (if necessary) into a format suitable for your analysis

```r
Data_na_rm <- na.omit(DT)
```

## What is mean total number of steps taken per day?
1. Make a histogram of the total number of steps taken each day

```r
steps_per_day <- ddply(Data_na_rm, .(date), summarise, steps=sum(steps))
hist(steps_per_day$steps,main="Histogram Total number of steps taken per day",
     xlab="Steps per day")
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png) 

2. Calculate and report the mean and median total number of steps taken per day

```r
mean  (steps_per_day$steps)
```

```
## [1] 10766.19
```

```r
median(steps_per_day$steps)
```

```
## [1] 10765
```

## What is the average daily activity pattern?
1. Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)

```r
average_date <- ddply(Data_na_rm, .(interval), summarise, steps=mean(steps))
plot(average_date$interval, average_date$steps, type="l", xlab="5-minute interval", 
ylab="Average steps",main="Average daily activity")
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png) 

2. Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

```r
average_date[average_date$steps==max(average_date$steps),]
```

```
##     interval    steps
## 104      835 206.1698
```

```r
# colnames(average_date)[2] <- "intervalAvg"
```

## Imputing missing values
1. Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)

```r
sum(is.na(DT$steps))
```

```
## [1] 2304
```

2. Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.

3. Create a new dataset that is equal to the original dataset but with the missing data filled in.

4. Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?


## Are there differences in activity patterns between weekdays and weekends?
1. Create a new factor variable in the dataset with two levels – “weekday” and “weekend” indicating whether a given date is a weekday or weekend day.

2. Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.

