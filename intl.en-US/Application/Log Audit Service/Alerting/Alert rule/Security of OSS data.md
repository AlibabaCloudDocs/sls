# Security of OSS data

This topic describes the alert rules for the security of OSS data. You can configure and enable alert rules in the Log Service console to monitor the security of OSS data. If an alert is triggered, you can identify the error cause and fix the error at the earliest opportunity.

## Alert rules

The following alert rules are supported. For information about how to set alert parameters, configure whitelists, and perform other relevant operations, see [Manage alert rules]().

-   [OSS Object Frequent Deletion Alert](#section_w02_agu_82a)
-   [OSS Bucket Account Access Control](#section_lkb_bbc_0ul)

## OSS Object Frequent Deletion Alert

|**ID**|sls\_app\_audit\_storage\_at\_oss\_obj\_del|
|**Name**|OSS Object Frequent Deletion Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Data Security, and OSS Data Security|
|**Usage**|Monitors the delete operations in OSS buckets. If the delete operations exceed the specified threshold, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|You can specify the following parameters:-   **Alert Name**: The name of the alert. You can create multiple alerts.
-   **Severity**: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2.
-   **Threshold**: The threshold for the number of delete operations. Default value:10. If the number of delete operations in an OSS bucket exceeds the threshold within 2 minutes, an alert is triggered.
-   **Account ID \(Aliuid\)**: The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   **Bucket Name**: The name of the OSS bucket to be monitored. You can use regular expressions when you specify this parameter.
    -   You can also use wildcards for the regular expressions, such as `.*`.
    -   Default value: `.*`. This indicates that all OSS buckets of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs in the OSS bucket that triggered the alert.|
|**Prerequisites**|The **Access Log** switch of OSS is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## OSS Bucket Account Access Control

|**ID**|sls\_app\_audit\_storage\_at\_oss\_access\_control|
|**Name**|OSS Bucket Account Access Control|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Data Security, and OSS Data Security|
|**Usage**|Monitors the access to OSS bucket. If an OSS bucket is accessed by an unspecified Alibaba Cloud account or RAM user, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|You can specify the following parameters:**Severity**: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. |
|**External Configurations**|You can specify a whitelist and add the Alibaba Cloud account and RAM user to the whitelist. If an OSS bucket is accessed by a whitelist account, no alert is triggered.|
|**Solution**|Do not allow Alibaba Cloud accounts or RAM users that are not included in the whitelist to access OSS buckets.|
|**Prerequisites**|The **Access Log** switch of OSS is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

