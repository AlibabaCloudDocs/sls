# No-data alerts

If data loss occurs during a collection process, Log Service collect no data. To detect and prevent this issue, you can use the no-data alert feature.

For example, an alert monitoring rule is created to monitor the CPU metrics for each host. If the following conditions are met, an alert is triggered and an alert notification is sent.

-   The CPU utilization of a host exceeds 95%.
-   No data exists in query results.

Specify the following parameters:

-   **Query Statistics**: Enter `* | select promql_query_range('cpu_util') from metrics limit 1000`.

    This query statement is used to calculate the CPU utilization of each host.

-   **Trigger Condition**: Select **data matches the expression** and enter **value \> 95**.

    If the value of a **value** field in the query result is greater than 95, an alert is triggered.

-   **Threshold of Continuous Triggers**: If the number of continuous triggers reaches the specified threshold, an alert is triggered.
-   **No Data Alert**: Turn on the **No Data Alert** switch, and set the Severity and Add Annotation parameters.

    If no data is returned for a query, an alert is triggered. If you enable the no-data alert feature, an alert is triggered only when the number of continuous triggers exceeds the specified threshold of continuous triggers.

    You can specify the severity and annotations for a no-data alert.


![No Data Alert](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5088812261/p263537.png)

