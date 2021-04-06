# Operation compliance of Cloud Firewall

This topic describes the alert rules for the operation compliance of Cloud Firewall. You can configure and enable alert rules in the Log Service console. This allows you to monitor the operation compliance of Cloud Firewall. If an alert is triggered, you can identify the cause and fix the error at the earliest opportunity.

## Cloudfirewall Control Policy Change Alert the operation compliance of Cloud Firewall

|**ID**|sls\_app\_audit\_cis\_at\_cloudfirewall\_conf\_change|
|**Name**|Cloudfirewall Control Policy Change Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Cloudfirewall Operation Compliance|
|**Usage**|Monitors the control policy changes of Cloud Firewall. If a control policy of Cloud Firewall is changed, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: Medium-6|
|**External Configurations**|You can configure a whitelist of accounts whose control policies of Cloud Firewall can be changed. If the control policy of Cloud Firewall for accounts on the whitelist is changed, no alert is triggered.|
|**Solution**|Disable the control policy change of Cloud Firewall for accounts that are not included in the whitelist.|
|**Prerequisites**|The **Operations Log** switch next to Action Trail is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

