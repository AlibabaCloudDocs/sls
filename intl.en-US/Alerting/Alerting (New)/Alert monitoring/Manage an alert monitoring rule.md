# Manage an alert monitoring rule

This topic describes how to manage an alert monitoring rule. After you create an alert monitoring rule, you can view the status and other information of the rule. You can also modify and delete the alert monitoring rule.

## View the information of an alert monitoring rule

After you create an alert monitoring rule, you can view the information of the rule on the Alert Overview page.

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the name of the project where the alert monitoring rule is created.

3.  In the left-side navigation pane, click the **Alert** icon.

4.  In the Alerts pane, click the alert monitoring rule to open the Alert Overview page.

    On the Alert Overview page, view the information of the alert monitoring rule. You can view the name of the dashboard with which the rule is associated. You can also view the time when the rule is created, the last update time, the check frequency, the rule status, and the alert notification status.

    ![chakan](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5868812261/p263139.png)


## Modify an alert monitoring rule

On the Alert Overview page, click **Modify Settings** to modify an alert monitoring rule. The displayed parameters are the same as the parameters that are required when you create the alert monitoring rule. For more information, see [Create an alert monitoring rule for logs](/intl.en-US/Alerting/Alerting (New)/Alert monitoring/Create an alert monitoring rule for logs.md).

## Enable and disable an alert monitoring rule

After you create an alert monitoring rule, click the **Enable** or **Close** button next to the **Status** parameter on the Alert Overview page.

**Note:** After you disable the alert monitoring rule, Log Service no longer checks the related data or send notifications.

![Status](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5868812261/p263140.png)

## Suspend or resume the alert notification feature of an alert monitoring rule

If an alert monitoring rule is in the **Enabled** state, you can suspend or resume the alert notification feature of the rule. On the Alert Overview page, click the **Modify** button next to the **Monitoring Status** parameter. In the Disable Alert Notifications panel, set the Disabled Duration parameter.

**Note:**

-   During the disabled duration that you have specified, Log Service checks data at the specified frequency based on the alert monitoring rule. However, Log Service does not send alert notifications even if the specified trigger condition is met.
-   During the disabled duration that you have specified, the resumption time is displayed in the **Monitoring Status** field. If you want to resume the alert notification feature before the resumption time, click the **Modify** button next to the **Monitoring Status** parameter. In the message that appears, click OK.

![guanbi](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5868812261/p263130.png)

## Delete an alert monitoring rule

In the upper-right corner of the Alert Overview page, click **Delete Alert**.

**Note:** After you delete an alert monitoring rule, the rule cannot be restored. Proceed with caution.

