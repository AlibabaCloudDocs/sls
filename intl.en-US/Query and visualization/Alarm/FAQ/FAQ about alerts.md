# FAQ about alerts

This topic provides answers to some commonly asked questions \(FAQ\) about alerts in the Log Service console.

## How do I add raw log entries to an alert notification?

For example, assume that more than five error log entries have been detected in the past 5 minutes and an alert has been triggered. To add the raw log entries to the alert notification, perform the following steps:

1.  In the Projects list, click the destination project.
2.  In the left-side navigation pane, click the ![Alert - 001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9768068061/p76975.png) icon.
3.  In the dashboard list, click the destination dashboard.
4.  In the upper-right corner of the dashboard page, choose **Alerts** \> **Create**.
5.  In the Create Alert dialog box, set the parameters, as shown in the following figure.

    ![Configure alerts](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4133359951/p39406.png)

    The following example shows how to set the parameters.

    -   Query
        -   Number 0: `level: ERROR`
        -   Number 1: `level: ERROR | select COUNT(*) as count`
    -   Trigger Condition: `$1.count > 5`
    -   Content: `${results[0].rawresults}`
6.  Click **Submit**.

## What can I do if the DingTalk chatbot fails to send a notification?

For example, after you set the notification method to WebHook-DingTalk Bot, the following error message is returned when the DingTalk chatbot sends a notification:

```
{"errcode":310000,"errmsg":"sign not match"}
{"errcode":310000,"errmsg":"keywords not in content"}
```

This error message returned because the security settings of the latest chatbot are invalid. You can reset the security settings. For more information, see [DingTalk chatbot webhooks](/intl.en-US/Query and visualization/Alarm/Configure notification methods.md). If the problem persists or other error messages are returned, [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm).

