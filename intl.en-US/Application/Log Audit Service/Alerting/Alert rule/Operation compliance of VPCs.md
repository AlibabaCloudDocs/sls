# Operation compliance of VPCs

This topic describes the alert rules for the operation compliance of a virtual private cloud \(VPC\). You can configure and enable alert rules in the Log Service console to monitor the operation compliance of VPCs. If an alert is triggered, you can identify the error cause and fix the error at the earliest opportunity.

## Alert rules

The following alert rules are supported. For information about how to set alert parameters, configure whitelists, and perform other relevant operations, see [Manage alert rules]().

-   [VPC Network Routing Change Alert](#section_yev_fi1_d93)
-   [VPC Flow Log Abnormally Configured Alert](#section_box_x9p_bme)
-   [VPC Configuration Change Alert](#section_6dq_wea_uz4)

## VPC Network Routing Change Alert

|**ID**|sls\_app\_audit\_cis\_at\_vpc\_route\_change|
|**Name**|VPC Network Routing Change Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, and VPC Operation Compliance|
|**Usage**|Monitors the routing configurations of a VPC. If the configurations are changed, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: Low-4.|
|**External Configurations**|You can specify a whitelist of accounts that can change the configurations of a VPC. If the configurations of a VPC are changed by an account on the whitelist, no alert is triggered.|
|**Solution**|Do not change the configurations of a VPC by using an account that is not included in the whitelist.|
|**Prerequisites**|The **Operations Log** switch of ActionTrail is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## VPC Flow Log Abnormally Configured Alert

|**ID**|sls\_app\_audit\_cis\_at\_vpc\_flowlog\_detection|
|**Name**|VPC Flow Log Abnormally Configured Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, and VPC Operation Compliance|
|**Usage**|Monitors VPC flow logs. We recommend that you enable the flow log feature of a VPC. If the flow log feature is disabled or deleted, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8.|
|**External Configurations**|You can specify a whitelist of accounts that can disable the flow log feature of a VPC. If the flow log feature of a VPC is disabled by an account on the whitelist, no alert is triggered.|
|**Solution**|Do not disable or delete the flow log feature of a VPC by using an account that is not included in the whitelist.|
|**Prerequisites**|The **Operations Log** switch of ActionTrail is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## VPC Configuration Change Alert

|**ID**|sls\_app\_audit\_cis\_at\_vpc\_conf\_change|
|**Name**|VPC Configuration Change Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, and VPC Operation Compliance|
|**Usage**|Monitors whether the configurations of a VPC are changed. If the configurations of a VPC are changed, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: Low-4.|
|**External Configurations**|You can specify a whitelist of accounts that can change the configurations of a VPC. If the configurations of a VPC are changed by an account on the whitelist, no alert is triggered.|
|**Solution**|Do not change the configurations of a VPC by using an account that is not included in the whitelist.|
|**Prerequisites**|The **Operations Log** switch of ActionTrail is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

