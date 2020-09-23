# Configure a notification method

This topic describes how to configure a notification method. Log Service can send alert notifications by using one or more methods. Available notification methods include emails, DingTalk chatbot webhooks, custom webhooks, and Alibaba Cloud Message Center.

## \(Recommended\) Set up Message Center

If you select the Message Center method, Message Center sends alert notifications to the specified contacts when alerts are triggered in Log Service.

1.  Specify the contacts to whom you want to send alert notifications in Message Center.

    1.  Log on to the [Message Center](https://notifications.console.aliyun.com/?spm=5176.202052012811.aliyun_topbar.162.zRRPhO#/subscribeMsg).

    2.  On the **Message Settings** \> **Common Settings** page, click **Modify** to the right of **Log Service Alarm Notification**.

        ![Message Settings](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9460277951/p7269.png)

    3.  In the **Modify Contact** dialog box, select the contacts to whom you want to send alert notifications. Then, click **Save**.

        If you need to add a contact, click **Add Receiver** and specify the email address and position of the contact.

        **Note:**

        -   If you add a contact, Alibaba Cloud sends a verification message to the mobile phone number and a verification email to the email address of the contact. The contact can receive alert notifications only after the verification is passed.
        -   You must select at least one contact.
        -   Alert notifications are sent using emails. The text message and email notification methods cannot be disabled.
        -   Message Center sends a maximum of 100 alert notification emails to each email address within 24 hours, for example, from 18:00 on July 19, 2020 to 18:00 on July 20, 2020.
2.  Configure a notification method in the Log Service console.

    1.  When you [configure an alert rule](/intl.en-US/Query and visualization/Alarm/Create an alert rule.md), select **Notifications** from the **Notifications** drop-down list and click **Add**.

    2.  Enter the content of an alert notification.

        The content must be 1 to 500 characters in length. You can use system variables in the content. For more information, see [System variables](/intl.en-US/Query and visualization/Alarm/Relevant syntax and fields for reference/System variables.md).

    3.  Click **Submit**.

        In this section, only one notification method is configured. If multiple notification methods are required, configure other notification methods before you click **Submit**.


## Set up emails

If you select the email method, Log Service sends emails that contain notifications to the specified email addresses.

1.  When you [configure an alert rule](/intl.en-US/Query and visualization/Alarm/Create an alert rule.md), select **Email** from the **Notifications** drop-down list and click **Add**.

2.  Set the following parameters.

    |Parameter|Description|
    |---------|-----------|
    |**Recipients**|The email addresses that receive alert notifications. Separate each email address with a comma \(,\).**Note:** Log Service sends a maximum of 100 emails to each email address within 24 hours, for example, from 18:00 on July 19, 2020 to 18:00 on July 20, 2020. |
    |**Subject**|The subject of an alert notification email. The subject must be 1 to 100 characters in length. You can use system variables in the subject. For more information, see [System variables](/intl.en-US/Query and visualization/Alarm/Relevant syntax and fields for reference/System variables.md).|
    |**Content**|The content of an alert notification email. The content must be 1 to 500 characters in length. You can use system variables in the content. For more information, see [System variables](/intl.en-US/Query and visualization/Alarm/Relevant syntax and fields for reference/System variables.md).|


## Set up a DingTalk chatbot webhook

If you select the DingTalk chatbot webhook method, Log Service sends notifications by using a DingTalk chatbot to the DingTalk group to which the specified webhook URL points.

**Note:** Each DingTalk chatbot can be used to send a maximum of 20 alert notifications every minute.

1.  Create a DingTalk chatbot.

    1.  Open DingTalk and click a DingTalk group.

    2.  In the upper-right corner of the chatbox, click the Group Settings icon and choose **Group Assistant** \> **Add Robot**.

    3.  In the ChatBot dialog box, click Custom under **Please choose which robot to add**.

    4.  In the **Robot details** dialog box, click **Add**.

    5.  In the Add Robot dialog box, enter a chatbot name and select security options based on your business requirements. Then, select **I have read and accepted DingTalk Custom Robot Service Terms of Service** and click **Finished**.

        **Note:** We recommend that you select **Custom Keywords** next to **Security Settings**. You can enter a maximum of 10 custom keywords. After you set keywords, the DingTalk robot sends a message only if the message contains at least one of the keywords.

    6.  Click **Copy** to copy the webhook URL.

2.  Configure a notification method in the Log Service console.

    1.  When you [configure an alert rule](/intl.en-US/Query and visualization/Alarm/Create an alert rule.md), select **WebHook-DingTalk Bot** from the **Notifications** drop-down list and click **Add**.

    2.  Set the following parameters.

        |Parameter|Description|
        |---------|-----------|
        |**Request URL**|The webhook URL of the DingTalk chatbot. Paste the webhook URL that you copied in [Step 1](#step_4jc_x7d_u5y).|
        |**Title**|The title of an alert notification message. You can use system variables in the title. For more information, see [System variables](/intl.en-US/Query and visualization/Alarm/Relevant syntax and fields for reference/System variables.md). The title must be 1 to 100 characters in length.|
        |**Recipients**|The group members that you want to remind to read an alert notification. Select None, All, or Specified Members. If you select **Specified Members**, enter the mobile phone numbers of the group members in the **Tagged List** field. Separate each mobile phone number with a comma \(,\).|
        |**Content**|The content of an alert notification. The default content is provided. You can modify the content based on your requirements. The content must be 1 to 500 characters in length. You can use system variables in the content. For more information, see [System variables](/intl.en-US/Query and visualization/Alarm/Relevant syntax and fields for reference/System variables.md). **Note:** If you need to remind a group member to read the alert notification, use the `@<Mobile phone number of the group member>` syntax in the **Content** field. |

    3.  Click **Submit**.

        In this section, only one notification method is configured. If multiple notification methods are required, configure other notification methods before you click **Submit**.


## Set up a custom webhook

If you select the custom webhook method, Log Service sends notification requests to the specified webhook URL.

**Note:** If Log Service receives no response within 5 seconds after a notification request is sent, the notification request fails.

1.  When you [configure an alert rule](/intl.en-US/Query and visualization/Alarm/Create an alert rule.md), select **WebHook-Custom** from the **Notifications** drop-down list and click **Add**.

2.  Set the following parameters.

    |Parameter|Description|
    |---------|-----------|
    |**Request URL**|The custom webhook URL.|
    |**Request Method**|The method to send a notification request. Supported request methods include GET, POST, DELETE, PUT, and OPTIONS. The default request header is Content-Type: application/json;charset=utf-8. If you need to add request headers, click **Add Request Headers**. |
    |**Content**|The content of an alert notification. The default content is provided. You can modify the content based on your requirements. The content must be 1 to 500 characters in length. You can use system variables in the content. For more information, see [System variables](/intl.en-US/Query and visualization/Alarm/Relevant syntax and fields for reference/System variables.md).|


