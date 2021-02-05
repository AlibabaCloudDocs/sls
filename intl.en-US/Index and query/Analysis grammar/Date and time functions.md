# Date and time functions

This topic describes the date and time functions that you can use in Log Service to analyze log data. The following types of functions are provided: time function, date function, truncation function, interval function, and time series padding function. You can use the functions to convert the date and time formats of log data. You can also use the functions to group and aggregate log data.

**Note:**

-   The timestamp of a log entry is accurate to seconds. Therefore, you can specify the time format only to seconds.
-   You need to specify the time format only for the time in a time string. Other parameters such as the time zone are not required.
-   Each log entry in Log Service contains the reserved \_\_time\_\_ field. The value of the field is a UNIX timestamp. For example, 1592374067 indicates 2020-06-17 14:07:47.

## Date functions

|Function|Description|Example|
|:-------|:----------|:------|
|current\_date|Returns the current date.-   Return value format: `YYYY-MM-DD`, for example, 2021-01-12.
-   Return value type: date.

|```
* | select current_date
``` |
|current\_time|Returns the current time.-   Return value format: `HH:MM:SS.Ms Time zone`, for example, 01:14:51.967 Asia/Shanghai.
-   Return value type: time.

|```
* | select current_time
``` |
|current\_timestamp|Returns the current date and time.-   Return value format: `YYYY-MM-DD HH:MM:SS.Ms Time zone`, for example, 2021-01-12 17:16:09.035 Asia/Shanghai.
-   Return value type: timestamp.

|```
* | select current_timestamp
``` |
|current\_timezone\(\)|Returns the current time zone.Return value type: varchar, for example, Asia/Shanghai.

|```
* | select current_timezone()
``` |
|localtime|Returns the local time.-   Return value format: `HH:MM:SS.Ms`
-   Return value type: time.

|```
* | select localtime
``` |
|localtimestamp|Returns the local date and time.-   Return value format: `YYYY-MM-DD HH:MM:SS.Ms`
-   Return value type: timestamp.

|```
* | select localtimestamp
``` |
|now\(\)|Returns the current date and time. This function is equivalent to the current\_timestamp function.-   Return value format: `YYYY-MM-DD HH:MM:SS.Ms Time zone`.
-   Return value type: timestamp.

|```
* | select now()
``` |
|from\_iso8601\_timestamp\(ISO8601\)|Converts an ISO 8601-formatted datetime expression to a timestamp expression that contains a time zone.-   Return value format: `YYYY-MM-DD HH:MM:SS.Ms Time zone`.
-   Return value type: timestamp.

|```
* | select from_iso8601_timestamp('2020-05-03T17:30:08')
``` |
|from\_iso8601\_date\(ISO8601\)|Converts an ISO 8601-formatted date expression to a date expression.-   Return value format: `YYYY-MM-DD`
-   Return value type: date.

|```
* | select from_iso8601_date('2020-05-03')
``` |
|from\_unixtime \(UNIX timestamp\)|Converts a UNIX timestamp to a timestamp expression.-   Return value format: `YYYY-MM-DD HH:MM:SS.Ms`.
-   Return value type: timestamp.

|```
* | select from_unixtime(1494985275)
``` |
|from\_unixtime \(UNIX timestamp,time zone\)|Converts a UNIX timestamp to a timestamp expression that contains a time zone.-   Return value format: `YYYY-MM-DD HH:MM:SS.Ms Time zone`.
-   Return value type: timestamp.

|```
* | select from_unixtime (1494985275,'Asia/Shanghai')
``` |
|to\_unixtime\(timestamp\)|Converts a timestamp expression to a UNIX timestamp.Return value type: long, for example, 1494985500.848.

|```
*| select to_unixtime('2017-05-17 09:45:00.848 Asia/Shanghai')
``` |

## Time functions

|Function|Description|Example|
|:-------|:----------|:------|
|date\_format\(timestamp,format\)|Converts a timestamp expression to a datetime format string.|```
* | select date_format (date_parse('2017-05-17 09:45:00','%Y-%m-%d %H:%i:%S'), '%Y-%m-%d')
``` |
|date\_parse\(string,format\)|Represents a datetime format string, and then converts the datetime format string to a timestamp expression. The following table describes the formats.|```
* | select date_format (date_parse(time,'%Y-%m-%d %H:%i:%S'), '%Y-%m-%d')
``` |

|format|Description|
|:-----|:----------|
|%a|The abbreviated day name, for example, Sun or Sat.|
|%b|The abbreviated month name, for example, Jan or Dec.|
|%c|The month. Valid values: 1 to 12.|
|%D|The day of the month, for example, 0th, 1st, 2nd, or 3rd.|
|%d|The day of the month, Valid values: 01 to 31.|
|%e|The day of the month, Valid values: 1 to 31.|
|%H|The hour in the 24-hour clock.|
|%h|The hour in the 12-hour clock.|
|%I|The hour in the 12-hour clock.|
|%i|The minutes. Valid values: 00 to 59.|
|%j|The day of the year. Valid values: 001 to 366.|
|%k|The hours. Valid values: 0 to 23.|
|%l|The hours. Valid values: 1 to 12.|
|%M|The full month name, for example, January or December.|
|%m|The month. Valid values: 01 to 12.|
|%p|The abbreviation that indicates the morning or afternoon. Valid values: AM and PM.|
|%r|The time in the 12-hour clock. The time is in the `hh:mm:ss AM/PM` format.|
|%S|The seconds. Valid values: 00 to 59.|
|%s|The seconds. Valid values: 00 to 59.|
|%T|The time in the 24-hour clock. The time is in the `hh:mm:ss` format.|
|%V|The week number of the year. Sunday is the first day of each week. Valid values: 01 to 53.|
|%v|The week number of a year. Monday is the first day of each week. Valid values: 01 to 53.|
|%W|The full day name, for example, Sunday or Saturday.|
|%w|The day of the week as a number. The value 0 indicates Sunday.|
|%Y|The four-digit year number, for example, 2020.|
|%y|The two-digit year number, for example, 20.|
|%%|Escapes the second percent sign \(%\).|

## Truncation function

The date\_trunc\(\) function truncates a datetime expression based on the specified part of a time. You can use a truncation function to truncate a time by second, minute, hour, day, month, or year. This function is suitable for time-based statistics.

-   Function syntax:

    ```
    date_trunc('unit',x)
    ```

-   Parameters:

    The value of the x parameter can be a datetime expression, for example, 2021-01-12 03:04:05.000 or 1610350836. The value of the x parameter can be a time field, for example, \_\_time\_\_. The valid values of the unit parameter are second, minute, hour, day, week, month, quarter, and year. The following table describes examples of this parameter.

    |Example|Result|Description|
    |:------|:-----|-----------|
    |\* \| select date\_trunc\('second', 2021-01-12 03:04:05.000\)|2021-01-12 03:04:05.000|None|
    |\* \| select date\_trunc\('minute', 2021-01-12 03:04:05.000\)|2021-01-12 03:04:00.000|None|
    |\* \| select date\_trunc\('hour', 2021-01-12 03:04:05.000\)|2021-01-12 03:00:00.000|None|
    |\* \| select date\_trunc\('day', 2021-01-12 03:04:05.000\)|2021-01-12 00:00:00.000|Returns 00:00:00.000 of the specified date.|
    |\* \| select date\_trunc\('week', 2021-01-12 03:04:05.000\)|2021-01-11 00:00:00.000|Returns 00:00:00.000 of the Monday of the specified week.|
    |\* \| select date\_trunc\('month', 2021-01-12 03:04:05.000\)|2021-01-01 00:00:00.000|Returns 00:00:00.000 of the first day of the specified month.|
    |\* \| select date\_trunc\('quarter', 2021-01-11 03:04:05.000\)|2021-01-01 00:00:00.000|Returns 00:00:00.000 of the first day of the specified quarter.|
    |\* \| select date\_trunc\('year', 2021-01-11 03:04:05.000\)|2021-01-01 00:00:00.000|Returns 00:00:00.000 of the first day of the specified year.|

-   Query and analysis examples:

    To truncate the average request durations by minute, and group and sort the average request durations by time, execute the following query statement:

    ```
    * | select  date_trunc('minute' ,  __time__)  as time,
           truncate (avg(request_time) ) as avg_time ,
           current_date as date
           group by  time
           order by time  desc 
           limit 100
    ```

    You can use the date\_trunc\('unit', x\) function to truncate a time only by second, minute, hour, day, month, or year. To truncate a time based on specified intervals such as 5 minutes, you must use a GROUP BY clause based on the modulus method.

    ```
    * | select count(1) as pv,  __time__ - __time__ %300 as time group by time limit 100
    ```

    In the preceding statement, `%300` indicates that modulo and truncation are performed every 5 minutes.


## Interval functions

You can use interval functions to perform the interval-related calculations. For example, you can add or subtract an interval based on a date, or calculate the interval between two dates.

|Function|Description|Example|
|:-------|:----------|:------|
|date\_add\(unit, N,timstamp\)|Adds N units to a `timestamp`.To subtract an interval, set the value of N to a negative value.

|```
*| select date_add('day', -7, '2018-08-09 00:00:00')
```

Indicates seven days before August 9, 2018 \(2018-08-02 00:00:00.000\).|
|date\_diff\(unit, timestamp1, timestamp2\)|Returns the time difference between two time expressions. For example, you can calculate the difference between `timestamp1` and `timestamp2` by unit.|```
*| select date_diff('day', '2018-08-02 00:00:00', '2018-08-09 00:00:00')
``` |

The following table describes the valid values of the unit parameter.

|unit|Description|
|:---|:----------|
|millisecond|Unit: milliseconds.|
|second|Unit: seconds.|
|minute|Unit: minutes.|
|hour|Unit: hours.|
|day|Unit: days.|
|week|Unit: weeks.|
|month|Unit: months.|
|quarter|Unit: quarters.|
|year|Unit: years.|

## Time series padding function

You can use the time\_series\(\) function to add the missing data when you query in the time window.

**Note:** The time\_series\(\) function must be used together with the GROUP BY and ORDER BY clauses. The ORDER BY clause does not support the desc sorting method.

-   Function syntax:

    ```
    time_series(time\_column, window, format, padding\_data)
    ```

-   Parameters:

    |Parameter|Description|
    |:--------|:----------|
    |time\_column|The sequence of time \(KEY\), for example, \_\_time\_\_.The value of this parameter can be a long datetime or timestamp expression. |
    |window|The size of the window. Valid units: s \(seconds\), m \(minutes\), h \(hours\), and d \(days\). For example, you can set the window to 2h, 5m, or 3d.|
    |format|The returned result is a format string.|
    |padding\_data|The content to be added. Valid values:     -   0: adds 0.
    -   null: adds null.
    -   last: adds the value of the last time point.
    -   next: adds the value of the next time point.
    -   avg: adds the average value of the last and next values. |

-   Example:

    To add missing data by 2 hours, execute the following query statement:

    ```
    * | select time_series(__time__, '2h', '%Y-%m-%d %H:%i:%s', '0')  as time, count(*) as num from log group by time order by time                        
    ```

    ![Time series padding function](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3854994061/p37530.png)


