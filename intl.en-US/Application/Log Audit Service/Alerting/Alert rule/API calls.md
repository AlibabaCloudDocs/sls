# API calls

This topic describes the alert rules for the API call. You can configure and enable alert rules in the Log Service console. This allows you to monitor API calls. If an alert is triggered, you can identify the cause and fix the error at the earliest opportunity.

## Unauthorized Api Call Alert

|**ID**|sls\_app\_audit\_cis\_at\_unauth\_apicall|
|**Name**|Unauthorized Api Call Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, API Call|
|**Usage**|Monitors the number of unauthorized API calls. If the number of unauthorized API calls is greater than the specified Maximum times of unauthorized api call parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|-   Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: Medium-6
-   Maximum times of unauthorized api call: The maximum number of unauthorized API calls that can be initiated from one IP address to one single service every two minutes. Default value: 5. |
|**External Configurations**|You can configure a whitelist of IP addresses from which unauthorized API calls can be initiated. Unauthorized API calls from the IP addresses on the whitelist do not trigger an alert.|
|**Solution**|Disable the initiation of a large number of unauthorized API calls from IP addresses that are not included in the whitelist.|
|**Prerequisites**|The **Operations Log** switch next to Action Trail is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

