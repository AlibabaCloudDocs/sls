# Configure notification methods

Log Service supports setting one or more alert notification methods.Available notification methods include emails, DingTalk chatbot webhooks, custom webhooks, and Alibaba Cloud Message Center. This topic describes how to configure notification methods.

## \(Recommended\) Message Center

If you set the notification method to Notifications, Message Center sends alert notifications to specified contacts when alerts are triggered in Log Service.

1.  Specify the contacts to whom you want to send alert notifications in Message Center.

    1.  Log on to the [Message Center console](https://notifications.console.aliyun.com/?spm=5176.202052012811.aliyun_topbar.162.zRRPhO#/subscribeMsg).

    2.  Choose **Message Settings** \> **Common Settings**. On the page that appears, click **Modify** next to **Log Service Alarm Notification**.

        ![Message Settings](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9460277951/p7269.png)

    3.  In the **Modify Contact** dialog box, select the contacts and click **Save**.

        If you need to add a contact, click **+ Add Receiver**. Then, specify the email address and position of the contact.

        **Note:**

        -   Alibaba Cloud sends a verification email to the email address. The contact can receive alert notifications only after the email address is verified.
        -   You must set at least one contact.
        -   The default notification method is set to emails,and cannot be changed.
2.  Configure a notification method in the Log Service console.

    1.  When you [configure an alert rule](/intl.en-US/Query and visualization/Alarm/Create an alert rule.md), select **Notifications** from the **Notifications** drop-down list and click **Add**.

    2.  Enter the content of an alert notification.

        The content must be 1 to 500 characters in length. You can use system variables in the content. For more information, see [System variables](/intl.en-US/Query and visualization/Alarm/Relevant syntax and fields for reference/System variables.md).

    3.  Click **Submit**.

        In this example, only one notification method is configured. If multiple notification methods are required, configure the notification methods before you click **Submit**.


## Emails

If you set the notification method to Email, Log Service sends emails that contain alert notifications to specified email addresses.

1.  When you [configure an alert rule](/intl.en-US/Query and visualization/Alarm/Create an alert rule.md), select **Email** from the **Notifications** drop-down list and click **Add**.

2.  Set the following parameters.

    |Parameter|Description|
    |---------|-----------|
    |**Recipients**|The email addresses that receive the alert notification. Separate multiple email addresses with commas \(,\).|
    |**Subject**|The subject of the alert notification email. The subject must be 1 to 100 characters in length. You can use system variables in the subject. For more information, see [System variables](/intl.en-US/Query and visualization/Alarm/Relevant syntax and fields for reference/System variables.md).|
    |**Content**|The content of the alert notification email. The content must be 1 to 500 characters in length. You can use system variables in the content. For more information, see [System variables](/intl.en-US/Query and visualization/Alarm/Relevant syntax and fields for reference/System variables.md).|


## DingTalk chatbot webhooks

If you set the notification method to Webhook DingTalk Bot, Log Service sends alert notifications by using a DingTalk chatbot to the DingTalk group to which a specified webhook URL points.

**Note:** Each DingTalk chatbot can be used to send a maximum of 20 alert notifications every minute.

1.  Create a DingTalk chatbot.

    1.  Open DingTalk and go to a DingTalk group.

    2.  In the upper-right corner of the chatbox, click the Group Settings icon and choose **Group Assistant** \> **Add Robot**.

    3.  In the ChatBot dialog box, click Custom under **Please choose which robot to add**.

    4.  In the **Robot details** dialog box, click **Add**.

    5.  In the Add Robot dialog box, enter a chatbot name**** and select security options ****based on your business requirements. Then, select **I have read and accepted DingTalk Custom Robot Service Terms of Service** and click **Finished**.

        **Note:** We recommend that you set the **Security Settings** parameter to **Custom Keywords**. You can set a maximum of 10 keywords. Each message must contain at least one keyword. We recommend that you set a keyword to **Alert**.

    6.  Click **Copy** to copy the webhook URL.

2.  Configure a notification method in the Log Service console.

    1.  When you [configure an alert rule](/intl.en-US/Query and visualization/Alarm/Create an alert rule.md), select **WebHook-DingTalk Bot** from the **Notifications** drop-down list and click **Add**.

    2.  Set the following parameters.

        |Parameter|Description|
        |---------|-----------|
        |**Request URL**|The webhook URL of the DingTalk chatbot. Paste the webhook URL that you copied in [Step 1](#step_4jc_x7d_u5y).|
        |**Title**|The title of the alert notification. You can use system variables in the title. For more information, see [System variables](/intl.en-US/Query and visualization/Alarm/Relevant syntax and fields for reference/System variables.md). The title must be 1 to 100 characters in length.|
        |**Recipients**|The group members whom you want to remind to read the alert notification. Select None, All, or Specified Members as needed. If you select **Specified Members**, enter the mobile phone numbers of the group members in the **Tagged List** field. Separate multiple mobile phone numbers with commas \(,\).|
        |**Content**|The content of the alert notification. The default content is provided. You can modify the content based on your requirements. The content must be 1 to 500 characters in length. You can use system variables in the content. For more information, see [System variables](/intl.en-US/Query and visualization/Alarm/Relevant syntax and fields for reference/System variables.md). **Note:** If you need to remind a group member to read the alert notification, use the **@<Mobile phone number of the group member\>** syntax in the `Content` field. |

    3.  Click **Submit**.

        In this example, only one notification method is configured. If multiple notification methods are required, configure the notification methods before you click **Submit**.


## Custom webhooks

If you set the notification method to Webhook-Custom, Log Service sends alert notifications to a specified webhook URL.

**Note:** If Log Service receives no response within 5 seconds after a notification is sent, the request times out.

1.  When you [configure an alert rule](/intl.en-US/Query and visualization/Alarm/Create an alert rule.md), select **WebHook-Custom** from the **Notifications** drop-down list and click **Add**.

2.  Set the following parameters.

    |Parameter|Description|
    |---------|-----------|
    |**Request URL**|The custom webhook URL.|
    |**Request Method**|The method to send the notification. Available request methods include GET, POST, DELETE, PUT, and OPTIONS. The default request header is Content-Type: application/json;charset=utf-8. If you need to add request headers, click **Add Request Headers**. |
    |**Content**|The content of the alert notification. The default content is provided. You can modify the content based on your requirements. The content must be 1 to 500 characters in length. You can use system variables in the content. For more information, see [System variables](/intl.en-US/Query and visualization/Alarm/Relevant syntax and fields for reference/System variables.md).|


