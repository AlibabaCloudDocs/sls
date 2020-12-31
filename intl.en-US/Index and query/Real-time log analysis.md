# Real-time log analysis

This topic describes the real-time analysis feature of Log Service and the limits and SQL syntax of this feature.

## Overview

**Note:** To use the real-time analysis feature, you must turn on the **Enable Analytics** switch for one or more fields in the field index. For more information, see [Enable and configure the indexing feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).

After you turn on the **Enable Analytics** switch, analysis within seconds is supported and incurs no additional charges.

The real-time analysis feature of Log Service allows you to search for log data and then analyze the log data based on SQL syntax. In addition, the feature returns the results within seconds.

You can execute a query statement to search and analyze log data. Each query statement consists of a search statement and an analytic statement. They are separated by a vertical bar \(\|\). The search statement uses proprietary syntax. For more information, see [Search syntax](/intl.en-US/Index and query/Query/Search syntax.md).

-   Format of a query statement

    ```
    Search statement|Analytic statement
    ```

-   Example

    ```
    status>200 |select avg(latency),max(latency),count(1) as c GROUP BY  method  ORDER BY c DESC  LIMIT 20
    ```


## SQL syntax

This section lists the SQL syntax that Log Service supports.

-   Aggregate functions that can be used in SELECT statements
    -   [General aggregate functions](/intl.en-US/Index and query/Analysis grammar/General aggregate functions.md)
    -   [Security check functions](/intl.en-US/Index and query/Analysis grammar/Security check functions.md)
    -   [Map functions](/intl.en-US/Index and query/Analysis grammar/Map functions.md)
    -   [Approximate functions](/intl.en-US/Index and query/Analysis grammar/Approximate functions.md)
    -   [Mathematical statistic functions](/intl.en-US/Index and query/Analysis grammar/Mathematical statistic functions.md)
    -   [Mathematical calculation functions](/intl.en-US/Index and query/Analysis grammar/Mathematical calculation functions.md)
    -   [String functions](/intl.en-US/Index and query/Analysis grammar/String functions.md)
    -   [Date and time functions](/intl.en-US/Index and query/Analysis grammar/Date and time functions.md)
    -   [URL functions](/intl.en-US/Index and query/Analysis grammar/URL functions.md)
    -   [Regular expression functions](/intl.en-US/Index and query/Analysis grammar/Regular expression functions.md)
    -   [JSON functions](/intl.en-US/Index and query/Analysis grammar/JSON functions.md)
    -   [Type conversion functions](/intl.en-US/Index and query/Analysis grammar/Type conversion functions.md)
    -   [IP functions](/intl.en-US/Index and query/Analysis grammar/IP functions.md)
    -   [Array functions](/intl.en-US/Index and query/Analysis grammar/Array functions.md)
    -   [Binary string functions](/intl.en-US/Index and query/Analysis grammar/Binary string functions.md)
    -   [Bitwise functions](/intl.en-US/Index and query/Analysis grammar/Bitwise functions.md)
    -   [Interval-valued comparison and periodicity-valued comparison function](/intl.en-US/Index and query/Analysis grammar/Interval-valued comparison and periodicity-valued comparison function.md)
    -   [Comparison functions and operators](/intl.en-US/Index and query/Analysis grammar/Comparison functions and operators.md)
    -   [Lambda functions](/intl.en-US/Index and query/Analysis grammar/Lambda functions.md)
    -   [Logical functions](/intl.en-US/Index and query/Analysis grammar/Logical functions.md)
    -   [Geospatial functions](/intl.en-US/Index and query/Analysis grammar/Geospatial functions.md)
    -   [Geo functions](/intl.en-US/Index and query/Analysis grammar/Geo functions.md)
    -   [Machine learning functions](/intl.en-US/Index and query/Machine learning syntax and functions/Overview.md)
-   [GROUP BY syntax](/intl.en-US/Index and query/Analysis grammar/GROUP BY syntax.md)
-   [Window functions](/intl.en-US/Index and query/Analysis grammar/Window functions.md)
-   [HAVING syntax](/intl.en-US/Index and query/Analysis grammar/HAVING syntax.md)
-   [ORDER BY syntax](/intl.en-US/Index and query/Analysis grammar/ORDER BY syntax.md)
-   [LIMIT syntax](/intl.en-US/Index and query/Analysis grammar/LIMIT syntax.md)
-   [CASE WHEN and IF syntax](/intl.en-US/Index and query/Analysis grammar/CASE WHEN and IF syntax.md)
-   [UNNEST function](/intl.en-US/Index and query/Analysis grammar/UNNEST function.md)
-   [Column aliases](/intl.en-US/Index and query/Analysis grammar/Column aliases.md)
-   [Nested subquery](/intl.en-US/Index and query/Analysis grammar/Nested subquery.md)
-   [INSERT syntax](/intl.en-US/Index and query/Analysis grammar/INSERT syntax.md)

## Additional considerations about SQL syntax

-   Do not specify the FROM or WHERE clause in an analytic statement. This is because logs are queried from the current Logstore and the WHERE clause is replaced by the search statement in Log Service.
-   An analytic statement can include the following clauses: SELECT, GROUP BY, ORDER BY \[ASC,DESC\], LIMIT, and HAVING.

## Scenarios

-   Interactive analysis
-   [Visual chart generation](/intl.en-US/Query and visualization/Analysis graph/Overview.md)
-   [Alerting](/intl.en-US/Query and visualization/Alarm/Overview.md)

## Limits

-   You can perform a maximum of 15 concurrent queries in each project.
-   By default, you can analyze only the log data that is collected after the Enable Analytics switch is turned on.
-   The maximum size of a field value is 2 KB. If the size of a field value exceeds the maximum, the value is truncated.
-   By default, a maximum of 100 rows of data is returned for each query. For information about how to retrieve more rows for a query, see [LIMIT syntax](/intl.en-US/Index and query/Analysis grammar/LIMIT syntax.md).

## System fields

Log service provides some system fields to facilitate log analysis. For more information, see [Reserved fields](/intl.en-US/Product Introduction/Limits/Reserved fields.md).

|System field|Data type|Description|
|:-----------|:--------|:----------|
|\_\_time\_\_|bigint|The timestamp of a log entry.|
|\_\_source\_\_|varchar|The source of a log entry. **Note:** To reference this field, use source in a search statement and \_\_source\_\_ in an analytic statement. |
|\_\_topic\_\_|varchar|The topic of a log entry.|

## Example

To count the number of page views \(PVs\) and unique visitors \(UVs\) per hour and query the user requests with the 10 longest latency periods in each hour, you can use the following statement:

```
*|select date_trunc('hour',from_unixtime(__time__)) as time, 
     count(1) as pv, 
     approx_distinct(userid)  as uv,
     max_by(url,latency) as top_latency_url,
     max(latency,10) as top_10_latency
     group by  1
     order by time
```

