# Labels and annotations

When you create an alert monitoring rule, you can specify labels and annotations. Labels are used to denoise alerts, manage notification routes, and dispatch alerts. Annotations are used to configure alert templates and dispatch alerts. This topic describes labels and annotations. This topic also describes the scenarios where you can use labels and annotations.

## Labels

Labels have the following features:

-   Labels are identifying attributes for alerts, and are formatted in key-value pairs. Labels are the part of alert fingerprints and can be used to deduplicate alerts. For example, the label of the Host1 host is `"labels": {"host": "host1"}`. The alert management system can deduplicate alerts based on the label.
-   Labels can be quoted in alert templates by using `${labels}`.
-   Alerts can be managed and dispatched based on labels in the alert management system and action management system.
-   In alert policies, labels can be used in route consolidation policies to denoise alerts.
-   The fields that you specify when you set the Group Evaluation parameter are automatically used as labels.
-   Labels are static texts. You can customize a label. The label is automatically added as the attribute of an alert.

For example, you can add the labels in the following figure when you create an alert monitoring rule. The alert management system can silence alerts with the Group label to denoise alerts.

![biaoqian](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8998812261/p262329.png)

## Annotations

Annotations have the following features:

-   Annotations are non-identifying attributes for alerts, and are formatted in key-value pairs. The value of an annotation can be a variable. For example, you can create an annotation as `"annotations": {"title": "CPU utilization of ${service} is too high","desc": "Current CPU utilization of ${service} is 90%"}`.
-   When you specify the content of an annotation, you can quote the field variables that are specified for the Group Evaluation parameter. If an alert is triggered, the specified variables are replaced by actual values.
-   Alerts can be managed and dispatched based on annotations in the alert management system and action management system.
-   An annotation consists of a title and a description \(desc\).
    -   A title is a fixed and non-identifying attribute of an alert. You can quote titles in an alert template by using $\{annotations.title\}.
    -   A description is a fixed and non-identifying attribute of an alert. You can quote descriptions in an alert template by using $\{annotations.desc\}.

For example, you can add the annotation in the following figure when you create an alert monitoring rule. The notification management system can quote the annotation in an alert template to send alert notifications.

![biaozhu](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8998812261/p262331.png)

The following table describes the built-in variables that you can use to specify the attribute values of an annotation.

|Variable|Description|
|--------|-----------|
|\_\_count\_\_|The number of rows that are scanned in each group after these rows are grouped. If you set the Group Evaluation parameter to No Grouping, all data is in one group.|
|\_\_pass\_count\_\_|The number of rows that meet specified conditions in each group after these rows are grouped. If you set the Group Evaluation parameter to No Grouping, all data is in one group.|
|\_\_0\_count\_\_|The number of rows that are returned for the first query.|
|\_\_1\_count\_\_|The number of rows that are returned for the second query.|
|\_\_2\_count\_\_|The number of rows that are returned for the third query.|
|aliuid|The ID of an Alibaba Cloud account.|
|alert\_instance\_id|The ID of an alert.|
|alert\_id|The ID of an alert monitoring rule.|
|alert\_name|The name of an alert monitoring rule.|
|project|The project to which an alert monitoring rule belongs.|

