# Notification methods

Log Service provides multiple notification methods, including email, DingTalk, webhook, and Alibaba Cloud Message Center. This topic describes the usage notes, prerequisites, and parameters for each notification method.

## Emails

If you choose to send alert notifications by using emails, Log Service sends alert notifications to specified users, user groups, or on-duty groups after alerts are triggered.

-   Usage notes

    Log Service sends notification emails by using the monitor-sg@monitor.alibabacloud.com email address. You can add this email address to the email whitelist to ensure that notification emails are not blocked.

-   Prerequisites

    A user, user group, or on-duty group is created. For more information, see [Create users and user groups](/intl.en-US/Alerting/Alerting (New)/User management/Create users and user groups.md) and [Create an on-duty group](/intl.en-US/Alerting/Alerting (New)/User management/Create an on-duty group.md).

-   Parameters

    Select Email as the notification method and set the following parameters in the Action Group dialog box. For more information, see [Create an action policy](/intl.en-US/Alerting/Alerting (New)/Notification management/Create an action policy.md).

    |Parameter|Description|
    |---------|-----------|
    |**Notification Method**|Select **Email**.|
    |**Recipient**|Select a user, user group, or an on-duty group to which the alert notification is sent.|
    |**Alert Template**|The content of an alert notification. Log Service sends alert notifications based on the selected alert template.|
    |**Period**|Select the time when alert notifications are sent. For more information, see [Configure notification sending periods](/intl.en-US/Alerting/Alerting (New)/Notification management/Dynamically send alert notifications/Configure notification sending periods.md).|


## DingTalk

If you select DingTalk as the notification method, Log Service sends alert notifications by using a DingTalk chatbot to the specified DingTalk group. The chatbot can also remind specified members in the group of the alert notifications.

-   Usage notes

    Each DingTalk chatbot can send a maximum of 20 alert notifications every minute.

-   Prerequisites

    Before you can use DingTalk to send alert notifications, you must complete the following configurations on DingTalk.

    1.  Open DingTalk and go to a DingTalk group.
    2.  Click ![DingTalk group settings](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2239872261/p264468.png) in the upper-right corner and choose **Group Assistant** \> **Add Robot**.
    3.  In the ChatBot dialog box, click the **+** icon in the Add Robot section.
    4.  In the Robot details dialog box, select **Custom \(Custom message services via Webhook\)** and click **Add**.
    5.  In the Add Robot dialog box, set the parameters.

        We recommend that you set **Security Settings** to **Custom Keywords** and set one of the keywords to **Alert**. Notifications can be sent only if the content contains at least one keyword. You can set a maximum of 10 keywords.

    6.  Click **Copy** to copy the webhook URL.
-   Parameters

    Select DingTalk as the notification method and set the following parameters in the Action Group dialog box. For more information, see [Create an action policy](/intl.en-US/Alerting/Alerting (New)/Notification management/Create an action policy.md).

    |Parameter|Description|
    |---------|-----------|
    |**Notification Method**|Select **DingTalk**.|
    |**Request URL**|Set the value to the webhook URL generated in the DingTalk group.|
    |**Alert Template**|The content of an alert notification. Log Service sends alert notifications based on the selected alert template.|
    |**Notification Method**|Log Service can remind specified members in a group of the alert notifications. You can choose to remind all members, specified members, or not to remind any members. If you set this parameter to **Specified Members**, you must specify a user, user group, or duty group. |
    |**Period**|Select the time when alert notifications are sent. For more information, see [Configure notification sending periods](/intl.en-US/Alerting/Alerting (New)/Notification management/Dynamically send alert notifications/Configure notification sending periods.md).|


## Webhook

If you select Webhook as the notification method, alert notifications are sent to a custom webhook URL.

-   Usage notes

    The timeout period of a webhook is 5 seconds. If no response is received within 5 seconds after a notification request is sent, the notification request fails.

-   Prerequisites

    A webhook URL is obtained.

-   Parameters

    Select Webhook as the notification method and set the following parameters in the Action Group dialog box. For more information, see [Create an action policy](/intl.en-US/Alerting/Alerting (New)/Notification management/Create an action policy.md).

    |Parameter|Description|
    |---------|-----------|
    |**Notification Method**|Select **Webhook**.|
    |**Request URL**|The webhook URL.|
    |**Alert Template**|The content of an alert notification. Log Service sends alert notifications based on the selected alert template.|
    |**Request Method**|You can select GET, POST, DELETE, PUT, or OPTIONS.|
    |**Request Headers**|Click **Add Request Header** to add a request header, such as, Content-Type: application/json;charset=utf-8.|
    |**Period**|Select the time when alert notifications are sent. For more information, see [Configure notification sending periods](/intl.en-US/Alerting/Alerting (New)/Notification management/Dynamically send alert notifications/Configure notification sending periods.md).|


## Message Center

If you select Message Center as the notification method, notifications are sent to specified recipients when alerts are triggered.

-   Prerequisites

    Before you can use Message Center to send alert notifications, you must complete the following configurations in Message Center.

    1.  Log on to the [Message Center console](https://notifications.console.aliyun.com/?spm=5176.202052012811.aliyun_topbar.162.zRRPhO#/subscribeMsg).
    2.  Choose **Message Settings** \> **Common Settings**. On the page that appears, click **Modify** next to **Log Service Alarm Notification**.
    3.  In the **Modify Contact** dialog box, select the contacts to whom you want to send alert notifications. Then, click **Save**.

        Only the owner of the Alibaba Cloud account can add new contacts. After you select a contact, a verification message is automatically sent to the specified email address. Alert notifications can be sent only after the email address is verified.

        **Note:**

        -   You must set at least one contact.
        -   The default notification is email and cannot be changed.
-   Parameters

    Select Message Center as the notification method and set the following parameters in the Action Group dialog box. For more information, see [Create an action policy](/intl.en-US/Alerting/Alerting (New)/Notification management/Create an action policy.md).

    |Parameter|Description|
    |---------|-----------|
    |**Notification Method**|Select **Message Center**.|
    |**Alert Template**|The content of an alert notification. Log Service sends alert notifications based on the selected alert template.|
    |**Period**|Select the time when alert notifications are sent. For more information, see [Configure notification sending periods](/intl.en-US/Alerting/Alerting (New)/Notification management/Dynamically send alert notifications/Configure notification sending periods.md).|


