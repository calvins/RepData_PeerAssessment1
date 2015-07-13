# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

```r
# load  # use dplyr # activity
# activityByDay, totalStepsByDay - group by day and compute totals
# activityByInterval, meansByInterval - group by interval and compute means
# totalsByInterval do I need this?
# missingSteps, add1, add2, add3, add4, add5, add6, add7, add8
# completeActivity  - make new dataset with missing values filled in
# completeActivityByDay, totalStepsByDayCompleteActivity - group new dataset with missing values filled in by day and compute totals
# create dayType factor variable on new dataset with missing values filled in
# completeActivityByIntervalByDayType, meansByIntervalByDayTypeCompleteActivity - group new dataset with missing values filled in by interval and day type and compute means

# - [1] "activity"                                 "activityByDay"                           
# - [3] "activityByInterval"                       "completeActivity"                        
# - [5] "completeActivityByDay"                    "completeActivityByIntervalByDayType"     
# - [7] "makeDayTypeFactor"                        "meansByInterval"                         
# - [9] "meansByIntervalByDayTypeCompleteActivity" "missingSteps"                            
# - [11] "totalsByInterval"                         "totalStepsByDay"                         
# - [13] "totalStepsByDayCompleteActivity" 
```


## What is mean total number of steps taken per day?

```r
# For this part of the assignment, you can ignore the missing values in the dataset.
# Calculate the total number of steps taken per day
# If you do not understand the difference between a histogram and a barplot, research the difference between them. Make a histogram of the total number of steps taken each day
# Calculate and report the mean and median of the total number of steps taken per day
```


## What is the average daily activity pattern?

```r
# Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)
# Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?
```



## Imputing missing values

```r
# Note that there are a number of days/intervals where there are missing values (coded as NA). The presence of missing days may introduce bias into some calculations or summaries of the data.
# 
# Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)
# 
# Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.
# 
# Create a new dataset that is equal to the original dataset but with the missing data filled in.
# 
# Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?
```



## Are there differences in activity patterns between weekdays and weekends?

```r
# For this part the weekdays() function may be of some help here. Use the dataset with the filled-in missing values for this part.
# 
# Create a new factor variable in the dataset with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or weekend day.
# 
# Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.
```
