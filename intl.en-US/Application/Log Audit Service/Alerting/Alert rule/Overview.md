# Overview

This topic describes the built-in alert rules of the Log Audit Service application. You can use the alert rules to monitor the operation compliance, account security, permissions, and traffic security of the Log Audit Service application. If an alert is triggered, you can identify the error cause and fix the error at the earliest opportunity.

## Alert rules

The following alert rules are supported. For information about how to set alert parameters, configure whitelists, and perform other relevant operations, see [Manage alert rules]().

|Type|Alert rule|
|----|----------|
|[Log audit compliance](/intl.en-US/Application/Log Audit Service/Alerting/Alert rule/Log audit compliance.md)|Cloud Security Center Log Audit Configuration Check|
|RDS Log Audit Configuration Check|
|PolarDB\(DRDS\) Log Audit Configuration Check|
|K8s Log Audit Configuration Check|
|Web Application Firewall \(WAF\) Log Audit Configuration Check|
|Bastion Log Audit Configuration Check|
|APIGateway Log Audit Configuration Check|
|Cloudfirewall Log Audit Configuration Check|
|Log Audit Status Check|
|ActionTrail Log Audit Configuration Check|
|[Account security](/intl.en-US/Application/Log Audit Service/Alerting/Alert rule/Account security.md)|RAM Sub-Account Login without MFA Alert|
|RAM Password Expiration Policy Exception Alert|
|Root Account Login without MFA Alert|
|RAM Password Login Retry Policy Exception Alert|
|Root Account Frequent Login Alert|
|RAM History Password Check Policy Exception Alert|
|KMS Key Configuration Change Alert|
|Account Continuous Login Failure Alert|
|Root Account AK Usage Detection|
|RAM Password Length Policy Exception Alert|
|[Permission control]()|OSS Bucket Authority Change Alert|
|RAM Policy Change Alert|
|RAM Policy Abnormal Attach Alert|
|[OSS operation compliance]()|OSS Bucket Encryption Shutdown Alert|
|OSS Newly Created Bucket Encryption Not Enabled Alert|
|OSS Bucket Logging Shutdown Alert|
|OSS Newly Created Bucket Logging Not Enabled Alert|
|[Operation compliance of RDS instances]()|RDS Instance SQL Insight Disabled Alert|
|RDS Instance Access Whitelist Abnormal Setting Alert|
|Newly Created RDS Instance's SSL Not Enabled AlertNot CreatedEnable Settings|
|Newly Created RDS Instance's TDE Not Enabled Alert|
|RDS Instance SSL Disabled Alert|
|RDS Instance Configuration Change Alert|
|[Server Load Balancer \(SLB\) operation compliance]()|SLB Modification Protection Shutdown Alert|
|SLB Health Check Shutdown Alert|
|[Operation compliance of ECS instances]()|ECS Disk Encryption Shutdown Alert|
|ECS Automatic Snapshot Strategy Shutdown Alert|
|Security Group Configuration Change Alert|
|ECS Network Type Check|
|[Operation compliance of VPCs]()|VPC Network Routing Change Alert|
|VPC Flow Log Abnormally Configured Alert|
|VPC Configuration Change Alert|
|[Operation compliance of Cloud Firewall]()|Cloudfirewall Control Policy Change Alert|
|[API calls]()|Unauthorized Api Call Alert|
|[Operation compliance of TDI]()|TDI Webpage Anti-tampering Disabled Alert|
|[Kubernetes security]()|Too Many K8s Warning Events Alert|
|K8s Frequent Delete Event Alert|
|Too Many K8s Error Events Alert|
|[Security of RDS instances]()|RDS Slow SQL detection|
|RDS Data Mass Deletion Alert|
|Detection of RDS Visit through Internet|
|RDS Query SQL Average Execution Time Monitoring|
|RDS Instance Update Peak Monitoring|
|RDS Instance Query Peak Monitoring|
|RDS Instance Released Alert|
|RDS Frequent Visit IP Detection|
|RDS Update SQL Average Execution Time Monitoring|
|Too Many RDS Login Failures Alert|
|Rds Mass Data Update Event Alert|
|RDS Dangerous SQL Execution Alert|
|Too Many RDS SQL Execution Errors Alert|
|[Flow security of SLB]()|Inspection of SLB Abnormal Response Length|
|Inspection of SLB Abnormal Request Length|
|SLB Average Response Delay Too High Alert|
|SLB HTTP Access Protocol Enabled Alert|
|Load Balance Access UV Anomaly Inspection|
|Load Balance Access PV Anomaly Inspection|
|[Flow security of API Gateway]()|APIgateway Server Average Delay Too High Alert|
|APIGateway Backend Server Error Rate Too High Alert|
|APIgateway Request Success Rate Too Low Alert|
|[Security of OSS traffic]()|OSS Inflow Anomaly Inspection|
|OSS Bucket Valid Request Rate Too Low Alert|
|Detection of OSS Bucket Visit through Internet|
|OSS Access PV Anomaly Inspection|
|OSS Flow Anomaly Inspection|
|OSS Outflow Anomaly Inspection|
|OSS Access UV Anomaly Inspection|
|[t2025361.md\#]()|Too Many K8s Illegal Access Alert|
|K8s Ingress Average Request Latency Too High Alert|
|K8s Ingress Response Delay Too High Alert|
|K8s Ingress Request Success Rate Too Low Alert|
|[t2025362.md\#]()|OSS Bucket Account Access Control|
|OSS Object Frequent Deletion Alert|
|[t2025363.md\#]()|NAS Error Operation Detection|
|NAS Mass Deletion Alert|
|[t2025364.md\#]()|Application Firewall Valid Request Rate Too Low Alert|
|Too Many Attacks on Hosts Protected by WAF Alert|
|[t2025365.md\#]()|Too Many High-Priority Alarms In Cloud Security Center|
|Too Many New Vulnerabilities In Cloud Security Centers|
|Cloud Security Center Valid Request Rate Too Low Alert|
|Too Many New Alarms In Cloud Security Center|
|Cloud Security Center Request Success Rate Too Low|
|[t2025368.md\#]()|Cloudfirewall Outflow Block Alert|
|Cloudfirewall Inflow Block Alarm|

