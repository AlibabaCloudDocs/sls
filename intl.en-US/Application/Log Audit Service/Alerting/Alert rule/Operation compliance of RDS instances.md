# Operation compliance of RDS instances

This topic describes the alert rules for the operation compliance of RDS instances. You can configure and enable alert rules in the Log Service console to monitor the operation compliance of RDS instances. If an alert is triggered, you can identify the error cause and fix the error at the earliest opportunity.

## Alert rules

The following alert rules are supported. For information about how to set alert parameters, configure whitelists, and perform other relevant operations, see [Manage alert rules]().

-   [RDS Instance SQL Insight Disabled Alert](#section_73j_8io_kq4)
-   [RDS Instance Access Whitelist Abnormal Setting Alert](#section_iq5_q2p_tbv)
-   [Newly Created RDS Instance's SSL Not Enabled AlertNot CreatedEnable Settings](#section_6rz_9ae_vgm)
-   [Newly Created RDS Instance's TDE Not Enabled Alert](#section_xna_e9n_gcl)
-   [RDS Instance SSL Disabled Alert](#section_gpw_yin_o2t)
-   [RDS Instance Configuration Change Alert](#section_0qw_4rr_ep4)

## RDS Instance SQL Insight Disabled Alert

|**ID**|sls\_app\_audit\_cis\_at\_rds\_sql\_audit|
|**Name**|RDS Instance SQL Insight Disabled Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, and RDS Operation Compliance|
|**Usage**|Monitors whether the SQL Explorer feature is disabled for an RDS instance. The SQL Explorer feature must be enabled for RDS instances. Otherwise, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8.|
|**External Configurations**|You can specify a whitelist of accounts that can disable the SQL Explorer feature for RDS instances. If the SQL Explorer feature is disabled by an account on the whitelist, no alert is triggered.|
|**Solution**|Do not disable the SQL Explorer feature for an RDS instance by using an account that is not included in the whitelist.|
|**Prerequisites**|The **Operations Log** switch of ActionTrail is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## RDS Instance Access Whitelist Abnormal Setting Alert

|**ID**|sls\_app\_audit\_cis\_at\_rds\_access\_whitelist|
|**Name**|RDS Instance Access Whitelist Abnormal Setting Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, and RDS Operation Compliance|
|**Usage**|Monitors whether the whitelist of IP addresses to access RDS instances is invalid. The IP address on the whitelist to access an RDS instance cannot be set to 0.0.0.0. Otherwise, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8.|
|**External Configurations**|You can specify a whitelist of accounts. If an RDS instance belongs to an account on the whitelist and the whitelist of IP addresses to access the instance is set to 0.0.0.0, no alert is triggered.|
|**Solution**|Allow only the RDS instance that belongs to an account on the whitelist to set the whitelist IP address to 0.0.0.0|
|**Prerequisites**|The **Operations Log** switch is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## Newly Created RDS Instance's SSL Not Enabled AlertNot CreatedEnable Settings

|**ID**|sls\_app\_audit\_cis\_at\_rds\_ssl\_off|
|**Name**|Newly Created RDS Instance's SSL Not Enabled AlertNot CreatedEnable Settings|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, and RDS Operation Compliance|
|**Usage**|Monitors whether the SSL feature is disabled for newly created RDS instances. We recommend that you enable the SSL feature within 1 hour after you create an RDS instance. Otherwise, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last hour is checked.|
|**Parameter Settings**|Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8.|
|**External Configurations**|You can specify a whitelist of accounts. If an RDS instance belongs to an account on the whitelist and the SSL feature is not enabled for the instance, no alert is triggered.|
|**Solution**|If an RDS instance does not belong to an account in the whitelist, we recommend that you enable the SSL feature within 1 hour after you create the instance.|
|**Prerequisites**|The **Operations Log** switch of ActionTrail is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## Newly Created RDS Instance's TDE Not Enabled Alert

|**ID**|sls\_app\_audit\_cis\_at\_rds\_tde\_off|
|**Name**|Newly Created RDS Instance's TDE Not Enabled Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, and RDS Operation Compliance|
|**Usage**|Monitor whether TDE is disabled for a newly created RDS instance. We recommend that you enable TDE within 1 hour after you create an RDS instance. Otherwise, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last hour is checked.|
|**Parameter Settings**|Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: Medium-6.|
|**External Configurations**|You can specify a whitelist of accounts. If an RDS instance belongs to an account on the whitelist and TDE is not enabled for the instance, no alert is triggered.|
|**Solution**|If an RDS instance does not belong to an account on the whitelist, we recommend that you enable TDE within 1 hour after you create the RDS instance.|
|**Prerequisites**|The **Operations Log** switch of ActionTrail is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## RDS Instance SSL Disabled Alert

|**ID**|sls\_app\_audit\_cis\_at\_rds\_ssl\_config|
|**Name**|RDS Instance SSL Disabled Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, and RDS Operation Compliance|
|**Usage**|Monitors if the SSL feature is disabled for RDS instances. We recommend that you do not disable the SSL feature for RDS instances. Otherwise, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8.|
|**External Configurations**|You can specify a whitelist of accounts. If an RDS instance belongs to an account on the whitelist and the SSL feature is disabled for the instance, no alert is triggered.|
|**Solution**|Do not disable the SSL feature for an RDS instance that is not included in the whitelist.|
|**Prerequisites**|The **Operations Log** switch of ActionTrail is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## RDS Instance Configuration Change Alert

|**ID**|sls\_app\_audit\_cis\_at\_rds\_conf\_change|
|**Name**|RDS Instance Configuration Change Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, and RDS Operation Compliance|
|**Usage**|Monitors whether the configurations of RDS instances are changed. If the configurations of an RDS instance are changed, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: Low-4.|
|**External Configurations**|You can specify a whitelist of accounts. If an RDS instance belongs to an account on the whitelist and the configurations of the instance are changed, no alert is triggered.|
|**Solution**|Check whether an exception occurs on the RDS instance that triggered the alert.|
|**Prerequisites**|The **Operations Log** switch of ActionTrail is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

