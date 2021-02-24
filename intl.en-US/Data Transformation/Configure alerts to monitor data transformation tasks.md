# Configure alerts to monitor data transformation tasks

This topic describes how to configure alerts to monitor data transformation tasks.

When you perform a data transformation task, a dashboard named Data Transformation Troubleshooting is created for the task. The dashboard displays the metrics that indicate the execution status of the task. You can subscribe to the dashboard and configure alerts to monitor the metrics on the dashboard. This allows you to detect and troubleshoot exceptions such as data traffic exceptions, transformation logic exceptions, and system operation exceptions with higher efficiency.

We recommend that you take note of the following metrics on the Data Transformation Troubleshooting dashboard:

-   System metrics: the data consumption delay and relevant exceptions
-   Application metrics: the number of log entries that are read and the number of output log entries

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the target project.

3.  In the left-side navigation pane, click ![Configure alerts to monitor data transformation tasks - 001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p104975.png). A list of dashboards appear.

4.  Click the dashboard named **Data Transformation Troubleshooting**.

5.  On the Data Transformation Troubleshooting page, filter jobs and configure alerts for metrics.

    For information about how to create an alert, see [Create an alert rule](/intl.en-US/Query and visualization/Alarm/Create an alert rule.md). For information about how to subscribe to the dashboard, see [Subscribe to a dashboard](/intl.en-US/Query and visualization/Dashboard/Subscribe to a dashboard.md).


## Consumption delay

1.  In the **shard consumption delay** chart, choose **![Configure alerts to monitor data transformation tasks](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p104976.png)** \> **Create Alert**.

2.  Configure an alert.

    For example, if you set **Trigger Condition** to \[delay \(s\)\] \> 120, the alert is triggered if the data consumption delay is greater than 120 seconds. For information about other parameters, see [Create an alert rule](/intl.en-US/Query and visualization/Alarm/Create an alert rule.md).

    ![Condition for triggering a consumption delay alert](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7353749951/p59369.png)

3.  Configure the notification method.

    In this example, select WebHook-DingTalk Bot. For more information about notification methods, see [Configure notification methods](/intl.en-US/Query and visualization/Alarm/Configure notification methods.md).

    ![Consumption delay notification method](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p59371.png)

4.  View alert notifications in the specified DingTalk group.

    ![Consumption delay notification](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7353749951/p59372.png)


## Exception reporting

1.  In the **Exception detail** chart, choose **![Configure alerts to monitor data transformation tasks](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p104976.png)** \> **Create Alert**.

2.  Configure an alert.

    For example, you can set **Trigger Condition** to level == 'ERROR'. For information about other parameters, see [Create an alert rule](/intl.en-US/Query and visualization/Alarm/Create an alert rule.md).

    ![Condition for triggering an exception alert](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8353749951/p59375.png)

3.  Configure the notification method.

    In this example, select WebHook-DingTalk Bot. For more information about notification methods, see [Configure notification methods](/intl.en-US/Query and visualization/Alarm/Configure notification methods.md).

    ![Consumption delay notification method](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p59371.png)

4.  View alert notifications in the specified DingTalk group.

    ![Exception alert notification](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8353749951/p59377.png)

    **Note:** In most cases, error logs are generated due to the invalid transformation script of a task. You can modify the transformation script. After you modify the script, the task is restarted. Then you can check whether new error logs are generated.


## Transformation traffic \(absolute value\)

1.  In the **Transform speed** chart, choose **![Configure alerts to monitor data transformation tasks](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p104976.png)** \> **Create Alert**.

2.  Configure an alert.

    For example, if you set **Trigger Condition** to accept < 40000, the alert is triggered if the number of log entries that are transformed every second is less than 40,000. For information about other parameters, see [Create an alert rule](/intl.en-US/Query and visualization/Alarm/Create an alert rule.md).

    ![Absolute value trigger condition](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8353749951/p59379.png)

3.  Configure the notification method.

    In this example, select WebHook-DingTalk Bot. For more information about notification methods, see [Configure notification methods](/intl.en-US/Query and visualization/Alarm/Configure notification methods.md).

    ![Consumption delay notification method](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p59371.png)

4.  View alert notifications in the specified DingTalk group.

    ![Absolute value alert](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p59381.png)


## Transformation traffic \(day-on-day comparison\)

1.  Customize monitoring metrics.

    1.  Choose **Log Management** \> **Logstores**. Click the Logstore named **internal-etl-log**.

    2.  Enter the following query statement in the search box and then click **Search & Analyze**.

        This query statement calculates the ratio of the number of log entries that are written every 5 minutes on the current day to that of the day before.

        ```
        __topic__:  __etl-log-status__ AND __tag__:__schedule_type__:  Resident and event_id:  "shard_worker:metrics:checkpoint" | 
        select dt, today, yesterday, round((today - yesterday) * 100.0 / yesterday, 3) as inc_ration from
        (select dt, (case when diff[1] is null then 0 else diff[1] end) as today, (case when diff[2] is null then 0 else diff[2] end) as yesterday from 
        (select dt, compare("delivered lines", 86400) as diff from 
        (select date_format(__time__ - __time__ % 300, '%H:%i') as dt, sum("progress.delivered") as "delivered lines" from log group by dt order by dt asc limit 5000)
        group by dt order by dt asc limit 5000))
        ```

        **Note:** You can modify the query statement to create a more fine-grained alert. For example, you can set an alert only for the task whose ID is 06f239b7362ad238e613abb3f7fe3c87.

        ```
        __topic__:  __etl-log-status__ AND __tag__:__schedule_type__:  Resident and event_id:  "shard_worker:metrics:checkpoint" and __tag__:__schedule_id__:  06f239b7362ad238e613abb3f7fe3c87 | 
        select dt, today, yesterday, round((today - yesterday) * 100.0 / yesterday, 3) as inc_ration from
        (select dt, (case when diff[1] is null then 0 else diff[1] end) as today, (case when diff[2] is null then 0 else diff[2] end) as yesterday from
        (select dt, compare("delivered lines", 86400) as diff from
        (select date_format(__time__ - __time__ % 300, '%H:%i') as dt, sum("progress.delivered") as "delivered lines" from log group by dt order by dt asc limit 5000)
        group by dt order by dt asc limit 5000))
        ```

    3.  On the Graph tab, select the line chart and click **Add to Dashboard**. In this example, the query statement is saved as a chart on the dashboard named **etl-monitor**.

        ![Save the dashboard](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p59383.png)

2.  In the left-side navigation pane, click ![Configure alerts to monitor data transformation tasks - 001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p104975.png). A list of dashboards appear.

3.  Click the dashboard named etl-monitor.

4.  On the etl-monitor dashboard, find the target chart, and choose **![Configure alerts to monitor data transformation tasks](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p104976.png)** \> **Create Alert**.

    ![Create an alert](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p59393.png)

5.  Configure an alert.

    For example, if you set **Trigger Condition** to inc\_ration < \(-40\), the alert is triggered if the log transformation speed is 40% lower than that of the previous day. For information about other parameters, see [Create an alert rule](/intl.en-US/Query and visualization/Alarm/Create an alert rule.md).

    ![Alert trigger condition](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p59394.png)

6.  Configure the notification method.

    In this example, select WebHook-DingTalk Bot. For more information about notification methods, see [Configure notification methods](/intl.en-US/Query and visualization/Alarm/Configure notification methods.md).

    ![Consumption delay notification method](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p59371.png)

7.  View alert notifications in the specified DingTalk group.

    ![Dashboard alert notification](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p59396.png)


## Alert-related operations

You can delete, modify, or disable an alert on the Alert Overview page of the alert. For more information, see [Manage an alert rule](/intl.en-US/Query and visualization/Alarm/Manage an alert rule.md).

