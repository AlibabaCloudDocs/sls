# Kubernetes security

This topic describes the alert rules for Kubernetes security, including excessive number of Kubernetes events and error messages and frequent delete events. You can configure and enable alerts in the Log Service console. This allows you to monitor Kubernetes security issues. If an alert is triggered, you can identify the cause and fix the error at the earliest opportunity.

## Alert rules

The following alert rules are supported. For information about how to set alert parameters, configure whitelists, and perform other related operations, see [Manage alert rules](/intl.en-US/Application/Log Audit Service/Alerting/Manage alert rules.md).

-   [Too Many K8s Warning Events Alert](#section_cxc_jou_ix4)
-   [K8s Frequent Delete Event Alert](#section_615_knr_crk)
-   [Too Many K8s Error Events Alert](#section_tzv_xec_7cs)

## Too Many K8s Warning Events Alert

|**ID**|sls\_app\_audit\_container\_at\_k8s\_warn|
|**Name**|Too Many K8s Warning Events Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Container Security, K8s Security|
|**Usage**|Monitors the number of warning events on a Kubernetes cluster. If the number of warning events on a Kubernetes cluster is greater than or equal to the Threshold parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|-   Alarm Name: The name of the alert. The default value is Too Many K8s Warning Events Alert. You can specify a unique name for each alert based on the metrics that you want to monitor.
-   Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: Medium-6
-   Threshold: The maximum number of warning events that are broadcast by a Kubernetes cluster every 2 minutes. Default value: 10.
-   Account ID \(Aliuid\): The ID of the Alibaba Cloud account that you want to monitor. Regular expressions are supported.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use regular expressions `.*` in the IDs. For example, 156133.\* indicates the Alibaba Cloud accounts that start with 156133.
    -   The default value is `.*`, which indicates the Alibaba Cloud accounts that are configured in the Log Audit Service application.
-   K8s Cluster Name: The name of the Kubernetes cluster that you want to monitor. Regular expressions are supported. The default value is `.*`, which indicates the Kubernetes clusters that are configured in the Log Audit Service application. |
|**External Configurations**|None|
|**Solution**|You can check whether exceptions have occurred on clusters that broadcast a great number of warning events.|
|**Prerequisites**|The **K8s Event Center** switch next to Kubernetes is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## K8s Frequent Delete Event Alert

|**ID**|sls\_app\_audit\_container\_at\_k8s\_del|
|**Name**|K8s Frequent Delete Event Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud Container, Security K8s, Security|
|**Usage**|Monitors frequent delete events on Kubernetes clusters. If a delete event on a Kubernetes cluster is greater than or equal to the Threshold parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|-   Alert Name: The name of the alert. The default value is K8s Frequent Delete Event Alert. You can specify a unique name for each alert based on the metrics that you want to monitor.
-   Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8
-   Threshold: The maximum number of delete events that are allowed on a Kubernetes cluster every 2 minutes. Default value: 5.
-   Account ID \(Aliuid\): The ID of the Alibaba Cloud account that you want to monitor. Regular expressions are supported.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use regular expressions `.*` in the IDs. For example, 156133.\* indicates the Alibaba Cloud accounts that start with 156133.
    -   The default is `.*`, which indicates the Alibaba Cloud accounts that are configured in the Log Audit Service application.
-   K8s Cluster Name: The name of the K8s cluster that you want to monitor. Regular expressions are supported. The default value is `.*`, which indicates all Kubernetes clusters within an Alibaba Cloud account. |
|**External Configurations**|None|
|**Solution**|Check whether exceptions have occurred on the Kubernetes cluster where delete events occur too frequently.|
|**Prerequisites**|The **K8s Event Center** switch next to Kubernetes is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## Too Many K8s Error Events Alert

|**ID**|sls\_app\_audit\_container\_at\_k8s\_err|
|**Name**|Too Many K8s Error Events Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Container Security, K8s Security|
|**Usage**|Monitors the error events of a Kubernetes cluster. If the number of error events on a Kubernetes cluster is greater than the Threshold parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|-   Alarm Name: The name of the alert. The default value is Too Many K8s Error Events Alert. You can specify a unique name for each alert based on the metrics that you want to monitor.
-   Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8
-   Threshold: The maximum number of error events that are broadcast by a Kubernetes cluster every 2 minutes. Default value: 5.
-   Account ID \(Aliuid\): The ID of the Alibaba Cloud account that you want to monitor. Regular expressions are supported.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use regular expressions `.*` in the IDs. For example, 156133.\* indicates the Alibaba Cloud accounts that start with 156133.
    -   The default value is `.*`, which indicates the Alibaba Cloud accounts that are configured in the Log Audit Service application.
-   K8s Cluster Name: The name of the Kubernetes cluster that you want to monitor. Regular expressions are supported. The default value is `.*`, which indicates the Kubernetes clusters within an Alibaba Cloud account. |
|**External Configurations**|None|
|**Solution**|Check whether exceptions have occurred on the Kubernetes cluster where an excessive number of error events occur.|
|**Prerequisites**|The **K8s Event Center** switch next to Kubernetes is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

