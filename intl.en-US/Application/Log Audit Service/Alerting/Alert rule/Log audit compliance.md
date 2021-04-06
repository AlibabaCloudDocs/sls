# Log audit compliance

This topic describes the alert rules for the log audit compliance of multiple Alibaba Cloud services. These services include Object Storage Service \(OSS\), ApsaraDB RDS, PolarDB, Server Load Balancer \(SLB\), Apsara File Storage NAS \(NAS\), and Container Service for Kubernetes. You can configure and enable alert rules in the Log Service console. This allows you to monitor the log audit compliance of these services. If an alert is triggered, you can identify the cause and fix the error at the earliest opportunity.

## Alert rules

The following alert rules are supported. For information about how to set alert parameters, configure whitelists, and perform other related operations, see [Manage alert rules]().

-   [Cloud Security Center Log Audit Configuration Check](#section_icr_h2r_oy3)
-   [RDS Log Audit Configuration Check](#section_jx4_ub5_e74)
-   [Log Audit Status Check](#section_9h9_r0o_rp1)
-   [PolarDB\(DRDS\) Log Audit Configuration Check](#section_aih_7ey_jb3)
-   [K8s Log Audit Configuration Check](#section_9i5_73i_wi1)
-   [ActionTrail Log Audit Configuration Check](#section_3ks_wn2_c15)
-   [OSS Log Audit Configuration Check](#section_hnh_blm_tyu)
-   [Web Application Firewall \(WAF\) Log Audit Configuration Check](#section_by7_y6m_ho1)
-   [Bastion Log Audit Configuration Check](#section_y1f_csd_cum)
-   [NAS \(File Storage\) Log Audit Configuration Check](#section_i1b_8nw_r78)
-   [APIGateway Log Audit Configuration Check](#section_5fj_jz4_kzx)
-   [SLB Log Audit Configuration Check](#section_w7k_bdb_h2a)
-   [Cloudfirewall Log Audit Configuration Check](#section_2yl_3vh_o6t)

## Cloud Security Center Log Audit Configuration Check

|**ID**|sls\_app\_audit\_cis\_at\_sas\_audit\_check|
|**Name**|Cloud Security Center Log Audit Configuration Check|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Log Audit Compliance|
|**Usage**|Checks whether log audit is properly configured in the Log Audit Service application for Security Center logs. If the audit switch is turned off for Security Center logs or the storage duration is smaller than the value of the Min storage duration\(ttl\) parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Min storage duration\(ttl\): The minimum duration for which Security Center logs are stored. Default value: 180 days.|
|**External Configurations**|None|
|**Solution**|On the Log Audit Service page, choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**. On the page that appears, turn on the Audit Logs switch next to Security Center\(SAS\). Make sure that the storage duration is greater than the value of the Min storage duration\(ttl\) parameter.|
|**Prerequisites**|None|

## RDS Log Audit Configuration Check

|**ID**|sls\_app\_audit\_cis\_at\_rds\_audit\_check|
|**Name**|RDS Log Audit Configuration Check|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Log Audit Compliance|
|**Usage**|Checks whether log audit is properly configured in the Log Audit Service application for RDS logs. If the audit switch is turned off for the RDS logs or the storage duration is smaller than the value of the Min storage duration\(ttl\) parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Min storage duration\(ttl\): The minimum duration for which RDS logs are stored. Default value: 180 days.|
|**External Configurations**|None|
|**Solution**|On the Log Audit Service page, choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**. On the page that appears, turn on the SQL Audit Log switch next to RDS. Make sure that the storage duration is greater than the value of the Min storage duration\(ttl\) parameter.|
|**Prerequisites**|None|

## Log Audit Status Check

|**ID**|sls\_app\_audit\_cis\_at\_audit\_status\_check|
|**Name**|Log Audit Status Check|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Log Audit Compliance|
|**Usage**|Checks the status of the log audit service. If the status is abnormal, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|None|
|**External Configurations**|None|
|**Solution**|On the Log Audit Service page, choose **Audit Configurations** \> **Access to Cloud Products** \> **Status Dashboard**. On the page that appears, check the status of the log audit service and identify the cause of the abnormal status.|
|**Prerequisites**|None|

## PolarDB\(DRDS\) Log Audit Configuration Check

|**ID**|sls\_app\_audit\_cis\_at\_drds\_audit\_check|
|**Name**|PolarDB\(DRDS\) Log Audit Configuration Check|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Log Audit Compliance|
|**Usage**|Checks whether log audit is properly configured in the Log Audit Service application for PolarDB logs. If the audit switch is turned off for the PolarDB \(DRDS\) logs or the storage duration is smaller than the value of the Min storage duration\(ttl\) parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Min storage duration\(ttl\): The minimum duration for which PolarDB logs are stored. Default value: 180 days.|
|**External Configurations**|None|
|**Solution**|On the Log Audit Service page, choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**. On the page that appears, turn on the Audit Log switch next to PolarDB. Make sure that the storage duration is greater than the value of the Min storage duration\(ttl\) parameter.|
|**Prerequisites**|None|

## K8s Log Audit Configuration Check

|**ID**|sls\_app\_audit\_cis\_at\_k8s\_audit\_check|
|**Name**|K8s Log Audit Configuration Check|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Log Audit Compliance|
|**Usage**|Checks whether log audit is properly configured in the Log Audit Service application for Kubernetes logs, including Kubernetes audit logs, Kubernetes events, and Ingress access logs. If the audit switch is turned off for K8s logs or the storage duration is smaller than the value of the Min storage duration\(ttl\) parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Min storage duration\(ttl\): The minimum duration for which K8s logs are stored. Default value: 180 days.|
|**External Configurations**|None|
|**Solution**|On the Log Audit Service page, choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**. On the page that appears, turn on the Kubernetes Audit Log switch, K8s Event Center switch and Ingress Log switch next to Kubernetes. Make sure that the storage duration is greater than the value of the Min storage duration\(ttl\) parameter.|
|**Prerequisites**|None|

## ActionTrail Log Audit Configuration Check

|**ID**|sls\_app\_audit\_cis\_at\_actiontrail\_audit\_check|
|**Name**|ActionTrail Log Audit Configuration Check|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Log Audit Compliance|
|**Usage**|Checks whether log audit is properly configured in the Log Audit Service application for ActionTrail logs. If the audit switch is turned off for the of Action Trail logs or the storage duration is smaller than the value of the Min storage duration\(ttl\) parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Min storage duration\(ttl\): The minimum duration for which Action Trail logs are stored. Default value: 180 days.|
|**External Configurations**|None|
|**Solution**|On the Log Audit Service page, choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**. Turn on the Operations Log switch next to ActionTrail. Make sure that the storage duration is greater than the value of the Min storage duration\(ttl\) parameter.|
|**Prerequisites**|None|

## OSS Log Audit Configuration Check

|**ID**|sls\_app\_audit\_cis\_at\_oss\_audit\_check|
|**Name**|OSS Log Audit Configuration Check|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Log Audit Compliance|
|**Usage**|Checks whether log audit is properly configured in the Log Audit Service application for Object Storage Service \(OSS\) logs, including access logs and metering logs. If the audit switches are turned off for OSS logs or the storage duration is smaller than the value of the Min storage duration\(ttl\) parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Min storage duration\(ttl\): The minimum duration for which OSS logs are stored. Default value: 180 days.|
|**External Configurations**|None|
|**Solution**|On the Log Audit Service page, choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**. Turn on the Metering Log switch and the Access Log switch next to OSS. Make sure that the storage duration is greater than the value of the Min storage duration\(ttl\) parameter.|
|**Prerequisites**|None|

## Web Application Firewall \(WAF\) Log Audit Configuration Check

|**ID**|sls\_app\_audit\_cis\_at\_waf\_audit\_check|
|**Name**|Web Application Firewall \(WAF\) Log Audit Configuration Check|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Log Audit Compliance|
|**Usage**|Checks whether log audit is properly configured in the Log Audit Service application for the Web Application Firewall \(WAF\) logs. If the audit switch is turned off for Web Application Firewall \(WAF\) logs or the storage duration is smaller than the value of the Min storage duration\(ttl\) parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Min storage duration\(ttl\): The minimum duration for which WAF Logs are stored. Default value: 180 days.|
|**External Configurations**|None|
|**Solution**|On the Log Audit Service page, choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**. Turn on the Access Log switch next to Web Application Firewall \(WAF\). Make sure that the storage duration is greater than the value of the Min storage duration\(ttl\) parameter.|
|**Prerequisites**|None|

## Bastion Log Audit Configuration Check

|**ID**|sls\_app\_audit\_cis\_at\_bastion\_audit\_check|
|**Name**|Bastion Log Audit Configuration Check|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Log Audit Compliance|
|**Usage**|Checks whether log audit is properly configured in the Log Audit Service application for Bastionhost logs. If the audit switch is turned off for the Bastionhost log or its storage duration is smaller than the value of the Min storage duration\(ttl\) parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Min storage duration\(ttl\): The minimum duration for which Bastionhost log is stored. Default value: 180 days.|
|**External Configurations**|None|
|**Solution**|On the Log Audit Service page, choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**. Turn on the Operations Log switch next to Bastion Host. Make sure that the storage duration is greater than the value of the Min storage duration\(ttl\) parameter.|
|**Prerequisites**|None|

## NAS \(File Storage\) Log Audit Configuration Check

|**ID**|sls\_app\_audit\_cis\_at\_nas\_audit\_check|
|**Name**|NAS \(File Storage\) Log Audit Configuration Check|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Log Audit Compliance|
|**Usage**|Checks whether log audit is properly configured in the Log Audit Service application for the Apsara File Storage NAS logs. If the audit switch is turned off for NAS \(file storage\) logs or the storage duration is smaller than the value of the Min storage duration\(ttl\) parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Min storage duration\(ttl\): The minimum duration for which NAS \(file storage\) logs are stored. Default value: 180 days.|
|**External Configurations**|None|
|**Solution**|On the Log Audit Service page, choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**. Turn on the Access Log switch next to NAS. Make sure that the storage duration is greater than the value of the Min storage duration\(ttl\)parameter.|
|**Prerequisites**|None|

## APIGateway Log Audit Configuration Check

|**ID**|sls\_app\_audit\_cis\_at\_apigateway\_audit\_check|
|**Name**|APIGateway Log Audit Configuration Check|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Log Audit Compliance|
|**Usage**|Checks whether log audit is properly configured in the Log Audit Service application for the API Gateway logs. If the audit switch is turned off for API Gateway logs or the storage duration is smaller than the value of the Min storage duration\(ttl\) parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Min storage duration\(ttl\): The minimum duration for which API Gateway logs are stored. Default value: 180 days.|
|**External Configurations**|None|
|**Solution**|On the Log Audit Service page, choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**. Turn on the Access Log switch next to API Gateway. Make sure that the storage duration is greater than the value of the Min storage duration\(ttl\) parameter.|
|**Prerequisites**|None|

## SLB Log Audit Configuration Check

|**ID**|sls\_app\_audit\_cis\_at\_slb\_audit\_check|
|**Name**|SLB Log Audit Configuration Check|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Log Audit Compliance|
|**Usage**|Checks whether log audit is properly configured in the Log Audit Service application for SLB logs. If the audit switch is turned off for SLB logs or the storage duration is smaller than the value of the Min storage duration\(ttl\) parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Min storage duration\(ttl\): The minimum duration for which SLB logs are stored. Default value: 180 days.|
|**External Configurations**|None|
|**Solution**|On the Log Audit Service page, choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**. Turn on the Lay-7 Access Log switch next to SLB. Make sure that the storage duration is greater than the value of the Min storage duration\(ttl\) parameter.|
|**Prerequisites**|None|

## Cloudfirewall Log Audit Configuration Check

|**ID**|sls\_app\_audit\_cis\_at\_cloudfirewall\_audit\_check|
|**Name**|Cloudfirewall Log Audit Configuration Check|
|**Version**|1|
|**Type**|Cloud Platform, Alicloud, CIS Standard, Log Audit Compliance|
|**Usage**|Checks whether log audit is properly configured in the Log Audit Service application for Cloud Firewall logs. If the audit switch is turned off for the Cloud Firewall log or its storage duration is smaller than the value of the Min storage duration\(ttl\)parameter, an alert is triggered.|
|**Check Frequency**|Fixed interval: 1 minute.|
|**Time Range**|The data of the last 2 minutes is checked.|
|**Parameter Settings**|Min storage duration\(ttl\): The minimum duration for which Cloud Firewall logs are stored. Default value: 180 days.|
|**External Configurations**|None|
|**Solution**|On the Log Audit Service page, choose **Audit Configurations** \> **Access to Cloud Products** \> **Global Configurations**. Turn on the Internet Access Log switch next to Cloud Firewall. Make sure that the storage duration is greater than the value of the Min storage duration\(ttl\) parameter.|
|**Prerequisites**|None|

