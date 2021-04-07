# Manage alert rules

The Log Audit Service application provides built-in alert rules. You can enable alert instances next to the alert rules to monitor logs in real time. This topic describes how to enable, delete and disable alert instances. This topic also describes how to set whitelists and alert parameters.

## Enable alert instances

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Log Application section, click **Log Audit Service**.

3.  In the left-side navigation pane, choose **Audit Alert** \> **Policy Settings** \> **Alert Rules**.

4.  Find the alert rule and click the ![Enable](../images/p187950.png) icon.

    Action policies are attached to the alert rules in the Log Audit Service application based on the built-in alert policy. After you enable alert instances, you can use the built-in action policy. You can also customize action policies. For more information, see [Configure alert policies]() and [Configure action policies]().


## Configure whitelists

Log Service allows you to configure whitelists. If an account in the whitelist is used to perform operations, no alerts are triggered. The check of an ECS network type is used in this example.

1.  In the alert rule list, click **whitelist** next to **ECS Network Type Check**.

2.  In the Data Management dialog box, click **Create**.

3.  In the Add Data dialog box, set the **aliuid** parameter and click **OK**.

    For example, assume that you add the account 174\*\*\*\*857602745 to the whitelist. If this account is used to create ECS instances or perform other operations on ECS instances over the classic network, no alert is triggered.


## Set alert parameters

Log Service pre-defines alert rules in the Log Audit Service application and allows you to customize alert rules. Configurations check for Kubernetes is used in this example.

1.  In the alert rule list, click the ![Settings](../images/p188165.png) icon next to **K8s Log Audit Configuration Check**.

2.  In the Parameter Settings dialog box, set the **Min storage duration \(ttl\)** parameter and click **Save**. When you collect Kubernetes logs, an alert is triggered if you set a time value that is less than the value that is specified in Step 2 .


## Related operations

You can perform the following operations on the alert rule tab:

|Operation|Description|
|---------|-----------|
|Disable alert instances|Find the alert rule and click the ![Disable](../images/p187968.png) icon next to the alert rule.You can also select multiple alert rules and click **Disable**. |
|Temporarily disable alert instances|Find the alert rule and click the ![Pause](../images/p187970.png) icon next to the alert rule.You can also select multiple alert rules, click **Pause**, and then set a time range. |
|Restart alert instances|You can restart alert instances that are temporarily disabled.Find the alert rule and click the ![Restart](../images/p188012.png) icon next to the alert rule.

You can also select multiple alert rules and click **Resume**. |
|Delete alert instances|Find the alert rule and click the ![Delete](../images/p188014.png) icon next to the alert rule.You can also select multiple alert rules and click **Delete**. |
|Upgrade alert instances|An upgrade message will be sent to you if Log Service upgrades alert rules on a large scale and additional alert configurations are required. In most cases, Log Service automatically upgrades alert rules.You can select multiple alert rules that you want to upgrade and click **Upgrade**. |

