# Overview

Log Service allows you to configure alerts based on the charts in a dashboard to monitor the service status in real time.

You can create an alert rule on the Search & Analyze page or the Dashboard page of the Log Service console. After you [create an alert rule](/intl.en-US/Query and visualization/Alarm/Create an alert rule.md), Log Service checks the query results of the associated charts at a specified interval. If a query result meets the trigger condition that is specified in the alert rule, Log Service sends an alert notification.

## Limits

The following table lists the limits of alert rules in Log Service.

|Limit|Description|
|:----|:----------|
|Associated charts|Each alert rule can be associated with one to three charts.|
|Field value size|If the size of a field value exceeds 1,024 characters, Log Service extracts only the first 1,024 characters for processing.|
|Trigger condition|-   Each trigger condition must be 1 to 128 characters in length.
-   If a query result includes more than 100 rows, Log Service only checks whether the first 100 rows meet the trigger condition.
-   Log Service checks a trigger condition for a maximum of 1,000 times. |
|Query time range|The maximum time range that can be specified for each query is 24 hours.|

## Query statements in alert rules

An alert rule is associated with one or more charts in a dashboard. Each chart displays the result of a query statement. A query statement can be a search statement or an analytic statement. For more information, see [Search syntax](/intl.en-US/Index and query/Query/Search syntax.md).

-   A search statement returns the log entries that meet the search condition.

    For example, you can use the error statement to search for the log entries that contain error in the last 15 minutes. A total of 154 log entries are returned. Each log entry consists of key-value pairs. You can set a trigger condition based on the value of a key.

    **Note:** If the number of returned log entries exceeds 100, only the first 100 log entries are checked. If any of the log entries meets the condition, an alert is triggered.

-   An analytic statement analyzes the log entries that meet the search condition and returns a result.

    For example, the \* \| select sum\(case when status='ok' then 1 else 0 end\) \*1.0/count\(1\) as ratio statement returns the percentage of the log entries in which the value of the status field is ok. If you set the trigger condition of an alert rule to ratio < 0.9, Log Service triggers an alert when the percentage is less than 90%.


