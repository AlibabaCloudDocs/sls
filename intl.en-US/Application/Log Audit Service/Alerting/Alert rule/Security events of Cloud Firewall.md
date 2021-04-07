# Security events of Cloud Firewall

This topic describes the alert rules for the security events of Cloud Firewall. You can configure and enable alert rules in the Log Service console to trigger alerts to monitor the security events of Cloud Firewall. If an alert is triggered, you can identify the error cause and fix the error at the earliest opportunity.

## Alert rules

The following alert rules are supported. For information about how to set alert parameters, configure whitelists, and perform other relevant operations, see [Manage alert rules]().

-   [Cloudfirewall Inflow Block Alarm](#section_rno_1r3_akd)
-   [Cloudfirewall Outflow Block Alert](#section_dmz_yng_e69)

## Cloudfirewall Inflow Block Alarm

|**ID**|sls\_app\_audit\_secure\_at\_cfw\_in\_block|
|**Name**|Cloudfirewall Inflow Block Alarm|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Security Event, and Cloudfirewall Security Event|
|**Usage**|Monitors the inbound traffic that is intercepted by Cloud Firewall. If the number of inbound traffic interception for an access protocol exceeds the specified threshold, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|You can specify the following parameters:-   **Alert Name**: The name of the alert. You can create multiple alerts.
-   **Severity**: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2.
-   **Threshold**: The threshold for the number of inbound traffic interception. Default value: 10. If the number exceeds the threshold within 2 minutes, an alert is triggered.
-   **Account ID \(Aliuid\)**: The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   **Access Protocol Name**: The name of the access protocol to be monitored. You can use regular expressions when you specify this parameter.
    -   You can also use wildcards for the regular expressions, such as`.*`.
    -   Default value: `.*`. This indicates that all access protocols of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs in the inbound traffic that is intercepted by the Cloud Firewall.|
|**Prerequisites**|The **Internet Access Log** switch of Cloud Firewall is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## Cloudfirewall Outflow Block Alert

|**ID**|sls\_app\_audit\_secure\_at\_cfw\_out\_block|
|**Name**|Cloudfirewall Outflow Block Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Security Event, and Cloudfirewall Security Event|
|**Usage**|Monitors the outbound traffic that is intercepted by Cloud Firewall. If the number of outbound traffic interceptions for an access protocol exceeds the specified threshold, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|You can specify the following parameters:-   **Alert Name**: The name of the alert. You can create multiple alerts.
-   **Severity**: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2.
-   **Threshold**: The threshold for the number of times outbound traffic is intercepted. Default value: 10. If the number exceeds the threshold within 2 minutes, an alert is triggered.
-   **Account ID \(Aliuid\)**: The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   **Access Protocol Name**: The name of the access protocol to be monitored. You can use regular expressions when you specify this parameter.
    -   You can also use wildcards for the regular expressions, such as`.*`.
    -   Default value: `.*`. This indicates that all access protocols under the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs in the outbound traffic that is intercepted by the Cloud Firewall.|
|**Prerequisites**|The **Internet Access Log** switch of Cloud Firewall is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

