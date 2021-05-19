# Log search

Log Service allows you to search for 1 billion lines of log data at a time within 1 second. This topic describes the syntax and limits of the log search feature and how to use the syntax.

## Syntax

Each query statement consists of a search statement and an analytic statement that are separated by a vertical bar \(\|\). For more information about the query statement, see [Search syntax](/intl.en-US/Index and query/Query/Search syntax.md#).

**Note:**

-   A search statement can be executed alone. However, an analytic statement must be executed together with a search statement. The log analysis feature is based on search results or all data in a Logstore.
-   If you need to search for tens of billions of lines, you can repeatedly execute a search statement up to 10 times to obtain the complete result.

-   Syntax

    ```
    Search statement|Analytic statement
    ```

    |Statement|Description|
    |:--------|:----------|
    |Search statement|A search statement specifies one or more search conditions and returns the log entries that meet the specified conditions. A condition can be a keyword, a value, a value range, a space character, or an asterisk \(\*\). If you leave the search statement unspecified or specify an asterisk \(\*\) as the search statement, it indicates that no condition is specified and all log data is returned. For more information, see [Search syntax](/intl.en-US/Index and query/Query/Search syntax.md#). |
    |Analytic statement|An analytic statement is used to aggregate or analyze a search result. For more information, see [Log analysis](/intl.en-US/Index and query/Log analysis.md).|

-   Example

    ```
    * | SELECT status, count(*) AS PV GROUP BY status
    ```


## Limits

-   Each project supports up to 100 concurrent search statements at a time.

    For example, 100 users can concurrently search for data in all Logstores of a project at the same time.

-   You can specify up to 30 keywords for each search statement.
-   The maximum size of a field value to search for is 10 KB.
-   The returned log entries are displayed on multiple pages. Each page displays up to 100 search results.
-   Log Service performs the DOM operation only on the first 10,000 characters of a single log entry.
-   If you perform a fuzzy search, Log Service searches log entries for 100 words that meet the specified conditions. Log entries that contain the 100 words and meet the search conditions are returned.

## Search methods

**Note:** Before you use the log search feature, make sure that you have collected logs and configured the indexes for the fields. Indexes are used in a storage structure to sort one or more columns of log data. For more information, see [Configure indexes](/intl.en-US/Index and query/Configure indexes.md).

-   Use the Log Service console

    You can specify a time range and a search statement on the Search & Analysis page in the Log Service console to search for log data. For more information, see [Query logs](/intl.en-US/Index and query/Query logs.md) and [Search syntax](/intl.en-US/Index and query/Query/Search syntax.md).

-   Call API operations

    You can call the [GetLogs](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/GetLogs.md) or [GetHistograms](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/GetHistograms.md) API operation to search for log data.


