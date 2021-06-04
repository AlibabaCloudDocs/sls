# Group evaluation

When you create an alert monitoring rule, you must specify the Group Evaluation parameter. When the alert monitoring system calculates query results, it can group the query results based on specified fields. Each group is evaluated based on the specified trigger condition. If the trigger condition is met in a group, an alert is triggered. You can use an alert monitoring rule to monitor multiple groups of query results at the same time. You can manage alerts and incidents for each group.

## Example 1: Monitor time series data by group

In this example, the metric data of multiple servers is stored in a Metricstore. If the CPU utilization \(cpu\_util\) of a server exceeds 95%, an alert is triggered and Log Service sends the related alert notification. To meet this requirement, you can use the group evaluation feature when you create an alert monitoring rule.

![Time series data](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2609872261/p262877.png)

Specify the following parameters:

-   **Query Statistics**: Enter `* | select promql_query_range('cpu_util') from metrics limit 1000`.

    This query statement is used to calculate the CPU utilization of each server.

-   **Group Evaluation**: Select **Custom Tag**.

    This setting indicates that the query result of time series data is grouped.

-   **Trigger Condition**: Select **data matches the expression** and enter **value \> 95**.

    If the value of a **value** field in the query result is greater than 95, an alert is triggered.

-   **Add Annotation**: Specify the title and description of an annotation. You can quote field variables such as $\{host\} in the annotation. For more information, see [Labels and annotations]().

![Monitor time series data by group](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8418812261/p263165.png)

## Example 2: Monitor logs by group

In this example, Object Storage Service \(OSS\) access logs are monitored. If the number of HTTP status code 500 error responses in a bucket exceeds 1,000, an alert is triggered. To meet this requirement, you can use the group evaluation feature when you create an alert monitoring rule.

Specify the following parameters:

-   **Query Statistics**: Enter `http_status=500 | select bucket,count(1) as pv group by bucket having pv >1000 order by pv desc`.

    This query statement is used to query the buckets in which the number of HTTP status code 500 error responses exceeds 1,000.

-   **Group Evaluation**: Select **Custom Tag** and **bucket**.

    This setting indicates that the query result is grouped by bucket.

-   **Trigger Condition**: Select **Data is returned**.

    This setting indicates that an alert is triggered if data is returned for the query.

-   **Severity**: Select **data matches the expression** and enter **pv \> 3000**. Set **Severity** to High. Set **Default Severity to** Medium.
    -   If the value of a **pv** field in the query result is greater than 3000, an alert whose severity is High is triggered.
    -   If the value of a **pv** field in the query result is within the \(1000,3000\] value range, an alert whose severity is Medium is triggered.
-   **Add Annotation**: Specify the title and description of an annotation. You can quote field variables such as $\{host\} in the annotation. For more information, see [Labels and annotations]().

![Monitor logs by group](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8418812261/p262885.png)

