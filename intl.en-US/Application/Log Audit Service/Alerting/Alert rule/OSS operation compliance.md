# OSS operation compliance

This topic describes alert rules for OSS operation compliance. The alert rules include OSS Bucket Encryption Shutdown and OSS Newly Created Bucket Encryption Not Enabled. You can configure and enable alert rules in the Log Service console. This allows you to monitor the issues of OSS operational compliance. If an alert is triggered, you can identify the cause and fix the error at the earliest opportunity.

## Alert rules

The following alert rules are supported. For information about how to set alert parameters, configure whitelists, and perform other related operations, see [Manage alert rules]().

-   [OSS Bucket Encryption Shutdown Alert](#section_aqc_gut_own)
-   [OSS Newly Created Bucket Encryption Not Enabled Alert](#section_0l1_7uj_lda)
-   [OSS Bucket Logging Shutdown Alert](#section_13n_df6_hq9)
-   [OSS Newly Created Bucket Logging Not Enabled Alert](#section_fa3_u4h_y8i)

## OSS Bucket Encryption Shutdown Alert

|**ID**|sls\_app\_audit\_cis\_at\_oss\_encry\_config|
|**Name**|OSS Bucket Encryption Shutdown Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, OSS Operation Compliance|
|**Usage**|Monitors the encryption of OSS Bucket is disabled. All OSS buckets must be encrypted on the server side, and you are not recommended shutting down the encryption feature. If you disable the encryption, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8|
|**External Configurations**|A whitelist of RAM users who can shut down the encryption for OSS buckets. RAM users on the whitelist can shut down encryption of OSS buckets without triggering an alert.|
|**Solution**|Enable the encryption of OSS buckets for accounts that are not included in the whitelist.|
|**Prerequisites**|The **Access Log** switch next to OSS is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## OSS Newly Created Bucket Encryption Not Enabled Alert

|**ID**|sls\_app\_audit\_cis\_at\_oss\_bucket\_encry\_off|
|**Name**|OSS Newly Created Bucket Encryption Not Enabled Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, OSS Operation Compliance|
|**Usage**|Monitors whether the encryption of newly created OSS buckets is disabled. You must enable encryption when OSS Bucket is created or within one hour after it is created. Otherwise, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 1 hour is checked.|
|**Parameter Settings**|Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8|
|**External Configurations**|You can configure a whitelist of RAM users can keep the encryption of newly created OSS buckets disabled. RAM on the whitelist users can keep the encryption of newly created OSS buckets disabled.|
|**Solution**|You can turn on the encryption switch when the OSS bucket is created or enable it soon, within one hour after the creation.|
|**Prerequisites**|The **Access Log** switch next to OSS is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## OSS Bucket Logging Shutdown Alert

|**ID**|sls\_app\_audit\_cis\_at\_oss\_log\_config|
|**Name**|OSS Bucket Logging Shutdown Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, OSS Operation Compliance|
|**Usage**|Monitors the access logs of OSS Bucket are disabled. You are recommended to enable all access logs of OSS Bucket. If you disable access logs of OSS bucket, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: Medium-6|
|**External Configurations**|You can configure a whitelist of RAM users who can shut down access logs of OSS buckets. Users can shut down access logs of OSS buckets in RAM users on the whitelist without triggering an alert.|
|**Solution**|Enable the access logs of OSS buckets for RAM users that are not included in the whitelist.|
|**Prerequisites**|The **Access Log** switch next to OSS is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## OSS Newly Created Bucket Logging Not Enabled Alert

|**ID**|sls\_app\_audit\_cis\_at\_oss\_log\_off|
|**Name**|OSS Newly Created Bucket Logging Not Enabled Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, OSS Operation Compliance|
|**Usage**|Monitors whether the access logs of OSS Bucket are disabled. You must enable the access logs of OSS Bucket as soon as it is created. If you do not enable access logs for OSS Bucket within one hour after it is created, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 1 hour is checked.|
|**Parameter Settings**|Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: Medium-6|
|**External Configurations**|You can configure a whitelist of RAM users who can keep the access logs of newly created OSS buckets disabled. RAM users on the whitelist can keep the access logs of newly created OSS buckets.|
|**Solution**|Turn on the access log switch within one hour after the OSS bucket is created.|
|**Prerequisites**|The **Access Log** switch next to OSS is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

