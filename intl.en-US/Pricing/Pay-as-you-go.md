# Pay-as-you-go

Log Service supports the pay-as-you-go billing method that charges you based on the amount of resources used. The default billing method is pay-as-you-go after you activate Log Service. This topic describes the billing details of pay-as-you-go.

For more information about the billing items and billing method of Log Service, see [t125823.dita\#concept\_xzl\_hjg\_vgb](/intl.en-US/Pricing/Billing overview.md). For more information about the pricing of Log service, see[Log Service pricing](https://www.alibabacloud.com/product/log-service/pricing?spm=a3c0i.139163.9288850920.1.7690637avzyiqo).

## Billing period

Fees are deducted from the balance of your Alibaba Cloud account on a daily basis. A bill is generated every day based on the amount of resources that you used on the previous day.

## Billing examples

-   Example 1: free quotas

    You have three servers. Each server generates 5 MB of log data every day. You want to use Log Service to perform the following operations.

    1.  Query logs in real time and create a dashboard to visualize data query and analysis results.
    2.  Use a Java program to subscribe to log processing events and track log processing results in real time.
    |Billing item|Description|
    |------------|-----------|
    |Shard|Only one Logstore and one shard are created for 31 days in a month. This billing item is within the free quota.**Note:** By default, two shards are created when you create a Logstore. You can change this number to 1 to make sure that the number of active shards is within the free quota. |
    |Read/write traffic|A total of 15 MB of raw logs are generated every day. Assume that the compression ratio is 20%. The daily read/write traffic is 6 MB \(15 MB × 20% × 2 = 6 MB\). The monthly cumulative read/write traffic is 186 MB \(6 MB/day × 31 days = 186 MB\). This billing item is within the free quota.|
    |Indexing traffic \(logs\)|The indexing traffic is 465 MB \(15 MB/day × 31 days = 465 MB\). This billing item is within the free quota.|
    |Read/write operations|The number of monthly read/write operations is less than 1 million. This billing item is within the free quota.|

-   Example 2: real-time computing and offline computing

    A website receives 100 million API requests per day. Each request generates a 200-byte log. The size of logs generated per day is 20 GB. The peak traffic is 1.16 MB/s, which is five times the average traffic. The lifecycle of these logs is two days. They are read once a day for real-time computing, and shipped to OSS for offline computing.

    The fee incurred for one day is USD 3.3375:\(USD 0.01 + USD 0.09 + USD 0.09 + USD 0.0115 + USD 0.136 + USD 3 = USD 3.3375\). The following table shows the billing details.

    |Billing item|Description|
    |------------|-----------|
    |Shard|Reserve a shard and you are chargedUSD 0.01 for the shard.|
    |Read/write traffic|A total of 20 GB of raw logs are written every day. Assume that the compression ratio is 10%. The daily write traffic is 2 GB and you are charged USD 0.09\(2 GB × USD 0.045/GB = USD 0.09\).|
    |The same amount of log data is pulled for real-time computing. The fee isUSD 0.09.|
    |The storage fee is USD 0.0115\(2 GB × 2 days × USD 0.002875/GB/day = USD 0.0115\).|
    |The fee for shipping logs to OSS in the CSV format is USD 0.136\(20 GB × USD 0.0068/GB = USD 0.136\).|
    |API requests|The fee incurred for 100 million requests is USD 3\(USD 0.00000003/time × 100,000,000 times = USD 3\).|

-   Example 3: log indexing and online queries

    A cloud service receives 1 million API requests per day. Each request generates a 200-byte log. The size of logs generated per day is 200 MB. These logs are stored in Log Service for 30 days so that you can query data of the last 30 days.

    The fee incurred for one day is USD 0.0690625\(USD 0.0175 + USD 0.0215625 + USD 0.03 = USD 0.0690625\). The following table shows the billing details.

    |Billing item|Description|
    |------------|-----------|
    |Indexing traffic \(logs\)|Create indexes for all fields. The indexing fee is USD 0.0175\(0.2 GB × USD 0.0875/GB = USD 0.0175\).|
    |Storage space \(logs\)|200 MB +50 MB \(indexing traffic\) = 250 MB. The maximum storage space is 7.5 GB \(0.25 GB × 30 days = 7.5 GB\). The maximum daily storage fee is USD 0.0215625\(7.5 GB × 0.002875 USD/GB/day = USD 0.0215625\). Indexing traffic is compressed from raw logs. The compression ratio is 25%. |
    |API requests|The fee incurred for 1 million requests is USD 0.03\(USD 0.00000003/time × 1,000,000 times = USD 0.03\).|

-   Example 4: time series data storage

    A cloud service writes 400 MB of time series data to Log Service per day. The data is stored for 15 days.

    The fee incurred for one day is USD 0.014124\(USD 0.010884 + USD 0.00324 = USD 0.014124\). The following table shows the billing details.

    |Billing item|Description|
    |------------|-----------|
    |Indexing traffic \(time series\)|Create indexes for all fields. The indexing fee is USD 0.010884\(0.4 GB × 0.02721 USD/GB = USD 0.010884\).|
    |Storage space \(time series\)|400 MB + 50 MB \(indexing traffic\) = 450 MB. The maximum storage space is 6.75 GB \(0.45 GB × 15 days = 6.75 GB\). The maximum daily storage fee is USD 0.00324\(6.75 GB × 0.00048 USD/GB/day = USD 0.00324\). Indexing traffic is compressed from raw logs. The compression ratio is 12.5%. |


