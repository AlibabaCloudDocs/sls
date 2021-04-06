# Permission control

This topic describes the alert rules for permission control. These alert rules include the alert rules that you can use to monitor the changes of RAM policies, unexpected attachments of RAM policies, and changes of OSS bucket permissions. You can configure and enable alert rules in the Log Service console. This allows you to monitor permission control issues. If an alert is triggered, you can identify the cause and fix the error at the earliest opportunity.

## Alert rules

The following alert rules are supported. For information about how to set alert parameters, configure whitelists, and perform other related operations, see [Manage alert rules]().

-   [OSS Bucket Authority Change Alert](#section_xbs_pon_i9x)
-   [RAM Policy Change Alert](#section_d2y_j1v_mrr)
-   [RAM Policy Abnormal Attach Alert](#section_axf_01e_fei)

## OSS Bucket Authority Change Alert

|**ID**|sls\_app\_audit\_cis\_at\_oss\_policy\_change|
|**Name**|OSS Bucket Authority Change Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Permission Control|
|**Usage**|Monitors the change of OSS Bucket permission. Changes of OSS Bucket permission will trigger an alert.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8|
|**External Configurations**|You can configure a whitelist of RAM users who can change the permissions of OSS buckets. If the RAM users on the whitelist change the permissions of OSS bucket, no alert is triggered.|
|**Solution**|Use only the RAM users who are included in the whitelist to change the permissions of OSS buckets.|
|**Prerequisites**|The **Operations Log** switch next to Action Trail is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## RAM Policy Change Alert

|**ID**|sls\_app\_audit\_cis\_at\_ram\_policy\_change|
|**Name**|RAM Policy Change Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Permission Control|
|**Usage**|Monitors the changes of RAM policy. If a RAM policy is changed, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: Medium-6|
|**External Configurations**|You can configure a whitelist of RAM users who can change RAM policies. If the RAM users on the whitelist change RAM policies, no alert is triggered.|
|**Solution**|Disable the change of RAM policy for RAM users that are not included in the whitelist.|
|**Prerequisites**|The **Operations Log** switch next to Action Trail is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## RAM Policy Abnormal Attach Alert

|**ID**|sls\_app\_audit\_cis\_at\_ram\_policy\_attach|
|**Name**|RAM Policy Abnormal Attach Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Permission Control|
|**Usage**|Monitors whether RAM policies are unexpectedly attached to RAM users. You can attach RAM policies only to RAM user groups or RAM roles. If you attach RAM policies to RAM users, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: Medium-6|
|**External Configurations**|You can configure a whitelist of RAM users to whom RAM policies can be attached. RAM policies can be attached to RAM users on the whitelist without triggering an alert.|
|**Solution**|Attach RAM policies to user groups or roles instead of users.|
|**Prerequisites**|The **Operations Log** switch next to Action Trail is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

