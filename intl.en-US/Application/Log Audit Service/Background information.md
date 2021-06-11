# Background information

This topic describes the features, background information, scenarios, and benefits of the Log Audit Service application in Log Service. This topic also describes the Alibaba Cloud services that are supported by the Log Audit Service application.

## Features

The Log Audit Service application supports all features of Log Service. This application allows you to collect and audit logs of cloud services across Alibaba Cloud accounts. You can store all logs in one project and query the logs. You can use the Log Audit Service application to audit logs collected from the following Alibaba Cloud services: ActionTrail, Container Service for Kubernetes \(ACK\), Object Storage Service \(OSS\), Apsara File Storage NAS, Server Load Balancer \(SLB\), API Gateway, ApsaraDB RDS, PolarDB-X, PolarDB for MySQL, Web Application Firewall \(WAF\), Anti-DDoS, Cloud Firewall, and Security Center. You can also audit logs collected from third-party cloud services and self-managed security operations centers \(SOCs\).

![Log audit - 005](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5506498951/p101352.png)

## Background information

-   Log audit is required by law.

    Audit logs are an essential tool for enterprises around the world to ensure that their business meets regulatory requirements. For example, the Cybersecurity Law of the People's Republic of China came into effect in mainland China in 2017. In addition, the Baseline for Classified Protection of Cybersecurity 2.0 came into effect in December 2019.

    ![Log audit - 001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6506498951/p101330.png)

-   Audit logs are the foundation for data security compliance of enterprises.

    A large number of enterprises have their own regulations and compliance teams to audit device operations, network behavior, and logs. You can use the Log Audit Service application to consume raw logs, audit logs, and generate compliance audit reports. If you have an SOC, you can consume logs in the Log Audit Service application or use Security Center to consume logs.

    ![Log audit - 002](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6506498951/p102203.png)

-   Log audit is crucial for data security and protection.

    According to the M-Trends 2018 report published by FireEye, most enterprises, especially those in Asia Pacific, are vulnerable to cybersecurity attacks. The global median dwell time was 101 days in 2017. In Asia Pacific, the median dwell time was 498 days. To shorten this time period, enterprises need reliable log data, durable storage, and audit services.


## Scenarios

-   Log Service

    Log Service is a real-time logging service that allows you to collect, cleanse, and analyze log data. You can visualize log data on charts and dashboards. You can also configure alerts for logs. Log Service is applicable to DevOps, operation, security, and audit scenarios.

    ![Log audit - 003](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6506498951/p101349.png)

-   Typical log audit scenarios

    Requirements for audit logs can be classified into the following four levels:

    ![Log audit - 004](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6506498951/p101350.png)

    -   Basic requirements: Most small and medium-sized enterprises need automatic log collection and storage. These enterprises must meet the minimum requirements that are specified in the Baseline for Classified Protection of Cybersecurity 2.0.
    -   Intermediate requirements: Multinational enterprises, large enterprises, and some medium-sized enterprises have multiple departments that use different Alibaba Cloud accounts and separately pay bills. Therefore, these enterprises need to collect the logs of the accounts by using an automatic process, send these logs to the same storage resource, and then audit the logs. In addition to the basic requirements, these types of enterprises also need to collect logs and manage accounts in a centralized manner. Most of these enterprises have an audit system and want to connect their audit systems to the Log Audit Service application in real time.
    -   Advanced requirements: Large enterprises that have dedicated audit compliance teams need to monitor logs, analyze logs, and configure alerts for logs. Some enterprises collect data to their audit systems for further operations. Other enterprises, especially those who want to build an audit system on the cloud, can use the query, analysis, alerting, chart, and dashboard features provided by Log Service to audit logs.
    -   Top requirements: Most large enterprises that have professional auditing and compliance teams have SOCs or audit systems. These enterprises need to integrate their systems with the Log Audit Service application and manage data in a centralized manner.
    The Log Audit Service application of Log Service satisfies all four levels of requirements.


## Benefits

-   Centralized log collection
    -   Log collection across accounts: You can collect logs from multiple Alibaba Cloud accounts to a project of one Alibaba Cloud account.
    -   Ease of use: You can use the Log Audit Service application to collect logs in real time from Alibaba Cloud resources that belong to different accounts. You only need to turn on the relevant switches on the Audit-Related Logs tab. Logs are automatically collected in real time when new resources such as RDS instances, SLB instances, or OSS buckets are detected.
    -   Centralized storage: Logs are collected and stored in the central project that resides in a region. This improves efficiency when you query, analyze, and visualize the collected logs. You can also configure alerts for the logs and perform secondary development.
-   Multiple Log Service features
    -   The Log Audit Service application supports all features of Log Service. You can query, analyze, transform, and export logs. You can visualize log data on dashboards and configure alerts for logs. You can also audit logs in a centralized manner.
    -   You can integrate the Log Audit Service application with Alibaba Cloud services, open source software, and third-party SOC software to extract more data value.

## Supported Alibaba Cloud services

You can use the Log Audit Service application to audit logs collected from the following Alibaba Cloud services: ActionTrail, ACK, OSS, NAS, SLB, API Gateway, ApsaraDB RDS, PolarDB-X, PolarDB for MySQL, WAF, Cloud Firewall, Security Center, and Anti-DDoS. Logs that are collected from an Alibaba Cloud service are automatically stored in the corresponding Logstores and Mertricstores. Dashboards are automatically generated for the Logstores and Metricstores. The following table describes the detailed information.

|Alibaba Cloud service|Audit-related log|Region|Prerequisites|Assets|
|---------------------|-----------------|------|-------------|------|
|ActionTrail|-   RAM logon logs
-   Resource operation logs of Alibaba Cloud services
-   Logs that are generated by using OpenAPI Explorer

|All supported regions|None|-   Logstore

actiontrail\_log

-   Dashboard
    -   ActionTrail Audit Center
    -   ActionTrail Core Configuration Center
    -   ActionTrail Login Center |
|Load Balancing|Layer 7 access logs of HTTP or HTTPS listeners|All supported regions|None|-   Logstore

slb\_log

-   Dashboard
    -   SLB Audit Log
    -   SLB Access Log
    -   SLB Overall View |
|API Gateway|Access logs|All supported regions|None|-   Logstore

apigateway\_log

-   Dashboard

API Gateway Audit Center |
|WAF|-   Access log
-   Attack logs

|All supported regions|-   The Pro Edition or later
-   The log analysis feature must be enabled on the **Log Service** page in the WAF console. For more information, see [Enable the log analysis feature](/intl.en-US/Data Collection/Cloud product collection/WAF logs/Enable the log analysis feature.md).

|-   Logstore

waf\_log

-   Dashboard
    -   WAF Audit Center
    -   WAF Security Center
    -   WAF Access Center |
|Security Center|-   Seven types of host logs
-   Four types of network logs
-   Three types of security logs

|All supported regions|-   Enterprise Edition
-   The **log analysis** feature must be enabled in the Security Center console. For more information, see [Enable the log analysis feature](/intl.en-US/Data Collection/Cloud product collection/Security Center logs/Enable the log analysis feature.md).

|-   Logstore

sas\_log

-   Dashboard
    -   Hosts
        -   SAS Login Center
        -   SAS Process Center
        -   SAS Connection Center
    -   Networking
        -   SAS Session Center
        -   SAS DNS Center
    -   Security
        -   SAS Web Access Center
        -   SAS Baseline Center
        -   SAS Alarm Center |
|Cloud Firewall|Internet access logs|None|-   The Pro Edition or later
-   The **log analysis** feature must be enabled in the Cloud Firewall console. For more information, see [Enable the log analysis feature](/intl.en-US/Data Collection/Cloud product collection/Cloud Firewall logs/Enable the log analysis feature.md).

|-   Logstore

cloudfirewall\_log

-   Dashboard

Cloud Firewall Audit Center |
|Bastionhost|Operation logs|All supported regions|Version 3.2 or later|-   Logstore

baston\_log

-   Dashboard

None |
|OSS|-   Resource operation logs
-   Data operation logs
-   Data access logs and metering logs
-   Deletion logs of expired files
-   CDN back-to-origin traffic logs

|All supported regions|None|-   Logstore

oss\_log

-   Dashboard
    -   OSS Audit Center
    -   OSS Access Center
    -   OSS Operation Center
    -   OSS Performance Center
    -   OSS Overall View |
|ApsaraDB RDS|-   Audit logs of ApsaraDB RDS for MySQL
-   Audit logs of SQL Server
-   Audit logs of PostgreSQL databases
-   MySQL slow query logs
-   MySQL performance logs

|-   China \(Qingdao\), China \(Beijing\), China \(Zhangjiakou\), China \(Hohhot\), China \(Hangzhou\), China \(Shanghai\), China \(Shenzhen\), China \(Guangzhou\), China \(Chengdu\), and China \(Hong Kong\)
-   Singapore \(Singapore\), Malaysia \(Kuala Lumpur\), Indonesia \(Jakarta\), Germany \(Frankfurt\), Australia \(Sydney\), India \(Mumbai\), and Japan \(Tokyo\)

|-   Audit logs
    -   MySQL is not supported in the Basic Edition of ApsaraDB RDS.
    -   PostgreSQL and SQL Server are supported in all available editions of ApsaraDB RDS.
    -   SQL Explorer of related databases is automatically enabled.
-   Slow query and performance logs

MySQL instances that are not in the Basic Edition are supported.


|-   Audit logs
    -   Logstore

rds\_log

    -   Dashboard
        -   RDS Audit Center
        -   RDS Security Center
        -   RDS Performance Center
        -   RDS Overall View
-   Slow query logs
    -   Logstore

rds\_log

    -   Dashboard

None

-   Performance logs
    -   Metricstore

rds\_metrics

    -   Dashboard

RDS Performance Monitoring |
|PolarDB-X|PolarDB-X audit logs|China \(Qingdao\), China \(Shenzhen\), China \(Shanghai\), China \(Beijing\), China \(Hangzhou\), China \(Zhangjiakou\), China \(Chengdu\), and China \(Hong Kong\)|None|-   Logstore

drds\_log

-   Dashboard
    -   PolarDB-X Operation Center
    -   PolarDB-X Performance Center
    -   PolarDB-X Security Center |
|PolarDB for MySQL|-   PolarDB audit logs
-   PolarDB slow query logs
-   PolarDB performance logs

|China \(Qingdao\), China \(Beijing\), China \(Hangzhou\), China \(Shanghai\), China \(Shenzhen\), and China \(Hong Kong\)|-   SQL Explorer of related databases

is automatically enabled.

-   Only MySQL-compatible clusters are supported.

|-   Slow query and audit logs
    -   Logstore

polardb\_log

    -   Dashboard

None

-   Performance logs
    -   Metricstore

polardb\_metrics

    -   Dashboard

PolarDB Performance Monitoring |
|NAS|Access logs|All supported regions|None|-   Logstore

nas\_log

-   Dashboard
    -   NAS Summary
    -   NAS Audit Center
    -   NAS Detail |
|Alibaba Cloud Mobile Push|Callback events|Mainland China|None|-   Logstore

cps\_log

-   Dashboard
    -   Android Callback Center
    -   iOS Callback Center |
|ACK|-   Kubernetes audit logs
-   Kubernetes event centers
-   Ingress access logs

|China \(Shanghai\), China \(Beijing\), China \(Hangzhou\), China \(Shenzhen\), China \(Hohhot\), China \(Zhangjiakou\), China \(Chengdu\), and China \(Hong Kong\)|You must manually enable the log collection feature for Kubernetes logs. **Note:**

-   You must use the automatically created project named in the k8s-log-\{ClusterID\} format. Custom projects are not supported.
-   The collection of Kubernetes logs depends on the data transformation feature. When you collect Kubernetes logs, you are charged for the data transformation feature. For more information, see [Billable items and billing method](/intl.en-US/Pricing/Billable items and billing method.md).

-   For more information about Kubernetes audit logs, see [Collect log files from containers by using Log Service](/intl.en-US/User Guide for Kubernetes Clusters/Observability/Log management/Collect log files from containers by using Log Service.md).
-   For more information about Kubernetes event centers, see [Create and use a Kubernetes event center](/intl.en-US/Application/K8s Event Center/Create and use a Kubernetes event center.md).
-   For more information about Ingress access logs, see[Monitor nginx-ingress and analyze the access log of nginx-ingress](/intl.en-US/User Guide for Kubernetes Clusters/Network/Ingress management/Monitor nginx-ingress and analyze the access log of nginx-ingress.md).

|-   Logstore
    -   k8s\_log
    -   k8s\_ingress\_log
-   Dashboard
    -   Kubernetes Audit Center Overview
    -   Kubernetes Event Center
    -   Kubernetes Resource Operations Overview
    -   Ingress Overview
    -   Ingress Access Center |
|Anti-DDoS|Anti-DDoS Pro access logs|None|The log analysis feature must be enabled in the Anti-DDoS Pro console. For more information, see [Enable the log analysis feature](/intl.en-US/Data Collection/Cloud product collection/Anti-DDoS Pro and Anti-DDoS Premium logs/Enable the log analysis feature.md).|-   Logstore

ddos\_log

-   Dashboard
    -   DDoS Operation Center
    -   DDoS Access Center |
|Application Integration|Operation logs|None|None|-   Logstore

appconnect\_log

-   Dashboard

None |

