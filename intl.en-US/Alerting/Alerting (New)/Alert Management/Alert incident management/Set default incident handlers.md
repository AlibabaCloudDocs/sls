# Set default incident handlers

You can set handlers for alert incidents. You can also set a default incident handler. This way, when alerts are triggered, the default handler is automatically set by the system.

## Set default incident handlers

If you specify a default handler, the handler is automatically set in one of the following conditions:

-   An alert is triggered for the first time.
-   After an alert is resolved or ignored, another alert is triggered by the same alert monitoring rule.

![Set default incident handlers](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1539872261/p264452.png)

## Example

When you create action policies, you can select DingTalk as the notification method and turn on the Auto Dispatch switch. Then, you can select a group and set the Order parameter to Random. For more information, see [Create an action policy](/intl.en-US/Alerting/Alerting (New)/Notification management/Create an action policy.md).

![Set default incident handlers](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7331172261/p264560.png)

