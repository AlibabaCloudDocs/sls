# Create an alert policy

After the alert monitoring system generates an alert, Log Service merges, suppresses, or silences the alert based on the specified alert policy. This topic describes how to create an alert policy.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Go to the alert policy page.

    1.  In the Projects section, click the name of a destination project.

    2.  In the left-side navigation pane, click ![Alerts](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8943972261/p110115.png).

    3.  Click **Open Alert Center** and choose **Alert Management** \> **Alert Policy**.

3.  Click **Create** on the Alert Policy tab.

4.  In the Add Policy dialog box, set the following parameters and click **OK**.

    |Parameter|Description|
    |---------|-----------|
    |**ID**|The ID of the alert policy. The ID must be unique.|
    |**Name**|The name of the alert policy.|
    |**Inheritance**|Select a parent alert policy. If you select a parent alert policy, Log Service executes the parent alert policy before executing the alert policy that you configure. You can set this parameter to inherit silence policies. |
    |**Route Consolidation Policy**|If a large number of repeated alerts are triggered, you can configure a route consolidation policy to merge these alerts and send only one alert notification. For more information, see [Route consolidation policies](#section_vru_v38_f6x).|
    |**Suppression Policy**|You can configure a suppression policy to prevent similar alerts from sending alert notifications. For more information, see [Suppression policies](#section_db8_0tu_zun).|
    |**Silence Policy**|When you configure a silence policy, you can specify a silence period during which no alert notifications can be sent. For more information, see [Silence policies](#section_o2v_vkj_9ct).|


## Route consolidation policies

If a large number of repeated alerts are triggered, you can configure a route consolidation policy to merge these alerts and send only one alert notification. You can specify rules to merge alerts in the **Condition** and **Merge Alerts** dialog boxes.

The following information shows how to configure rules to merge alerts.

1.  On the Route Consolidation Policy tab, click ![Condition](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6841072261/p243402.png).
2.  Configure conditions to merge alerts. For example, you can set the value of AliUid to 1246\*\*\*\*\*.

    ![Condition](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2409872261/p254075.png)

3.  Configure rules to merge alerts. For example, you can merge alerts based on Alert Project and Severity.

    ![Merge alerts](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1318072261/p254076.png)

    |Parameter|Description|
    |---------|-----------|
    |Merge by|Specifies the conditions to merge alerts.     -   Alert ID and All Labels: If you merge alerts by Alert ID and All Labels, alerts that are triggered by the same alert monitoring rule and have the same labels are merged.
    -   Alert ID: If you merge alerts by Alert ID, alerts that are triggered by the same alert monitoring rule are merged.
    -   Alert Project: If you merge alerts by Alert Project, alerts that belong to the same project are merged.
    -   Alert Project and Severity: If you merge alerts by Alert Project and Severity, alerts that belong to the same project and have the same alert severity are merged.
    -   Alert Project and All Labels: If you merge alerts by Alert Project and All Labels, alerts that belong to the same project and have the same labels are merged.
    -   Custom: You can merge alerts by specifying alert attributes such as AliUids, alert IDs, or Alert names. |
    |Action policy|Select an action policy to process the triggered alert. **Note:** You can select an action policy when you merge alerts. You can also select an action policy when you configure an alert monitoring rule.

    -   If you select **Dynamic Action Policy**, the action policy that you specified when you configure an alert monitoring rule is used.
    -   If you do not select **Dynamic Action Policy** and select another action policy, the specified action policy is used. |
    |Group Wait|Configure the group wait interval. We recommend that you set the unit to Seconds. After an alert is grouped, Log Service sends the first alert notification after the specified group wait interval. |
    |Group Interval|Configure the group interval. We recommend that you set the unit to Minutes. Log Service checks the alert data in a group based on the specified group interval. |
    |Repeat Interval|Configure the group interval. We recommend that you set the unit to Hours. If the alert data in a group does not change, Log Service sends an alert notification after the specified repeat interval. |

4.  Click ![End ](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0356352261/p243468.png) in the Condition and **Merge Alerts** dialog boxes to end the configuration.

Example of a route consolidation policy

The following figure shows an example of a route consolidation policy. If the env label of an alert is prd, the alert is merged based on alert project and the built-in action policy of Log Service is used. If the env label of an alert is test, the alert is merged based on alert ID and the test action policy is used.

![Route consolidation policies](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2409872261/p254114.png)

## Suppression policies

You can configure suppression policies to prevent receiving alert notifications triggered by the same alert monitoring rule. You can specify rules to suppress alerts in the **Condition** and **Merge Alerts** dialog boxes.

The following information shows how to configure rules to suppress alerts.

1.  On the Suppression Policy tab, click ![Condition](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6841072261/p243402.png).
2.  Configure a condition to suppress alerts. For example, you can set the region to which the alert monitoring rule belongs to Hangzhou.

    ![Condition 2](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2409872261/p254080.png)

3.  Configure suppression conditions. For example, you can suppress an alert if the alert severity is low.

    ![Suppression conditions](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2409872261/p254085.png)

4.  Click ![End ](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0356352261/p243468.png) in the Condition and **Merge Alerts** dialog boxes to end the configuration.

Example of a suppression policy

You can set the alert name to K8s, alert severity to critical, and alert status to triggered. Then, if an alert whose severity is lower than critical is triggered and the alert has the same cluster ID as the source alert, the alert is suppressed.

![Example of a suppression policy](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2409872261/p254103.png)

## Silence policies

If you specify a silence period, no alert notifications are sent during this period. You can specify rules to silence alerts in the **Condition** and **Merge Alerts** dialog boxes.

To silence alerts, perform the following steps:

1.  On the Silence Policy tab, click ![Condition](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6841072261/p243402.png).
2.  Configure a condition to silence alerts. For example, you can set the alert severity to low.

    ![Condition 3](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3409872261/p254087.png)

3.  Configure a silence period. For example, you can set the duration to 1 hour.

    ![Silence period](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3409872261/p254088.png)

4.  Click ![End ](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0356352261/p243468.png) in the Condition and **Merge Alerts** dialog boxes to end the configuration.

Example of a silence policy

The following figure shows an example of a silence policy. You can silence an alert for 1 hour if the alert severity is medium, alert project is test-project, and the value of the expired label is True. If an alert does not have the Owner label, the alert is continuously silenced.

![Example of a silence policy](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3409872261/p254101.png)

