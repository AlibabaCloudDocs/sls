# Overview of time series search and analysis

This topic describes the query languages and limits of time series search and analysis.

Log Service supports the following query languages for time series search and analysis:

-   SQL: You can use the SQL query language to search and analyze time series data based on the time series data format.
-   Combination of SQL and PromQL: This combination provides an easy-to-use method to search and analyze time series data. The combination is implemented by using nested queries. PromQL is the query language that is provided by Prometheus. For more information, see the [Prometheus documentation](https://prometheus.io/docs/prometheus/latest/querying/basics/).

## SQL query language

Sample query statements:

-   Query all data records from a Metricstore.

    ```
    *| SELECT * FROM "my_metric_store.prom" WHERE __name__ != '' 
    ```

-   Query the data records in which the value of the \_\_labels\_\_, 'domain' field is www.example.com and obtain the sum of the \_\_value\_\_ field.

    ```
    *| SELECT sum(__value__) FROM "my_metric_store.prom" WHERE element_at(__labels__, 'domain')='www.example.com' 
    ```

-   Query the data records in which the value of the \_\_labels\_\_, 'domain' field is www.example.com, obtain the sum of the \_\_value\_\_ field, and aggregate data records by hour.

    ```
    *| SELECT sum(__value__),date_trunc('hour', __time_nano__/1000000) as t
    FROM "my_metric_store.prom" 
    WHERE element_at(__labels__, 'domain')='www.example.com'
    GROUP BY t
    ORDER BY t DESC
    ```


Description:

-   The SQL search and analysis syntax for time series data is the same as that for log data. For more information, see [Log analysis](/intl.en-US/Index and query/Log analysis.md). For SQL queries on time series data, the table name in the FROM clause must be \{metrics\_store\_name\}.prom. \{metrics\_store\_name\} indicates the name of the Metricstore that you want to query.

    **Note:** You must enclose the table name in double quotation marks \("\).

-   You can use the element\_at\(\) function to obtain the value of a key from the \_\_labels\_\_ field, for example, element\_at\(\_\_labels\_\_, 'key'\).
-   For information about the table schema, see [Format](/intl.en-US/Product Introduction/Basic concepts/Time series.md).

## Combination of SQL and PromQL

If you use the combination of SQL and PromQL, you can also use the advanced features such as the [machine learning syntax and functions](/intl.en-US/Index and query/Machine learning syntax and functions/Overview.md) and [Security check functions](/intl.en-US/Index and query/Analysis grammar/Security check functions.md).

**Note:** If you use the combination of SQL and PromQL, the table name in the FROM clause must be metrics.

Log Service supports five PromQL functions: promql\_query, promql\_labels, promql\_label\_values, and promql\_series functions. These functions can only be invoked on the Search & Analysis page of a specific Logstore. The following table describes the functions.

|Function|Description|Example|
|--------|-----------|-------|
|promql\_query\(string\)|Evaluates an instant query on the latest value. This function is the same as the [/query API](https://prometheus.io/docs/prometheus/latest/querying/api/#instant-queries) for which the parameter settings are query=<string\>.|\*\| SELECT promql\_query\('up'\) FROM metrics|
|promql\_query\_range\(string, string\)|Evaluates a query over a range of time. This function is the same as the [/query\_range API](https://prometheus.io/docs/prometheus/latest/querying/api/#range-queries) for which the parameter settings are query=<string\> and step=<duration\>.|\*\| SELECT promql\_query\_range\('up', '5m'\) FROM metrics|
|promql\_labels\(\)|Returns all label keys.|\*\| SELECT promql\_labels\(\) FROM metrics|
|promql\_label\_values\(string\)|Returns the values of a label key.|\*\| SELECT promql\_label\_values\('\_\_name\_\_'\) FROM metrics|
|promql\_series\(string\)|Return the matching time series.|\*\| SELECT promql\_series\('up'\) FROM metrics|

Each PromQL function is similar to a user-defined table generating function \(UDTF\) and returns a table.

-   The following table describes the schema of the table that is returned by the promql\_query\(string\) or promql\_query\_range\(string, string\) function.

    |Field|Type|Description|
    |-----|----|-----------|
    |metric|varchar|The metric name of the time series. If the Group By clause is included in the query statement, this field may be empty.|
    |labels|map<varchar, varchar\>|The labels.|
    |time|bigint|The timestamp.|
    |value|double|The value that corresponds to the timestamp.|

-   The following table describes the schema of the table that is returned by the promql\_labels\(\) or promql\_label\_values\(string\) function.

    |Field|Type|Description|
    |-----|----|-----------|
    |label|varchar|Label Key|

-   The following table describes the schema of the table that is returned by the promql\_series\(string\) function.

    |Field|Type|Description|
    |-----|----|-----------|
    |series|map<varchar, varchar\>|The time series.|


## Limits

-   A Metricstore only supports the query API operations of Prometheus, such as /query and /query\_range. Other API operations such as /admin API, /alerts API, and /rules API are not supported.
-   If you use the combination of SQL and PromQL for search and analysis, a maximum of 11,000 timestamps are returned for a query.
-   If you use the combination of SQL and PromQL for search and analysis, the metric name and label names must comply with the naming conventions. For more information, see [Metric identifier](/intl.en-US/Product Introduction/Basic concepts/Time series.md).

