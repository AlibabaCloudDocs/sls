# FAQ

This topic provides answers to some frequently asked questions \(FAQ\) about the alerting feature of Log Service.

## General FAQ

-   What can I do when I use the new alerting feature of Log Service?

    Log Service provides an intelligent alerting system to monitor log data and time series data collected by Log Service. If an exception occurs, an alert is triggered and an alert notification is sent to specified recipients. This way, you can identify and resolve exceptions at the earliest opportunity. You can configure denoising policies, manage alert incidents, and configure action policies to send alert notifications. You can use the alerting feature of Log Service in more than 40 scenarios to meet the requirements of developers, O&M engineers, and security engineers. For more information, see [Features](/intl.en-US/Alerting/Alerting (New)/Feature overview/Features.md).

-   Who can use the alerting feature of Log Service?

    Developers, O&M engineers, security engineers, business engineers, and managers can use this feature.

-   What type of data can I monitor by using the alerting feature of Log Service?

    You can use the alerting feature of Log Service to monitor log data and time series data.

-   What features does the alerting feature of Log Service provide for on-duty groups?
    -   The alerting feature of Log Service allows you to use a calendar and provides information about business days, holidays, and business hours.
    -   The alerting feature allows you to set rotating shifts and substitute shifts. For more information, see [Create an on-duty group](/intl.en-US/Alerting/Alerting (New)/User management/Create an on-duty group.md).
-   Can I ingest external alerts to Log Service and manage the alerts?

    Yes, you can. Log Service allows you to ingest alerts from Prometheus and Grafana.

-   How do I control alert storms?
    -   When you configure alert monitoring rules, you can set continuous thresholds to control alert storms. You can also use interval-valued comparison functions, periodicity-valued comparison functions, and machine learning functions to manage alert storms.
    -   You can deduplicate, suppress, silence, delay, and merge alerts when you configure alert policies and action policies to manage alert storms.

## FAQ about regions and storage of alert incidents

-   Which regions does the alerting feature of Log Service support?

    The alerting feature is supported in all Alibaba Cloud regions.

-   Where can I store alert incident?

    If you use the alerting feature of Log Service for the first time, you must initialize the feature. After you initialize the feature, a project named sls-alert-Alibaba Cloud account ID-region and a Logstore named internal-alert-center-log are automatically created in the selected region to store alert incident.

-   How long can I store alert incident?

    Log Service stores alert incident for 30 days.

-   Can I export alert incidents from Log Service to self-managed systems?

    Yes, you can. You can export alert incidents to self-managed systems by using webhooks. You can also export alert incidents from a Logstore to self-managed systems.

-   How do I reset a region where alert incidents are stored?

    On the **Alert Center** \> **Alert Management** \> **Resource Data** tab, find **sls.alert.global\_config** and modify the default\_config parameter. After you modify the parameter, refresh the page and you can select another region.


## FAQ about billing

-   How am I charged for using the alerting feature of Log Service?

    You are charged only when you receive alert notifications by using SMS messages and voice calls. For more information, see [Billing method](/intl.en-US/Alerting/Alerting (New)/Feature overview/The alerting feature of Log Service.md).

-   Am I charged if a voice call is not answered and I receive a notification message?

    You are charged only once for the voice call whether the call is answered or not. The notification message does not incur fees.

-   Am I charged only once if I receive an alert notification that is sent to me in two messages.

    Yes, you are charged only once in this case.

    If an SMS message is longer than 70 characters, it may be sent in two messages. In this case, you are charged only once.


## FAQ about alert monitoring

-   Can I dynamically set alert severities?

    Yes, you can. Log Service allows you to dynamically set alert severities by specifying conditions when you configure alert monitoring rules. For more information, see [Create an alert monitoring rule for logs](/intl.en-US/Alerting/Alerting (New)/Alert monitoring/Create an alert monitoring rule for logs.md).

-   Can I receive no-data alert notifications?

    Yes, you can. You can enable the no-data alert feature and set the alert severity level when you configure alert monitoring rules. For more information, see [Create an alert monitoring rule for logs](/intl.en-US/Alerting/Alerting (New)/Alert monitoring/Create an alert monitoring rule for logs.md).

-   Can Log Service send alert recovery notifications if alerts are cleared?

    Yes, Log Service can send alert recovery notifications if you enable the alert recovery feature when you configure alert monitoring rules. For more information, see [Create an alert monitoring rule for logs](/intl.en-US/Alerting/Alerting (New)/Alert monitoring/Create an alert monitoring rule for logs.md).

-   Can I configure a single alert monitoring rule to send multiple alert notifications?

    Yes, you can. You can enable the group evaluation feature when you configure alert monitoring rules. For more information, see [Create an alert monitoring rule for logs](/intl.en-US/Alerting/Alerting (New)/Alert monitoring/Create an alert monitoring rule for logs.md).

-   Can I pause an alert monitoring rule?

    Yes, you can. To pause an alert monitoring rule, click **Close** in the **Status** field on the Alert Overview tab.


## FAQ about alert denoising

-   Can I suppress alerts?

    Yes, you can. To suppress alerts, add suppression policies when you configure alert policies. If an alert matches the specified conditions, the alert is suppressed. For example, if a server cannot be accessed and you want to suppress alerts triggered by this error, you can configure alert suppression policies. For more information, see [Suppression policies](/intl.en-US/Alerting/Alerting (New)/Alert Management/Create an alert policy.md).

-   Can I silence specific alerts or alerts with specific labels?

    Yes, you can. To silence alerts, you can add silence policies when you configure alert polices. For example, you can specify conditions, such as alert names, alert severities, source monitoring rules, and monitored objects when you add silence policies. If conditions are met, the related alerts are silenced. For more information, see [Silence policies](/intl.en-US/Alerting/Alerting (New)/Alert Management/Create an alert policy.md).

-   Can I merge alerts?

    Yes, you can. To merge alerts, you can specify conditions when you configure alert policies. This way, if a large number of repeated alerts are triggered, you can merge these alerts and send only one notification. For more information, see [Route consolidation policies](/intl.en-US/Alerting/Alerting (New)/Alert Management/Create an alert policy.md).

-   Can I view the status of an alert on a dashboard?

    Yes, you can. Log Service allows you to view alert data in Monitoring Rule Center, Alert Link Center, and Troubleshooting Center.


## FAQ about action policies

-   Can I escalate an alert if the alert remains unconfirmed for a long time?

    Yes, you can. To escalate an alert, you can configure action policies on the Secondary Action Policy tab. This way, if the alert remains unconfirmed or unresolved for a long time, an alert notification is sent to recipients specified on the Secondary Action Policy tab.

-   Can I create an on-duty group to handle alert incidents?

    Yes, you can. After you create an on-duty group, you can select the group as the recipient when you set the **Recipient** parameter.

-   Can I choose different notification methods for holidays?

    Yes, you can. When you set the **Period** parameter on the Edit Action Policy tab, you can select a time range to send notifications based on the settings of the global default calendar. You can specify different notification methods and recipients for different time ranges.

-   Can I stop sending alert notifications to an O&M engineer If the O&M engineer is on leave?
    -   Yes, you can. If the O&M engineer is in the on-duty group, you can create a substitute shift for the engineer. For more information, see [Create an on-duty group](/intl.en-US/Alerting/Alerting (New)/User management/Create an on-duty group.md).
    -   If the engineer is not in the on-duty group, you can turn off the Enabled switch in the Edit User dialog box. For more information, see [Create users and user groups](/intl.en-US/Alerting/Alerting (New)/User management/Create users and user groups.md).
-   Can I stop sending alert notifications to an O&M engineer If the O&M engineer is on a long-term leave?

    Yes, you can. You can turn off the Enabled switch in the Edit User dialog box.


## FAQ about notification methods

-   What notification methods does Log Service provide?

    Log Service provides multiple notification methods, including voice calls, SMS messages, emails, DingTalk messages, webhooks, and Alibaba Cloud Message Center. Notifications can be sent to Enterprise WeChat, Slack, or Feishu by using webhooks.

-   Which notification method incurs fees?

    You are charged if alert notifications are sent by using voice calls and SMS messages. You are charged based on the number of times that you receive alert notifications. For more information, see [Billing method](/intl.en-US/Alerting/Alerting (New)/Feature overview/The alerting feature of Log Service.md)

-   Can I customize the content of alert notifications?

    Yes, you can. You can customize the format and content of notifications for different notification methods by using an alert template. For more information, see [Create an alert template](/intl.en-US/Alerting/Alerting (New)/Notification management/Create an alert template.md).

-   Can I set the maximum number of alert notifications that can be received?

    Yes, you can set the maximum number of SMS messages, voice calls, and emails that a user, user group, or on-duty group can receive per day. For more information, see [Configure notification quotas](/intl.en-US/Alerting/Alerting (New)/Notification management/Configure notification quotas.md).


