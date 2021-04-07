# Account security

This topic describes the alert rules for account security. You can configure and enable alerts in the Log Service console. This allows you to monitor account security issues. If an alert is triggered, you can identify the cause and fix the error at the earliest opportunity.

## Alert rules

The following alert rules are supported. For information about how to set alert parameters, configure whitelists, and perform other related operations, see [Manage alert rules](/intl.en-US/Application/Log Audit Service/Alerting/Manage alert rules.md).

-   [RAM Sub-Account Login without MFA Alert](#section_z34_l6v_isj)
-   [RAM Password Expiration Policy Exception Alert](#section_wro_6cr_ztd)
-   [Root Account Login without MFA Alert](#section_ktk_gpb_7ks)
-   [RAM Password Login Retry Policy Exception Alert](#section_t5b_37t_x09)
-   [Root Account Frequent Login Alert](#section_q2b_zps_mjw)
-   [RAM History Password Check Policy Exception Alert](#section_ljz_9gn_hba)
-   [KMS Key Configuration Change Alert](#section_j9i_2x1_p6q)
-   [Account Continuous Login Failure Alert](#section_itu_52c_ssm)
-   [Root Account AK Usage Detection](#section_c3s_jf9_vk1)
-   [RAM Password Length Policy Exception Alert](#section_ucf_15q_jyq)

## RAM Sub-Account Login without MFA Alert

|**ID**|sls\_app\_audit\_cis\_at\_ram\_mfa|
|**Name**|RAM Sub-Account Login without MFA Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Account Security|
|**Usage**|Monitors whether multi-factor authentication \(MFA\) is disabled for a Resource Access Management \(RAM\) user who log to the console of an Alibaba Cloud service. When a RAM user logs onto the console, MFA must be enabled for the RAM user. In addition, the number of logons without MFA must be less than or equal to the specified Max logins parameter. Otherwise, an alert is triggered.|
|**Check Frequency**|Fixed interval: 4 minutes.|
|**Time Range**|The data of the last 5 minutes is checked.|
|**Parameter Settings**|-   Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: Medium-6
-   Max logins: The maximum number of logons allowed every five minutes for a RAM user whose MFA is disabled. Default value: 0. |
|**External Configurations**|You can configure a whitelist of RAM users who can log on to the consoles of Alibaba Cloud services without the need to enable MFA. If MFA is disabled for a RAM user on the whitelist when the RAM user logs on to the console of an Alibaba Cloud Service, no alert is triggered.|
|**Solution**|Make sure that the number of logons without MFA for of a RAM user within 5 minutes is less than or equal to the specified Max logins parameter.|
|**Prerequisites**|The **Operations Log** switch next to ActionTrail is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## RAM Password Expiration Policy Exception Alert

|**ID**|sls\_app\_audit\_cis\_at\_pwd\_expire\_policy|
|**Name**|RAM Password Expiration Policy Exception Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Account Security|
|**Usage**|Monitors whether the validity period specified in a RAM password policy is valid. In the RAM password policy, the validity period of a RAM password less than or equal to the specified Max validity period parameter in the alert rule. Otherwise, an alert is triggered.|
|**Check Frequency**|Fixed interval: 5 minutes.|
|**Time Range**|The data of the last 5 minutes is checked.|
|**Parameter Settings**|-   Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: Medium-6
-   Max validity period. The default value is 90 days. To meet the Center for Internet Security \(CIS\) rules of Alibaba Cloud, we recommend that you set the value to 90 or less. |
|**External Configurations**|None|
|**Solution**|Make sure that the validity period in the RAM password policy is less than or equal to the specified Max validity period parameter.|
|**Prerequisites**|The **Operations Log** switch next to ActionTrail is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## Root Account Login without MFA Alert

|**ID**|sls\_app\_audit\_cis\_at\_root\_mfa|
|**Name**|Root Account Login without MFA Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Account Security|
|**Usage**|Monitors the logons of a root user to the console without MFA being enabled. When a root user wants to log on to the console, the MFA must be enabled. Also, the number of logons without MFA must be smaller than or equal to the specified Max logins parameter. Otherwise, an alert is triggered.|
|**Check Frequency**|Fixed interval: 4 minutes.|
|**Time Range**|The data of the last 5 minutes is checked.|
|**Parameter Settings**|-   Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: Medium-6
-   Max logins: The maximum number of logons allowed on a single day for a root user whose MFA is disabled. Default value: 0. |
|**External Configurations**|You can configure a whitelist of root users who are allowed to log on without MFA. The root users on the whitelist can log on for an unlimited number of times without MFA. No alert is triggered by such logons.|
|**Solution**|Make sure that the number of non-MFA logins of the root user within 5 minutes is smaller than or equal to the specified Max logins parameter.|
|**Prerequisites**|The **Operations Log** switch next to ActionTrail is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## RAM Password Login Retry Policy Exception Alert

|**ID**|sls\_app\_audit\_cis\_at\_pwd\_login\_attemp\_policy|
|**Name**|RAM Password Login Retry Policy Exception Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Account Security|
|**Usage**|Monitors whether the logon retry policy specified in a RAM password policy is valid. In the RAM password policy, the number of failed logons attempts within one hour due to invalid passwords cannot be greater than the specified Max login failures/h parameter in the alert rule. Otherwise, an alert is triggered.|
|**Check Frequency**|Fixed interval: 5 minutes.|
|**Time Range**|The data of the last 5 minutes is checked.|
|**Parameter Settings**|-   Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: Medium-6
-   Max login failures/h: In the RAM password policy, the maximum number of failed logons due to invalid passwords that are allowed within a single hour. Default value: 5. To meet the Center for Internet Security \(CIS\) rules of Alibaba Cloud, we recommend that you set the value to 5. |
|**External Configurations**|None|
|**Solution**|You can reset the number of failed logons that are allowed within one hour due to invalid passwords in the RAM password policy. Make sure that it is smaller than or equal to the specified Max login failures/h parameter.|
|**Prerequisites**|The **Operations Log** switch next to ActionTrail is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## Root Account Frequent Login Alert

|**ID**|sls\_app\_audit\_cis\_at\_root\_login|
|**Name**|Root Account Frequent Login Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Account Security|
|**Usage**|Monitors frequent logins of a root user. Root users cannot frequently log on to the console of an Alibaba Cloud service. If the number of logons of a root user within 5 minutes exceeds the Max login Times parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 5 minutes.|
|**Time Range**|The data of the last 5 minutes is checked.|
|**Parameter Settings**|-   Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: Medium-6
-   Max login Times: The maximum number of logons that are allowed for a root user within 5 minutes. Default value: 2. |
|**External Configurations**|A whitelist of root users who are allowed to log on frequently. The root users on the whitelist can log on for an unlimited number of times within 5 minutes. No alert is triggered by such logons.|
|**Solution**|On a daily basis, you can limit the number of frequent logons of the root user. Make sure that it is smaller than or equal to the specified Max login Times parameter.|
|**Prerequisites**|The **Operations Log** switch next to ActionTrail is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## RAM History Password Check Policy Exception Alert

|**ID**|sls\_app\_audit\_cis\_at\_pwd\_reuse\_policy|
|**Name**|RAM History Password Check Policy Exception Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Account Security|
|**Usage**|Monitors whether the historical password check policy specified in the RAM password policy is valid. In a historical password check policy, the previous N passwords cannot be reused. You can specify the minimum value of N in the parameter settings of the alert rule. If the value in the historical password policy is less than this threshold, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|-   Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8
-   Minimum password reuse value: The minimum value of N in the **previous N passwords is prohibited** parameter in the historical password check of the RAM password policy. Default value: 4. To meet the Center for Internet Security \(CIS\) rules of Alibaba Cloud, we recommend that you set the value to 4. |
|**External Configurations**|None|
|**Solution**|Make sure that the value of N in the **previous N passwords is prohibited** is greater than or equal to the specified Minimum password reuse value parameter.|
|**Prerequisites**|The **Operations Log** switch next to ActionTrail is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## KMS Key Configuration Change Alert

|**ID**|sls\_app\_audit\_cis\_at\_ak\_conf\_change|
|**Name**|KMS Key Configuration Change Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Account Security|
|**Usage**|Monitors whether the key configuration in Key Management Service \(KMS\) is changed. When the key configuration in KMS is changed \(such as deleted or disabled\), an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8|
|**External Configurations**|You can configure a whitelist of RAM users who are allowed to modify the key configuration in KMS. RAM users on the whitelist can modify the key configuration in KMS without triggering an alert.|
|**Solution**|Prohibit the RAM users that are not included in the whitelist from modifying the key configuration.|
|**Prerequisites**|The **Operations Log** switch next to ActionTrail is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## Account Continuous Login Failure Alert

|**ID**|sls\_app\_audit\_cis\_at\_abnormal\_login\_count|
|**Name**|Account Continuous Login Failure Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Account Security|
|**Usage**|Monitors the number of consecutive logon failures within a specific period of time. When the number of failed logons within 5 minutes is greater than the specified Failed logins parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 4 minutes.|
|**Time Range**|The data of the last 5 minutes is checked.|
|**Parameter Settings**|-   Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8
-   Failed logins: The maximum number of failed logons that is allowed within 5 minutes for an account. Default value: 5. |
|**External Configurations**|None|
|**Solution**|Make sure that the number of failed logons within 5 minutes is less than or equal to the Failed logins parameter.|
|**Prerequisites**|The **Operations Log** switch next to ActionTrail is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## Root Account AK Usage Detection

|**ID**|sls\_app\_audit\_cis\_at\_root\_ak\_usage|
|**Name**|Root Account AK Usage Detection|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Account Security|
|**Usage**|Monitors the usage of the AccessKey pair of a root account. Root users cannot create or use AccessKey pairs for their root accounts. Otherwise, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8|
|**External Configurations**|You can configure a whitelist of root users who are allowed to use AccessKey pairs. Root users on the whitelist can use AccessKey pairs without triggering an alert.|
|**Solution**|Make sure that the Root account AccessKey pair is not used.|
|**Prerequisites**|The **Operations Log** switch next to ActionTrail is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

## RAM Password Length Policy Exception Alert

|**ID**|sls\_app\_audit\_cis\_at\_pwd\_length\_policy|
|**Name**|RAM Password Length Policy Exception Alert|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Account Security|
|**Usage**|Monitors whether the minimum password length specified in the RAM password policy is valid. In the RAM password policy, the minimum length of a RAM password must be greater than or equal to the value of the specified Min password length parameter. Otherwise, an alert is triggered.|
|**Check Frequency**|Fixed interval: 5 minutes.|
|**Time Range**|The data of the last 5 minutes is checked.|
|**Parameter Settings**|-   Severity: Critical-10, High-8, Medium-6, Low-4, and Report-2. Default value: High-8
-   Min password length: The minimum value of the setting of the minimum password length in the password policy Default value: 14. To meet the Center for Internet Security \(CIS\) rules of Alibaba Cloud, we recommend that you set the value to 14. |
|**External Configurations**|None|
|**Solution**|You can reset the minimum password length in the RAM password policy. Make sure that it is greater than or equal to the specified Min password length parameter.|
|**Prerequisites**|The **Operations Log** switch next to ActionTrail is turned on. To turn on the switch, go to the Log Audit Service page, and then choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**.|

