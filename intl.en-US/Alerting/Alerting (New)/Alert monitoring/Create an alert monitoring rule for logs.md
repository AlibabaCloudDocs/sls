# Create an alert monitoring rule for logs

After you create an alert monitoring rule, Log Service checks query results based on the specified check frequency and trigger condition of the rule. If an alert is triggered, an alert notification is sent based on the selected alert policy and action policy. This topic describes how to create an alert monitoring rule in Log Service.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the project in which you want to query and analyze logs.

3.  On the **Log Storage** \> **Logstores** tab, click the Logstore where logs are stored.

4.  In the upper-right corner of the page, click **Save as Alert**.

5.  In the Alert Rule panel, set the required parameters of the alert monitoring rule, and click **OK**.

    |Parameter|Description|
    |---------|-----------|
    |**Rule Name**|Specify the name of the alert monitoring rule. The name must be 1 to 64 characters in length.|
    |**Check Frequency**|Specify a check frequency at which query results are checked.     -   **Hourly**: Query results are checked every hour.
    -   **Daily**: Query results are checked at a specified time every day.
    -   **Weekly**: Query results are checked at a specified time on a specified day of every week.
    -   **Fixed Interval**: Query results are checked at a specified interval.
    -   **Cron**: Query results are checked at an interval that is specified by using a CRON expression.

If you use CRON expressions, the minimum precision is 1 minute. The time format is based on the 24-hour clock. For example, `0 0/1 * * *` indicates that a check is performed every hour from 00:00. |
    |**Query Statistics**|Specify a query statement. You can click the search bar to edit the query statement. If you specify multiple query statements, you can set the **Set Operations** parameter to associate multiple query results. |
    |**Group Evaluation**|Log Service allows you to group query results.     -   If you set the Group Evaluation parameter to **Custom Tag**, query results are grouped based on the specified fields. After Log Service groups the query results, each group is evaluated. If the specified trigger condition is met in a group within each check period, the related alert is triggered.

You can specify multiple fields. You must use commas \(,\) to separate these fields.

    -   If you set the Group Evaluation parameter to **No Grouping**, only one alert is triggered within each check period. |
    |**Trigger Condition**|Specify the trigger condition of alerts.     -   **Data is returned**: If data is returned for a query, an alert is triggered.
    -   **the query result contains**: If the number of returned rows of a query reaches N, an alert is triggered.
    -   **data matches the expression**: If the returned data of a query matches a specified expression, an alert is triggered.
    -   **the query result contains**: If the number of returned rows of a query reaches N, and the N rows of data matches a specified expression, an alert is triggered.
For more information, see [Syntax of trigger conditions in alert rules](/intl.en-US/Alerting/Alerting (Old)/Relevant syntax and fields for reference/Syntax of trigger conditions in alert rules.md). |
    |**Severity**|Specify the alert level. This parameter is used to denoise alerts and manage alert notifications. When you create an alert policy or action policy, you can add conditions based on severities.     -   Simple mode: If you only select a severity, all alerts that are triggered based on the alert monitoring rule have the same severity.
    -   Conditional mode: You can click **Create** to specify a condition and the related severity. For information about conditional expressions, see [Syntax of trigger conditions in alert rules](/intl.en-US/Alerting/Alerting (Old)/Relevant syntax and fields for reference/Syntax of trigger conditions in alert rules.md). |
    |**Add Tag**|Log Service allows you to add identifying attributes for alerts. Labels are formatted in key-value pairs. This parameter is used to denoise alerts and manage alert notifications. When you create an alert policy or action policy, you can add conditions based on labels.|
    |**Add Annotation**|Log Service allows you to add non-identifying attributes for alerts. Annotations are formatted in key-value pairs. This parameter is used to denoise alerts and manage alert notifications. When you create an alert policy or action policy, you can add conditions based on annotations.|
    |**Threshold of Continuous Triggers**|Specify the threshold of continuous triggers. An alert is triggered only when the specified trigger condition is met during continuous check periods. If the trigger condition is not met, no alert is triggered.|
    |**No Data Alert**|If you turn on the **No Data Alert** switch, and no data is returned for a query, an alert is triggered. If no data is returned for the result of multiple queries for which set operations are used, an alert is also triggered. However, an alert is triggered only when the number of no-data results that are returned during continuous check periods exceeds the value of the **Threshold of Continuous Triggers** parameter.|
    |**Recovery Notifications**|If you turn on the **Recovery Notifications** switch, and the related alert is cleared, a recovery notification is sent. The severity of the recovery is the same as that of the alert.|
    |**Alert Policy**|Select an alert policy to merge, silence, and suppress related alerts. For more information, see [Create an alert policy]().|
    |**Action Policy**|Select an action policy to manage notification methods and the frequency at which alert notifications are sent. For more information, see [Create an action policy]().|
    |**Cycle**|If duplicate alerts are triggered in the specified duration, the selected action policy is executed only once, and only one alert notification is sent.|


