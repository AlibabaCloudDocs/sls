# Security of OSS traffic

This topic describes the alert rules for the security of Object Storage Service \(OSS\) traffic. You can configure and enable alert rules in the Log Service console to monitor the security of OSS traffic. If an alert is triggered, you can identify the error cause and fix the error at the earliest opportunity.

## Alert rules

The following alert rules are supported. For information about how to set alert parameters, configure whitelists, and perform other relevant operations, see [Manage alert rules]().

-   [OSS Flow Anomaly Inspection](#section_32d_i5q_u0w)
-   [OSS Inflow Anomaly Inspection](#section_qh4_es4_nbx)
-   [OSS Outflow Anomaly Inspection](#section_tc7_ly5_vrw)
-   [OSS Access PV Anomaly Inspection](#section_0g1_3i3_eva)
-   [OSS Access UV Anomaly Inspection](#section_sbx_vat_0uc)
-   [OSS Bucket Valid Request Rate Too Low Alert](#section_7lq_h5t_kys)
-   [Detection of OSS Bucket Visit through Internet](#section_ic9_hef_54q)

## OSS Flow Anomaly Inspection

|**ID**|sls\_app\_audit\_dataflow\_at\_oss\_flow\_detc|
|**Name**|OSS Flow Anomaly Inspection|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Data Security, and OSS Flow Security.|
|**Usage**|Monitors the inbound and outbound traffic of OSS. If the number of traffic exceptions exceeds the specified threshold, an alert is triggered.|
|**Check Frequency**|Fixed interval: 4 hours.|
|**Time Range**|The data of the last 4 hours is checked.|
|**Parameters Settings**|You can specify the following parameters:-   **Alert Name**: The name of the alert. You can create multiple alerts.
-   **Severity**: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2.
-   **Threshold**: The threshold for the number of OSS traffic exceptions. Default value: 10. If the number of traffic exceptions exceeds the threshold within 4 hours, an alert is triggered.

A traffic value is calculated every minute.

-   **Account ID \(Aliuid\)**: The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   **Bucket Name**: The name of the OSS bucket to be monitored. You can use regular expressions when you specify this parameter.
    -   You can also use wildcards for the regular expressions, such as `.*`.
    -   Default value: `.*`. This indicates that all OSS buckets of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs in the OSS bucket that triggered the alert.|
|**Prerequisites**|The **Access Log** switch of OSS is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## OSS Inflow Anomaly Inspection

|**ID**|sls\_app\_audit\_dataflow\_at\_oss\_inflow\_detc|
|**Alert Name**|OSS Inflow Anomaly Inspection|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Data Security, and OSS Flow Security.|
|**Usage**|Monitors the inbound traffic of OSS. If the number of traffic exceptions exceeds the specified threshold, an alert is triggered.|
|**Check Frequency**|Fixed interval: 4 hours.|
|**Time Range**|The data of the last 4 hours is checked.|
|**Parameters Settings**|You can specify the following parameters:-   **Alert Name**: The name of the alert. You can create multiple alerts.
-   **Severity**: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2.
-   **Threshold**: The threshold for the number of inbound traffic exceptions of OSS. Default value: 10. If the number exceeds the threshold within 4 hours, an alert is triggered.

An inbound traffic value is calculated every minute.

-   **Account ID \(Aliuid\)**: The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   **Bucket Name**: The name of the OSS bucket to be monitored. You can use regular expressions when you specify this parameter.
    -   You can also use wildcards for the regular expressions, such as `.*`.
    -   Default value: `.*`. This indicates that all OSS buckets of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs in the OSS bucket that triggered the alert.|
|**Prerequisites**|The **Access Log** switch of OSS is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## OSS Outflow Anomaly Inspection

|**ID**|sls\_app\_audit\_dataflow\_at\_oss\_outflow\_detc|
|**Name**|OSS Outflow Anomaly Inspection|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Data Security, and OSS Flow Security.|
|**Usage**|Monitors the outbound traffic of OSS. If the number of outbound traffic exceptions exceeds the specified threshold, an alert is triggered.|
|**Check Frequency**|Fixed interval: 4 hours.|
|**Time Range**|The data of the last 4 hours is checked.|
|**Parameter Settings**|You can specify the following parameters:-   **Alert Name**: The name of the alert. You can create multiple alerts.
-   **Severity**: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2.
-   **Threshold**: The threshold for the number of outbound traffic exceptions of OSS. Default value: 10. If the number of traffic exceptions exceeds the threshold within 4 hours, an alert is triggered.

A traffic value is calculated every minute.

-   **Account ID \(Aliuid\)**: The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   **Bucket Name**: The name of the OSS bucket to be monitored. You can use regular expressions when you specify this parameter.
    -   You can also use wildcards for the regular expressions, such as `.*`.
    -   Default value: `.*`. This indicates that all OSS buckets of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs in the OSS bucket that triggered the alert.|
|**Prerequisites**|The **Access Log** switch of OSS is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## OSS Access PV Anomaly Inspection

|**ID**|sls\_app\_audit\_dataflow\_at\_oss\_pv\_detc|
|**Name**|OSS Access PV Anomaly Inspection|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Data Security, and OSS Flow Security.|
|**Usage**|Monitors the PVs of OSS. If the number of PV exceptions exceeds the specified threshold, an alert is triggered.|
|**Check Frequency**|Fixed interval: 4 hours.|
|**Time Range**|The data of the last 4 hours is checked.|
|**Parameters Settings**|You can specify the following parameters:-   **Alert Name**: The name of the alert. You can create multiple alerts.
-   **Severity**: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2.
-   **Threshold**: The threshold for the number of PV exceptions of OSS. Default value: 10. If the number of PV exceptions exceeds the threshold within 4 hours, an alert is triggered.

A PV value is calculated every minute.

-   **Account ID \(Aliuid\)**: The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   **Bucket Name**: The name of the OSS bucket to be monitored. You can use regular expressions when you specify this parameter.
    -   You can also use wildcards for the regular expressions, such as `.*`.
    -   Default value: `.*`. This indicates that all OSS buckets of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs in the in the OSS bucket that triggered the alert.|
|**Prerequisites**|The **Access Log** switch of OSS is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## OSS Access UV Anomaly Inspection

|**ID**|sls\_app\_audit\_dataflow\_at\_oss\_uv\_detc|
|**Name**|OSS Access UV Anomaly Inspection|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Data Security, and OSS Flow Security.|
|**Usage**|Monitors the UVs of OSS. If the number of UV exceptions exceeds the specified threshold, an alert is triggered.|
|**Check Frequency**|Fixed interval: 4 hours.|
|**Time Range**|The data of the last 4 hours is checked.|
|**Parameters Settings**|You can specify the following parameters:-   **Alert Name**: The name of the alert. You can create multiple alerts.
-   **Severity**: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2.
-   **Threshold**: The threshold for the number of UV exceptions of OSS. Default value: 10. If the number of UV exceptions exceeds the threshold within 4 hours, an alert is triggered.

A value is calculated every minute.

-   **Account ID \(Aliuid\)**: The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   **Bucket Name**: The name of the OSS bucket to be monitored. You can use regular expressions when you specify this parameter.
    -   You can also use wildcards for the regular expressions, such as `.*`.
    -   Default value: `.*`. This indicates that all OSS buckets of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs in the OSS bucket that triggered the alert.|
|**Prerequisites**|The **Access Log** switch of OSS is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## OSS Bucket Valid Request Rate Too Low Alert

|**ID**|sls\_app\_audit\_dataflow\_at\_oss\_req\_rate|
|**Name**|OSS Bucket Valid Request Rate Too Low Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Data Security, and OSS Flow Security.|
|**Usage**|Monitors the valid request rate of OSS buckets. If the rate is lower than the specified threshold, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameters Settings**|You can specify the following parameters:-   **Alert Name**: The name of the alert. You can create multiple alerts.
-   **Severity**: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2.
-   **Threshold**: The threshold for the valid request rate for OSS buckets. Default value: 95. If the rate is lower than the threshold, an alert is triggered.
-   **Account ID \(Aliuid\)**: The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   **Bucket Name**: The name of the OSS bucket to be monitored. You can use regular expressions when you specify this parameter.
    -   You can also use wildcards for the regular expressions, such as `.*`.
    -   Default value: `.*`. This indicates that all OSS buckets of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs in the OSS bucket that triggered the alert.|
|**Prerequisites**|The **Access Log** switch of OSS is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## Detection of OSS Bucket Visit through Internet

|**ID**|sls\_app\_audit\_dataflow\_at\_oss\_internet\_access|
|**Name**|Detection of OSS Bucket Visit through Internet|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Data Security, and OSS Flow Security.|
|**Usage**|Monitors the access of OSS buckets over the Internet. If an OSS bucket is accessed over the Internet, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|You can specify the following parameters:**Severity**: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. |
|**External Configurations**|You can specify a whitelist of accounts. If an OSS bucket belongs to an account on the whitelist and the OSS bucket is accessed over the Internet, no alert is triggered.|
|**Solution**|Do not allow OSS buckets that do not belong to an account on the whitelist to be accessed over the Internet.|
|**Prerequisites**|The **Access Log** switch of OSS is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

