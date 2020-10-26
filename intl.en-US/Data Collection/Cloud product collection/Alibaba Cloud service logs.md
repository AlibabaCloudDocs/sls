# Alibaba Cloud service logs

Log Service can collect logs from multiple types of Alibaba Cloud services, such as elastic computing, storage, security, and database services. The logs record operational statistics, such as user operations, running statuses, and business dynamics of Alibaba Cloud services.

The following tables describe the Alibaba Cloud services from which Log Service can collect logs.

**Note:** Log Service uses the Log Audit Service application to collect logs of the following Alibaba Cloud services across accounts: ActionTrail, Alibaba Cloud Container Service for Kubernetes \(ACK\), Object Storage Service \(OSS\), Apsara File Storage NAS, Server Load Balancer \(SLB\), API Gateway, ApsaraDB for RDS, PolarDB-X, Web Application Firewall \(WAF\), Cloud Firewall, Security Center \(SAS\), Alibaba Cloud Mobile Push, and Bastionhost \(BH\). For more information, see [Log Audit Service](/intl.en-US/Application/Log Audit Service/Overview.md).

-   Elastic computing

    |Alibaba Cloud service|Log|Project and Logstore|Dashboard|
    |---------------------|---|--------------------|---------|
    |Elastic Compute Service \(ECS\)|Full log|Custom project and LogstoreLogs are collected by using Logtail. For more information, see [Logtail overview](/intl.en-US/Data Collection/Logtail collection/Text logs/Overview.md).

|Custom dashboard|
    |ACK|    -   Kubernetes audit log
    -   Kubernetes event center
    -   Ingress log
|Custom project and LogstoreLogs are collected by using Logtail. For more information, see [Container log collection](/intl.en-US/Data Collection/Logtail collection/Container log collection/Overview.md).

|Custom dashboard|
    |Function Compute|[Execution log](/intl.en-US/Data Collection/Cloud product collection/Function Compute execution logs/Overview.md)|    -   Project: aliyun-fc-region ID-bb47c1e4-cdd4-5318-978e-d748952652c8
    -   Logstore: function-log
|Custom dashboard|

-   Storage

    |Alibaba Cloud service|Log|Project and Logstore|Dashboard|
    |---------------------|---|--------------------|---------|
    |OSS|[Access log](/intl.en-US/Data Collection/Cloud product collection/OSS access logs/Usage notes.md)|    -   Project: oss-log-Alibaba Cloud account ID-region ID
    -   Logstore: oss-log-store
|    -   Access Center
    -   Audit Center
    -   Operation Center
    -   Performance Center |
    |NAS|[Access log](/intl.en-US/Data Collection/Cloud product collection/NAS access logs/Usage notes.md)|    -   Project: nas-Alibaba Cloud account ID-region ID
    -   Logstore: nas-nfs
|    -   nas-nfs-nas\_summary\_dashboard\_cn
    -   nas-nfs-nas\_audit\_dashboard\_cn
    -   nas-nfs-nas\_detail\_dashboard\_cn |

-   Security

    |Alibaba Cloud service|Log|Project and Logstore|Dashboard|
    |---------------------|---|--------------------|---------|
    |Anti-DDoS Origin|[Full log](/intl.en-US/Data Collection/Cloud product collection/Anti-DDoS Pro logs/Usage notes.md)|    -   Anti-DDoS Origin instances in mainland China
        -   Project: ddos-pro-project-Alibaba Cloud account ID-cn-hangzhou
        -   Logstore: ddos-pro-logstore
    -   Anti-DDoS Origin instances outside mainland China
        -   Project: ddos-pro-Alibaba Cloud account ID-ap-southeast-1
        -   Logstore: ddos-pro-logstore
|    -   DDoS Operation Center
    -   DDoS Access Center |
    |Anti-DDoS Pro and Anti-DDoS Premium|[Full log](/intl.en-US/Data Collection/Cloud product collection/Anti-DDoS Pro and Anti-DDoS Premium logs/Usage notes.md)|    -   Anti-DDoS Pro
        -   Project: ddoscoo-project-Alibaba Cloud account ID-cn-hangzhou
        -   Logstore: ddoscoo-logstore
    -   Anti-DDoS Premium
        -   Project: ddosdip-project-Alibaba Cloud account ID-ap-southeast-1
        -   Logstore: ddoscoo-logstore
|    -   DDoS Operation Center
    -   DDoS Access Center |
    |SAS|    -   [Security log](/intl.en-US/Data Collection/Cloud product collection/Security Center logs/Usage notes.md)
        -   Vulnerability log
        -   Baseline log
        -   Security alert log
    -   Network log
        -   DNS log
        -   Local DNS log
        -   Network session log
        -   Web access log
    -   Host log
        -   Process startup log
        -   Network connection log
        -   Logon log
        -   Brute-force attack log
        -   Process snapshot log
        -   Account snapshot log
        -   Port snapshot log
|    -   Project: sas-log-Alibaba Cloud ID-region ID
    -   Logstore: sas-log
|    -   Network log
        -   DNS Access Center
        -   Network Session Center
        -   Web Access Center
    -   Host log
        -   Login Center
        -   Process Center
        -   Connection Center
    -   Security log
        -   Baseline Center
        -   Vulnerability Center
        -   Alert Center |
    |WAF|    -   [Access log](/intl.en-US/Data Collection/Cloud product collection/WAF logs/Usage notes.md)
    -   Attack log
|    -   WAF instances in mainland China
        -   Project: waf-project-Alibaba Cloud account ID-cn-hangzhou
        -   Logstore: waf-logstore
    -   WAF instances outside mainland China
        -   Project: waf-project-Alibaba Cloud account ID-ap-southeast-1
        -   Logstore: waf-logstore
|    -   Operation Center
    -   Access Center
    -   Security Center |
    |Cloud Firewall|[Internet traffic log](/intl.en-US/Data Collection/Cloud product collection/Cloud Firewall logs/Usage notes.md)|    -   Project: cloudfirewall-project-Alibaba Cloud account ID-cn-hangzhou
    -   Logstore: cloudfirewall-logstore
|Report|

-   Networking

    |Alibaba Cloud service|Log|Project and Logstore|Dashboard|
    |---------------------|---|--------------------|---------|
    |SLB|[Layer-7 access log](/intl.en-US/Data Collection/Cloud product collection/SLB layer-7 access logs/Usage notes.md)|Custom project and Logstore|    -   slb-user-log-slb\_layer7\_operation\_center\_cn
    -   slb-user-log-slb\_layer7\_access\_center\_cn |
    |Virtual Private Cloud \(VPC\)|[Flow log](/intl.en-US/Data Collection/Cloud product collection/VPC flow logs/Usage notes.md)|Custom project and Logstore|    -   Logstore Name-vpc\_flow\_log\_traffic\_cn
    -   Logstore Name-vpc\_flow\_log\_rejection\_cn
    -   Logstore Name-vpc\_flow\_log\_overview\_cn |
    |Elastic IP Address \(EIP\)|[Internet traffic log](/intl.en-US/Data Collection/Cloud product collection/EIP logs/Overview.md)|Custom project and Logstore|eip\_monitoring|
    |API Gateway|[Access log](/intl.en-US/Data Collection/Cloud product collection/API Gateway access logs/Usage notes.md)|Custom project and Logstore|Logstore Name\_apigateway\_access\_log|

-   Database

    |Alibaba Cloud service|Log|Project and Logstore|Dashboard|
    |---------------------|---|--------------------|---------|
    |ApsaraDB for RDS|[SQL audit log](/intl.en-US/Data Collection/Cloud product collection/RDS SQL execution logs/Usage notes.md)|Custom project and Logstore|    -   RDS Audit Operation Center
    -   RDS Audit Performance Center
    -   RDS Audit Security Center |
    |ApsaraDB for Redis|    -   [Audit log](/intl.en-US/Data Collection/Cloud product collection/ApsaraDB for Redis logs/Overview.md)
    -   Slow query log
    -   Operational log
|    -   Project: nosql-Alibaba Cloud account ID-region ID
    -   Logstore:
        -   redis\_audit\_log
        -   redis\_slow\_run\_log
|    -   Redis Audit Center
    -   Redis Slow Log Center |
    |ApsaraDB for MongoDB|    -   [Audit log](/intl.en-US/Data Collection/Cloud product collection/MongoDB logs/Overview.md)
    -   Slow query log
    -   Operational log
|    -   Project: nosql-Alibaba Cloud account ID-region ID
    -   Logstore:
        -   mongo\_audit\_log
        -   mongo\_slow\_run\_log
|Mongo Audit Log Center|

-   Management and monitoring

    |Alibaba Cloud service|Log|Project and Logstore|Dashboard|
    |---------------------|---|--------------------|---------|
    |ActionTrail|[ActionTrail access log](/intl.en-US/Data Collection/Cloud product collection/ActionTrail access logs/Usage notes.md)|    -   Project: custom project
    -   Logstore: actiontrail\_trail name
|actiontrail\_trail name\_audit\_center\_cn|
    |[Operations log](/intl.en-US/Data Collection/Cloud product collection/Inner-ActionTrail/Usage notes.md)|Custom project and Logstore|actiontrail\_trail name\_audit\_center\_cn|

-   Cloud communication

    |Alibaba Cloud service|Log|Project and Logstore|Dashboard|
    |---------------------|---|--------------------|---------|
    |Message Service \(MNS\)|[Operations log](/intl.en-US/Data Collection/Cloud product collection/MNS operations logs/Overview.md)|Custom project and Logstore|Custom dashboard|

-   Internet of Things \(IoT\)

    |Alibaba Cloud service|Log|Project and Logstore|Dashboard|
    |---------------------|---|--------------------|---------|
    |IoT Platform|[IoT Platform log](/intl.en-US/Data Collection/Cloud product collection/IoT Platform logs/Usage notes.md)|    -   Project: iot-log-Alibaba Cloud account ID-region ID
    -   Logstore: iot\_logs
|IoT Operation Center|


