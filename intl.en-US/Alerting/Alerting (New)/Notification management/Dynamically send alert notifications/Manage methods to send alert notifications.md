# Manage methods to send alert notifications

This topic describes how to manage methods that are used to send alert notifications.

## Distribute alerts

After the notification management system receives alert sets, the system distributes the alert sets to different channels based on alert attributes.

-   For alerts of the critical severity level, notifications are sent to users by using DingTalk. For alerts of other severity levels, notifications are sent to users by using emails.

    ![Distribute alerts](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0039872261/p264401.png)

-   For alerts with the sensitive=p1 tag, notifications are sent to security groups by using emails in addition to SMS messages.

    ![Distribution mechanism](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0039872261/p265203.png)


## Merge alerts

After the notification management system receives alert sets, the system distributes the alert sets to specified channels.

For example, if you merge alerts by service, alerts that are created for the same service belong to an alert set. The notification management system sends notifications to users within specified periods.

![Send notifications](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0039872261/p265016.png)

