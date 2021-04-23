# Billable items and billing method

You are charged for all billable items in Log Service. For example, when you store logs, you are charged for used storage space. When you collect logs, you are charged for write traffic. This topic describes the billable items and billing method of Log Service.

**Note:**

-   For more information about the pricing of Log Service, see [Log Service pricing](https://www.alibabacloud.com/product/log-service/pricing?spm=a3c0i.139163.9288850920.1.7690637avzyiqo).
-   For information about billing-related FAQ, see [FAQ about billing](/intl.en-US/Pricing/FAQ/FAQ about billing.md).

## Billing cycle

The amount of consumed resources is calculated on a daily basis in Log Service. You are charged based on the amount of consumed resources.

## Billing method

Log Service supports the pay-as-you-go billing method. When you use this billing method, you are charged based on the amount of consumed resources. Log Service offers monthly quotas free of charge. For more information, see [Pay-as-you-go](/intl.en-US/Pricing/Pay-as-you-go.md).

## Billable items

The following table describes the billable items of Log Service.

**Note:**

-   You can use Log Service to collect log data and time series data. The pricing of stored data and indexes for time series data is different from the pricing of stored data and indexes for log data. However, the pricing of other billable items for time series data is the same as the pricing of other billable items for log data. The billable items include data transformation, data shipping, read and write traffic, and number of requests.
-   You can view the following statistics of the previous day in the [Log Service console](https://sls.console.aliyun.com): write traffic, read traffic, number of read and write operations, transformation traffic, shipping traffic, and used storage space.
-   When you use Log Service to collect logs, log data is automatically compressed. The compression ratio is 10:1 to 5:1.

![Billable items](../images/p238972.png)

|Billable item|Description|Billing formula|
|-------------|-----------|---------------|
|Storage space occupied by log data|The storage space is the total size of compressed log data and indexed log data. For example, the volume of raw log data that is uploaded to Log Service is 1 GB, and indexes are created for two fields. The compression ratio is 5:1, and the size of indexes that are created for the two fields is 0.5 GB. In this case, the storage space occupied by log data is 0.7 GB \(0.2 GB + 0.5 GB = 0.7 GB\).

|Fee of the storage space occupied by log data = Used storage space per day × Price per GB of log data|
|Storage space occupied by time series data|The storage space is the total size of raw time series data and indexed time series data. For example, the volume of raw time series data that is uploaded to Log Service is 1 GB, and indexes are automatically created. In this case, the storage space occupied by time series data is 2 GB \(1 GB + 1 GB = 2G GB\).

|Fee of the storage space occupied by time series data = Used storage space per day × Price per GB of time series data|
|Read and write traffic|Read and write traffic includes write traffic and read traffic.-   Write traffic: Write traffic is calculated based on the compressed data that is uploaded to Log Service.

For example, 10 GB of raw data is uploaded to Log Service and the compression ratio is 5:1. In this case, the write traffic is 2 GB.

-   Read traffic: Read traffic is calculated based on the compressed data that is transformed, shipped to AnalyticDB or TSDB, or consumed.

For example, 10 GB of raw data is uploaded to Log Service and then shipped to TSDB, and the compression ratio is 5:1. In this case, the write traffic is 2 GB and the read traffic is 2 GB.


**Note:** The consumption preview feature in the Log Service console can also be used to generate a small amount of read traffic.

|Fee of the read and write traffic = \(Write traffic per day + Read traffic per day\) × Price per GB of traffic|
|Index traffic of log data|The index traffic is calculated based on the indexes that are created or recreated for raw log data. Indexes are created for fields. The size of index traffic depends on the length of indexed fields and field values. -   For example, 1 GB of raw log data is written to Log Service and the full-text indexing feature is enabled. In this case, the index traffic is 1 GB.
-   For example, 1 GB of raw log data is written to Log Service and indexes are created for two fields whose data length is 0.5 GB. In this case, the index traffic is 0.5 GB.

**Note:**

-   By default, the indexing feature is disabled. If you use this feature, you are charged for index traffic. Indexes occupy storage space. Therefore, you are charged for the occupied storage space.
-   If you create both full-text indexes and field indexes, index traffic is calculated only once.
-   Log Service automatically creates indexes for reserved fields such as \_\_time\_\_ and \_\_source\_\_. This generates a small amount of index traffic. For more information, see [Reserved fields](/intl.en-US/Product Introduction/Limits/Reserved fields.md).

|Fee of the index traffic of log data = Index traffic per day × Price per GB of index traffic|
|Index traffic of time series data|The index traffic is calculated based on the indexes that are created for raw time series data. Indexes are created for fields. The size of index traffic depends on the length of indexed fields and field values. When you upload time series data, Log Service automatically creates indexes for the time series data.

For example, 1 GB of raw time series data is written to Log Service. In this case, the index traffic is 1 GB.

|Fee of the index traffic of time series data = Index traffic per day × Price per GB of index traffic|
|Read traffic over the Internet|-   If the data collected by Log Service is read and consumed by applications outside the Alibaba Cloud internal network, you are charged for the generated data traffic. This traffic is calculated based on the compressed data.
-   If data is transformed across regions, you are charged for the generated data traffic. This traffic is calculated based on the transformed data that is compressed.

|Fee of the read traffic over the Internet = Read traffic over the Internet per day × Price per GB of read traffic over the Internet|
|Data transformation|You are charged for the transformed data that is not compressed. The **data transformation** billable item incurs fees based on the transformed data. When you use the data transformation feature, you are also charged for the following resources:

-   When you transform data, network resources are consumed and the API operation is called to read data. In this case, you are charged based on the generated read traffic and the number of read and write operations.
-   If data is transmitted across regions for data transformation, read traffic over the Internet is generated.

|Data transformation fee = Transformed data per day × Price per GB of transformed data|
|Data shipping|You are charged for the shipped data that is not compressed. You can ship data to Object Storage Service \(OSS\), AnalyticDB, and TSDB. **Note:** When you ship data to AnalyticDB or TSDB, network resources are consumed and the API operation is called to read data. In this case, you are charged based on the generated read traffic and the number of read and write operations.

|Data shipping fee = Shipped data per day × Price per GB of shipped data|
|Read and write operations|-   When you upload data to Log Service, you are charged based on the number of write operations. The number of write operations depends on the speed at which the data is generated. Log Service automatically minimizes the number of write operations.
-   When data is transformed, shipped to AnalyticDB or TSDB, or consumed, Log Service reads the data in batches. You are charged based on the number of read operations.

**Note:** The number of read and write operations is calculated regardless of whether these operations succeed.

|Fee of the read and write operations = Number of read and write operations per day × Price per operation|
|Active shards|Only shards in the readwrite state incur fees. Merged or split shards do not incur fees. For example, you need to merge three shards that are in the readwrite state. After you merge these shards, one shard is in the readwrite state, and the other two shards become read-only. On the day that you merge these three shards, you are charged for three shards. On the next day, you are charged for only one shard.

**Note:** By default, two shards are created when you create a Logstore. For more information about FAQ about shards, see [Why am I charged for active shards?](/intl.en-US/Pricing/FAQ/Why am I charged for active shards?.md)

|Rental fee of the active shards = Number of shards in the readwrite state × Price per shard|

