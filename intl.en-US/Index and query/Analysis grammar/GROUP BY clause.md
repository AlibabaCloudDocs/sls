# GROUP BY clause

The GROUP BY clause is used with aggregate functions to group analysis results based on one or more columns.

## Syntax

```
* | SELECT column name, aggregate function GROUP BY [ column name | alias | ordinal number ]
```

**Note:** In SQL statements, if you use the GROUP BY clause, you can only select the GROUP BY column or perform aggregate calculations on a random column when you execute a SELECT statement. You cannot select non-GROUP BY columns. For example, `* | SELECT status, request_time, COUNT(*) AS PV GROUP BY status` is an invalid query statement because request\_time is not a GROUP BY column.

The GROUP BY clause can be used to group data by column name, alias, and ordinal number. The following table describes the related parameters.

|Parameter|Description|
|---------|-----------|
|Column name|The name of a log field or the return column of an aggregate function. You can group data by log field name \(key\) or the result of an aggregate function.The GROUP BY clause supports single column or multiple columns. |
|Alias|Data is grouped by the alias of a log field name or the return column alias of an aggregate function.You can specify the alias of a log field in the Field Search section of the Search & Analysis panel. For more information, see [Column aliases](/intl.en-US/Index and query/Analysis grammar/Column aliases.md). |
|Ordinal number|The ordinal number of a column in a SELECT statement. The number starts from 1.For example, the ordinal number of the status column is 1. Therefore, the following two statements are equivalent:

-   ```
* | SELECT status, count(*) AS PV GROUP BY status
```

-   ```
* | SELECT status, count(*) AS PV GROUP BY 1
``` |
|Aggregate function|The GROUP BY clause can be used together with aggregate functions such as MIN, MAX, AVG, SUM, and COUNT. For more information, see [General aggregate functions](/intl.en-US/Index and query/Analysis grammar/General aggregate functions.md).|

## Examples

-   To calculate the number of access requests of different HTTP status codes, you can execute the following query statement:

    ```
    * | SELECT status, count(*) AS PV GROUP BY status
    ```

-   To calculate the number of page views \(PVs\) by 1 hour, you can execute the following query statement:

    ```
    * | SELECT count(*) AS PV , date_trunc('hour', __time__) AS time GROUP BY time ORDER BY time limit 1000                            
    ```

    The \_\_time\_\_ field is a reserved field in Log Service. This field indicates the time column. time is the alias of date\_trunc\('hour', \_\_time\_\_\). For more information about the date\_trunc\(\) function, see [Truncation function](/intl.en-US/Index and query/Analysis grammar/Date and time functions.md).

    **Note:**

    -   The clause limit 1000 indicates that a maximum of 1,000 rows of data can be returned. If you do not use the LIMIT clause, you can obtain a maximum of 100 rows of data by default.
    -   If you turn on the Enable Analytics switch for a log field in the Search & Analysis panel, the analysis feature is automatically enabled for the \_\_time\_\_ field.
-   To calculate the PVs by 5 minutes, you can execute the following query statement.

    The date\_trunc\(\) function can only collect statistics at a specified interval. If you want to perform statistical analysis by custom time, you can group data based on the modulus method. In this example, %300 in the following statement indicates that modulo and truncation are performed once every 5 minutes.

    ```
    * | SELECT count(*) AS PV,  __time__ - __time__%300 AS time GROUP BY time limit 1000
    ```

-   To extract columns that are not grouped by using the GROUP BY clause, you can execute the following query statement.

    In SQL statements, if you use the GROUP BY clause, you can only select the GROUP BY column or perform aggregate calculations on a column when you execute a SELECT statement. You cannot select non-GROUP BY columns. For example, `* | SELECT status, request_time, COUNT(*) AS PV GROUP BY status` is an invalid analytic statement because request\_time is not a GROUP BY column.

    ```
    * | SELECT status, arbitrary(request_time), count(*) AS PV GROUP BY status
    ```


