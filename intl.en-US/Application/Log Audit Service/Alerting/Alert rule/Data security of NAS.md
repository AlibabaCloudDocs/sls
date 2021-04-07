# Data security of NAS

This topic describes the alerts for the data security of Apsara File Storage NAS \(NAS\). You can set and then enable alert instances to trigger alerts in a timely manner. In this case, you can identify if errors exist in NAS.

## Alert rules

The following alert rules are supported. For information about how to set alert parameters, configure whitelists, and perform other relevant operations, see [Manage alert rules]().

-   [NAS Error Operation Detection](#section_5cl_qys_sut)
-   [NAS Mass Deletion Alert](#section_0gg_39o_cx3)

## NAS Error Operation Detection

|**ID**|sls\_app\_audit\_storage\_at\_nas\_err\_op|
|**Name**|NAS Error Operation Detection|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Data Security, and NAS Data Security|
|**Usage**|Monitors the error operations on NAS volumes. If the number of error operations on a NAS volume exceeds the specified threshold, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|You can specify the following parameters:-   **Alert Name**: The name of the alert. You can create multiple alerts.
-   **Severity**: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2.
-   **Threshold**: The threshold for the number of error operations. Default value: 5. If the number of error operations on a NAS volume exceeds the threshold within 2 minutes, an alert is triggered.
-   **Account ID \(Aliuid\)**: The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   **Volume**: The name of the volume to be monitored. You can use regular expressions when you specify this parameter.
    -   You can also use wildcards for the regular expressions, such as `.*`.
    -   Default value: `.*`. This indicates all the volumes of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs in the NAS volume that triggered the alert.|
|**Prerequisites**|The **Access Log** switch of NAS is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## NAS Mass Deletion Alert

|**ID**|sls\_app\_audit\_storage\_at\_nas\_file\_del|
|**Name**|NAS Mass Deletion Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Data Security, and NAS Data Security|
|**Usage**|Monitors the delete operations on NAS volumes. If the number of delete operations on a NAS volume exceeds the specified threshold, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|You can specify the following parameters:-   **Alert Name**: The name of the alert. You can create multiple alerts.
-   **Severity**: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2.
-   **Threshold**: The threshold for the number of delete operations. If the number of delete operations on a NAS volume exceeds the threshold within 2 minutes, an alert is triggered.
-   **Account ID \(Aliuid\)**: The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   **Volume**: The name of the volume to be monitored. You can use regular expressions when you specify this parameter.
    -   You can also use wildcards for the regular expressions, such as `.*`.
    -   Default value: `.*`. This indicates all the volumes of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs in the NAS volume that triggered the alert.|
|**Prerequisites**|The **Access Log** switch of NAS is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

