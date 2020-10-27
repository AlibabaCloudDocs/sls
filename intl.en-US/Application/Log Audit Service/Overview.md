# Overview

This topic describes the scenarios and benefits of the Log Audit Service application in Log Service. This topic also describes the Alibaba Cloud services that are supported by the Log Audit Service application.

The Log Audit Service application supports all features of Log Service. This application allows you to collect and audit logs of cloud services across Alibaba Cloud accounts. You can store all logs in one project and query the logs. You can use the Log Audit Service application to audit logs collected from the following Alibaba Cloud services: ActionTrail, Alibaba Cloud Container Service for Kubernetes \(ACK\), Object Storage Service \(OSS\), Apsara File Storage NAS, Server Load Balancer \(SLB\), API Gateway, ApsaraDB RDS, PolarDB-X, Web Application Firewall \(WAF\), Cloud Firewall, and Security Center. You can also audit logs collected from third-party cloud services and user-created security operations centers \(SOCs\).

![Log audit - 005](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5506498951/p101352.png)

## Background information

-   Log audit is a strict legal requirement.

    Log audit is a necessity for enterprises around the world to better comply with applicable rules and regulations. For example, the Cybersecurity Law of the People's Republic of China came into effect in mainland China in 2017. In addition, the Baseline for Classified Protection of Cybersecurity 2.0 came into effect in December 2019.

    ![Log audit - 001](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6506498951/p101330.png)

-   Log audit is the foundation for data security compliance of enterprises.

    A large number of enterprises have their own regulations and compliance teams to audit device operations, network behavior, and logs. You can use the Log Audit Service application to consume raw logs, audit logs, and generate compliance audit reports. If you have an SOC, you can consume logs in the Log Audit Service application or use Security Center to consume logs.

    ![Log audit - 002](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6506498951/p102203.png)

-   Log audit is a crucial part of data security and protection.

    Based on the M-Trends 2018 report published by FireEye, most enterprises, especially those in Asia Pacific, are vulnerable to cybersecurity attacks. The global median dwell time was 101 days in 2017. In Asia Pacific, the median dwell time was 498 days. Furthermore, the global median time for detection was 57.5 days in 2017. To shorten these time periods, enterprises need reliable log data, durable storage, and audit services.


## Scenarios

-   Log Service

    Log Service is a real-time logging service that provides data collection, cleansing, analysis, visualization, and alert features. Log Service is applicable to DevOps, operation, security, and audit scenarios.

    ![Log audit - 003](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6506498951/p101349.png)

-   Typical log audit scenarios

    Requirements for log audit can be classified into the following four levels:

    ![Log audit - 004](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6506498951/p101350.png)

    -   Basic requirements: Most small and medium-sized enterprises need automatic log collection and storage. These enterprises must meet the minimum requirements that are specified in the Baseline for Classified Protection of Cybersecurity 2.0.
    -   Intermediate requirements: Multinational enterprises, large enterprises, and some medium-sized enterprises have multiple departments that use different Alibaba Cloud accounts and separately pay bills. Therefore, these enterprises need to collect logs of the accounts by using an automatic process, send these logs to the same storage resource, and then audit the logs. In addition to the basic requirements, these types of enterprises also need to collect logs and manage accounts in a centralized manner. Most of these enterprises have an audit system and want to connect their audit systems to the Log Audit Service application in real time.
    -   Advanced requirements: Large enterprises that have dedicated audit compliance teams need to monitor logs, analyze logs, and configure alerts for logs. Some enterprises collect data to their audit systems for further operations. Other enterprises, especially those who want to build an audit system on the cloud, can use the query, analysis, alert, chart, and dashboard features provided by Log Service to audit logs.
    -   Top requirements: Most large enterprises that have professional audit compliance teams have SOCs or audit systems. These enterprises need to integrate their systems with the Log Audit Service application and manage data in a centralized manner.
    The Log Audit Service application of Log Service satisfies all four levels of requirements.


## Benefits

-   Centralized log collection
    -   Log collection across accounts: You can collect logs from multiple Alibaba Cloud accounts to a project of one Alibaba Cloud account.
    -   Ease of use: You can use the Log Audit Service application to collect logs in real time from Alibaba Cloud services that belong to different accounts. You only need to turn on the relevant switches on the Audit-Related Logs tab. Logs are automatically collected in real time when new resources such as RDS instances, SLB instances, or OSS buckets are detected.
    -   Centralized storage: Logs are collected and stored in the central project that resides in a region. This improves efficiency when you query, analyze, and visualize the collected logs. You can also configure alerts for the logs and perform secondary development.
-   Multiple Log Service features
    -   The Log Audit Service application supports all features of Log Service. You can query, analyze, transform, and export logs. You can visualize log data on dashboards and configure alerts for logs. You can also audit logs in a centralized manner.
    -   You can integrate the Log Audit Service application with Alibaba Cloud services, open source software, and third-party SOC software to extract more data value.

## Supported Alibaba Cloud services

You can use the Log Audit Service application to audit logs collected from the following Alibaba Cloud services: ActionTrail, ACK, OSS, NAS, SLB, API Gateway, ApsaraDB RDS, PolarDB-X, WAF, Cloud Firewall, and Security Center. Logs that are collected from an Alibaba Cloud service are automatically stored in corresponding Logstores and displayed in corresponding dashboards.

|Alibaba Cloud service|Audit-related log|Region|Prerequisites|Assets|
|---------------------|-----------------|------|-------------|------|
|ActionTrail|RAM logon logs, operations logs of Alibaba Cloud resources, and logs generated by using the API|All available regions|None|-   Logstore

actiontrail\_log

-   Dashboard
    -   ActionTrail Audit Center
    -   ActionTrail Core Configuration Center
    -   ActionTrail Login Center |
|SLB|Layer-7 access logs of HTTP or HTTPS listeners|All available regions|None|-   Logstore

slb\_log

-   Dashboard
    -   SLB Audit Log
    -   SLB Access Log
    -   SLB Overall View |
|API Gateway|Access logs|All available regions|None|-   Logstore

apigateway\_log

-   Dashboard

API Gateway Audit Center |
|WAF|Access logs and attack logs|All available regions|-   The Pro Edition or later
-   The log analysis feature must be enabled on the **Log Service** page in the WAF console.

|-   Logstore

waf\_log

-   Dashboard
    -   WAF Audit Center
    -   WAF Security Center
    -   WAF Access Center |
|Security Center|Seven types of host logs, four types of network logs, and three types of security logs|All available regions|-   Enterprise Edition
-   The **log analysis** feature must be enabled in the Security Center console.

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
-   The **log analysis** feature must be enabled in the Cloud Firewall console.

|-   Logstore

cloudfirewall\_log

-   Dashboard

Cloud Firewall Audit Center |
|Bastionhost|Operations logs of O&M personnel|China \(Hangzhou\), China \(Shanghai\), China \(Heyuan\), and China \(Chengdu\)|Version 3.2 or later|-   Logstore

baston\_log

-   Dashboard

None |
|OSS|Resource operations logs, data operations logs, data access logs, metering logs, deletion logs of expired files, and CDN back-to-origin traffic logs|All available regions|None|-   Logstore

oss\_log

-   Dashboard
    -   OSS Audit Center
    -   OSS Access Center
    -   OSS Operation Center
    -   OSS Performance Center
    -   OSS Overall View |
|ApsaraDB RDS|Audit logs of ApsaraDB RDS for MySQL, SQL Server, and PostgreSQL databases|-   China \(Qingdao\), China \(Beijing\), China \(Hohhot\), China \(Hangzhou\), China \(Shanghai\), China \(Shenzhen\), China \(Chengdu\), and China \(Hong Kong\)
-   Singapore \(Singapore\), Malaysia \(Kuala Lumpur\), Indonesia \(Jakarta\), Germany \(Frankfurt\), Australia \(Sydney\), India \(Mumbai\), and Japan \(Tokyo\)

|-   MySQL is not supported in the Basic Edition of ApsaraDB RDS.
-   PostgreSQL and SQL Server are supported in all available editions of ApsaraDB RDS.
-   SQL Explorer of related databases is automatically enabled.

|-   Logstore

rds\_log

-   Dashboard
    -   RDS Audit Center
    -   RDS Security Center
    -   RDS Performance Center
    -   RDS Overall View |
|PolarDB-X|Audit logs|China \(Qingdao\), China \(Shenzhen\), China \(Shanghai\), China \(Beijing\), China \(Hangzhou\), China \(Zhangjiakou-Beijing Winter Olympics\), China \(Chengdu\), and China \(Hong Kong\)|None|-   Logstore

drds\_log

-   Dashboard
    -   PolarDB-X Operation Center
    -   PolarDB-X Performance Center
    -   PolarDB-X Security Center |
|NAS|Access logs|All available regions|None|-   Logstore

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

|China \(Shanghai\), China \(Beijing\), China \(Hangzhou\), China \(Shenzhen\), China \(Hohhot\), China \(Zhangjiakou-Beijing Winter Olympics\), China \(Chengdu\), and China \(Hong Kong\)|You must manually enable the log collection feature for Kubernetes logs.**Note:**

-   You must use the automatically created project named in the k8s-log-\{ClusterID\} format. Custom projects are not supported.
-   Kubernetes log collection depends on the data transformation feature. When you collect Kubernetes logs, you are charged for the data transformation feature. For more information, see [New subscription resource plans](/intl.en-US/Pricing/New subscription resource plans.md).

-   For more information about Kubernetes audit logs, see [Use Log Service to collect Kubernetes logs](/intl.en-US/User Guide for Kubernetes Clusters/Log management/Use Log Service to collect Kubernetes cluster logs.md).
-   For more information about Kubernetes event centers, see [Create and use a Kubernetes event center](/intl.en-US/Application/K8s Event Center/Create and use a Kubernetes event center.md).
-   For more information about Kubernetes access logs, see [Monitor and analyze the logs of nginx-ingress](/intl.en-US/User Guide for Kubernetes Clusters/Network management/Ingress management/Analyze logs of Ingress to monitor access to Ingress.md).

|-   Logstore
    -   k8s\_log
    -   k8s\_ingress\_log
-   Dashboard
    -   Kubernetes Audit Center Overview
    -   Kubernetes Event Center
    -   Kubernetes Resource Operations Overview
    -   Ingress Overview
    -   Ingress Access Center |

