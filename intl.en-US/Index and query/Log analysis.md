# Log analysis

Log Service provides the log analysis feature. This feature allows you to search for log data and use SQL functions to analyze the data. This topic describes the syntax and limits of analytic statements. This topic also provides SQL functions that you can call when you use the log analysis feature.

**Note:**

-   To use the log analysis feature, you must turn on the **Enable Analytics** switch when you configure indexes for log fields. For more information, see [Configure indexes](/intl.en-US/Index and query/Configure indexes.md). If you turn on the **Enable Analytics** switch, you can analyze log data free of charge in 1 second.
-   Log Service provides reserved fields. For information about how to analyze reserved fields, see [Reserved fields](/intl.en-US/Product Introduction/Limits/Reserved fields.md).

## Syntax

Each query statement consists of a search statement and an analytic statement. The search statement and analytic statement are separated by a vertical bar \(\|\). A search statement can be executed alone. However, an analytic statement must be executed together with a search statement. The log analysis feature is used to analyze search results or all data in the Logstore.

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

-   Each project supports a maximum of 15 concurrent analytic statements at a time.

    For example, 15 users can concurrently execute analytic statements in all Logstores of a project at the same time.

-   You can analyze only the data that is written to Log Service after the log analysis feature is enabled.

    If you want to analyze historical data, you must re-index the historical data. For more information, see [Reindex logs for a Logstore](/intl.en-US/Index and query/Query/Reindex logs for a Logstore.md).

-   By default, the system returns a maximum of 100 rows of data after you execute an analytic statement.

    If you require more data, use the LIMIT clause. For more information, see [LIMIT syntax](/intl.en-US/Index and query/Analysis grammar/LIMIT syntax.md).

-   The maximum size of a field value to analyze is 16 KB.
-   The maximum timeout period for the analysis feature is 55 seconds.
-   Each shard can analyze only 1 GB of data.
-   Data of the double type is accurate for 52 digits after the decimal point.

    If the number of digits after the decimal point is greater than 52, the accuracy is affected for those digits.


## SQL functions and syntax

This section lists the SQL functions and syntax that Log Service supports.

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

## Example analysis result

The following figure shows an example dashboard that visualizes the analysis result.

![Analysis result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4646619161/p7348.png)

