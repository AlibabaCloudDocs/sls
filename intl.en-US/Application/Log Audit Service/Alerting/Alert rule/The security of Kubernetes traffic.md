# The security of Kubernetes traffic

This topic describes the alert rules for the security of Kubernetes traffic. You can configure and enable alert rules in the Log Service console to monitor the security of Kubernetes traffic.

## Alert rules

The following alert rules are supported. For information about how to set alert parameters, configure whitelists, and perform other relevant operations, see [Manage alert rules]().

-   [K8s Ingress Response Delay Too High Alert](#section_qso_n32_mfu)
-   [K8s Ingress Request Success Rate Too Low Alert](#section_5t6_wbv_qnx)
-   [K8s Ingress Average Request Latency Too High Alert](#section_b75_9vm_l4s)
-   [Too Many K8s Illegal Access Alert](#section_0f8_so7_nzg)

## K8s Ingress Response Delay Too High Alert

|**ID**|sls\_app\_audit\_dataflow\_at\_ingress\_resp|
|**Name**|K8s Ingress Response Delay Too High Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Data Security, and K8s Flow Security|
|**Usage**|Monitors the average backend response latency of Kubernetes Ingress. If the latency is higher than the specified threshold, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|You can specify the following parameters:-   **Alert Name**: The name of the alert. You can create multiple alerts.
-   **Severity**: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2.
-   **Threshold**: The threshold for the average backend response latency of Kubernetes Ingress. Default value: 500. Units: milliseconds. If the average latency exceeds the threshold within 2 minutes, an alert is triggered.
-   **Account ID \(Aliuid\)**: The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   **K8s Cluster Name**: The name of the Kubernetes cluster to be monitored. You can use regular expressions when you specify this parameter.
    -   You can also use wildcards for the regular expressions, such as `.*`.
    -   Default value: `.*`. This indicates all Kubernetes clusters of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs in the Kubernetes cluster that triggered the alert.|
|**Prerequisites**|The **Ingress Log** switch of Kubernetes is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## K8s Ingress Average Request Latency Too High Alert

|**ID**|sls\_app\_audit\_dataflow\_at\_ingress\_latency|
|**Name**|K8s Ingress Average Request Latency Too High Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Data Security, and K8s Flow Security|
|**Usage**|Monitors the average request latency of Kubernetes Ingress. If the latency is higher than the specified threshold, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|You can specify the following parameters:-   **Alert Name**: The name of the alert. You can create multiple alerts.
-   **Severity**: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2.
-   **Threshold**: The threshold for the average request latency of Kubernetes Ingress. Default value: 200. Units: milliseconds. If the average latency exceeds the threshold within 2 minutes, an alert is triggered.
-   **Account ID \(Aliuid\)**: The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   **K8s Cluster Name**: The name of the Kubernetes cluster to be monitored. You can use regular expressions when you specify this parameter.
    -   You can also use wildcards for the regular expressions, such as `.*`.
    -   Default value: `.*`. This indicates all Kubernetes clusters of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs in the Kubernetes cluster that triggered the alert.|
|**Prerequisites**|The **Ingress Log** switch of Kubernetes is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## K8s Ingress Request Success Rate Too Low Alert

|**ID**|sls\_app\_audit\_dataflow\_at\_ingress\_rate|
|**Name**|K8s Ingress Request Success Rate Too Low Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Data Security, and K8s Flow Security|
|**Usage**|Monitors the request success rate of Kubernetes Ingress. If the rate is lower than the specified threshold, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|You can specify the following parameters:-   **Alert Name**: The name of the alert. You can create multiple alerts.
-   **Severity**: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2.
-   **Threshold**: The threshold for the request success rate of Kubernetes Ingress. Default value: 90%. If the request success rate is lower than the threshold within 2 minutes, an alert is triggered.
-   **Account ID \(Aliuid\)**: The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   **K8s Cluster Name**: The name of the Kubernetes cluster to be monitored. You can use regular expressions when you specify this parameter.
    -   You can also use wildcards for the regular expressions, such as `.*`.
    -   Default value: `.*`. This indicates all Kubernetes clusters of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs in the Kubernetes cluster that triggered the alert.|
|**Prerequisites**|The **Ingress Log** switch of Kubernetes is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## Too Many K8s Illegal Access Alert

|**ID**|sls\_app\_audit\_dataflow\_at\_k8s\_visit|
|**Name**|Too Many K8s Illegal Access Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Data Security, and K8s Flow Security|
|**Usage**|Monitors the access to Kubernetes clusters. If the number of invalid access to a Kubernetes cluster exceeds the specified threshold, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameters Settings**|You can specify the following parameters:-   **Alert Name**: The name of the alert. You can create multiple alerts.
-   **Severity**: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2.
-   **Threshold**: The threshold for the number of invalid access to a Kubernetes cluster. Default value: 3. If the number of invalid access to a Kubernetes cluster exceeds the threshold within 2 minutes, an alert is triggered.
-   **Account ID \(Aliuid\)**: The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   **K8s Cluster Name**: The name of the Kubernetes cluster to be monitored. You can use regular expressions when you specify this parameter.
    -   You can also use wildcards for the regular expressions, such as `.*`.
    -   Default value: `.*`. This indicates all Kubernetes clusters of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs in the Kubernetes cluster that triggered the alert.|
|**Prerequisites**|The **Ingress Log** switch of Kubernetes is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

