# Security of RDS instances

This topic describes the alert rules for the security of RDS instances. You can configure and enable alert rules in the Log Service console to monitor the security of RDS instances. If an alert is triggered, you can identify the error cause and fix the error at the earliest opportunity.

## Alert rules

The following alert rules are supported. For information about how to set alert parameters, configure whitelists, and perform other relevant operations, see [Manage alert rules]().

-   [RDS Slow SQL detection](#section_j3n_8le_9ze)
-   [RDS Data Mass Deletion Alert](#section_dpl_pgt_f9q)
-   [Detection of RDS Visit through Internet](#section_qbg_bpr_mw1)
-   [RDS Query SQL Average Execution Time Monitoring](#section_r6k_k0m_s6t)
-   [RDS Instance Update Peak Monitoring](#section_g7s_gyy_cjq)
-   [RDS Instance Query Peak Monitoring](#section_wn6_l4j_bar)
-   [RDS Instance Released Alert](#section_924_ayp_1qs)
-   [RDS Frequent Visit IP Detection](#section_e6x_s3n_vvu)
-   [RDS Update SQL Average Execution Time Monitoring](#section_jwx_h6s_9fl)
-   [Too Many RDS Login Failures Alert](#section_0j0_ema_u8m)
-   [Rds Mass Data Update Event Alert](#section_fm1_jd3_da6)
-   [RDS Dangerous SQL Execution Alert](#section_5ej_zg6_xno)
-   [Too Many RDS SQL Execution Errors Alert](#section_j7h_vch_u4y)

## RDS Slow SQL detection

|**ID**|sls\_app\_audit\_db\_at\_rds\_slow\_sql|
|**Name**|RDS Slow SQL detection|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Database Security, and RDS Security|
|**Usage**|Monitors slow SQL queries in RDS instances. If the time to execute an SQL query exceeds the value of the Threshold, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|-   Alert Name: The name of the alert. By default, the value of this parameter is RDS Slow SQL detection. You can specify a unique name for each alert based on the metrics that you want to monitor.
-   Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8.
-   Threshold: The threshold for the time of SQL queries. If the time of an SQL query exceeds the specified threshold, the query is a slow query. Default value: 5000. Unit: microseconds.
-   Account ID \(Aliuid\): The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   RDS Instance Name: The name of the RDS instance to be monitored. You can use regular expressions when you specify this parameter. Default value`.*`. This indicates that all RDS instances of the specified Alibaba Cloud account are monitored.
-   Database Name: The name of the database to be monitored. You can use regular expressions when you specify this parameter. Default value`.*`. This indicates that all databases of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether slow SQL queries occur in the RDS database that triggered the alert.|
|**Prerequisites**|The **SQL Audit Log** switch of RDS is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## RDS Data Mass Deletion Alert

|**ID**|sls\_app\_audit\_db\_at\_rds\_batch\_del\_sql|
|**Name**|RDS Data Mass Deletion Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Database Security, and RDS Security|
|**Usage**|Monitors whether a large amount of data is deleted in RDS databases. If the number of data rows that are deleted is greater than or equal to the value of the Threshold parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|-   Alert Name: The name of the alert. By default, the value of this parameter is RDS Data Mass Deletion Alert. You can specify a unique name for each alert based on the metrics that you want to monitor.
-   Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8.
-   Threshold: The threshold for the maximum number of data rows that can be deleted. Default value: 10.
-   Account ID \(Aliuid\): The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   RDS Name: The name of the RDS instance to be monitored. You can use regular expressions when you specify this parameter. Default value`.*`. This indicates that all RDS instances of the specified Alibaba Cloud account are monitored.
-   Database Name: The name of the database to be monitored. You can use regular expressions when you specify this parameter. Default value`.*`. This indicates that all databases of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether a large amount of data is deleted in the RDS database that triggered the alert.|
|**Prerequisites**|The **SQL Audit Log** switch of RDS is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## Detection of RDS Visit through Internet

|**ID**|sls\_app\_audit\_db\_at\_rds\_internet\_access|
|**Name**|Detection of RDS Visit through Internet|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Database Security, and RDS Security|
|**Usage**|Monitors whether RDS instances are accessed by external IP addresses. If an RDS instance is accessed by an external IP address, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8.|
|**External Configurations**|You can specify a whitelist. If an RDS instance is in the whitelist and the RDS instance is accessed by an external IP address, no alert is triggered.|
|**Solution**|Do not allow RDS instances that are not included in the whitelist to be accessed by external IP addresses.|
|**Prerequisites**|The **SQL Audit Log** switch of RDS is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## RDS Query SQL Average Execution Time Monitoring

|**ID**|sls\_app\_audit\_db\_at\_rds\_select\_speed|
|**Name**|RDS Query SQL Average Execution Time Monitoring|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Database Security, and RDS Security|
|**Usage**|Monitors the average execution duration of an SQL query in RDS instances. If the average execution duration of an SQL query is greater than or equal to the value of the Threshold parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|-   Alert Name: The name of the alert. By default, the value of this parameter is RDS Query SQL Average Execution Time Monitoring. You can specify a unique name for each alert based on the metrics that you want to monitor.
-   Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8.
-   Threshold: The maximum average duration in which an SQL query statement is executed. Default value: 0.005. Unit: seconds.
-   Account ID \(Aliuid\): The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   RDS Name: The name of the RDS instance to be monitored. You can use regular expressions when you specify this parameter. Default value: `.*`. This indicates that all RDS instances of the specified Alibaba Cloud account are monitored.
-   Database Name: The name of the database to be monitored. You can use regular expressions when you specify this parameter. Default value`.*`. This indicates that all databases of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs in the RDS database that triggered the alert.|
|**Prerequisites**|The **SQL Audit Log** switch of RDS is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## RDS Instance Update Peak Monitoring

|**ID**|sls\_app\_audit\_db\_at\_rds\_update\_peak|
|**Name**|RDS Instance Update Peak Monitoring|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Database Security, and RDS Security|
|**Usage**|Monitors the data change in an RDS database. If the amount of data that is changed is greater than or equal to the value of the Threshold parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|-   Alert Name: The name of the alert. By default, the value of this parameter is RDS Instance Update Peak Monitoring. You can specify a unique name for each alert based on the metrics that you want to monitor.
-   Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8.
-   Threshold: The threshold for the maximum data amount that can be changed in an RDS database. Default value: 100. Unit: Rows.
-   Account ID \(Aliuid\): The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   RDS Instance Name: The name of the RDS instance to be monitored. You can use regular expressions when you specify this parameter. Default value`.*`. This indicates that all RDS instances of the specified Alibaba Cloud account are monitored.
-   Database Name: The name of the database to be monitored. You can use regular expressions when you specify this parameter. Default value`.*`. This indicates that all databases of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs on the RDS instance that triggered the alert.|
|**Prerequisites**|The **SQL Audit Log** switch of RDS is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## RDS Instance Query Peak Monitoring

|**ID**|sls\_app\_audit\_db\_at\_rds\_query\_peak|
|**Name**|RDS Instance Query Peak Monitoring|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Database Security, and RDS Security|
|**Usage**|Monitors the maximum rows of data to query each time. If the data rows that are queried is greater than or equal to the value of the Threshold parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|-   Alert Name: The name of the alert. By default, the value of this parameter is RDS Instance Query Peak Monitoring. You can specify a unique name for each alert based on the metrics that you want to monitor.
-   Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8.
-   Threshold: The threshold for the maximum rows of data to query each time in an RDS database. Default value: 1000. Unit: Rows.
-   Account ID \(Aliuid\): The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   RDS Instance Name: The name of the RDS instance to be monitored. You can use regular expressions when you specify this parameter. Default value`.*`. This indicates that all RDS instances of the specified Alibaba Cloud account are monitored.
-   Database Name: The name of the database to be monitored. You can use regular expressions when you specify this parameter. Default value`.*`. This indicates that all databases of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs in the RDS database that triggered the alert.|
|**Prerequisites**|The **SQL Audit Log** switch of RDS is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## RDS Instance Released Alert

|**ID**|sls\_app\_audit\_db\_at\_rds\_query\_peak|
|**Name**|RDS Instance Released Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Database Security, and RDS Security|
|**Usage**|Monitors the release of RDS instances. If an RDS instance is released, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8.|
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs in the RDS database that triggered the alert.|
|**Prerequisites**|The **SQL Audit Log** switch of RDS is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## RDS Frequent Visit IP Detection

|**ID**|sls\_app\_audit\_db\_at\_rds\_visit|
|**Name**|RDS Frequent Visit IP Detection|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Database Security, and RDS Security|
|**Usage**|Monitors the frequent access from an IP address to an RDS instance. If the time of access from an IP address to an RDS instance is greater than or equal to the value of the Threshold parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|-   Alert Name: The name of the alert. By default, the value of this parameter is RDS Frequent Visit IP Detection. You can specify a unique name for each alert based on the metrics that you want to monitor.
-   Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8.
-   Threshold: The threshold for the maximum number of times that an IP address can access an RDS instance every 2 minutes. Default value: 30.
-   Account ID \(Aliuid\): The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   RDS Instance Name: The name of the RDS instance to be monitored. You can use regular expressions when you specify this parameter. Default value`.*`. This indicates that all RDS instances of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|You can specify a whitelist of IP addresses. If an RDS instance is frequently accessed by an IP address on the whitelist, no alert is triggered.|
|**Solution**|Check whether an exception occurs on the RDS instance that triggered the alert.|
|**Prerequisites**|The **SQL Audit Log** switch of RDS is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## RDS Update SQL Average Execution Time Monitoring

|**ID**|sls\_app\_audit\_db\_at\_rds\_update\_speed|
|**Name**|RDS Update SQL Average Execution Time Monitoring|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Database Security, and RDS Security|
|**Usage**|Monitors the time interval to change the average execution duration of an SQL query in RDS instances. If the time interval to change the average execution duration of an SQL query is greater than or equal to the value of the Threshold parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|-   Alert Name: The name of the alert. By default, the value of this parameter is RDS Update SQL Average Execution Time Monitoring. You can specify a unique name for each alert based on the metrics that you want to monitor.
-   Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8.
-   Threshold: The threshold for the maximum time interval to change the average execution duration of an SQL query. Default value: 0.005. Unit: Seconds.
-   Account ID \(Aliuid\): The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   RDS Instance Name: The name of the RDS instance to be monitored. You can use regular expressions when you specify this parameter. Default value`.*`. This indicates that all RDS instances of the specified Alibaba Cloud account are monitored.
-   Database Name: The name of the database to be monitored. You can use regular expressions when you specify this parameter. Default value`.*`. This indicates that all databases of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs on the RDS instance that triggered the alert.|
|**Prerequisites**|The **SQL Audit Log** switch of RDS is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## Too Many RDS Login Failures Alert

|**ID**|sls\_app\_audit\_db\_at\_rds\_login\_err\_cnt|
|**Name**|Too Many RDS Login Failures Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Database Security, and RDS Security|
|**Usage**|Monitors the logon failures of RDS instances. If the number of logon failures of an RDS instance within 5 minutes is greater than or equal to the value of the Threshold parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 4 minutes.|
|**Time Range**|The data of the last 5 minutes is checked.|
|**Parameter Settings**|-   Alert Name: The name of the alert. By default, the value of this parameter is Too Many RDS Login Failures Alert. You can specify a unique name for each alert based on the metrics that you want to monitor.
-   Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8.
-   Threshold: The threshold for the maximum number of logon failures for an RDS instance within 5 minutes. Default value: 3.
-   Account ID \(Aliuid\): The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   RDS Instance Name: The name of the RDS instance to be monitored. You can use regular expressions when you specify this parameter. Default value`.*`. This indicates that all RDS instances of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs on the RDS instance that triggered the alert.|
|**Prerequisites**|The **SQL Audit Log** switch of RDS is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## Rds Mass Data Update Event Alert

|**ID**|sls\_app\_audit\_db\_at\_rds\_batch\_update\_sql|
|**Name**|Rds Mass Data Update Event Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Database Security, and RDS Security|
|**Usage**|Monitors whether a large amount of data is changed on RDS instances. If the number of data rows changed on an RDS instance is greater than or equal to the value of the Threshold parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|-   Alert Name: The name of the alert. By default, the value of this parameter is Rds Mass Data Update Event Alert. You can separate multiple IDs with vertical bars \(\|\).
-   Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8.
-   Threshold: The threshold for the maximum number of data rows that can be changed. Default value: 10.
-   Account ID \(Aliuid\): The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   RDS Instance Name: The name of the RDS instance to be monitored. You can use regular expressions when you specify this parameter. Default value`.*`. This indicates that all RDS instances of the specified Alibaba Cloud account are monitored.
-   Database Name: The name of the database to be monitored. You can use regular expressions when you specify this parameter. Default value`.*`. This indicates that all databases of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs on the RDS instance that triggered the alert.|
|**Prerequisites**|The **SQL Audit Log** switch of RDS is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## RDS Dangerous SQL Execution Alert

|**ID**|sls\_app\_audit\_db\_at\_rds\_danger\_sql|
|**Name**|RDS Dangerous SQL Execution Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Database Security, and RDS Security|
|**Usage**|Monitors invalid SQL queries for RDS instances. If an invalid SQL query is detected, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|-   Alert Name: The name of the alert. By default, the value of this parameter is RDS Dangerous SQL Execution Alert. You can specify a unique name for each alert based on the metrics that you want to monitor.
-   Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8.
-   Account ID \(Aliuid\): The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   RDS Instance Name: The name of the RDS instance to be monitored. You can use regular expressions when you specify this parameter. Default value`.*`. This indicates that all RDS instances of the specified Alibaba Cloud account are monitored.
-   Database Name: The name of the database to be monitored. You can use regular expressions when you specify this parameter. Default value`.*`. This indicates that all databases of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs on the RDS instance that triggered the alert.|
|**Prerequisites**|The **SQL Audit Log** switch of RDS is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

## Too Many RDS SQL Execution Errors Alert

|**ID**|sls\_app\_audit\_db\_at\_rds\_sql\_err\_cnt|
|**Name**|Too Many RDS SQL Execution Errors Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, Database Security, and RDS Security|
|**Usage**|Monitors the errors that occur when SQL queries are executed. If the number of errors that occur is greater than or equal to the value of the Max errors parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|-   Alert Name: The name of the alert. By default, the value of this parameter is Too Many RDS SQL Execution Errors Alert. You can specify a unique name for each alert based on the metrics that you want to monitor.
-   Severity: The severity level of the alert. Valid values: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8.
-   Threshold: The threshold for the maximum number of errors that can occur within 2 minutes when SQL queries are executed for an RDS instance. Default value: 10.
-   Account ID \(Aliuid\): The ID of the Alibaba Cloud account that you want to monitor. You can use regular expressions when you specify this parameter.
    -   You can separate multiple IDs with vertical bars \(\|\). You can also use wildcards for the regular expressions, such as `.*`. For example, 156133.\* indicates that Alibaba Cloud accounts that start with 156133 are monitored.
    -   Default value: `.*`. This indicates all Alibaba Cloud accounts configured in the Log Audit Service application are monitored.
-   RDS Instance Name: The name of the RDS instance to be monitored. You can use regular expressions when you specify this parameter. Default value`.*`. This indicates that all RDS instances of the specified Alibaba Cloud account are monitored.
-   Database Name: The name of the database to be monitored. You can use regular expressions when you specify this parameter. Default value`.*`. This indicates that all databases of the specified Alibaba Cloud account are monitored. |
|**External Configurations**|None.|
|**Solution**|Check whether an exception occurs on the RDS instance that triggered the alert.|
|**Prerequisites**|The **SQL Audit Log** switch of RDS is turned on. To turn on the switch, go to the Log Audit Service console, and then choose **Log Audit Service** \> **Access to Cloud Products** \> **Global Configurations**.|

