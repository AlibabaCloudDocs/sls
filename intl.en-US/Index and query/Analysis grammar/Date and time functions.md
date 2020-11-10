# Date and time functions

This topic describes the date and time functions that you can use in Log Service to analyze log data. These functions include the time functions, date functions, interval functions, and a time series padding function.

## Date and time data types

-   Unix timestamp: an integer that indicates the number of seconds that have elapsed since 00:00:00 Thursday, January 1, 1970. For example, `1512374067` indicates `Mon Dec 4 15:54:27 CST 2017`. The `__time__` field in each log entry is of this type.
-   TimeStamp: a string that specifies the date and time, for example, `2017-11-01 13:30:00`.

## Date functions

The following table lists the date functions that are commonly used in Log Service.

|Function|Description|Example|
|:-------|:----------|:------|
|`current_date`|Returns the current date.|`latency>100| select current_date`|
|`current_time`|Returns the current time.|`latency>100| select current_time`|
|`current_timestamp`|Returns the current TimeStamp by combining the results of current\_date and current\_time functions.|`latency>100| select current_timestamp`|
|`current_timezone()`|Returns the current time zone.|`latency>100| select current_timezone()`|
|`from_iso8601_timestamp(string)`|Converts an ISO 8601-formatted time into a TimeStamp that contains the time zone.|`latency>100| select from_iso8601_timestamp(iso8601)`|
|`from_iso8601_date(string)`|Converts an ISO 8601-formatted time into a date.|`latency>100| select from_iso8601_date(iso8601)`|
|`from_unixtime(unixtime)`|Converts a UNIX timestamp into a TimeStamp.|`latency>100| select from_unixtime(1494985275)`|
|`from_unixtime(unixtime,string)`|Converts a Unix timestamp into a TimeStamp based on the time zone that is specified by the string parameter.|`latency>100| select from_unixtime (1494985275,'Asia/Shanghai')`|
|`localtime`|Returns the current local time.|`latency>100| select localtime`|
|`localtimestamp`|Returns the current local TimeStamp.|`latency>100| select localtimestamp`|
|`now()`|Equivalent to the `current_timestamp` function.|-|
|`to_unixtime(timestamp)`|Converts a TimeStamp into a Unix timestamp.|`*| select to_unixtime('2017-05-17 09:45:00.848 Asia/Shanghai')`|

## Time functions

The following table lists the time functions that you can use in Log Service to convert time in the formats supported in MySQL, such as %a, %b, and %y.

|Function|Description|Example|
|:-------|:----------|:------|
|`date_format(timestamp, format)`|Converts a TimeStamp into a string in the specified format.|`latency>100`\| `select date_format (date_parse('2017-05-17 09:45:00','%Y-%m-%d %H:%i:%S'), '%Y-%m-%d')`|
|`date_parse(string, format)`|Converts a string into a TimeStamp in the specified format.|`latency>100`\|`select date_format (date_parse(time,'%Y-%m-%d %H:%i:%S'), '%Y-%m-%d')`|

|Format|Description|
|:-----|:----------|
|%a|Abbreviated day name, such as Sun or Sat.|
|%b|Abbreviated month name, such as Jan or Dec.|
|%c|Month number. Valid values: 1 to 12.|
|%D|Day of month with a suffix, such as 0th, 1st, 2nd, and 3rd.|
|%d|Day of month. Valid values: 01 to 31.|
|%e|Day of month. Valid values: 1 to 31.|
|%H|The hour in the 24-hour clock.|
|%h|The hour in the 12-hour clock.|
|%I|The hour in the 12-hour clock.|
|%i|Minutes. Valid values: 00 to 59.|
|%j|Day of year. Valid values: 001 to 366.|
|%k|Hours. Valid values: 0 to 23.|
|%l|Hours. Valid values: 1 to 12.|
|%M|Month name. Valid values: January, February, March, April, May, June, July, August, September, October, November, and December.|
|%m|Month number. Valid values: 01 to 12.|
|%p|Meridian indicators. Valid values: AM and PM.|
|%r|The time in the 12-hour clock. The time is in the `hh:mm:ss AM/PM` format.|
|%S|Seconds. Valid values: 00 to 59.|
|%s|Seconds. Valid values: 00 to 59.|
|%T|The time in the 24-hour clock. The time is in the `hh:mm:ss` format.|
|%V|Week number of year. Sunday is the first day of the week. This format is used together with %X. Valid values: 01 to 53.|
|%v|Week number of year. Monday is the first day of the week. This format is used together with %x. Valid values: 01 to 53.|
|%W|Day name. Valid values: Sunday, Monday, Tuesday, Wednesday, Thursday, Friday, and Saturday.|
|%w|Day of week. Valid values: 0 to 6. The value 0 indicates Sunday.|
|%Y|4-digit year number.|
|%y|2-digit year number.|
|%%|Escapes the second percent sign \(%\).|

## Truncation function

You can use a truncation function in Log Service to truncate a time by the second, minute, hour, day, month, or year. The truncation function is applicable to time-based statistics.

-   Function syntax:

    ```
    date_trunc(unit, x)
    ```

-   Parameters:

    The value of the x parameter can be a TimeStamp or Unix timestamp.

    Assume that you set the x parameter to `2001-08-22 03:04:05.000`. The following table lists the valid values of the unit parameter and the execution result.

    |Unit|Result|
    |:---|:-----|
    |second|2001-08-22 03:04:05.000|
    |minute|2001-08-22 03:04:00.000|
    |hour|2001-08-22 03:00:00.000|
    |day|2001-08-22 00:00:00.000|
    |week|2001-08-20 00:00:00.000|
    |month|2001-08-01 00:00:00.000|
    |quarter|2001-07-01 00:00:00.000|
    |year|2001-01-01 00:00:00.000|

-   Example:

    You can use the date\_trunc function to truncate a time only by the second, minute, hour, day, month, or year. To truncate a time by specified intervals such as five minutes, you must use a group by clause based on the modulus method.

    ```
    * | SELECT count(1) as pv,  __time__ - __time__% 300 as minute5 group by minute5 limit 100
    ```

    In the preceding statement, `%300` indicates that modulo and truncation are performed every 5 minutes.

    The following example shows how to use the date\_trunc function:

    ```
    *|select  date_trunc('minute' ,  __time__)  as t,
           truncate (avg(latency) ) ,
           current_date  
           group by   t
           order by t  desc 
           limit 60
    ```


## Interval functions

You can use interval functions to perform the interval-related calculations. For example, you can add or subtract an interval based on a date, or calculate the interval between two dates.

|Function|Description|Example|
|:-------|:----------|:------|
|``date_add`(unit, value, timestamp)`|Adds an interval `value` of the `unit` type to a `timestamp`. To subtract an interval, use a negative `value`.|`date_add('day', -7, '2018-08-09 00:00:00')`: indicates seven days before August 9.|
|`date_diff(unit, timestamp1, timestamp2)`|Calculates the time difference between `timestamp1` and `timestamp2` by `unit`.|`date_diff('day', '2018-08-02 00:00:00', '2018-08-09 00:00:00') = 7`|

The following table lists the valid values of the unit parameter.

|Value|Description|
|:----|:----------|
|millisecond|Unit: milliseconds.|
|second|Unit: seconds.|
|minute|Unit: minutes.|
|hour|Unit: hours.|
|day|Unit: days.|
|week|Unit: weeks.|
|month|Unit: months.|
|quarter|Unit: quarters, namely, three months.|
|year|Unit: years.|

## Time series padding function

You can use the time series padding function to pad time series and corresponding data.

**Note:** This function must be used together with the `group by time order by time` clause. In this case, the `order by` clause does not support the `desc` sorting method.

-   Function syntax:

    ```
    time_series(time\_column, window, format, padding\_data)
    ```

-   Parameters:

    |Parameter|Description|
    |:--------|:----------|
    |time\_column|The sequence of time, for example, the default time field `__time__` that is provided by Log Service. The value is of the long or timestamp type.|
    |window|The size of the time window. The size is composed of a number and a unit. Valid units: s \(seconds\), m \(minutes\), h \(hours\), and d \(days\). For example, you can set the window to 2h, 5m, or 3d.|
    |format|The MySQL time format. It is the format of the output.|
    |padding\_data|The content to be added. Valid values:     -   0: adds 0.
    -   null: adds null.
    -   last: adds the last value.
    -   next: adds the next value.
    -   avg: adds the average value of the last and next values. |

-   Example:

    The following statement is used to format data every 2 hours:

    ```
    * | select time_series(__time__, '2h', '%Y-%m-%d %H:%i:%s', '0')  as stamp, count(*) as num from log group by stamp order by stamp
                            
    ```

    The following figure shows the output.

    ![Time series padding function](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3854994061/p37530.png)


