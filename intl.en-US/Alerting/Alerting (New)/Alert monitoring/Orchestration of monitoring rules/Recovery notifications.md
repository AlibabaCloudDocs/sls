# Recovery notifications

You can use the recovery notification feature to monitor and detect whether an exception is recovered. If you enable the recovery notification feature and an alert is cleared, Log Service sends a recovery notification in the format of an alert notification.

For example, you have created an alert monitoring rule to monitor the CPU metrics of each host. If the CPU utilization of a host exceeds 95%, an alert is triggered. Then, if the CPU utilization decreases and is smaller than or equal to 95%, a recovery notification is sent. The following figure shows the required settings. For more information, see [Create an alert monitoring rule for logs](/intl.en-US/Alerting/Alerting (New)/Alert monitoring/Create an alert monitoring rule for logs.md).

Specify the following parameters:

![Recovery Notifications parameter](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9788812261/p263234.png)

-   **Query Statistics**: Enter `* | select promql_query_range('cpu_util') from metrics limit 1000`.

    This query statement is used to calculate the CPU utilization of each host.

-   **Group Evaluation**: Select **Custom Tag**.

    This setting indicates that the query result of time series data is grouped.

-   **Trigger Condition**: Select **data matches the expression** and enter **value \> 95**.

    If the value of a **value** field in the query result is greater than 95, an alert is triggered.

-   **Add Annotation**: Specify the title and description of an annotation. You can quote field variables such as $\{host\} in the annotation. For more information, see [Labels and annotations](/intl.en-US/Alerting/Alerting (New)/Alert monitoring/Orchestration of monitoring rules/Labels and annotations.md).
-   **Recovery Notifications**: Turn on the **Recovery Notifications** switch.

    Recovery notifications are special alert notifications. In a recovery notification, the alert status is Resolved. In a normal alert notification, the alert status is Firing. For example, the recovery notification feature is enabled in an alert monitoring rule. If an alert is triggered in the last check, and the trigger condition is not met in the current check, a recovery notification is sent.


Log Service sends a recovery notification in the format of an alert notification. In the recovery notification, **Resolved** is displayed in the **Alert Status** field.

