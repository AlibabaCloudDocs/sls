# Deduplicate alerts

If the alert management system receives an alert that matches the conditions of a route consolidation policy, the alert is merged and grouped into a merge set. After the alert is suppressed, silenced, or deduplicated based on the route consolidation policy, the merge set is sent to the notification management system.

## Configure a route consolidation policy

Alerts are merged based on merge conditions, action policies, the group wait interval, group interval, and repeat interval. Alerts are merged into one merge set only when the preceding configurations are the same.

For example, a service is deployed on Host 1 and Host 2. Host 1 and Host 2 both trigger a high CPU alert every minute from 20:00 and 20:01. If you want to delay the time to send alert notifications, you can create an alert policy to merge alerts based on the service name. Then, after the first and the second alert notifications are sent, notifications for alerts that are triggered afterward by the same alert monitoring rule are delayed.

![Reference](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8229872261/p260153.png)

## Merge conditions

You can specify merge conditions to merge alerts based on alert attributes and labels. You can use the built-in merge conditions or customize merge conditions.

|Type|Description|
|----|-----------|
|Built-in merge conditions|Log Service provides the following built-in merge conditions:

-   Alert ID and All Labels
-   Alert ID
-   Alert Project
-   Alert Project and Severity
-   Alert Project and All Labels |
|Custom merge conditions|You can customize merge conditions to merge alerts based on alert attributes and labels.

-   Alert attributes that can be used as conditions to merge alerts include AliUids, monitoring rule IDs, alert names, alert severities, alert regions, and alert projects.
-   If you want to merge alerts based on labels, you can merge alerts by using all labels, custom labels, or no labels. |

The following example shows how to merge alerts by alert project, the env label, and service label.

-   Example

    ![Custom merge conditions](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6470172261/p254991.png)

    -   Set the **Merge by** parameter to **Custom**.
    -   Set the **Alert Attributes** parameter to **Alert Project**.
    -   Set the **Alert Label** parameter to **Custom**.
    -   Set the **Custom Label** parameter to **env,service**.
    -   Set the **Action Policy** parameter to **Log Service Built-in Action Policy**.
    -   Set the **Group Wait** parameter to **30 Seconds**.
-   Alert incidents

    ```
    // Alert A
    {
      "alert_name": "Alert1",
      "project": "Project1",
      "labels": {
        "env": "test",
        "service": "service1"
      }
    }
    
    // Alert B
    {
      "alert_name": "Alert2",
      "project": "Project1",
      "labels": {
        "env": "prod",
        "service": "service2"
      }
    }
    
    // Alert C
    {
      "alert_name": "Alert3",
      "project": "Project1",
      "labels": {
        "env": "test",
        "service": "service1"
      }
    }
    
    // Alert D
    {
      "alert_name": "Alert4",
      "project": "Project1",
      "labels": {
        "env": "prod",
        "service": "service2"
      }
    }
    ```

-   Result

    Alert A and Alert C are grouped into one merge set. Alert B and Alert D are grouped into another merge set.


## Action policies

Action policies specify how to send alert notifications. You can view and change the settings of an action policy on the Action Policy tab. For more information, see [Create an action policy](/intl.en-US/Alerting/Alerting (New)/Notification management/Create an action policy.md).

Log Service also allows you to configure dynamic action policies. If you select **Dynamic Action Policy** on the **Route Consolidation** tab and specify an action policy when you configure an alert monitoring rule, an alert notification is sent. The alert notification is sent based on the action policy you specified when you configured the alert monitoring rule. For more information, see [Dynamic action policies](/intl.en-US/Alerting/Alerting (New)/Alert Management/Denoise alerts/Dynamic action policies.md).

## Group wait, group interval, and repeat interval

After you merge alerts, alert notifications are sent in a unified manner. Alerts in a merge set are not triggered at the same time. Therefore, Log Service provides group wait, group interval, and repeat interval to manage the frequency of alert notifications. You can prevent alert storms by specifying the Group Wait, Group Interval, and Repeat Interval parameters. The following table describes the parameters.

|Parameter|Description|
|---------|-----------|
|**Group Wait**|The interval that the first alert notification waits to be sent after a merge set is created. We recommend that you set the unit to Seconds to prevent alert storms.|
|**Group Wait**|The interval that the alert notification waits to be sent after the data in a merge set is changed. We recommend that you set the unit to Minutes. You can also set the unit to Seconds to receive alert notifications in a timely manner. Alert data is changed when a new alert is added to the merge set or the alert status is changed. |
|**Repeat Interval**|The interval that the alert notification waits to be sent after the data in a merge set remains the same. We recommend that you set the unit to Hours. Alert data remains the same if alert attributes change, such as the alert title and alert content. Alert data changes if a new alert is added to the merge set or the alert status is changed.

**Note:** If you specify a dynamic action policy when you configure an action policy, you do not need to set this parameter. By default, the repeat interval that you specify when you configure an alert monitoring rule is used. |

-   Scenario 1: Only Alert A is triggered during the group wait interval.

    Assume that the group wait interval is 5 seconds. The group wait interval is 1 minute. The repeat interval is 4 hours. The following figure shows two sample alerts. The orange sign indicates Alert A and the blue sign indicates Alert B.

    ![Examples](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6470172261/p255146.png)

    -   Alert A is triggered at 00:00:00 and a merge set is created at the same time. However, the alert notification is not immediately sent because of the specified group wait interval.
    -   At 00:00:05 when the group wait interval is reached, Log Service sends the first alert notification.
    -   After the first notification is sent, the system periodically checks the data in the merge set based on the specified group interval. Alert B is grouped into the merge set during the first group interval. Therefore, the second notification is sent at 00:01:05.
    -   Then, the system checks the data in the merge set based on the specified group wait interval and detects only Alert A and Alert B exist in the merge set. In this case, the third notification is sent at 04:01:05 only when the specified repeat interval is reached.
-   Scenario 2: Alert A and Alert B are triggered during the group wait interval.

    Assume that the group wait interval is 5 seconds. The group interval is 1 minute. The repeat interval is 4 hours. The following figure shows two sample alerts. The orange sign indicates Alert A and the blue sign indicates Alert B.

    ![Merge process](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7470172261/p264963.png)

    -   Alert A and Alert B are triggered from 00:00:00 to 00:00:05 and a merge set is created. However, the alert notification is not immediately sent because of the specified group wait interval.
    -   At 00:00:05 when the group wait interval is reached, Log Service sends the first alert notification.
    -   After the first notification is sent, the system periodically checks the data in the merge set based on the specified group interval. The system detects only Alert A and Alert B exist in the merge set from 00:00:05 to 04:01:05. In this case, the third notification is sent at 04:01:05 only when the specified repeat interval is reached.

