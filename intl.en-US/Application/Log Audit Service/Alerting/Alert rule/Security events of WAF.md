# Security events of WAF

This topic describes the alerts for the security events of Web Application Firewall \(WAF\). You can configure and enable alert rules in the Log Service console to monitor the security events of WAF. If an alert is triggered, you can identify the error cause and fix the error at the earliest opportunity.

## Alert rules

The following alert rules are supported. For information about how to set alert parameters, configure whitelists, and perform other relevant operations, see [Manage alert rules]().

-   [Too Many Attacks on Hosts Protected by WAF Alert](#section_3pg_usp_8yz)
-   [Application Firewall Valid Request Rate Too Low Alert](#section_ycb_2yy_6mx)

## Too Many Attacks on Hosts Protected by WAF Alert

|**ID**|sls\_app\_audit\_secure\_at\_waf\_attack|
|**Name**|Too Many Attacks on Hosts Protected by WAF Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Security Event, and WAF Security Event|
|**Usage**|Monitors website attacks. If the number of attacks on a website that is protected by WAF exceeds the specified threshold, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|You can specify the following parameters:-   **Alert Name**: The name of the alert. You can create multiple alerts.
-   **Severity**: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2.
-   **Threshold**: The threshold for the number of attacks on a website. Default value: 5. If the number exceeds the threshold within 2 minutes, an alert is triggered.
-   **Account ID \(Aliuid\)**: The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   **Hosts**: The name of the website to be monitored.
    -   You can use regular expressions when you specify this parameter. You can also use wildcards for the regular expressions, such as `.*`.
    -   Default value: `.*`. This indicates that all websites protected by WAF of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs in the website that triggered the alert.|
|**Prerequisites**|The **Access Log** switch of Web Application Firewall is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## Application Firewall Valid Request Rate Too Low Alert

|**ID**|sls\_app\_audit\_secure\_at\_waf\_rate|
|**Name**|Application Firewall Valid Request Rate Too Low Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Security Event, and WAF Security Event|
|**Usage**|Monitors the valid request rate to a website WAF blocks and filters inbound traffic to your website. If the valid request rate to a website is lower than the specified threshold, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|You can specify the following parameters:-   **Alert Name**: The name of the alert. You can create multiple alerts.
-   **Severity**: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2.
-   **Threshold**: The threshold for the valid request rate to a website. Default value: 90%. If the rate is lower than the specified threshold, an alert is triggered.
-   **Account ID \(Aliuid\)**: The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   **Hosts**: The name of the website to be monitored.
    -   You can use regular expressions when you specify this parameter. You can also use wildcards for the regular expressions, such as `.*`.
    -   Default value: `.*`. This indicates that all websites protected by WAF of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs in the website that triggered the alert.|
|**Prerequisites**|The **Access Log** switch of Web Application Firewall is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

