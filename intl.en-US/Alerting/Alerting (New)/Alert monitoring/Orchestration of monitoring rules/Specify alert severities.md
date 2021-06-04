# Specify alert severities

Log Service supports static or dynamic settings for alert severities. If you do not add an evaluate condition when you set the Severity parameter, the setting of the alert severity is static. If you add an evaluate condition when you set the Severity parameter, the settings of the alert severities are dynamic.

In this example, the access logs of a website are monitored. The ratio of the number of 500 error responses within the current 15 minutes to that within the same period of the previous day is in different ranges. In this case, alerts that have different severities are triggered. The following figure shows how to set the Severity parameter.

![Severity](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7858812261/p263272.png)

Specify the following parameters:

-   **Query Statistics**: Enter `host:www.example.com and status = 500 | select coalesce(diff[2],0) as ratio from (select compare(cnt,86400) as diff from (select count(1) as cnt from log))`.

    This query statement is used to calculate a ratio. The ratio is the number of 500 error responses on a specified website within the current 15 minutes to that within the same period of the previous day.

-   **Trigger Condition**: Select **data matches the expression** and enter **ratio\>0.05**.

    If the value of a **ratio** field in the query result is greater than 0.05, an alert is triggered.

-   **Severity**: Add two evaluate conditions to dynamically specify alert severities.
    -   Condition 1: Select **data matches the expression** and enter **ratio\>=1**. Set **Severity** to Critical.

        If the value of a **ratio** field is greater than or equal to 1, an alert whose severity is Critical is triggered.

    -   Condition 2: Select **data matches the expression** and enter **ratio\>=0.5**. Set **Severity** to High.

        If the value of a **ratio** field is greater than or equal to 0.5, an alert whose severity is High is triggered.

    -   Default Severity: Select **Medium**.

        If the value of a **ratio** field is in the \(0.05,0.5\) value range, an alert whose severity is Medium is triggered. If the value of a **ratio** field meets the specified trigger condition but does not meet the specified evaluate conditions, the default severity is used.


