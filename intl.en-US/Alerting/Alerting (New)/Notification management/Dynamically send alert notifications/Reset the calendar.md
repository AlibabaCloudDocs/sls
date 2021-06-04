# Reset the calendar

Log Service supports resetting the global default calendar. You can modify the default attributes of dates, such as holidays or business days.

For example, if a company decided to organize a team building activity on March 10, 2021 to March 12, 2021, the company staff can reset these three days to holidays in the global default calendar. Then, alert notifications are sent based on the calendar settings.

-   The attributes that you reset for dates in the calendar have a higher priority than the default attributes. For example, if April 10, 2021 is a holiday and you reset the day as a business day, the alerting system of Log Service determines that the day is a business day.
-   If a date is reset multiple times, the attribute that is reset last time prevails. For example, if you reset April 12, 2021 to April 14, 2021 as holidays and then reset April 13, 2021 as a business day, Log Service determines that April 12, 2021 and April 14, 2021 are holidays, and April 13, 2021 is a business day.

![Reset the calendar](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1139872261/p264260.png)

