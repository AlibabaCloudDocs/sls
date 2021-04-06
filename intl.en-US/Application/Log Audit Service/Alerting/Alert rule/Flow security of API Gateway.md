# Flow security of API Gateway

This topic describes the alert rules for traffic security of API Gateway. You can configure and enable alert rules in the Log Service console. This allows you to monitor the flow security of API Gateway. If an alert is triggered, you can identify the cause and fix the error at the earliest opportunity.

## Alert rules

The following alert rules are supported. For information about how to set alert parameters, configure whitelists, and perform other related operations, see [Manage alert rules]().

-   [APIgateway Server Average Delay Too High-8 Alert](#section_nni_z6f_mcc)
-   [APIGateway Backend Server Error Rate Too High-8 Alert](#section_e8r_vhx_16g)
-   [APIgateway Request Success Rate Too Low Alert](#section_uvt_kqd_hqz)

## APIgateway Server Average Delay Too High-8 Alert

|**ID**|sls\_app\_audit\_dataflow\_at\_api\_latency|
|**Name**|APIgateway Server Average Delay Too High-8 Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Flow Security, APIGateway Flow Security|
|**Usage**|Monitors the average server-side delay of API requests in API Gateway. If the average server-side delay of API requests is greater than or equal to the value of the Threshold parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|-   Alert Name: The name of the alert. By default, the value of this parameter is APIgateway Server Average Delay Too High-8 Alert. You can specify a unique name for each alert based on the metrics that you want to monitor.
-   Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8
-   Threshold: The maximum average server-side delay of API requests during the 2 minute window. Default value: 100. Unit: milliseconds.
-   Account ID \(Aliuid\): The ID of the Alibaba Cloud account related to the API gateway that you want to monitor. Regular expressions are supported.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use regular expressions `.*` in the IDs. For example, 156133.\* indicates the Alibaba Cloud accounts that start with 156133.
    -   The default value is`.*`. This is used to identify the Alibaba Cloud accounts that are configured in the Log Audit Service application.
-   API Name: The name of the API to be monitored. Regular expressions are supported. The default value is `.*`. This is used to identify all APIs. |
|**External Configurations**|None|
|**Solution**|Check whether exceptions have occurred on the API requests whose average server-side delay is too high.|
|**Prerequisites**|The **Access Log** switch next to API Gateway instance is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## APIGateway Backend Server Error Rate Too High-8 Alert

|**ID**|sls\_app\_audit\_dataflow\_at\_api\_err\_rate|
|**Name**|APIGateway Backend Server Error Rate Too High-8 Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Flow Security, APIGateway Flow Security|
|**Usage**|Monitors the error rate of API requests at the backend server in API Gateway. If the error rate of API requests at the backend server in API Gateway is greater than or equal to the value of the Threshold parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|-   Alert Name: The name of the alert. By default, the value of this parameter is APIGateway Backend Server Error Rate Too High-8 Alert. You can specify a unique name for each alert based on the metrics that you want to monitor.
-   Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8
-   Threshold: The maximum error rate of API requests at the backend during the 2 minute window. Default value: 0.
-   Account ID \(Aliuid\): The ID of the Alibaba Cloud account related to the API gateway that you want to monitor. Regular expressions are supported.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use regular expressions `.*` in the IDs. For example, 156133.\* indicates the Alibaba Cloud accounts that start with 156133.
    -   The default value is `.*`. This is used to identify all the Alibaba Cloud accounts that are configured in the Log Audit Service application.
-   API name: The name of the API to be monitored. Regular expressions are supported. The default value is `.*`. This is used to identify all all APIs. |
|**External Configurations**|None|
|**Solution**|Check whether exceptions have occurred on the APIs whose error rates at the server end are too high.|
|**Prerequisites**|The **Access Log** switch next to API Gateway instance is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## APIgateway Request Success Rate Too Low Alert

|**ID**|sls\_app\_audit\_dataflow\_at\_api\_req\_rate|
|**Name**|APIgateway Request Success Rate Too Low Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Flow Security, APIGateway Flow Security|
|**Usage**|Monitors the request success rate of API requests in an API gateway. If the success rate of API requests in an API gateway is lower than the value of the Threshold parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|-   Alert Name: The name of the alert. By default, the value of this parameter is APIgateway Request Success Rate Too Low Alert. You can specify a unique name for each alert based on the metrics that you want to monitor.
-   Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8
-   Threshold: The minimum success rate of API requests in an API gateway. Default value: 95%.
-   Account ID \(Aliuid\): The ID of the Alibaba Cloud account related to the API gateway that you want to monitor. Regular expressions are supported.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use regular expressions `.*` in the IDs. For example, 156133.\* indicates that the Alibaba Cloud accounts that start with 156133.
    -   The default value is `.*`. This is used to identify the Alibaba Cloud accounts that are configured in the Log Audit Service application.
-   API Name: The name of the API to be monitored. Regular expressions are supported. The default value is `.*`. This is used to identify all APIs. |
|**External Configurations**|None|
|**Solution**|Check whether exceptions have occurred on the APIs whose success rates of requests are too low.|
|**Prerequisites**|The **Access Log** switch next to API Gateway instance is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

