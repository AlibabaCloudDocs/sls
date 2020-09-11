# Overview

This topic describes the scenarios, benefits, supported cloud services, and limits of the Log Audit Service application in the Log Service console.

The Log Audit Service application allows you to use all existing features of Log Service. In addition, the application allows you to collect and audit cloud service logs of multiple Alibaba Cloud accounts. You can store all logs in one project. You can also query and analyze the logs. You can integrate the Log Audit Service application with various Alibaba Cloud services. These services include audit services such as ActionTrail and Alibaba Cloud Container Service for Kubernetes \(ACK\), storage services such as Object Storage Service \(OSS\) and Apsara File Storage NAS, and network services such as Server Load Balancer \(SLB\) and API Gateway. The services also include database services such as ApsaraDB for RDS and PolarDB-X, and network security services such as Web Application Firewall \(WAF\), Cloud Firewall, and Security Center. You can also use the Log Audit Service application for other cloud services that support integration or on-premises security operations centers \(SOCs\).

![Log audit - 005](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5506498951/p101352.png)

## Background information

-   Log audit is a strict legal requirement.

    Log audit is becoming a necessity for enterprises around the world to better comply with applicable rules and regulations. For example, the Cybersecurity Law of the People's Republic of China came into effect in mainland China in 2017. In addition, the Baseline for Classified Protection of Cybersecurity 2.0 will come into effect in December 2020.

    ![Log audit - 001](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6506498951/p101330.png)

-   Log audit is the foundation for data security compliance of enterprises.

    A large number of enterprises have their own regulations and compliance teams to audit device operations, network behavior, and logs. You can use the Log Audit Service application to consume raw logs, audit logs, and generate compliance audit reports. If you have a Security Operations Center, you can consume logs in the Log Audit Service application or use Security Center to consume logs.

    ![Log audit - 002](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6506498951/p102203.png)

-   Log audit is crucial for data security and protection.

    Based on the M-Trends 2018 report published by FireEye, most enterprises, especially those in Asia Pacific, are vulnerable to cybersecurity attacks. The global median dwell time was 101 days in 2017. In Asia Pacific, the median dwell time was 498 days. Furthermore, the global median time for detection was 57.5 days in 2017. To shorten these time periods, enterprises require reliable log data, durable storage, and audit services.


## Scenarios

-   Log Service

    Log Service is a real-time logging service that provides data collection, cleansing, visualized search and analysis, and alert features. Log Service is applicable to DevOps, operation, security, and audit scenarios.

    ![Log audit - 003](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6506498951/p101349.png)

-   Typical log audit scenarios

    Log audit can be divided into the following four levels of requirements:

    ![Log audit - 004](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6506498951/p101350.png)

    -   Basic requirements: Most small and medium-sized enterprises require automatic log collection and storage. They must satisfy the minimum requirements that are specified in the Baseline for Classified Protection of Cybersecurity 2.0.
    -   Intermediate requirements: Multinational enterprises, large enterprises, and some medium-sized enterprises have multiple departments that use different Alibaba Cloud accounts and are charged in separate bills. However, these enterprises need to automatically collect logs of the accounts, send these logs to a centralized location, and then perform log audit. In addition to the basic requirements, these types of enterprises also need to collect logs and manage accounts in a centralized manner. Most of these enterprises have an audit system, so the demand for log audit is to be able to establish an easy connection in real time.
    -   Advanced requirements: Large enterprises that have dedicated audit compliance teams need to monitor, analyze, and configure alerts for logs. Some enterprises collect data to their audit systems for further operations. Other enterprises, especially those who want to build an audit system on the cloud, can use the search and analysis, alert, chart, and dashboard features provided by Log Service to audit log data.
    -   Top requirements: Most large enterprises that have professional audit compliance teams have security centers or audit systems. These enterprises need to integrate their systems with the Log Audit Service application and manage data in a centralized way.
    The Log Audit Service application of Log Service can satisfy all four levels of requirements.


## Benefits

-   Centralized collection
    -   Log collection across accounts: You can collect and send log data from multiple Alibaba Cloud accounts to the project of a single Alibaba Cloud account.
    -   Ease of use: You can use the Log Audit Service application to collect logs in real time from Alibaba Cloud services that belong to different accounts. You only need to create a configuration file to automatically collect logs in real time when new resources \(such as RDS instances, SLB instances, or OSS buckets\) are detected.
    -   Centralized storage: Logs are collected and stored in a single project. This improves efficiency when you perform operations such as data search and analysis, data visualization, alert configuration, and secondary development.
-   Support for multiple Log Service features
    -   The Log Audit Service application supports all existing features of Log Service such as data search, analysis, transformation, dashboards, alerts, and log exports. You can also audit logs that are collected from multiple accounts in a centralized manner.
    -   You can integrate the Log Audit Service application with Alibaba Cloud services, open-source software, and third-party SOC software to extract more value from log data.

## Supported cloud services

You can integrate the Log Audit Service application with various Alibaba Cloud services. These services include audit services such as ActionTrail and ACK, storage services such as OSS and NAS, network services such as SLB and API Gateway, database services such as RDS and PolarDB-X, and network security services such as WAF, Cloud Firewall, and Security Center. You can also use the application for other cloud services that support integration or on-premises SOCs.

|Alibaba Cloud service|Logs to be audited|Regions|Prerequisites|Logstore name|Dashboard name|
|---------------------|------------------|-------|-------------|-------------|--------------|
|ActionTrail|RAM logon logs, operations logs of Alibaba Cloud resources, and logs generated by using the API|All available regions|None|actiontrail\_log|-   ActionTrail Audit Center
-   ActionTrail Core Configuration Center
-   ActionTrail Login Center |
|Server Load Balancer \(SLB\)|Layer 7 logs of HTTP or HTTPS listeners|All available regions|None|slb\_log|-   SLB Audit Log
-   SLB Access Log
-   SLB Overall View |
|API Gateway|Access logs|All available regions|None|apigateway\_log|API Gateway Audit Center|
|Web Application Firewall \(WAF\)|Access logs and attack logs|All available regions|-   The Pro Edition or later.
-   **Log Service** is purchased in the WAF console.

|waf\_log|-   WAF Audit Center
-   WAF Security Center
-   WAF Access Center |
|Security Center|Seven types of host logs, four types of network logs, and three types of security logs|All available regions|-   Enterprise Edition
-   The **Log Analysis** feature is activated in the Security Center console.

|sas\_log|-   Hosts
    -   SAS Login Center
    -   SAS Process Center
    -   SAS Connection Center
-   Networking
    -   SAS Session Center
    -   SAS DNS Center
-   Security
    -   SAS Web Access Center
    -   SAS HC Center
    -   SAS Alarm Center |
|Cloud Firewall|Network access logs|All available regions|-   The Pro Edition or higher.
-   For information about the additional fees that are incurred after you activate the **Log Analysis** feature in the Cloud Firewall console.

|cloudfirewall\_log|Cloud Firewall Audit Center|
|Bastionhost|Operations logs of O&M personnel|China \(Hangzhou\), China \(Shanghai\), China \(Heyuan\), and China \(Chengdu\)|Version 3.2 or later.|baston\_log|None|
|Object Storage Service \(OSS\)|Resources operations logs, data operations logs, data access logs, metering logs, deletion logs of expired files, and CDN back-to-origin traffic logs|All available regions|None|oss\_log|-   OSS Audit Center
-   OSS Access Center
-   OSS Operation Center
-   OSS Performance Center
-   OSS Overall View |
|ApsaraDB for RDS|Audit logs of ApsaraDB RDS for MySQL, SQL Server, and PostgreSQL databases|-   China \(Qingdao\), China \(Beijing\), China \(Hohhot\), China \(Hangzhou\), China \(Shanghai\), China \(Shenzhen\), China \(Chengdu\), and China \(Hong Kong\)
-   Singapore \(Singapore\), Malaysia \(Kuala Lumpur\), Indonesia \(Jakarta\), Germany \(Frankfurt\), Australia \(Sydney\), India \(Mumbai\), and Japan \(Tokyo\)

|-   MySQL is not supported in the Basic Edition of ApsaraDB RDS.
-   PostgreSQL and SQL Server are supported in all available editions of ApsaraDB RDS.
-   SQL Explorer of related databases is enabled. The feature is automatically enabled.

|rds\_log|-   RDS Audit Center
-   RDS Security Center
-   RDS Performance Center
-   RDS Overall View |
|PolarDB-X|Audit logs|China \(Qingdao\), China \(Shenzhen\), China \(Shanghai\), China \(Beijing\), China \(Hangzhou\), China \(Zhangjiakou-Beijing Winter Olympics\), China \(Chengdu\), and China \(Hong Kong\)|Shared instances are not supported.|drds\_log|-   PolarDB-X Operation Center
-   PolarDB-X Security Center
-   PolarDB-X Performance Center |
|Apsara File Storage NAS|Access logs|All available regions|None|nas\_log|-   NAS Summary
-   NAS Audit Center
-   NAS Detail |
|Alibaba Cloud Mobile Push|Callback events|Mainland China|None|cps\_log|-   Android Callback Center
-   iOS Callback Center |
|Alibaba Cloud Container Service for Kubernetes \(ACK\)|-   Kubernetes Audit Log
-   Kubernetes Event Center
-   Ingress Access Log

|China \(Shanghai\), China \(Beijing\), China \(Hangzhou\), China \(Shenzhen\), China \(Hohhot\), China \(Zhangjiakou-Beijing Winter Olympics\), China \(Chengdu\), and China \(Hong Kong\)|You must manually enable the log collection feature for Kubernetes log collection.**Note:** You must use the automatically created project named k8s-log-\{ClusterID\}. Custom projects are not supported.

-   Kubernetes Audit Log. For more information, see [Use Log Service to collect Kubernetes logs](/intl.en-US/User Guide for Kubernetes Clusters/Log management/Use Log Service to collect Kubernetes cluster logs.md).
-   Kubernetes Event Center. For more information, see [Create and use a Kubernetes event center](/intl.en-US/Application/K8s Event Center/Create and use a Kubernetes event center.md).
-   Ingress Access Log. For more information, see [Monitor and analyze the logs of nginx-ingress](/intl.en-US/User Guide for Kubernetes Clusters/Network management/Ingress management/Analyze logs of Ingress to monitor access to Ingress.md).

|-   k8s\_log
-   k8s\_ingress\_log

|-   Kubernetes Audit Center Overview
-   Kubernetes Event Center
-   Kubernetes Resource Operations Overview
-   Ingress Overview
-   Ingress Access Center |

## Limits

-   Storage methods and regions
    -   Centralized project storage

        Log data collected from multiple Alibaba Cloud accounts across multiple regions is stored in a project that belongs to an Alibaba Cloud account. The project can reside in the following regions:

        -   China \(Beijing\), China \(Hohhot\), China \(Hangzhou\), China \(Shanghai\), and China \(Shenzhen\)
        -   Singapore \(Singapore\), and Japan \(Tokyo\)
    -   Regional project storage

        The regional project storage feature is applicable only to SLB, OSS, and PolarDB-X. The Log Audit Service application allows you to store access logs across multiple projects of an Alibaba Cloud account. These projects can reside in the same regions as the services from which logs are collected. For example, you can store the access logs of an OSS bucket that resides in the China \(Hangzhou\) region to a project that resides in the same region.

    -   Synchronize log data to one Logstore

        You can synchronize the Logstores of regional projects to a central Logstore. This improves efficiency when you perform operations such as data search and analysis, data visualization, alert configuration, and secondary development.

        The synchronization mechanism relies on the data transformation feature of Log Service. All available regions support log data synchronization except for the China \(Qingdao\) and China \(Heyuan\) regions.

-   Resource limits
    -   The name of the project that is used for centralized storage is in the format of slsaudit-center-<Alibaba Cloud account ID\>-<Region\>, for example, slsaudit-center-1234567890-cn-beijing. You cannot use the Log Service console to delete a project that is used for centralized storage. However, you can delete the project by using the command line tool or by using the API.
    -   The names of the projects that store access logs of SLB instances, OSS buckets, and PolarDB-X instances respectively are in the format of slsaudit-region--<Alibaba Cloud account ID\>-<Region\>, for example, slsaudit-region-1234567890-cn-beijing. You cannot use the Log Service console to delete regional projects. However, you can delete the project by using the command line tool or by using the API.
    -   After you create Logtail configurations for log data collection from cloud services, the Log Audit Service application creates one or more dedicated Logstores for data storage. You can manage the Logstores as normal Logstores. However, the dedicated Logstores have the following limits:
        -   You cannot perform write operations on dedicated Logstores. This prevents data tampering.
        -   You can modify the retention period of log data and delete the dedicated Logstores only on the Audit Configurations page of the Log Audit Service application or by using the API.
        -   If you turn on the **Synchronization to Central Project** switch for services that support regional project storage, a data transformation task is generated in the regional projects. These tasks synchronize the logs of the activated services in the regional projects to the specified central project.
            -   The task name is Internal Job: SLS Audit Service Data Sync for OSS Access, or Internal Job: SLS Audit Service Data Sync for SLB.
            -   You can disable the task only on the Audit Configurations page of the Log Audit Service application or by using the API.
            -   If you turn on the **Synchronization to Central Project** switch for SLB, OSS, or PolarDB-X, the dedicated Logstores that are used for log data storage become dedicated to data synchronization. You cannot perform operations on these Logstores. If you want to query and analyze log data, you can perform these operations on the Logstore that is used for centralized storage.

## Billing

-   Log Service

    You must activate Log Service and the Log Audit Service application for the Alibaba Cloud account that is used for centralized storage of log data collected from other Alibaba Cloud accounts. By default, Log Service on other Alibaba Cloud accounts is not activated except for those Alibaba Cloud accounts whose services have modules on which the Log Audit Service application depends. You are not billed for Log Service if it is not activated. When you use the Log Audit Service application, you are billed for the data storage and read/write traffic based on the pay-as-you-go method. .

    You can use free resource quotas and resource plans to offset costs.

-   Alibaba Cloud services

    Additional fees may be incurred after you activate the Log Audit Service application and the corresponding log collection or analysis feature for the relevant Alibaba Cloud services. The following table describes the additional fees for Alibaba Cloud services.

    |Alibaba Cloud service|Additional fee|
    |---------------------|--------------|
    |WAF|For information about the additional fees that may be incurred after you activate the **Log Service** feature in the WAF console, see [Billing](/intl.en-US/Log Management/Log service/Billing.md).|
    |Security Center|For more information about the additional fees that may be incurred after you activate the **Log Analysis** feature in the Security Center console, see [Billing methods](/intl.en-US/Pricing/Billing methods.md).|
    |Cloud Firewall|For information about the additional fees incurred after you activate the **Log Analysis** feature in the Cloud Firewall console, see [Log analysis billing method](/intl.en-US/Logs/Log analysis/Log analysis billing method.md).|
    |ApsaraDB for RDS|After you configure log audit for ApsaraDB for RDS, SQL Explorer is automatically enabled on the ApsaraDB for RDS instances that satisfy the requirements. For more information about pricing, see [Pricing, billable items, and billing methods](/intl.en-US/Purchase Guide/Pricing, billable items, and billing methods.md).|


