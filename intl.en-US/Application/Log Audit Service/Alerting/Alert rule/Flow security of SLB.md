# Flow security of SLB

This topic describes the alert rules for the flow security of Server Load Balancer \(SLB\). You can configure and enable alert rules in the Log Service console. This allows you to monitor the security of SLB instances. If an alert is triggered, you can identify the cause and fix the error at the earliest opportunity.

## Alert rules

The following alert rules are supported. For information about how to set alert parameters, configure whitelists, and perform other related operations, see [Manage alert rules]().

-   [Inspection of SLB Abnormal Response Length](#section_tqx_fw5_2ci)
-   [Inspection of SLB Abnormal Request Length](#section_ph3_7cj_962)
-   [SLB Average Response Delay Too High-8 Alert](#section_fjo_722_s17)
-   [SLB HTTP Access Protocol Enabled Alert](#section_iyu_af6_5p2)
-   [Load Balance Access UV Anomaly Inspection](#section_evi_2dx_h9u)
-   [Load Balance Access PV Anomaly Inspection](#section_trn_5qm_x08)

## Inspection of SLB Abnormal Response Length

|**ID**|sls\_app\_audit\_dataflow\_at\_slb\_resp\_detc|
|**Name**|Inspection of SLB Abnormal Response Length|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Flow Security, SLB Flow Security|
|**Usage**|Detects whether the length of SLB response is abnormal. If the number of SLB responses that have an abnormal length is greater than or equal to the value of the Threshold parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 4 hours.|
|**Time Range**|The data of the last 4 hours is checked.|
|**Parameter Settings**|-   Alert Name: The name of the alert. By default, the value of this parameter is Inspection of SLB Abnormal Response Length. You can specify a unique name for each alert based on the metrics that you want to monitor.
-   Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8
-   Threshold: The maximum number of SLB responses that have an abnormal length during the 4 hour window. An average response length is calculated per minute. Default value: 10.
-   Account ID \(Aliuid\): The ID of the Alibaba Cloud account related to the API gateway that you want to monitor. Regular expressions are supported.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use regular expressions `.*` in the IDs. For example, 156133.\* indicates the Alibaba Cloud accounts that start with 156133.
    -   The default value is `.*`, which indicates the Alibaba Cloud accounts that are configured in the Log Audit Service application.
-   SLB Instance Name: The name of the SLB instance that you want to monitor. Regular expressions are supported. The default value is `.*`, which indicates the SLB instances that are attached to your Alibaba Cloud accounts. |
|**External Configurations**|None|
|**Solution**|Check whether exceptions have occurred on the SLB instances that have an abnormal length in a large number of responses.|
|**Prerequisites**|The **Lay-7 Access** switch next to SLB is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## Inspection of SLB Abnormal Request Length

|**ID**|sls\_app\_audit\_dataflow\_at\_slb\_req\_detc|
|**Name**|Inspection of SLB Abnormal Request Length|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Flow Security, SLB Flow Security|
|**Usage**|Detects whether the length of SLB request is abnormal. If the number of SLB requests that have abnormal length is greater than or equal to the value of the Threshold parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 4 hours.|
|**Time Range**|The data of the last 4 hours is checked.|
|**Parameter Settings**|-   Alarm Name: The name of the alert. By default, the value of this parameter is Inspection of SLB Abnormal Request Length. You can specify a unique name for each alert based on the metrics that you want to monitor.
-   Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8
-   Threshold: The maximum number of SLB requests that have an abnormal length during the 4 hour window. An average request length is calculated per minute. Default value: 10.
-   Account ID \(Aliuid\): The ID of the Alibaba Cloud account related to the API gateway that you want to monitor. Regular expressions are supported.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use regular expressions `.*` in the IDs. For example, 156133.\* indicates the Alibaba Cloud accounts that start with 156133.
    -   The default value is `.*`, which indicates the Alibaba Cloud accounts that are configured in the Log Audit Service application.
-   SLB Instance Name: The name of the SLB instance that you want to monitor. Regular expressions are supported. The default value is `.*`, which indicates the SLB instances that are attached to your Alibaba Cloud accounts. |
|**External Configurations**|None|
|**Solution**|Check whether exceptions have occurred on the SLB instances that have an abnormal length in a large number of requests.|
|**Prerequisites**|The **Lay-7 Access** switch next to SLB is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## SLB Average Response Delay Too High-8 Alert

|**ID**|sls\_app\_audit\_dataflow\_at\_slb\_latency|
|**Name**|SLB Average Response Delay Too High-8 Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Flow Security, SLB Flow Security|
|**Usage**|Checks whether the average response delay of Server Load Balancer \(SLB\) instances is too high. If the average response time of SLB instances is greater than or equal to the value of the Threshold parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|-   Alarm Name: The name of the alert. By default, the value of this parameter is SLB Average Response Delay Too High-8 Alert. You can specify a unique name for each alert based on the metrics that you want to monitor.
-   Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8
-   Threshold: The maximum average response delay of the SLB instance during the 2 minute window. Default value: 0.5. Unit: seconds.
-   Account ID \(Aliuid\): The ID of the Alibaba Cloud account related to the API gateway that you want to monitor. Regular expressions are supported.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use regular expressions `.*` in the IDs. For example, 156133.\* indicates the Alibaba Cloud accounts that start with 156133.
    -   The default value is `.*`, which indicates the Alibaba Cloud accounts that are configured in the Log Audit Service application.
-   SLB Instance Name: The name of the SLB instance that you want to monitor. Regular expressions are supported. The default value is `.*`, which indicates the SLB instances that are attached to your Alibaba Cloud accounts. |
|**External Configurations**|None|
|**Solution**|Check whether exceptions have occurred on SLB instances whose average response delay is too high.|
|**Prerequisites**|The **Lay-7 Access** switch next to SLB is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## SLB HTTP Access Protocol Enabled Alert

|**ID**|sls\_app\_audit\_dataflow\_at\_slb\_http|
|**Name**|SLB HTTP Access Protocol Enabled Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Flow Security, SLB Flow Security|
|**Usage**|Detects whether the Server Load Balancer \(SLB\) accesses the server through HTTPS protocol. When the SLB accesses the server through HTTP protocol, an alert will be triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8|
|**External Configurations**|You can configure a whitelist of SLB instances for which HTTP protocol is enabled. If HTTP protocol is enabled for SLB instances on the whitelist, no alert is be triggered.|
|**Solution**|Disable HTTP protocol for the SLB instances that are not included in the whitelist.|
|**Prerequisites**|The **Operations Log** switch next to ActionTrail is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## Load Balance Access UV Anomaly Inspection

|**ID**|sls\_app\_audit\_dataflow\_at\_slb\_uv\_detc|
|**Name**|Load Balance Access UV Anomaly Inspection|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Flow Security, SLB Flow Security|
|**Usage**|Detects the anomaly of Unique Visitors \(UVs\) of Server Load Balancers \(SLB\). If the number of UVs of abnormal access to SLB instances is greater than or equal to the value of the Threshold parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 4 hours.|
|**Time Range**|The data of the last 4 hours is checked.|
|**Parameter Settings**|-   Alarm Name: The name of the alert. By default, the value of this parameter is Load Balance Access UV Anomaly Inspection. You can specify a unique name for each alert based on the metrics that you want to monitor.
-   Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8
-   Threshold: The maximum number of UVs of abnormal access during the 4 hour window. One UV value is calculated per minute. Default value: 10.
-   Account ID \(Aliuid\): The ID of the Alibaba Cloud account related to the API gateway that you want to monitor. Regular expressions are supported.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use regular expressions `.*` in the IDs. For example, 156133.\* indicates the Alibaba Cloud accounts that start with 156133.
    -   The default value is `.*`, which indicates the Alibaba Cloud accounts that are configured in the Log Audit Service application.
-   SLB Instance Name: The name of the SLB instance that you want to monitor. Regular expressions are supported. The default value is `.*`, which indicates the SLB instances that are attached to your Alibaba Cloud accounts. |
|**External Configurations**|None|
|**Solution**|Check whether exceptions have occurred on the SLB instances whose UVs of abnormal access are in a large number.|
|**Prerequisites**|The **Lay-7 Access** switch next to SLB is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## Load Balance Access PV Anomaly Inspection

|**ID**|sls\_app\_audit\_dataflow\_at\_slb\_pv\_detc|
|**Alert Name**|Load Balance Access PV Anomaly Inspection|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Flow Security, SLB Flow Security|
|**Usage**|Detects excessive number of page views \(PVs\) of Server Load Balancer \(SLB\) instances. If the number of PVs of abnormal access to SLB instances is greater than or equal to the value of the Threshold parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 4 hours.|
|**TimeRange**|The data of the last 4 hours is checked.|
|**Parameter Settings**|-   Alarm Name: The name of the alert. By default, the value of this parameter is Server Load Balancer access UV anomaly detection. You can specify a unique name for each alert based on the metrics that you want to monitor.
-   Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8
-   Threshold: The maximum number of PVs of abnormal access during the 4 hour window. One PV value is calculated per minute. Default value: 10.
-   Account ID \(Aliuid\): The ID of the Alibaba Cloud account related to the API gateway that you want to monitor. Regular expressions are supported.
    -   You can separate multiple IDs with vertical bars \(\|\). You can use regular expressions `.*` in the IDs. For example, 156133.\* indicates the Alibaba Cloud accounts that start with 156133.
    -   The default value is `.*`, which indicates the Alibaba Cloud accounts that are configured in the Log Audit Service application.
-   SLB Instance Name: The name of the SLB instance that you want to monitor. Regular expressions are supported. The default value is `.*`, which indicates the SLB instances that are attached to your Alibaba Cloud accounts. |
|**External Configurations**|None|
|**Solution**|Check whether exceptions have occurred on the SLB instances whose PVs of abnormal access are in a large number.|
|**Prerequisites**|The **Lay-7 Access** switch next to SLB is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

