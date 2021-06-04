# Alert attributes

This topic describes alert attributes.

|Type|Attribute|Child attribute|Identifying attribute \(Fingerprint\)|Description|
|----|---------|---------------|-------------------------------------|-----------|
|Alert monitoring rule|AliUid|None|Yes|The ID of the associated Alibaba Cloud account. The ID of the Alibaba Cloud account to which an alert monitoring rule belongs or the ID that is specified when you configure an ingested alert.|
|Alert type|None|No|The following alert types are supported:-   **Alert Monitoring Rule**: indicates an alert that is triggered by an alert monitoring rule.
-   **Alert Ingestion**: indicates that an alert is ingested. |
|Region|None|No|The region to which an alert monitoring rule belongs.|
|Project|None|Yes|The project to which an alert monitoring rule belongs.|
|Alert ID|None|Yes|The unique ID of an alert monitoring rule.|
|Alert name|None|No|The name of an alert monitoring rule.|
|Alert|Status|None|No|The following alert statuses are supported:-   Triggered: indicates that an alert is triggered.
-   Recovery notification: indicates that an alert is cleared. |
|Alert severity|None|No|The severity of an alert. Available alert severities include **Critical**, **High**, **Medium**, **Low**, and **Report**.|
|Title|None|No|The title of an annotation.|
|Description|None|No|The description of an annotation.|
|Label|Custom|Yes|An alert label.|
|Annotation|Custom|No|An alert annotation.|
|Time-related attributes|Trigger time|No|The time when an alert is triggered.|
|First trigger time|No|The time when the first alert is triggered by an alert monitoring rule. The time is a UNIX timestamp. If the alert is the first alert triggered by the alert monitoring rule, this value is equal to the trigger time.|
|Recovery time|No|If an alert is in the resolved status, the recovery time indicates the time when an alert is cleared. The time is a UNIX timestamp. If the alert is in the firing status, this value is 0.|
|Advanced settings|Alert policy|Alert policy ID|No|An alert policy ID that is configured in an alert monitoring rule or for an ingested alert.|
|Action policy ID|No|An action policy ID that is configured in an alert monitoring rule or for an ingested alert.|
|Alert ingestion|Service|No|The name of the service for an ingested alert.|
|Application|No|The name of the application for an ingested alert.|
|Protocol|No|The protocol for an ingested alert.|
|Region|No|The region in which an alert is ingested.|
|Query statistics 0, Query statistics 1, and Query statistics 2|Type|No|The following query types are supported:-   If you query data from a Logstore, the value is **Logstore**.
-   If you query data from a Metricstore, the value is **Metricstore**.
-   If you query resource data, the value is **Resource Data**. |
|Region|No|If you query data from a Logstore or a Metricstore, the value is the region where alert data resides. **Note:** This parameter does not exist if you query resource data. |
|Project|No|If you query data from a Logstore or a Metricstore, the value is the project name where alert data resides. **Note:** This parameter does not exist if you query resource data. |
|Destination Logstore|No|The name of the destination Logstore that is monitored.|
|Associated dashboards|No|The ID of the dashboard associated with a query.|
|Service role|No|The ARN of a RAM role.|
|Query statements|No|If you query data from a Logstore or a Metricstore, the value is a query statement. **Note:** This parameter does not exist if you query resource data. |
|The start time of a query|No|If you query data from a Logstore or a Metricstore, the value is the start time of a time range to query data, such as 2006-01-02 15:04:05. **Note:** This parameter does not exist if you query resource data. |
|The end time of a query|No|If you query data from a Logstore or a Metricstore, the value is the end of a time range to query data, such as 2006-01-02 15:04:05. **Note:** This parameter does not exist if you query resource data. |

