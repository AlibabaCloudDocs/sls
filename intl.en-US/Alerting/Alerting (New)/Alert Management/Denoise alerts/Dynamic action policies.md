# Dynamic action policies

If you select Dynamic Action Policy and specify an action policy when you configure an alert monitoring rule, the specified action policy is a dynamic action policy. This topic describes how dynamic action policies work.

You can specify an action policy to send alert notifications. You can view and change the settings of an action policy on the Action Policy tab. You can also configure an action policy when you create an alert monitoring rule.

When you create an alert monitoring rule, do not enable the **Advanced Settings** feature in the **Alert Policy** section. Select **sls.builtin.dynamic** for the **Action Policy** to specify how to send alert notifications.

For example, two sample alert monitoring rules are created.

-   Alert Monitoring Rule 1

    The Group Evaluation feature is enabled. The **Advanced Settings** feature in the **Alert Policy** section is disabled, and the value of the **Action Policy** parameter is Action Policy 1.

-   Alert Monitoring Rule 2

    The Group Evaluation feature is disabled. The **Advanced Settings** feature in the **Alert Policy** section is disabled, and the value of the **Action Policy** parameter is Action Policy 2.


If an alert is triggered, the alert notification is sent based on the specified action policy.

![dynamic-action](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1919872261/p264671.png)

