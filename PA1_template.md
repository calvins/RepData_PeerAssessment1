# Reproducible Research: Peer Assessment 1


## A Taste of the Quantified Self - Insights into an Individual's Personal Activity Data Over Two Months

## Synopsis
This report will describe some findings uncovered in the activity tracking data of an anonymous individual in 2012.  It is expected that patterns in the number of steps taken daily should match those of an individual's everyday activities like, rest, travel, work, exercise, or leisure.  The data will be summarized by day and by weekday or weekend.  Totals and averages will be calculated in several groupings such as daily, by 5-minute interval in a day, and by day type; weekday or weekend.  Histograms, five number summaries, time series plots, and multiple panel plots will help us drill down to the patterns' details.  We found that the distribution of the total number of steps taken per day looks Gaussian, or Normally distributed with missing values (mean of 9,354, median of 10,395) and after imputing missing values (mean and median of 10,766).  The greatest average number of steps taken (206 steps) occurred around 8:35am, suggesting an individual's commute to a typical work location or exercise routine.  Other patterns in the time series plot suggests a decrease in the average number of steps taken during standard business hours (9am to 5pm), and no steps taken during typical rest periods (before 5am and after 10pm).  Lastly, the patterns in the time series plot of average steps taken per 5-minute interval show more activity during weekends suggesting the individual is enjoying their leisure time outside and is not at work, sitting at a desk.

## Data Processing
The data set is a CSV file that is compressed to a zip file stored in the working directory.  It contains data submitted by an anonymous individual recording the number of steps taken using an activity monitoring device like Fitbit.  We'll load the data with read.csv() and use unz() to access the zip file without uncompressing it.  To make grouping and summarizing easier, we'll configure dplyr.

```r
mydf <- read.csv(unz("activity.zip","activity.csv"),header=TRUE,sep=",")
library(dplyr)
activity <- tbl_df(mydf)
rm("mydf")
```

## Data Exploration
We'll view the size of the activity data frame and data from the first few rows using dim() and head().  With range() and the subtraction operator, we determine the date range is October 2012 to November 2012, a total of 61 days.  Every 5 minutes, the number of steps were recorded and indexed using 5 minute intervals named 0 to 55, 100 to 155, 200 to 255, etc., and 2300 to 2355.  Essentially the hours are named using the 24-hour clock and each interval is a multiple of five from 5 to 55.  There are a total of 288 five minute intervals a day, which the unique() function displays.

```r
dim(activity)
```

```
## [1] 17568     3
```

```r
head(activity)
```

```
## Source: local data frame [6 x 3]
## 
##   steps       date interval
## 1    NA 2012-10-01        0
## 2    NA 2012-10-01        5
## 3    NA 2012-10-01       10
## 4    NA 2012-10-01       15
## 5    NA 2012-10-01       20
## 6    NA 2012-10-01       25
```

```r
range(as.character(activity$date))
```

```
## [1] "2012-10-01" "2012-11-30"
```

```r
as.Date("2012-12-01")-as.Date("2012-10-01")
```

```
## Time difference of 61 days
```

```r
unique(activity$interval)
```

```
##   [1]    0    5   10   15   20   25   30   35   40   45   50   55  100  105
##  [15]  110  115  120  125  130  135  140  145  150  155  200  205  210  215
##  [29]  220  225  230  235  240  245  250  255  300  305  310  315  320  325
##  [43]  330  335  340  345  350  355  400  405  410  415  420  425  430  435
##  [57]  440  445  450  455  500  505  510  515  520  525  530  535  540  545
##  [71]  550  555  600  605  610  615  620  625  630  635  640  645  650  655
##  [85]  700  705  710  715  720  725  730  735  740  745  750  755  800  805
##  [99]  810  815  820  825  830  835  840  845  850  855  900  905  910  915
## [113]  920  925  930  935  940  945  950  955 1000 1005 1010 1015 1020 1025
## [127] 1030 1035 1040 1045 1050 1055 1100 1105 1110 1115 1120 1125 1130 1135
## [141] 1140 1145 1150 1155 1200 1205 1210 1215 1220 1225 1230 1235 1240 1245
## [155] 1250 1255 1300 1305 1310 1315 1320 1325 1330 1335 1340 1345 1350 1355
## [169] 1400 1405 1410 1415 1420 1425 1430 1435 1440 1445 1450 1455 1500 1505
## [183] 1510 1515 1520 1525 1530 1535 1540 1545 1550 1555 1600 1605 1610 1615
## [197] 1620 1625 1630 1635 1640 1645 1650 1655 1700 1705 1710 1715 1720 1725
## [211] 1730 1735 1740 1745 1750 1755 1800 1805 1810 1815 1820 1825 1830 1835
## [225] 1840 1845 1850 1855 1900 1905 1910 1915 1920 1925 1930 1935 1940 1945
## [239] 1950 1955 2000 2005 2010 2015 2020 2025 2030 2035 2040 2045 2050 2055
## [253] 2100 2105 2110 2115 2120 2125 2130 2135 2140 2145 2150 2155 2200 2205
## [267] 2210 2215 2220 2225 2230 2235 2240 2245 2250 2255 2300 2305 2310 2315
## [281] 2320 2325 2330 2335 2340 2345 2350 2355
```

## What is mean total number of steps taken per day?
The dplyr group_by and summarize functions makes it easy to compute the total number of steps taken per day, when we combine them with the sum() function and ignore missing values with the na.rm=TRUE argument.
The results are saved to a new R object totalStepsByDay and names() is helpful in making the variable names more descriptive.  We calculate and report the mean and median of the total number of steps taken per day using the R function summary().

A histogram of the total steps will show the data density; how common or rare values are.  A rug in the histogram helps show all the data points under the histogram and provides some fine detail.  We'll add a magenta vertical line to the histogram to denote the mean and a black vertical line to denote the median.  The mean total number of steps taken is 9,354.23 and the median is 10,395.00.

```r
totalStepsByDay <- summarize(group_by(activity, date), sum(steps,na.rm=TRUE))
names(totalStepsByDay) <-c("date","total")
options(digits=10)
summary(totalStepsByDay$total)
```

```
##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
##     0.00  6778.00 10395.00  9354.23 12811.00 21194.00
```


```r
hist(totalStepsByDay$total, col="aquamarine", main="Histogram - Total Steps per Day", breaks=5)
rug(totalStepsByDay$total)
abline(v = mean(totalStepsByDay$total), col = "magenta", lwd = 4)
abline(v = median(totalStepsByDay$total), col = "black", lwd = 4)
```

![](figure/plot1-1.png) 

## What is the average daily activity pattern?
Using dplyr summarize and group_by, we compute the means of steps for each 5-minute interval and ignore missing values.  We'll set the digits option to 10 to increase the number of significant digits, when viewing the summary and subset output.

The R summary function tells us the maximum mean number of steps is 206.1698, which we can plug in to the subset function and find the 5-minute interval with this value.  The interval 835 contains the maximum mean number of steps of 206.1698.

We'll plot the mean steps taken by 5-minute interval across all days.

```r
meansByInterval <- summarize(group_by(activity,interval),mean(steps,na.rm=TRUE))
names(meansByInterval) <-c("interval","means")

options(digits=10)
summary(meansByInterval$means)
```

```
##       Min.    1st Qu.     Median       Mean    3rd Qu.       Max. 
##   0.000000   2.485849  34.113210  37.382600  52.834910 206.169800
```

```r
subset(meansByInterval,means>206)
```

```
## Source: local data frame [1 x 2]
## 
##   interval       means
## 1      835 206.1698113
```


```r
with(meansByInterval,plot(interval,means,type="l", main="Average number of steps taken"))
```

![](figure/plot2-1.png) 

## Imputing missing values
Values are only missing in the steps variable and the logical vector, missingSteps helps identify the days when used with the unique() function.  The 8 days with missing values are 2012-10-01 2012-10-08 2012-11-01 2012-11-04 2012-11-09 2012-11-10 2012-11-14 2012-11-30.

The function nrow returns 2,304 total missing values in the data set, when we subset using the missingSteps logical vector.

Since we already have the means by interval for the entire data set when we plotted the average daily activity pattern, our strategy is to set the missing value for each interval to the mean for each interval for each of the eight days.

First, we'll create a new data set called completeActivity by subsetting activity and selecting rows with dates NOT EQUAL to 2012-10-01 2012-10-08 2012-11-01 2012-11-04 2012-11-09 2012-11-10 2012-11-14 2012-11-30.  This gives us the rows with recorded values for steps.

Then, we create temporary R objects, (add1, add2, add3, add4, add5, add6, add7, and add8) for each day with missing values and apply our strategy and set the missing values for each interval to the mean for each interval in the R object meansByInterval.

We merge each day with imputed values into the new data set completeActivity and check for missing values in the new data set and find none.
We do some housekeeping by clearing the temporary R objects, add1 to add8.

To create our data set for the plot, we use dplyr group_by and summarize to compute total number of steps taken per day using the completeActivity data set that contains imputed missing values and save in a new R object, totalStepsByDayCompleteActivity and make the variable names more descriptive.

We calculate and report the mean and median total number of steps taken per day using the the data set with imputed missing values and compare to the summary from the original data set.

We make a histogram of the total number of steps taken each day using the data set with imputed missing values and compare to the histogram we did earlier.  It is easier to compare the plots, when one is placed on top of the other.  In the new histogram, the mean and median are the same, so we distinguish the mean by making it a wide magenta dotted line.


```r
sum(is.na(activity$date)) # 0 rows
```

```
## [1] 0
```

```r
sum(is.na(activity$interval)) # 0 rows
```

```
## [1] 0
```

```r
missingSteps <- is.na(activity$steps)
unique(activity[missingSteps,2]) 
```

```
## Source: local data frame [8 x 1]
## 
##         date
## 1 2012-10-01
## 2 2012-10-08
## 3 2012-11-01
## 4 2012-11-04
## 5 2012-11-09
## 6 2012-11-10
## 7 2012-11-14
## 8 2012-11-30
```

```r
nrow(activity[missingSteps,]) # 2,304 rows
```

```
## [1] 2304
```

```r
completeActivity <- subset(activity,!(date %in% c("2012-10-01","2012-10-08","2012-11-01","2012-11-04","2012-11-09","2012-11-10","2012-11-14","2012-11-30")))

add1 <- data.frame(steps=meansByInterval$means,date="2012-10-01",interval=meansByInterval$interval)
add2 <- data.frame(steps=meansByInterval$means,date="2012-10-08",interval=meansByInterval$interval)
add3 <- data.frame(steps=meansByInterval$means,date="2012-11-01",interval=meansByInterval$interval)
add4 <- data.frame(steps=meansByInterval$means,date="2012-11-04",interval=meansByInterval$interval)
add5 <- data.frame(steps=meansByInterval$means,date="2012-11-09",interval=meansByInterval$interval)
add6 <- data.frame(steps=meansByInterval$means,date="2012-11-10",interval=meansByInterval$interval)
add7 <- data.frame(steps=meansByInterval$means,date="2012-11-14",interval=meansByInterval$interval)
add8 <- data.frame(steps=meansByInterval$means,date="2012-11-30",interval=meansByInterval$interval)

completeActivity <- rbind(completeActivity,add1)
completeActivity <- rbind(completeActivity,add2)
completeActivity <- rbind(completeActivity,add3)
completeActivity <- rbind(completeActivity,add4)
completeActivity <- rbind(completeActivity,add5)
completeActivity <- rbind(completeActivity,add6)
completeActivity <- rbind(completeActivity,add7)
completeActivity <- rbind(completeActivity,add8)

sum(is.na(completeActivity$steps)) # 0 rows
```

```
## [1] 0
```

```r
rm("add1");rm("add2");rm("add3");rm("add4");rm("add5");rm("add6");rm("add7");rm("add8");

totalStepsByDayCompleteActivity <- summarize(group_by(completeActivity, date), sum(steps,na.rm=TRUE))
names(totalStepsByDayCompleteActivity) <-c("date","totals")

options(digits=10)
summary(totalStepsByDayCompleteActivity$totals)
```

```
##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
##    41.00  9819.00 10766.19 10766.19 12811.00 21194.00
```

```r
summary(totalStepsByDay$total)
```

```
##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
##     0.00  6778.00 10395.00  9354.23 12811.00 21194.00
```


```r
par(mfrow = c(2,1))
hist(totalStepsByDay$total, col="aquamarine", main="Histogram - Total Steps per Day", breaks=5)
rug(totalStepsByDay$total)
abline(v = mean(totalStepsByDay$total), col = "magenta", lwd = 4)
abline(v = median(totalStepsByDay$total), col = "black", lwd = 4)

hist(totalStepsByDayCompleteActivity$totals, col="rosybrown", main="Histogram -Total Steps per Day using Imputed Missing Values", breaks=5)
rug(totalStepsByDayCompleteActivity$totals)

abline(v = mean(totalStepsByDayCompleteActivity$totals), col = "magenta", lwd = 8,lty=3)
abline(v = median(totalStepsByDayCompleteActivity$totals), col = "black", lwd = 4)
```

![](figure/plot3-1.png) 

## Are there differences in activity patterns between weekdays and weekends?
We need a custom function makeDayTypeFactor that takes a vector of dates and returns a vector of factor variables with levels weekday and weekend, using the weekdays() function.  This can be used to create a new dayType variable in the completeActivity data set that has imputed missing values.

With the new factor variable, we use dplyr group_by and summarize to compute mean number of steps taken, by interval and day type and save in the new R object meansByIntervalByDayTypeCompleteActivity and make the variable names more descriptive.

Finally, we use lattice to make a panel plot containing a time series plot of the 5-minute interval and the average number of steps taken, averaged across all weekdays or weekends.

```r
makeDayTypeFactor <- function(v) {
    results <- vector()
    for (s in v) {
        if (weekdays(strptime(s,"%Y-%m-%d")) %in% c("Monday","Tuesday","Wednesday","Thursday","Friday")) {
            results <- c(results,"weekday")
        } else {
            results <- c(results,"weekend")
        }        
    }
    as.factor(results)
}

completeActivity$dayType <- makeDayTypeFactor(completeActivity$date)

meansByIntervalByDayTypeCompleteActivity <- summarize(group_by(completeActivity, interval, dayType), mean(steps))

names(meansByIntervalByDayTypeCompleteActivity) <-c("interval","dayType", "means")
```


```r
library(lattice)
xyplot(means ~ interval | dayType, type="l", data=meansByIntervalByDayTypeCompleteActivity, layout = c(1,2))
```

![](figure/plot4-1.png) 

## Results
### What is mean total number of steps taken per day?
The mean total number of steps taken per day is 9,354.23 with a median of 10,395.00.  The 1st Quartile is 6,778, 3rd Quartile is 12,811, and a maximum of 21,194.  The histogram shows total steps concentrated in the range 9,000 to 15,000.  There are more data values in the range 0 to 10,000, than in the range 15,000 to 25,000.

### What is the average daily activity pattern?
The largest mean number of steps taken within a day's 5-minute interval is 206.1698 in the 835 interval or at 8:35 am.  From intervals 0 to 500, the average number of steps is usually 0.  From time intervals 500 to about 900, the average number of steps increase from 0 to about 200.  There seems to be a pattern for the average number of steps for the intervals 1000 to 2000.  The average moves from around 50 to 100 and back to 50, during the ranges, 1000 to 1500, 1500 to 1750, and 1750 to 2000.  From intervals 2000 to 2355, the average moves gradually from 50 to 0.

### Imputing missing values - Do these values differ from the estimates from the first part of the assignment?
Yes, in the original data set, the mean total number of steps taken per day is 9,354.23 with a median of 10,395.00.  The 1st Quartile is 6,778, 3rd Quartile is 12,811, and a maximum of 21,194.  The histogram shows total steps concentrated in the range 9,000 to 15,000.  There are more data values in the range 0 to 10,000, than in the range 15,000 to 25,000.

After imputing missing values, the mean and median total number of steps taken per day is 10,766.19.  The 1st Quartile is 9,819, 3rd Quartile is 12,811, and a maximum of 21,194.  The histogram shows total steps concentrated in the range 7,500 to 15,000.  There are less data values in the ranges 0 to 5,000, and the range 20,000 to 25,000.

### Imputing missing values - What is the impact of imputing missing data on the estimates of the total daily number of steps?
After imputing missing values, and repeating the same analysis of the total number of steps per day, the new data set distribution looks more Gaussian or normal.  When we compare the two histograms, the new plot contains more data about the mean.  As a matter of fact, the mean and median are the same after imputing missing data.

### Are there differences in activity patterns between weekdays and weekends?
Yes, on weekends, the average steps taken per day goes to 50 later in the day than on the weekdays; about 750 compared to about 550 suggesting that the individual got out of bed later because there is no work.

Also, the pattern of the average in the intervals 1000 to 2000 is different on weekends.  The average moved to a high of about 150 on weekends, compared to 100 on weekdays.  There were also a couple of patterns visible during the weekends where the average moved from 50 to 100 and back to 50; the average also moved from 50 to 150 and back to 50.  These higher average steps taken daily suggest the individual is enjoying leisure activities, instead of sitting behind a desk doing work.

During weekends and weekdays, there was a similar pattern where the average steps moved from 50 to 0 in the intervals 2000 to 2300 suggesting the individual is resting after an eventful day!
