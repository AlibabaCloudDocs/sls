# ORDER BY clause

The ORDER BY clause is used to sort query and analysis results based on specified column names.

## Syntax

```
ORDER BY column name [DESC | ASC]
```

**Note:**

-   You can specify multiple column names to sort data in different orders. Example: `ORDER BY column name 1 [DESC | ASC], column name 2 [DESC | ASC]`.
-   If you do not specify the DESC or ASC keyword, the system sorts the query and analysis results in ascending order by default.
-   If multiple values in a specified column are the same, the sorting order may vary each time. If you want to display these values in the same order each time, you can specify multiple columns for sorting.

The following table describes the related parameters.

|Parameter|Description|
|---------|-----------|
|Column name|The name of a log field or the return column of an aggregate function. You can sort data by log field name \(key\) or the result of an aggregate function.|
|DESC|Data is sorted in descending order.|
|ASC|Data is sorted in ascending order.|

## Examples

-   Calculate the number of the requests for each status code and sort analysis results in descending order by the number of requests.

    ```
    * | SELECT count(*) as PV,status GROUP BY status ORDER BY PV DESC
    ```

-   Calculate the average write latency of each Logstore and sort analysis results in descending order by average latency.

    ```
    method:PostLogstoreLogs |SELECT avg(latency) AS avg_latency, LogStore GROUP BY LogStore ORDER BY avg_latency DESC
    ```

-   Calculate the number of requests for each request duration and sort analysis results in ascending order by request duration.

    In the following query statement, content, time, and request\_time are fields in JSON logs.

    **Note:**

    When you query and analyze JSON logs, make sure that the following requirements are met:

    -   You must add the parent path to a field name in JSON logs, for example, content.time.request\_time.
    -   You must use double quotation marks \(""\) to enclose the field name of JSON logs in an analytic statement, for example, "content.time.request\_time".
    ```
    * | SELECT "content.time.request_time", COUNT(*) AS count GROUP BY "content.time.request_time" ORDER BY "content.time.request_time"
    ```


