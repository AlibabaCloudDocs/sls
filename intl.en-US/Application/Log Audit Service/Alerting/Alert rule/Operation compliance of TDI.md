# Operation compliance of TDI

This topic describes the alert rules for the operation compliance of TDI. You can configure and enable alert rules in the Log Service console to monitor the operation compliance of TDI. If an alert is triggered, you can identify the error cause and fix the error at the earliest opportunity.

## TDI Webpage Anti-tampering Disabled Alert

|**ID**|sls\_app\_audit\_cis\_at\_unauth\_apicall|
|**Name**|TDI Webpage Anti-tampering Disabled Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, and TDI Operation Compliance|
|**Usage**|Monitors whether the web tamper protection feature of Security Center is disabled. If the web tamper protection feature of Security Center is disabled, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8.|
|**External Configurations**|You can specify a whitelist of accounts that can disable the web tamper protection feature of Security Center. If the web tamper protection feature of Security Center is disabled by an account on the whitelist, no alert is triggered.|
|**Solution**|Do not disable the web tamper protection feature of Security Center by using an account that is not included in the whitelist.|
|**Prerequisites**|The **Operations Log** switch of ActionTrail is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

