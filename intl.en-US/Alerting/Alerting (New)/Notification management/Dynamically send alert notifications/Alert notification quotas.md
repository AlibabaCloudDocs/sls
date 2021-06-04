# Alert notification quotas

Log Service allows you to set a daily quota on alert notifications that are sent to specified recipients by using SMS messages, voice calls, or emails. If the number of notifications sent to a recipient by using a specified notification method reaches the quota, the recipient can no longer receive notifications by using the same method.

## Examples

You can set a separate notification quota for users and user groups. Examples:

-   You can set a notification quota for test users. The users can receive 100 alert notifications each from SMS messages, voice calls, or emails.
-   You can set a notification quota for the built-in user group of Log Service. The users can receive 1,000 alert notifications each from SMS messages, voice calls, or emails.
-   You can use the default notification quota for other users. The users can receive 500 alert notifications each from SMS messages, voice calls, or emails.

For more information, see [Configure notification quotas](/intl.en-US/Alerting/Alerting (New)/Notification management/Configure notification quotas.md).

![Notification methods](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6139872261/p264164.png)

## Usage notes

If a user has a notification quota and the user group that the user is in has a different notification quota, Log Service dynamically adjusts the quota. Log Service adjusts the notification quota based on action policies. The following examples describe how the notification quota works.

-   If the recipient is only a user, the notification quota that is set for the user is the valid quota.
-   If the recipient is only a user group, the notification quota that is set for the user group is the valid quota.
-   If the recipient contains both a user and user group, the notification quota that is set for the user group is the valid quota.
-   If a user belongs to multiple user groups and the recipient contains multiple user groups, the user group that has the highest quota is the valid quota.

