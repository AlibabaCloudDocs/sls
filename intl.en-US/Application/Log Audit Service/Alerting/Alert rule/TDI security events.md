# TDI security events

This topic describes the alert rules for the security events of threat detection and identification \(TDI\). You can configure and enable alert rules in the Log Service console. This allows you to monitor the security events of TDI. If an alert is triggered, you can identify the cause and fix the error at the earliest opportunity.

## Alert rules

The following alert rules are supported. For information about how to set alert parameters, configure whitelists, and perform other related operations, see [Manage alert rules]().

-   [Cloud Security Center Request Success Rate Too Low](#section_k7e_c0n_baz)
-   [Cloud Security Center Valid Request Rate Too Low Alert](#section_fn2_qge_e0l)
-   [Too Many New Alarms In Cloud Security Center](#section_hdx_flo_zvm)
-   [Too Many New Vulnerabilities In Cloud Security Centers](#section_e12_t1j_ru6)
-   [Too Many High-Priority Alarms In Cloud Security Center](#section_eg2_ubq_wra)

## Cloud Security Center Request Success Rate Too Low

|**ID**|sls\_app\_audit\_secure\_at\_sas\_dns\_rate|
|**Name**|Cloud Security Center Request Success Rate Too Low|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Security Event, TDI Security Event|
|**Usage**|Monitors the success rate of DNS requests sent to Security Center If the success rate of DNS requests sent to Security Center is lower than this threshold, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|The following rules describe the parameter settings of the alert:-   **Alert Name**: The name of the alert. You can create multiple alert instances.
-   **Severity**: Critical-10, High-8, Medium-6, Low-4, and Report-2.
-   **Threshold**: The threshold success rate of DNS requests sent to Security Center. Default value: 90%. If the success rate of DNS request sent to Security Center is lower than the specified threshold during the 2 minute window, an alert is triggered.
-   **Account ID \(Aliuid\)**: The ID of the Alibaba Cloud account related to the API gateway that you want to monitor. Regular expressions are supported.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use regular expressions `.*` in the IDs. For example, 156133.\* indicates the Alibaba Cloud accounts that start with 156133.
    -   Default value: `.*`. This is used to identify the Alibaba Cloud accounts that are configured in the Log Audit Service application. |
|**External Configurations**|None|
|**Solution**|Check whether exceptions have occurred on DNS requests that are sent to Security Center.|
|**Prerequisites**|The switch next to Security Center\(SAS\) is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## Cloud Security Center Valid Request Rate Too Low Alert

|**ID**|sls\_app\_audit\_secure\_at\_sas\_rate|
|**Name**|Cloud Security Center Valid Request Rate Too Low Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Security Event, TDI Security Event|
|**Usage**|Monitors the rate of valid requests sent to Security Center. If the rate of valid requests sent to the website is lower than the specified threshold after all requests are filtered by Security Center, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|The following rules describe the parameter settings of the alert:-   **Alert Name**: The name of the alert. You can create multiple alert instances.
-   **Severity**: Critical-10, High-8, Medium-6, Low-4, and Report-2.
-   **Threshold**: The threshold rate of the valid request to the website. Default value: 90%. If the rate of valid request sent to the website is lower than the threshold after all requests are filtered by Security Center protection, an alert is triggered. This applies during the last 2 minutes when the requests are filtered.
-   **Account ID \(Aliuid\)**: The ID of the Alibaba Cloud account related to the API gateway that you want to monitor. Regular expressions are supported.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use regular expressions `.*` in the IDs. For example, 156133.\* indicates the Alibaba Cloud accounts that start with 156133.
    -   Default value: `.*`. This is used to identify the Alibaba Cloud accounts that are configured in the Log Audit Service application.
-   **Website \(host\)**: The name of the website that you want to monitor. Regular expressions are supported.
    -   You can use regular expressions, such as `.*` to make configurations.
    -   Default value: `.*`. This is used to identify all websites within the Alibaba Cloud account. |
|**External Configurations**|None|
|**Solution**|Check whether exceptions have occurred on the request events that are sent to Security Center. You can also check whether a large number of attack events have occurred.|
|**Prerequisites**|The switch next to Security Center\(SAS\) is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## Too Many New Alarms In Cloud Security Center

|**ID**|sls\_app\_audit\_secure\_at\_sas\_new\_alert|
|**Name**|Too Many New Alarms In Cloud Security Center|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Security Event, TDI Security Event|
|**Usage**|Monitors the number of new alerts in Security Center. If the number of new alerts in Security Center exceeds the specified threshold, an alert is triggered.|
|**Check Frequency**|Fixed interval: 4 minutes.|
|**Time Range**|The data of the last 5 minutes is checked.|
|**Parameter Settings**|The following rules describe the parameter settings of the alert:-   **Alert Name**: The name of the alert. You can create multiple alert instances.
-   **Severity**: Critical-10, High-8, Medium-6, Low-4, and Report-2.
-   **Threshold**: The threshold for the number of new alerts. Default value: 2. If the number of new alerts in Security Center exceeds the specified threshold during the 5 minute window, an alert is triggered.
-   **Account ID \(Aliuid\)**: The ID of the Alibaba Cloud account related to the API gateway that you want to monitor. Regular expressions are supported.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use regular expressions `.*`in the IDs. For example, 156133.\* indicates the Alibaba Cloud accounts that start with 156133.
    -   Default value: `.*`. This is used to identify the Alibaba Cloud accounts that are configured in the Log Audit Service application. |
|**External Configurations**|None|
|**Solution**|Check the new alerts in Security Center.|
|**Prerequisites**|The switch next to Security Center\(SAS\) is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## Too Many New Vulnerabilities In Cloud Security Centers

|**ID**|sls\_app\_audit\_secure\_at\_sas\_new\_vul|
|**Name**|Too Many New Vulnerabilities In Cloud Security Centers|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Security Event, TDI Security Event|
|**Usage**|Monitors the number of new vulnerabilities in Security Center. If the number of new vulnerabilities in Security Center exceeds the specified threshold, an alert is triggered.|
|**Check Frequency**|Fixed interval: 4 minutes.|
|**Time Range**|The data of the last 5 minutes is checked.|
|**Parameter Settings**|The following rules describe the parameter settings of the alert:-   **Alert Name**: The name of the alert. You can create multiple alert instances.
-   **Severity**: Critical-10, High-8, Medium-6, Low-4, and Report-2.
-   **Threshold**: The threshold for the number of new vulnerabilities. Default value: 1. If the number of new vulnerabilities in Security Center exceeds the specified threshold during the 5 minute window, an alert is triggered.
-   **Account ID \(Aliuid\)**: The ID of the Alibaba Cloud account related to the API gateway that you want to monitor. Regular expressions are supported.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use regular expressions `.*` in the IDs. For example, 156133.\* indicates the Alibaba Cloud accounts that start with 156133.
    -   Default value: `.*`. This is used to identify the Alibaba Cloud accounts that are configured in the Log Audit Service application. |
|**External Configurations**|None|
|**Solution**|Check the new vulnerabilities in Security Center.|
|**Prerequisites**|The switch next to Security Center\(SAS\) is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## Too Many High-Priority Alarms In Cloud Security Center

|**ID**|sls\_app\_audit\_secure\_at\_sas\_ser\_alert|
|**Name**|Too Many High-Priority Alarms In Cloud Security Center|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Security Event, TDI Security Event|
|**Usage**|Monitors the number of high-priority alerts in Security Center. If the number of high-priority alerts in Security Center exceeds the specified threshold, an alert is triggered.|
|**Check Frequency**|Fixed interval: 4 minutes.|
|**Time Range**|The data of the last 5 minutes is checked.|
|**Parameter Settings**|The following rules describe the parameter settings of the alert:-   **Alert Name**: The name of the alert. You can create multiple alert instances.
-   **Severity**: Critical-10, High-8, Medium-6, Low-4, and Report-2.
-   **Threshold**: The threshold for the number of high-priority alerts. Default value: 1. If the number of high-priority alerts in Security Center exceeds this threshold, an alert is triggered.
-   **Account ID \(Aliuid\)**: The ID of the Alibaba Cloud account related to the API gateway that you want to monitor. Regular expressions are supported.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use regular expressions `.*` in the IDs. For example, 156133.\* indicates the Alibaba Cloud accounts that start with 156133.
    -   Default value: `.*`. This is used to identify the Alibaba Cloud accounts that are configured in the Log Audit Service application. |
|**External Configurations**|None|
|**Solution**|You can monitor the high-priority alert in Security Center.|
|**Prerequisites**|The switch next to Security Center\(SAS\) is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

