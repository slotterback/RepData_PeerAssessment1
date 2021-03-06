# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data
This is my attempt to find a pattern in an individual's steps taken every day for a week.  

First, we must load the data (unzipped beforehand):


```r
raw.data <- read.csv(unz("C:/Users/Steve/Documents/Coursera Documents/Reproducible/Assignment 1/RepData_PeerAssessment1/activity.zip","activity.csv"))
```

Then, we convert the data in the date column using the as.Date function as follows:


```r
raw.data$date <- as.Date(raw.data$date)
```

Finally, we add a minutes.since.midnight column to facilitate time-based calculations:


```r
raw.data$minutes.since.midnight <-raw.data$interval %% 100 + 60*(raw.data$interval %/% 100)
```


## What is mean total number of steps taken per day?

Now that we have the Data are loaded and ready, we calculate the total number of steps per day, discounting missing data.


```r
Days <-unique(raw.data$date)
steps.per.day <- rep(0,length(Days))
j <- 1 #steps per day index
for(i in Days)  {
    temp <- raw.data[raw.data$date == i,]
    steps.per.day[j] <- sum(temp$steps,na.rm = TRUE)
    j <- j+1
}
```

Here is a histogram of the plots:


```r
hist(steps.per.day)
```

![plot of chunk unnamed-chunk-5](./PA1_submission_files/figure-html/unnamed-chunk-5.png) 

The mean and median steps per day are:


```r
mean(steps.per.day)
```

```
## [1] 9354
```

```r
median(steps.per.day)
```

```
## [1] 10395
```

```r
## What is the average daily activity pattern?
```

```r
mean.data <- NULL
mean.data$interval <- (0:287)*5
mean.data$steps <- rep(0,length(mean.data$interval))
j <- 1
for(i in mean.data$interval)  {
  mean.data$steps[j] <- mean(raw.data$steps[raw.data$minutes.since.midnight == i],na.rm = TRUE)
  j <- j +1
}
```

Here is a plot of the mean steps time series:


```r
plot(mean.data$interval,mean.data$steps)
```

![plot of chunk unnamed-chunk-8](./PA1_submission_files/figure-html/unnamed-chunk-8.png) 

The 5 minute interval (measured in minutes after midnight) where the maximum number of steps typically occurs is:


```r
mean.data$interval[mean.data$steps == max(mean.data$steps)]
```

```
## [1] 515
```

## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?
