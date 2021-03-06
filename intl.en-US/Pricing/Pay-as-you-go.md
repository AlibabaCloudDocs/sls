# Pay-as-you-go

After you activate Log Service, you are charged based on the pay-as-you-go billing method. This topic describes the details of the pay-as-you-go billing method.

For more information about the billable items and billing method of Log Service, see [t125823.dita\#concept\_xzl\_hjg\_vgb](/intl.en-US/Pricing/Billing overview.md). For more information about the pricing of Log Service, see [Log Service pricing](https://www.alibabacloud.com/product/log-service/pricing?spm=a3c0i.139163.9288850920.1.7690637avzyiqo).

## Billing period

A bill is generated every day at 00:00 based on the amount of resources that you consumed on the previous day. Fees are deducted from the balance of your Alibaba Cloud account on a daily basis.

## Free quotas

Log Service provides free quotas for some billable items. The following table shows the item details.

**Note:** Log Service offers monthly free quotas. These free quotas are cleared at the end of each month. If the amount of resources that you consume in a month is within the free quotas, you are not charged. Otherwise, you are charged for the excess amount.

|Billable item|Monthly free quota|
|:------------|:-----------------|
|Storage space occupied by log data|500 MB|
|Read and write traffic|500 MB|
|Indexes created for log data|500 MB|
|Read and write operations|1 million read and write operations|
|Active shards|31 shard days per monthFor more information, see [Why am I charged for active shards?](/intl.en-US/Pricing/FAQ/Why am I charged for active shards?.md) |

## Billing examples

-   Example 1: free quotas

    For example, you have a server that generates 10 MB of log data every day. You want to use Log Service to analyze the log data and use a Java program to subscribe to log processing events. The number of shards is one and the retention period is one day. Assume that the compression ratio is 5:1. The following table shows the billing details.

    |Billable item|Description|Monthly amount|Monthly fee|
    |-------------|-----------|--------------|-----------|
    |Active shards|Only one Logstore and one shard are created.|31 shard days|Free|
    |Read and write traffic|    -   The traffic generated by uploading logs to Log Service is 62 MB \(10 MB × 20% × 31 days = 62 MB\).
    -   The traffic generated by using a Java program to subscribe to log processing events is 62 MB \(10 MB × 20% × 31 days = 62 MB\).
|124 MB|Free|
    |Storage space occupied by log data|    -   Storage space occupied by log data is 62 MB \(10 MB × 20% × 31 days = 62 MB\).
    -   Log storage occupied by indexes is 310 MB \(10 MB × 31 days = 310 MB\).
|372 MB|Free|
    |Indexes created for log data|The full-text indexing feature is enabled and 10 MB of index traffic is generated per day.|310 MB|Free|
    |Read and write operations|The number of times that logs are uploaded to Log Service.|Less than 1 million read and write operations|Free|

-   Example 2: A website receives 100 million requests per day. Each request generates a 200-byte log. The size of logs that are generated per day is 18.6 GB. The website owner wants to upload the logs to Log Service and then analyze the logs. The number of shards is two and the retention period for each shard is three days. Assume that the compression ratio is 5:1 and no free quota is provided.

    You are charged USD 2.171985 per day. The following table shows the billing details.

    |Billable item|Description|Daily amount|Unit price|Daily fee|
    |-------------|-----------|------------|----------|---------|
    |Read and write traffic|The traffic generated by uploading logs to Log Service is 3.72 GB \(18.6 GB × 20% = 3.72 GB\).|3.72 GB|USD 0.045 per GB per day

|USD 0.1674 |
    |Indexes created for log data|The full-text indexing feature is enabled and 20 GB of index traffic is generated every day. The reserved fields generate 1.4 GB of index traffic.|20 GB|USD 0.0875 per GB per day

|USD 1.75 |
    |Storage space occupied by log data|    -   Storage space occupied by log data is 11.16 GB \(18.6 GB × 20% × 3 days = 11.16 GB\).
    -   Log storage occupied by creating indexes is 60 GB \(20 GB × 3 days = 60 GB\).
|71.16 GB|USD 0.002875 per GB per day

|USD 0.204585 |
    |Read and write operations|Logs are uploaded to Log Service about 1 million times.|1 million read and write operations|USD 0.03 per million operations per day

|USD 0.03 |
    |Active shards|The peak traffic is 6 MB/s and two shards are created.|2|USD 0.01 per shard per day

|USD 0.02 |

-   Example 3: An application generates 10 GB of time series data per day. The application uploads the data to Log Service by initiating 200,000 requests. Two shards are created and the retention period for each shard is three days. Assume that the compression ratio is 5:1 and no free quota is provided.

    You are charged 0.4169 per day. The following table shows the billing details.

    |Billable item|Description|Daily amount|Unit price|Daily fee|
    |-------------|-----------|------------|----------|---------|
    |Read and write traffic|The traffic generated by uploading time series data to Log Service is 2 GB \(10 GB × 20% = 2 GB\).|2 GB|USD 0.045 per GB per day

|USD 0.09 |
    |Indexes created for time series data|Log Service creates indexes for all fields.|10 GB|USD 0.02721 per GB per day

|USD 0.2721 |
    |Storage space occupied by time series data|    -   Storage space occupied by log data is 30 GB \(10 GB × 3 days = 30 GB\).
    -   Storage space occupied by indexes is 30 GB \(10 GB × 3 days = 30 GB\).
|60 GB|USD 0.00048 per GB per day

|USD 0.0288 |
    |Active shards|The peak traffic is 6 MB/s and two shards are created.|2|USD 0.01 per shard per day

|USD 0.02 |
    |Read and write operations|Logs are uploaded to Log Service about 200,000 times.|200,000 read and write operations|USD 0.03 per million operations per day

|USD 0.006 |


