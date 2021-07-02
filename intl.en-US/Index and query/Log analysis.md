# Log analysis

Log Service provides the log analysis feature. This feature allows you to search for log data and use SQL functions to analyze the data. This topic describes the syntax and limits of the analytic statements. This topic also provides SQL functions that you can call when you use the log analysis feature.

**Note:**

-   To use the log analysis feature, you must turn on the **Enable Analytics** switch when you configure indexes for log fields. For more information, see [Configure indexes](/intl.en-US/Index and query/Configure indexes.md). If you turn on the **Enable Analytics** switch, you can analyze log data free of charge.
-   Log Service provides reserved fields. For information about how to analyze reserved fields, see [Reserved fields](/intl.en-US/Product Introduction/Limits/Reserved fields.md).

## Syntax

Each query statement consists of a search statement and an analytic statement. The search statement and analytic statement are separated by a vertical bar \(\|\). A search statement can be executed alone. However, an analytic statement must be executed together with a search statement. Log analysis is based on search results or all data in the Logstore.

**Note:**

-   You do not need to specify the FROM or WHERE clause in an analytic statement. By default, all data of the current Logstore is analyzed.
-   You do not need to add a semicolon at the end of an analytic statement to end the statement.
-   Analytic statements are case-insensitive.

-   Syntax

    ```
    Search statement|Analytic statement
    ```

    |Statement|Description|
    |:--------|:----------|
    |Search statement|A search statement specifies one or more search conditions. A condition can be a keyword, a value, a value range, a space character, or an asterisk \(\*\). If you leave the search statement unspecified or specify an asterisk \(\*\) as the search statement, it indicates that no condition is specified and all log data is returned. For more information, see [Search syntax](/intl.en-US/Index and query/Query/Search syntax.md). |
    |Analytic statement|An analytic statement is used to aggregate or analyze search results or all data in a Logstore.|

-   Example

    ```
    * | SELECT status, count(*) AS PV GROUP BY status
    ```


## Limits

|Item|Standard instance|
|----|-----------------|
|Number of concurrent analytic statements|Each project supports a maximum of 15 concurrent analytic statements at a time. For example, 15 users can execute analytic statements in all Logstores of a project at the same time. |
|Data volume|Each shard supports only 1 GB of data for a single analytic statement.|
|Method to enable|Standard instances are enabled by default.|
|Resource usage fee|Free of charge.|
|Applicable scope|You can analyze only the data that is written to Log Service after the log analysis feature is enabled. If you want to analyze historical data, you must re-index the historical data. For more information, see [Reindex logs for a Logstore](/intl.en-US/Index and query/Query/Reindex logs for a Logstore.md). |
|Returned result|After you execute an analytic statement, a maximum of 100 rows of data are returned by default. If you require more data, use the LIMIT clause. For more information, see [LIMIT syntax](/intl.en-US/Index and query/Analysis grammar/LIMIT syntax.md). |
|Size of a field value|The maximum size of a field value is 16 KB. If the size of a field value exceeds 16 KB, the excess content is not analyzed.|
|Timeout period|The maximum timeout period for a single analytic statement is 55 seconds.|
|Number of digits that consists of a field value of the double type|Each field value of the double type consists of a maximum of 52 digits. If the number of digits is greater than 52, the accuracy of the field value is compromised. |

## Analytic functions and syntax

This section lists the Analytic functions and syntax that Log Service supports.

-   SQL functions
    -   [General aggregate functions](/intl.en-US/Index and query/Analysis grammar/General aggregate functions.md)
    -   [Security check functions](/intl.en-US/Index and query/Analysis grammar/Security check functions.md)
    -   [Map functions](/intl.en-US/Index and query/Analysis grammar/Map functions.md)
    -   [Approximate functions](/intl.en-US/Index and query/Analysis grammar/Approximate functions.md)
    -   [Mathematical statistics functions](/intl.en-US/Index and query/Analysis grammar/Mathematical statistics functions.md)
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
    -   [Interval-valued comparison and periodicity-valued comparison functions](/intl.en-US/Index and query/Analysis grammar/Interval-valued comparison and periodicity-valued comparison functions.md)
    -   [Comparison functions and operators](/intl.en-US/Index and query/Analysis grammar/Comparison functions and operators.md)
    -   [Lambda functions](/intl.en-US/Index and query/Analysis grammar/Lambda functions.md)
    -   [Logical functions](/intl.en-US/Index and query/Analysis grammar/Logical functions.md)
    -   [Geospatial functions](/intl.en-US/Index and query/Analysis grammar/Geospatial functions.md)
    -   [Geo functions](/intl.en-US/Index and query/Analysis grammar/Geo functions.md)
    -   [Machine learning syntax and functions](/intl.en-US/Index and query/Machine learning syntax and functions/Overview.md)
    -   [Window functions](/intl.en-US/Index and query/Analysis grammar/Window functions.md)
-   Machine learning functions
    -   [Smooth functions](/intl.en-US/Index and query/Machine learning syntax and functions/Smooth functions.md)
    -   [Multi-period estimation functions](/intl.en-US/Index and query/Machine learning syntax and functions/Multi-period estimation functions.md)
    -   [Change point detection functions](/intl.en-US/Index and query/Machine learning syntax and functions/Change point detection functions.md)
    -   [Maximum value detection functions](/intl.en-US/Index and query/Machine learning syntax and functions/Maximum value detection functions.md)
    -   [Prediction and anomaly detection functions](/intl.en-US/Index and query/Machine learning syntax and functions/Prediction and anomaly detection functions.md)
    -   [Sequence decomposition function](/intl.en-US/Index and query/Machine learning syntax and functions/Sequence decomposition function.md)
    -   [Time series clustering functions](/intl.en-US/Index and query/Machine learning syntax and functions/Time series clustering functions.md)
    -   [Frequent pattern statistical function](/intl.en-US/Index and query/Machine learning syntax and functions/Frequent pattern statistical function.md)
    -   [Differential pattern statistical function](/intl.en-US/Index and query/Machine learning syntax and functions/Differential pattern statistical function.md)
    -   [Request URL classification function](/intl.en-US/Index and query/Machine learning syntax and functions/Request URL classification function.md)
    -   [Root cause analysis function](/intl.en-US/Index and query/Machine learning syntax and functions/Root cause analysis function.md)
    -   [Correlation analysis functions](/intl.en-US/Index and query/Machine learning syntax and functions/Correlation analysis functions.md)
    -   [Kernal density estimation functions](/intl.en-US/Index and query/Machine learning syntax and functions/Kernal density estimation functions.md)
    -   [Time series padding function](/intl.en-US/Index and query/Machine learning syntax and functions/Time series padding function.md)
    -   [Anomaly comparison function](/intl.en-US/Index and query/Machine learning syntax and functions/Anomaly comparison function.md)
-   SQL syntax
    -   [GROUP BY clause](/intl.en-US/Index and query/Analysis grammar/GROUP BY clause.md)
    -   [ORDER BY clause](/intl.en-US/Index and query/Analysis grammar/ORDER BY clause.md)
    -   [HAVING syntax](/intl.en-US/Index and query/Analysis grammar/HAVING syntax.md)
    -   [LIMIT syntax](/intl.en-US/Index and query/Analysis grammar/LIMIT syntax.md)
    -   [UNNEST function](/intl.en-US/Index and query/Analysis grammar/UNNEST function.md)
    -   [INSERT syntax](/intl.en-US/Index and query/Analysis grammar/INSERT syntax.md)
    -   [Column aliases](/intl.en-US/Index and query/Analysis grammar/Column aliases.md)
    -   [Nested subquery](/intl.en-US/Index and query/Analysis grammar/Nested subquery.md)
    -   [Conditional expressions](/intl.en-US/Index and query/Analysis grammar/Conditional expressions.md)

## Sample analysis results

The following figure shows a sample dashboard that visualizes the analysis results.

![Analysis results](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2511133261/p7348.png)

