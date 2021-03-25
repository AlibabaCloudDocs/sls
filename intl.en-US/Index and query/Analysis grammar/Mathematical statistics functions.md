# Mathematical statistics functions

This topic describes the syntax of mathematical statistics functions and provides usage examples.

## Basic syntax

|Function|Description|
|:-------|:----------|
|corr\(key1, key2\)|Calculates the correlation between two specific columns.Valid values: 0 to 1. |
|covar\_pop\(key1, key2\)|Calculates the population covariance of two specific columns.|
|covar\_samp\(key1, key2\)|Calculates the sample covariance of two specific columns.|
|regr\_intercept\(key1, key2\)|Returns the linear regression intercept of input values. key1 is the dependent value and key2 is the independent value.|
|regr\_slope\(key1, key2\)|Returns the linear regression slope of input values. key1 is the dependent value and key2 is the independent value.|
|stddev\(key\)|Calculates the sample standard deviation of the key column. This function is equivalent to the stddev\_samp function.|
|stddev\_samp\(key\)|Calculates the sample standard deviation of the key column.|
|stddev\_pop\(key\)|Returns the population standard deviation of the key column.|
|variance\(key\)|Calculates the sample variance of the key column. This function is equivalent to the var\_samp function.|
|var\_samp\(key\)|Calculates the sample variance of the key column.|
|var\_pop\(key\)|Calculates the population variance of the key column.|

## Examples

-   Example 1: Return the correlation of two specific columns.
    -   Query statement

        ```
        * | SELECT corr(request_length, request_time)
        ```

    -   Query and analysis results

        ![corr() function](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1990466161/p245055.png)

-   Example 2: Query the sample standard deviation and population standard deviation of pre-tax income.
    -   Query statement

        ```
        * | SELECT stddev(PretaxGrossAmount) as "sample standard deviation", stddev_pop(PretaxGrossAmount) as "population standard deviation", time_series(__time__, '1m', '%H:% I:%s', '0') AS time GROUP BY time
        ```

    -   Query and analysis results

        ![stddev_pop() function](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1990466161/p245061.png)


