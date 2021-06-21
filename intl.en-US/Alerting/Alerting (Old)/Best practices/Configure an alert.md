# Configure an alert

Log Service enables you to configure alerts based on the charts in a dashboard to monitor the service status in real time.

## Set the query time range and execution interval

The configured query statement is executed regularly based on the execution interval to query logs generated in the specified query time range. The query result is used as a parameter in the alert condition. If the calculation result of the alert condition is true, an alert is triggered.

Do not set the query time range to the same time period as the execution interval. For example, the execution interval is 1 minute, and the query time range is also 1 minute. The reason is as follows \(taking the execution interval of 1 minute as an example\):

-   After data is written to Log Service, there is a latency before it can be queried. Even if the latency is low, data may fail to be queried. Assume that the alert execution time is 12:03:30 and the query time range is 1 minute, that is, \[12:02:30, 12:03:30\). For logs written at 12:03:29, they may not be queried at 12:03:30.
    -   If you have high requirements on alert accuracy \(including no repeated alerts and no missing alerts\), you can move the query time range forward, for example, from 70 seconds ago to 10 seconds ago. For example, set the query time range to \[12:02:20, 12:03:20\) if the alert execution time is 12:03:30. By setting a buffer period of 10 seconds, you can avoid missing alerts caused by indexing latency.
    -   If you have high requirements on real-time performance \(that is, you want to receive alerts once they are generated regardless of repeated alerts\), you can move the query time range forward, for example, from 70 seconds ago to now. For example, set the query time range to \[12:02:20, 12:03:30\) if the alert execution time is 12:03:30.
-   When logs at different time points within the same minute are written to Log Service, the index of a later log may fall into the time point of the earlier log due to the indexing method of Log Service. Assume that the alert execution time is 12:03:30 and the query time range is 1 minute, that is, \[12:02:30, 12:03:30\). Multiple logs are written at 12:02:50 and these logs are generated at different time points within the same minute, such as 12:02:20 and 12:02:50. In this case, the index of the logs may fall into the time point 12:02:20, and the logs cannot be queried in the specified query time range.
    -   If you have high requirements on alert accuracy \(including no repeated alerts and no missing alerts\), you can use the integral minute as the query time range, such as 1 minute, 5 minutes, and 1 hour, and set the execution interval to the same time period, correspondingly, 1 minute, 5 minutes, and 1 hour.
    -   If you have high requirements on real-time performance \(that is, you want to receive alerts once they are generated regardless of repeated alerts\), you can include at least 1 minute before the alert execution time to the query time range. For example, set the query time range to \[12:02:00, 12:03:30\) and the execution interval to 1 minute if the alert execution time is 12:03:30.

## Trigger an alert based on the query result

Assume that the alert condition is met as long as the result of a query is not empty. You can set the alert rule so that an alert will be triggered if a certain field exists. For example, search for logs containing the IP address 10.240.80.234.

![example query](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/7311559651/p47144.png)

If you find logs that contain the IP address 10.240.80.234, an alert is triggered. You can set an alert condition that is always true based on any field. Assume that the client\_ip field exists in each log and cannot be an empty string. You can set the alert rule so that an alert will be triggered as long as the client\_ip field is not empty.

![create alert](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/8311559651/p47147.png)

## Trigger an alert based on analysis result

Triggering alerts based on analysis results is one of the most common alerting scenarios. For example, an alert is triggered after aggregation of specific fields. Run the following query statement to collect statistics on the number of logs that contain the ERROR keyword. Set the alert rule so that an alert will be triggered if the number of logs that contain the ERROR keyword is greater than the threshold value.

```
ERROR | select count(1) as errorCount
```

The alert condition can be set to that errorCount is greater than a threshold value, for example, errorCount \> 0.

## Trigger an alert based on association query

When you create an alert in a dashboard, you can select multiple charts as the input for alert query.

-   Configure an alert that is triggered based on combined query results in different time ranges.

    For example, set the alert rule so that an alert will be triggered if the PV within 15 minutes is greater than 100,000 and the UV within 1 hour is smaller than 1,000.

    ![Create an alert in a dashboard](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/8311559651/p53878.png)

    **Note:** When multiple charts are selected, the query time ranges are independent of each other. In the trigger condition, use the $\{Number\}.\{Field\} format to reference the field in the query result. For example, $0.pv \> 100000 && $1.uv < 1000.

-   Configure an alert that is triggered based on some charts. Query results of other charts are used as auxiliary information.

    Run the following statement to collect statistics on the number of logs whose log level is ERROR:

    ```
    level: ERROR | select count(1) as errorCount
    ```

    Set the alert rule so that an alert will be triggered if the number of logs whose log level is ERROR is greater than the threshold value.

    ```
    errorCount > 10
    ```

    If you want to view the logs whose log level is ERROR in the alert notification, run the following query statement:

    ```
    level: ERROR
    ```

    Set the alert notification as follows:

    ```
    ${results[1].RawResultsAsKv}
    ```

    You can view the logs whose log level is ERROR.


## Suppress alerts

When an alert is triggered, you may receive the notification multiple times within a period of time. To prevent false alerts and repeated alerts caused by data jitter, you can suppress the alerts in the following two ways:

-   Set the trigger condition.

    An alert is triggered only when the alert conditions are met for each of the multiple consecutive detections.

    For example, if the alert execution interval is 1 minute and the trigger threshold is 5, the notification is sent only when each of the five consecutive alert detections meets the alert conditions. If any one of the five consecutive alert detections cannot meet the alert conditions, the count is reset.

-   Set the notification interval.

    When you set a small alert execution interval, you can set the minimum interval between two notifications to prevent frequent notifications. For example, if the alert execution interval is 1 minute and the notification interval is 30 minutes, no notification will be received even if an alert is triggered within 30 minutes.

    ![Notification interval](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/8311559651/p53880.png)


## Disable alert notifications

After receiving an alert notification, you can go to the alert overview page for temporarily disabling the notification feature, as shown in the following figure.

![告警概览](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6114424261/p285019.png)

In the Disable Alert Notifications dialog box, select a duration for which notifications are disabled, for example, 30 minutes.

![关闭告警通知](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7114424261/p285020.png)

No notification will be sent within 30 minutes, even if an alert is triggered. After 30 minutes, the notification feature is automatically restored.

## Allow DingTalk group members to process alerts

A DingTalk group is one of the most common alert notification channels. When configuring a DingTalk notification, you can @ a DingTalk group member to process the corresponding alert, as shown in the following figure.

![Allow DingTalk group members to process alerts](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/8311559651/p47162.png)

**Note:** You must specify the mobile phone number of the corresponding member in both the Tagged List and Content fields. The Tagged List field is used to determine whether the at sign \(@\) in the Content field is a reminder or a common character.

## Use template variables to enrich notifications

When configuring the notification method, you can use the template variables to enrich the notifications. Template variables can be used for the email title, DingTalk message title, and message content. Each time when an alert is triggered, an alert context is generated. Each variable in the context can be used as a template variable. For more information, see [Configure notification methods](/intl.en-US/Alerting/Alerting (Old)/Configure notification methods.md).

-   You can reference the Project, AlertName, and Dashboard variables in the $\{project\} format without case sensitivity.
-   The context of each query is included in the Results array. Each element in the array corresponds to a chart associated with the alert. In most cases, there is only one element.

    ```
    {
      "EndTime": "2006-01-02 15:04:05",
      "EndTimeTs": 1542507580,
      "FireResult": {
        "__time__": "1542453580",
        "field": "value1 ",
        "count": "100"
      },
      "FireResultAsKv": "[field:value1,count:100]",
      "Truncated": false,
      "LogStore": "test-logstore",
      "Query": "* | SELECT field, count(1) group by field",
      "QueryUrl": "http://xxxx",
      "RawResultCount": 2,
      "RawResults": [
        {
          "__time__": "1542453580",
          "field": "value1",
          "count": "100"
        },
        {
          "__time__": "1542453580",
          "field": "value2",
          "count": "20"
        }
      ],
      "RawResultsAsKv": "[field:value1,count:100],[field:value2,count:20]",
      "StartTime": "2006-01-02 15:04:05",
      "StartTimeTs": 1542453580
    }
    ```

    For more information about the fields, see [Fields in alert rule evaluation logs](/intl.en-US/Alerting/Alerting (Old)/Relevant syntax and fields for reference/Fields in alert rule evaluation logs.md). You can reference the fields in the Results array as follows:

    -   The fields of the array type are referenced in the $\{fieldName\[\{index\}\]\} format. The value of index starts from 0. For example, $\{results\[0\]\} indicates that the first element in the Results array is referenced.
    -   The fields of the object type are referenced in the $\{object.key\} format. For example, the result of $\{results\[0\].StartTimeTs\} is 1542453580.
    **Note:** Only the fields in RawResults and FireResult are query results. These fields are case-sensitive. Other fields are case-insensitive.


## Troubleshoot why the alert is not triggered

After an alert is configured, you can view alert statistics. For more information, see [View the evaluation results of alert rules](/intl.en-US/Alerting/Alerting (Old)/View the evaluation results of alert rules.md). You can view the context of a single alert in the internal-alert-history Logstore, as shown in the following figure.

![internal-alert-history](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/8311559651/p47195.png)

For more information about the fields, see [Fields in alert rule evaluation logs](/intl.en-US/Alerting/Alerting (Old)/Relevant syntax and fields for reference/Fields in alert rule evaluation logs.md).

Each execution generates a unique alert ID and a corresponding log. The log contains the alert execution status and query result. If the query result exceeds 2 KB, it will be truncated. Logs can be used to troubleshoot why the alert is not triggered.

