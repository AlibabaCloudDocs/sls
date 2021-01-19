# Time formats

When you use Logtail to collect logs, you must specify time formats based on the time strings of raw logs. Logtail extracts a time string from a raw log entry and parses the string into a UNIX timestamp. This topic describes common time formats and provides related examples.

## Commonly used time formats of logs

The following table describes the time formats that are supported by Logtail.

**Note:**

-   The timestamp of a log entry in Log Service is accurate to seconds. Therefore, you can specify the time format only to seconds.
-   You only need to specify the time format for the time in a time string. Other parameters such as the time zone are not required.
-   In Linux, Logtail supports all time formats provided by the strftime function. All log time strings that can be formatted by using the strftime function can be parsed and used by Logtail.

|Format|Description|Example|
|:-----|:----------|:------|
|%a|The abbreviated day name.|Fri|
|%A|The full day name.|Friday|
|%b|The abbreviated month name.|Jan|
|%B|The full month name.|January|
|%d|The day of a month. Valid values: 01 to 31.|07, 31|
|%h|The abbreviated month name. The format is equivalent to %b.|Jan|
|%H|The hour in the 24-hour clock.|22|
|%I|The hour in the 12-hour clock.|11|
|%m|The month. Valid values: 01 to 12.|08|
|%M|The minute. Valid values: 00 to 59.|59|
|%n|The line break.|A line break|
|%p|The abbreviation that indicates the morning or afternoon. Valid values: AM and PM.|AM or PM|
|%r|The time in the 12-hour format. The format is equivalent to %I:%M:%S %p.|11:59:59 AM|
|%R|The time expressed in hours and minutes. The format is equivalent to %H:%M.|23:59|
|%S|The second. Valid values: 00 to 59.|59|
|%t|The tab character.|N/A|
|%y|The two-digit year number. Valid values: 00 to 99.|04 or 98|
|%Y|The four-digit year number.|2004 or 1998|
|%C|The two-digit century number. Valid values: 00 to 99.|16|
|%e|The day of a month. Valid values: 1 to 31.Prefix a space to a single-digit number.

|7 or 31|
|%j|The day of a year. Valid values: 001 to 366.|365|
|%u|The day of a week as a number. Valid values: 1 to 7. The value 1 indicates Monday.|2|
|%U|The week of a year. Sunday is the first day of each week. Valid values: 00 to 53.|23|
|%V|The week of a year. Monday is the first day of each week. Valid values: 01 to 53. If a week that contains January 1 has four or more January days, the week is the first week of a year. Otherwise, the next week is considered the first week of a year.

|24|
|%w|The day of a week as a number. Valid values: 0 to 6. The value 0 indicates Sunday.|5|
|%W|The week of a year. Monday is the first day of each week. Valid values: 00 to 53.|23|
|%c|The date and time in the ISO 8601 format.|Tue Nov 20 14:12:58 2020|
|%x|The date in the ISO 8601 format.|Tue Nov 20 2020|
|%X|The time in the ISO 8601 format.|11:59:59|
|%s|The UNIX timestamp.|1476187251|

## Examples

The following table lists commonly used time formats. It also provides related examples and time expressions.

|Example|Time expression|Time format|
|:------|:--------------|-----------|
|2017-12-11 15:05:07|%Y-%m-%d %H:%M:%S|Custom|
|\[2017-12-11 15:05:07.012\]|\[%Y-%m-%d %H:%M:%S\]|Custom|
|02 Jan 06 15:04 MST|%d %b %y %H:%M|RFC822|
|02 Jan 06 15:04 -0700|%d %b %y %H:%M|RFC822Z|
|Monday, 02-Jan-06 15:04:05 MST|%A, %d-%b-%y %H:%M:%S|RFC850|
|Mon, 02 Jan 2006 15:04:05 MST|%A, %d %b %Y %H:%M:%S|RFC1123|
|2006-01-02T15:04:05Z07:00|%Y-%m-%dT%H:%M:%S|RFC3339|
|2006-01-02T15:04:05.999999999Z07:00|%Y-%m-%dT%H:%M:%S|RFC3339Nano|

