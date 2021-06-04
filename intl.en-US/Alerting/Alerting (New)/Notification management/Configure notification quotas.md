# Configure notification quotas

Log Service allows you to specify a quota for notifications that are sent by using SMS messages, voice calls, or emails. If the number of notifications sent to a recipient by using a specified notification method reaches the quota, the recipient can no longer receive notifications by using the same method. This topic describes how to configure notification quotas.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Go to the Notification Method Quota page.

    1.  In the Projects section, click the name of the required project.

    2.  In the left-side navigation pane, click the ![Alerts](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0356352261/p110115.png) icon.

    3.  Click **Open Alert Center** and choose **Alert Management** \> **Notification Method Quota**.

3.  Click the ![Condition](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6841072261/p243402.png) icon to configure a condition.

4.  Configure the condition and then click **OK**.

    For example, a Log Service O&M group wants to receive at most 1,000 SMS messages, 1,000 voice calls, and 1,000 emails per day. You can configure a condition, as shown in the following figure.

    ![Condition](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7019872261/p254124.png)

    You can add conditions in standard mode or advanced mode.

    -   **Standard Mode**: Multiple conditions are associated by the AND operator.
    -   **Advanced Mode**: Multiple conditions are associated by the AND or OR operator. You can use parentheses \(\) to group multiple conditions.
5.  Click the ![Method](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7019872261/p254126.png) icon to configure quotas for notification methods.

6.  Configure quotas and then click **OK**.

    If you configure notification quotas without specifying users, the quotas apply to all users and user groups. You can specify the quotas for the Log Service O&M group, as shown in the following figure.

    ![Quota](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7019872261/p254128.png)

7.  Click the ![End ](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0356352261/p243468.png) icon that corresponds to the Condition and Quota dialog boxes to complete the configuration.

    If you want to add conditions and quotas, click the ![Condition](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6841072261/p243402.png) icon.


