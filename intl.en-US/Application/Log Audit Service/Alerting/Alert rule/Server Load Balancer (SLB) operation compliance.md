# Server Load Balancer \(SLB\) operation compliance

This topic describes the alert rules for the compliance of Server Load Balancer \(SLB\) operations. The alert rules include SLB health check shutdown and SLB modification protection shutdown. You can configure and enable alert rules in the Log Service console to monitor the compliance of SLB operations. If an alert is triggered, you can identify the compliance problems of SLB operations at the earliest opportunity.

## Alert rules

The following alert rules are supported. For information about how to set alert parameters, configure whitelists, and perform other related operations, see [Manage alert rules]().

-   [SLB Modification Protection Shutdown Alert](#section_0y8_dbq_m4f)
-   [SLB Health Check Shutdown Alert](#section_r7d_vtz_kky)

## SLB Modification Protection Shutdown Alert

|**ID**|sls\_app\_audit\_cis\_at\_slb\_mod\_protec|
|**Name**|SLB Modification Protection Shutdown Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, SLB Operation Compliance|
|**Usage**|Monitors whether the modification protection feature is disabled for Server Load Balancer \(SLB\) instances. The modification protection feature must be enabled for Server Load Balancer \(SLB\) instances. Otherwise, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8|
|**External Configurations**|You can configure a whitelist of SLB instances whose modification protection feature can be disabled. If the modification protection feature is disabled for the SLB instances on the whitelist, no alert is triggered.|
|**Solution**|Enable the modification protection feature for the SLB instances that are not included in the whitelist.|
|**Prerequisites**|The **Access Log** switch next to API Gateway instance is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## SLB Health Check Shutdown Alert

|**ID**|sls\_app\_audit\_cis\_at\_slb\_health\_check|
|**Name**|SLB Health Check Shutdown Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, SLB Operation Compliance|
|**Usage**|Monitors whether the health check feature is disabled for Server Load Balancer \(SLB\) instances. The health check feature must be enabled for Server Load Balancer instances. Otherwise, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8|
|**External Configurations**|You can configure a whitelist of SLB instances whose health check feature can be disabled. If the health check feature is disabled for the SLB instances on the whitelist, no alert is triggered.|
|**Solution**|Enable the health check feature for the SLB instances that are not included in the whitelist.|
|**Prerequisites**|The **Access Log** switch next to API Gateway instance is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

