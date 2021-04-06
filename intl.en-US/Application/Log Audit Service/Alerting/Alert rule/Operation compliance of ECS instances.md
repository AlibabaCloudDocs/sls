# Operation compliance of ECS instances

This topic describes the alert rules for the operation compliance of ECS instances. The alert rules are applicable to monitor the encryption status of ECS disks, the automatic snapshot policies of ECS instances, and the configurations of ECS security groups. You can configure and enable alert rules in the Log Service console to monitor the operation compliance of ECS instances. If an alert is triggered, you can identify the error cause and fix the error at the earliest opportunity.

## Alert rules

The following alert rules are supported. For information about how to set alert parameters, configure whitelists, and perform other relevant operations, see [Manage alert rules]().

-   [ECS Disk Encryption Shutdown Alert](#section_y7t_61v_73x)
-   [ECS Automatic Snapshot Strategy Shutdown Alert](#section_hu1_6l7_jka)
-   [Security Group Configuration Change Alert](#section_j0n_rif_o0w)
-   [ECS Network Type Check](#section_7vd_x68_q87)

## ECS Disk Encryption Shutdown Alert

|**ID**|sls\_app\_audit\_cis\_at\_ecs\_disk\_encry\_detection|
|**Name**|ECS Disk Encryption Shutdown Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, and ECS Operation Compliance|
|**Usage**|Monitors the encryption status of ECS disks. ECS disks are encrypted on the server side. If the encryption is disabled, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8.|
|**External Configurations**|You can specify a whitelist of accounts that can disable the encryption feature of an ECS disk. If the encryption feature of an ECS disk is disabled by an account on the whitelist, no alert is triggered.|
|**Solution**|Do not disable the encryption feature of an ECS disk by using an account that is not included in the whitelist.|
|**Prerequisites**|The **Operations Log** switch of ActionTrail is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## ECS Automatic Snapshot Strategy Shutdown Alert

|**ID**|sls\_app\_audit\_cis\_at\_ecs\_auto\_snapshot\_policy|
|**Name**|ECS Automatic Snapshot Strategy Shutdown Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, and ECS Operation Compliance|
|**Usage**|Monitors if the automatic snapshot policies of ECS instances are disabled. To back up data for a disk, we recommend that you use automatic snapshot policies. If the automatic snapshot policies of ECS instances are disabled, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8.|
|**External Configurations**|You can specify a whitelist of accounts that can disable the automatic snapshot policy of a disk. If the automatic snapshot policy is disabled by an account on the whitelist, no alert is triggered.|
|**Solution**|Do not disable the automatic snapshot policy of a disk by using an account that is not included in the whitelist.|
|**Prerequisites**|The **Operations Log** switch of ActionTrail is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## Security Group Configuration Change Alert

|**ID**|sls\_app\_audit\_cis\_at\_securitygroup\_change|
|**Name**|Security Group Configuration Change Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, and ECS Operation Compliance|
|**Usage**|Monitors if the configurations of ECS security groups are changed. If the configurations of ECS security groups are changed, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8.|
|**External Configurations**|You can specify a whitelist of accounts that can change the configurations of ECS security groups. If the configurations of security groups are changed by an account on the whitelist, no alert is triggered.|
|**Solution**|Do not change the configurations of security groups by using an account that is not included in the whitelist.|
|**Prerequisites**|The **Operations Log** switch of ActionTrail is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## ECS Network Type Check

|**ID**|sls\_app\_audit\_cis\_at\_ecs\_network\_type|
|**Name**|ECS Network Type Check|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, and ECS Operation Compliance|
|**Usage**|Monitors the network type of ECS instances We recommend that you create ECS instances over a virtual private cloud \(VPC\). If you create an ECS instance over a classic network, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: Medium-6.|
|**External Configurations**|You can specify a whitelist of accounts that can create an ECS instance over a classic network. If an ECS instance is created over a classic network by an account on the whitelist, no alert is triggered.|
|**Solution**|Do not create an ECS instance over a classic network by using an account that is not included in the whitelist.|
|**Prerequisites**|The **Operations Log** switch of ActionTrail is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

