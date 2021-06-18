# Subscribe to a dashboard

You can subscribe to dashboards in Log Service. After you subscribe to a dashboard, Log Service periodically takes snapshots of the dashboard and sends the snapshots to specific recipients by using email or DingTalk group notifications. This topic describes how to subscribe to a dashboard in the Log Service console.

A dashboard is created. For more information, see [Create a dashboard](/intl.en-US/Visualization/Create a dashboard.md).

## Limits

-   You can create only one subscription task for each dashboard.
-   You can send up to 50 emails per day to each account.
-   You can create a total of 100 subscription tasks and alert tasks for each project. To increase quotas, submit a [ticket](https://workorder-intl.console.aliyun.com/console.htm?spm=a2796.7919406.0.dcontactus3.676a2d23RjosdV#/ticket/add/?productId=1210).
-   If data is displayed across multiple pages, Log Service sends only the snapshot of the first page to recipients.
-   The query time range of a subscribed dashboard is the same as that of the charts on the dashboard.

    **Note:**

    -   In the edit mode of a dashboard, double-click a chart. In the Edit dialog box, you can modify the query time range of the chart. The system saves the time range. The next time you open the dashboard, this query time range is displayed for the chart.
    -   In the display mode of a dashboard, you can set a time range to view the data that was obtained in the time range. However, the query time range is a temporary setting and will not be saved.

## Create a dashboard subscription task

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the project in which you want to query and analyze logs.

3.  In the left-side navigation pane, click the ![Create Alert for Monitoring - 001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p104975.png) icon.

4.  In the dashboard list that appears, click the target dashboard.

5.  Choose **Subscribe** \> **Create**.

6.  In the Create Subscription wizard, configure the subscription task, and then click **Next**.

    |Parameter|Description|
    |:--------|:----------|
    |**Subscription Name**|The name of the subscription task. The name must be 1 to 64 characters in length.|
    |**Frequency**|The frequency at which notifications are sent after you subscribe to the dashboard. Valid values:     -   Hourly: A notification is sent every hour.
    -   Daily: A notification is sent at a fixed time every day.
    -   Weekly: A notification is sent at a fixed time every week.
    -   Fixed Interval: A notification is sent at a fixed interval. Unit: days.
    -   Cron: A notification is sent at an interval that is specified by a CRON expression. The minimum interval is 1 minute. We recommend that you set an interval of more than one hour. For example, if you set the CRON expression to \* 0/1 \* \* \*, notifications are sent every hour, starting from 00:00. |
    |**Add Watermark**|Creates a watermark for a generated image. The watermark content is the email address or webhook address of the notification.|

7.  Set the notification method, and then click **Submit**.

    You can set the notification method to both Email and WebHook-DingTalk.

    -   Email
        -   Enter email addresses in the **Recipients** field. Separate each address with a comma \(,\).
        -   Specify a subject for an email notification in the **Subject** field. The default value of the field is Log Service Report.
    -   WebHook-DingTalk Bot
        -   Enter the webhook URL of DingTalk chatbot in the **Request URL** field. For more information about how to obtain the address, see [t21677.dita\#concept\_bzz\_vht\_2fb/section\_7gc\_prd\_349](t21677.dita#concept_bzz_vht_2fb/section_7gc_prd_349).
        -   Specify a title for a notification in the **Title** field.

## Related operations

If you have subscribed to a dashboard, you can modify or cancel your subscription to the dashboard.

-   Modify: Choose **Subscribe** \> **Modify**. In the wizard that appears, modify the subscription name, frequency, and notification method.
-   Cancel: Choose **Subscribe** \> **Cancel** to cancel your subscription to the dashboard.

    After you cancel your subscription to the dashboard, Log Service no longer sends notifications about the dashboard to recipients.


