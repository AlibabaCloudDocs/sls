# Merge alerts

When you create an alert monitoring rule, you can configure alert policies to merge alerts in groups. This topic describes how to merge alerts in groups based on attributes.

## Merge alerts

You can merge alerts that have similar attributes into an alert set. When you configure alert policies, you can merge alerts into alert sets by specifying conditions on the Route Consolidation Policy tab.

For example, you can merge the following four alerts by specifying different conditions.

```
// Alert1
{
  "alert_name": "Alert1",
  "project": "Project1",
  "labels": {
    "env": "test",
    "service": "service1"
  }
}

// Alert2
{
  "alert_name": "Alert2",
  "project": "Project1",
  "labels": {
    "env": "prod",
    "service": "service2"
  }
}

// Alert3
{
  "alert_name": "Alert3",
  "project": "Project1",
  "labels": {
    "env": "test",
    "service": "service1"
  }
}

// Alert4
{
  "alert_name": "Alert4",
  "project": "Project1",
  "labels": {
    "env": "prod",
    "service": "service2"
  }
}
```

-   Merge alerts if the alerts belong to one project.

    The alerts are merged because the alerts belong to `Project1`.

-   Merge alerts if the alerts belong to one project and the value of the `env` field is the same.
    -   Alert 1 and Alert 3 are grouped into an alert set because the two alerts belong to `Project1` and have the same value of the env field as `labels`.
    -   Alert 2 and Alert 4 are grouped into an alert set because the two alerts belong to`Project1`and have the same value of the env field as `labels`.
-   Merge alerts if the `alert_name` is the same value.

    The four alerts are not merged because the values of the `alert_name` fields are different.


## Configure conditions to merge alerts

When you configure alert policies to merge alerts, you can use the default conditions or customize conditions.

-   Default conditions
    -   **Alert ID and All Labels**
    -   **Alert ID**
    -   **Alert Project**
    -   **Alert Project and Severity**
    -   **Alert Project and All Labels**
-   Custom conditions

    You can customize conditions to merge alerts based on alert attributes and labels.

    -   Alert attributes that can be used as conditions to merge alerts include Aliuid, monitoring rule ID, alert name, alert severity, alert region, and alert project.
    -   If you want to merge alerts based on labels, you can customize labels to merge alerts. You can also merge alerts if the alerts have the same labels.

## Do not merge alerts

When you merge alerts by **Alert ID and All Labels**, alerts are merged into one alert set if the alerts have the same ID and labels.

-   If you use the preceding method to merge alerts, alerts with different IDs are not merged.
-   If you enable the group evaluation feature when you create an alert monitoring rule, the triggered alerts are grouped into different alert sets and are not merged. The alerts are not merged because the alerts have different labels.

When you create an alert monitoring rule, if you do not enable the **Advanced Settings** feature in the **Alert Policy** section, the default alert policy **sls.builtin.dynamic** is used. If you merge alert by **Alert ID and All Labels**, the triggered alerts are not merged.

For example, two sample alerts are created.

-   The group evaluation feature of Alert 1 is enabled. The **Advanced Settings** feature in the **Alert Policy** section is enabled. The value of the **Merge By** parameter is **Alert ID and All Labels**. Three alert notifications are sent.
-   The group evaluation feature of Alert 2 is disabled. The **Advanced Settings** feature in the **Alert Policy** section is not enabled. The value of the **Merge By** parameter is **Alert ID and All Labels**. If the alert is triggered, only one notification is sent.

![buhebing](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1229872261/p264676.png)

