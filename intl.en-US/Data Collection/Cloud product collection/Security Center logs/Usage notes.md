# Usage notes

The log analysis feature of Security Center allows you to collect, query, analyze, transform, and consume risk data in real time. You can use the log analysis feature to monitor and handle potential risks and implement centralized management of cloud resources. This topic describes the assets, billing, and limits of the log analysis feature in Security Center.

## Assets

-   Dedicated projects and dedicated Logstores

    After you enable the log analysis feature of Security Center, Log Service creates a project named sas-log-Alibaba Cloud account ID-region name and a Logstore named sas-log.

    **Note:** If you accidentally delete the dedicated Logstore, you are prompted that the sas-log Logstore does not exist. All log data in the Logstore is deleted. In this case, you must [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm?spm=a2796.7919406.0.dcontactus3.676a2d23RjosdV#/ticket/add/?productId=1210) to reset the log analysis feature. You must re-enable the log analysis feature after it is reset. The deleted data cannot be recovered.

-   Dedicated dashboards

    Security Center logs are classified into 3 types and 14 subtypes. After you enable the log analysis feature of Security Center, Log Service generates nine dashboards by default.

    |Log type|Dashboard|Description|
    |--------|---------|-----------|
    |Network log|DNS Access Center|Provides an overview of the DNS queries on the server. The metrics displayed on the dashboard include the success rate of external DNS queries, the distributions of internal and external DNS queries, and the trends of internal and external DNS queries.|
    |Network Session Center|Provides an overview of resource-related network sessions. The metrics displayed on the dashboard include the trend of network sessions and the distributions of network protocols, source and destination IP addresses, and relevant resources.|
    |Web Access Center|Provides an overview of external HTTP requests and access to the web services of a host. The metrics displayed on the dashboard include the request success rate, access trends, success efficiency, distribution of accessed domain names, and other related distributions.|
    |Host log|Login Center|Provides an overview of the logon information of hosts. The metrics displayed on the dashboard include the geographic distribution of source and destination IP addresses, logon trends and ports, and the distribution of logon methods.|
    |Process Center|Provides an overview of the startup of host processes. The metrics displayed on the dashboard include the trend of process startup and the distributions of processes, process types, and specific bash or Java processes.|
    |Connection Center|Provides an overview of the network connections of hosts. The metrics displayed on the dashboard include the trends and distributions of network connections, source hosts, and destination hosts.|
    |Security log|Baseline Center|Provides an overview of baseline checks. The metrics displayed on the dashboard include the distribution of pending issues, trend of new issues or resolved issues, and status of issues.|
    |Vulnerability Center|Provides an overview of vulnerabilities. The metrics displayed on the dashboard include the distribution of vulnerabilities, number of new vulnerabilities, number of vulnerabilities that are under verification, and number of vulnerabilities that are being fixed.|
    |Alert Center|Provides an overview of security alerts. The metrics displayed on the dashboard include the trend, distribution, and status of new and cleared alerts.|


## Billing

-   You are not billed for the statistics of read and write traffic, indexing traffic, storage space, number of shards, and number of read and write operations in the dedicated Logstore. You are billed when you transform logs, ship logs, and read data from the Internet. For more information, see [Log Service pricing](https://www.alibabacloud.com/product/log-service/pricing?spm=a3c0i.139163.9288850920.1.7690637avzyiqo).
-   You are billed for the log analysis feature after you enable the feature in the Security Center console. For more information, see [Billing](/intl.en-US/Pricing/Billing.md).

## Limits

-   You can write only log data of Security Center to a dedicated Logstore.
-   As required by the Cyber Security Law of the People’s Republic of China, logs are retained for at least six months. We recommend that you allocate a storage capacity of 30 GB to each server.

