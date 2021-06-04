# Silence policies

If you configure a silence policy, the alert management system does not send alert notifications during the specified period for alerts that meet the specified conditions. For example, you can silence alert notifications if the alerts are triggered during the maintenance of the test environment and the alerts have the `env=test` label. This topic describes how silence policies work.

You can specify a silence policy when you configure an alert policy. Then, alerts that use the specified alert policy are silenced if the alerts meet the specified silence conditions. The following three examples help you understand how a silence policy takes effect.

-   The silence policy takes effect before an alert notification is sent.

    Before an alert notification is sent, the alert management system filters the alerts in a merge set based on the specified silence policy. If an alert meets the condition of the silence policy, the alert notification is not sent. For example, the alert management system filters out Alert 1 that has the `env=test` label based on the silence policy and sends only alert notification for Alert 2. The following figure shows how the silence policy works.

    ![Silence policy 1](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1629872261/p255477.png)

-   The silence policy expires before an alert notification is sent.

    If the specified silence policy expires before an alert notification is sent, the alert management system ignores the policy for alerts that have the `env=test` label. Alert notifications for Alert 1 and Alert 2 are both sent. The following figure shows how the silence policy works.

    ![Expiration](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1629872261/p255482.png)

-   The silence policy is deleted before an alert notification is sent.

    If the specified silence policy is deleted before an alert notification is sent, the alert management system ignores the policy for alerts that have the `env=test` label. Alert notifications for Alert 1 and Alert 2 are both sent. The following figure shows how the silence policy works.

    ![Silence policy 3](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2629872261/p255483.png)


