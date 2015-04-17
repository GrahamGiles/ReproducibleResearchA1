---
title: "Reproducible Research Assignment 1"
author: "Graham Giles"
date: "Thursday, April 16, 2015"
output: html_document
---

This is an analysis of the number of steps taken by a certain individual.

**What is mean total number of steps taken per day?**

This is the R code.


```r
data1 <- na.omit(read.csv("activity.csv"))
```

```
## Warning in file(file, "rt"): cannot open file 'activity.csv': No such file
## or directory
```

```
## Error in file(file, "rt"): cannot open the connection
```

```r
data2 <- aggregate(steps ~ date, data = data1, FUN = sum)
```

```
## Error in eval(expr, envir, enclos): object 'data1' not found
```

```r
abc <- data2[2]
```

```
## Error in eval(expr, envir, enclos): object 'data2' not found
```

```r
abc1 <- abc[2:nrow(abc),]
```

```
## Error in eval(expr, envir, enclos): object 'abc' not found
```

This is the mean number of steps taken per day:

```r
mean(abc1)
```

```
## Error in mean(abc1): object 'abc1' not found
```

This is the median number of steps taken per day:

```r
median(abc1)
```

```
## Error in median(abc1): object 'abc1' not found
```

Here is a plot of the data:


```r
hist(data2$steps, main = "Histogram of Total Steps per Day", xlab = "Number of Steps per Day")
```

```
## Error in hist(data2$steps, main = "Histogram of Total Steps per Day", : object 'data2' not found
```



**What is the average daily activity pattern?**

This is the R code.


```r
data3 <- aggregate(steps ~ interval, data = data1, FUN = mean)
```

```
## Error in eval(expr, envir, enclos): object 'data1' not found
```

Here is a plot of the data.


```r
plot(data3, type = "l", main = "Average Daily Activity Pattern")
```

```
## Error in plot(data3, type = "l", main = "Average Daily Activity Pattern"): object 'data3' not found
```

This is the 5-minute interval which, on average accross all the days in the dataset, contains the maximum number of steps.


```r
abc <- data3[2]
```

```
## Error in eval(expr, envir, enclos): object 'data3' not found
```

```r
abc1 <- abc[2:nrow(abc),]
```

```
## Error in eval(expr, envir, enclos): object 'abc' not found
```

```r
abc2 <- subset(data3, data3[2] == max(abc1))
```

```
## Error in subset(data3, data3[2] == max(abc1)): object 'data3' not found
```

```r
abc3 <- abc2[1,1]
```

```
## Error in eval(expr, envir, enclos): object 'abc2' not found
```

```r
abc3
```

```
## Error in eval(expr, envir, enclos): object 'abc3' not found
```



**Imputing missing values**

The following is the number of NA values in the data set:


```r
data0 <- read.csv("activity.csv")
```

```
## Warning in file(file, "rt"): cannot open file 'activity.csv': No such file
## or directory
```

```
## Error in file(file, "rt"): cannot open the connection
```

```r
na1 <- nrow(is.na(data0[1]))
```

```
## Error in nrow(is.na(data0[1])): object 'data0' not found
```

```r
na1
```

```
## Error in eval(expr, envir, enclos): object 'na1' not found
```


In order to fill in the missing data in the data set, an NA values will be replaced with the average for that 5-second interval.

See R code below for the new data set which uses the strategy to replace NA values. This is described above.


```r
##Aggregate each interval by taking the mean.
data4 <- aggregate(steps ~ interval, data = data1, FUN = mean)
```

```
## Error in eval(expr, envir, enclos): object 'data1' not found
```

```r
df <- data.frame()

##For each interval:
for (n in data0[3]) {
  ##Create a subsetted data set (def0) for interval n.
  def0 <- subset(data0, data0[3] == n)
  ##From the mean aggregated data set above (data4), subset for interval n.
  def1 <- subset(data4, data4$interval == n)
  as.character(def0$steps)
  ##Replace NA values with the mean steps taken for interval n in data set def0
  def0[is.na(def0)] <- def1[1,2]
  ##Append processed data to new dataframe.
  df <- rbind(df, def0)
}
```

```
## Error in eval(expr, envir, enclos): object 'data0' not found
```

This is the mean number of steps taken per day, with NAs replace using methodology described above:

```r
data5 <- aggregate(steps ~ date, data = df, FUN = sum)
```

```
## Error in eval(expr, envir, enclos): object 'steps' not found
```

```r
abc5 <- data5[2]
```

```
## Error in eval(expr, envir, enclos): object 'data5' not found
```

```r
abc51 <- abc5[2:nrow(abc5),]
```

```
## Error in eval(expr, envir, enclos): object 'abc5' not found
```

```r
mean(abc51)
```

```
## Error in mean(abc51): object 'abc51' not found
```

This is the median number of steps taken per day, with NAs replace using methodology described above:

```r
median(abc51)
```

```
## Error in median(abc51): object 'abc51' not found
```

Here is a plot of the data:


```r
hist(data5$steps, main = "Histogram of Total Steps per Day", xlab = "Number of Steps per Day")
```

```
## Error in hist(data5$steps, main = "Histogram of Total Steps per Day", : object 'data5' not found
```

The following differences were noted in the original dataset, and the dataset with NA values replaced using the methodolgy described above.

1. The mean and median values are slightly different
2. The data is more left skewed

Furthermore, there appears to have been certain days within the date range that had NA values for the entire day. Replacing the NA values using the methodology described above adds days that were not included in the original data set.



**Are there differences in activity patterns between weekdays and weekends?**

Here is the associated R code:


```r
##Convert the date column into days of the week
df$date <- as.Date(as.character(df$date))
df$date <- weekdays(df$date)

##Subset data into 2 data sets: one for weekdays (dfwkdy) and one for weekends (dfwknd)

dfwkdy <- subset(df, df$date == "Monday" | df$date == "Tuesday" | df$date == "Wednesday" | df$date == "Thursday" | df$date == "Friday")
dfwknd <- subset(df, df$date == "Saturday" | df$date == "Sunday")

##Aggregate data to show mean number of steps taken per 5-minute interval, averaged accross all days. Perform this aggregation on both weekday and weekend data sets.
dfwkdyagg <- aggregate(steps ~ interval, data = dfwkdy, FUN = mean)
```

```
## Error in eval(expr, envir, enclos): object 'steps' not found
```

```r
dfwkndagg <- aggregate(steps ~ interval, data = dfwknd, FUN = mean)
```

```
## Error in eval(expr, envir, enclos): object 'steps' not found
```

Here are the plots:


```r
par(mfrow = c(2,1))
plot(dfwkndagg$interval, dfwkndagg$steps, xlab = "Interval", ylab = "Number of steps", main ="Weekend", type ="l")
```

```
## Error in plot(dfwkndagg$interval, dfwkndagg$steps, xlab = "Interval", : object 'dfwkndagg' not found
```

```r
plot(dfwkdyagg$interval, dfwkdyagg$steps, xlab = "Interval", ylab = "Number of steps", main = "Weekday", type ="l")
```

```
## Error in plot(dfwkdyagg$interval, dfwkdyagg$steps, xlab = "Interval", : object 'dfwkdyagg' not found
```
