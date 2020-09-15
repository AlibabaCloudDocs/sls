# New subscription resource plans and billing items

This topic describes the new subscription resource plans and billing items of Log Service.

## Features and billing items

Log Service provides the time series data storage, data transformation and shippingfeatures. This section describes the features and billing items.

-   Effective date: You are charged for using these features from September 25, 2020.
-   Features and billing items:
    -   Time series data storage: allows you to perform one-stop imports of time series data, store the data in Metricstores, and visualize the data by using charts and dashboards. You can also create alerts and implement intelligent O&M for the data.

        **Note:**

        -   The storage space that time series data occupies is measured based on the size of the raw data.
        -   The occupied storage space and indexes of time series data are charged at a different unit price compared with the occupied storage space and indexes of log data. The data transformation, data shipping, read/write traffic, read/write requests, and other billing items are charged the same between time series data and log data.
        |Billing item|Alibaba Cloud|
        |------------|-------------|
        |Storage space occupied by time series data|USD 0.00048 per GB per day|
        |Indexes created for time series data|USD 0.02721/GB|

    -   Data transformation and shipping: provides more functions for data transformation and offers improved performance and more features for data shipping. You are charged for transforming and shipping data based on the formula: Data volume × Unit price.

        |Billing item|Alibaba Cloud|
        |------------|-------------|
        |Data transformation|USD 0.0204/GB|

        |Billing item|Alibaba Cloud|
        |------------|-------------|
        |Shipping data to OSS \(Data shipped to OSS is in the JSON or CSV format.\)|USD 0.0068/GB|
        |Shipping data to OSS \(Data shipped to OSS is in the Parquet format.\)|USD 0.0272/GB|
        |Shipping data to MaxCompute|USD 0.0272/GB|
        |Shipping data to AnalyticDB|USD 0.0272/GB|
        |Shipping data to TSDB|USD 0.0272/GB|
        |Shipping data to ClickHouse|USD 0.0272/GB|

    -   Remarks

        Other features are still charged based on the pay-as-you-go method. The unit price of these features remains unchanged. For more information, see [Price details page](https://www.alibabacloud.com/product/log-service/pricing?spm=a3c0i.139163.9288850920.1.6498637aCPWnWp).


## New subscription resource plans

A subscription resource plan specifies a quota on resource usage. If you purchase a subscription resource plan, the resources that you use are deducted from the resource plan. If the quota is exceeded, you are charged for the excess resources based on the pay-as-you-go method.

**Note:** You are charged for using new subscription resource plans from October 25, 2020.

Log Service provides multiple new subscription resource plans. You can save more money by purchasing a resource plan that has a higher quota or a longer validity period. You can use a new subscription resource plan to deduct the resource usage of multiple billing items such as occupied storage space, read/write requests, data transformation, and data shipping.

-   Type: New subscription resource plans are classified into yearly subscription resource plans and monthly subscription resource plans. A yearly subscription resource plan expires when the quota is reached or the one-year validity period expires. A monthly subscription resource plan provides the same quota on resource usage for each month within the validity period.
-   Discounts: All new subscription resource plans are provided at a discount. The most competitive plan reduces your costs by 63%.

    **Note:** The prices of pay-as-you-go resource plans remains unchanged. Only new subscription resource plans are provided at a discount.

-   Quota on resource usage: New subscription resource plans specify the quotas on resource usage in charge units \(CUs\). Each CU is charged at USD 1.
-   The following table lists the price of each billing item in CUs.

    |Billing item|Alibaba Cloud|
    |------------|-------------|
    |Storage space occupied by log data \(per GB per day\)|0.002875 CU|
    |Storage space occupied by time series data \(per GB per day\)|0.00048 CU|
    |Read/write data volume over the internal network \(per GB\)|0.045 CU|
    |Indexes created or recreated for log data \(per GB\)|0.0875 CU|
    |Indexes created for time series data \(per GB\)|0.02721 CU|
    |Read traffic over the external network \(per GB\)|0.2 CU|
    |Active shards \(per shard per day\)|0.01 CU|
    |Number of read/write requests \(per million requests\)|0.03 CU|
    |Data transformation \(per GB\)|0.0204 CU|
    |Shipping data to OSS \(Data shipped to OSS is in the JSON or CSV format, per GB\)|0.0068 CU|
    |Shipping data to OSS \(Data shipped to OSS is in the Parquet format, per GB\)|0.0272 CU|
    |Shipping data to MaxCompute \(per GB\)|0.0272 CU|
    |Shipping data to AnalyticDB \(per GB\)|0.0272 CU|
    |Shipping data to TSDB \(per GB\)|0.0272 CU|
    |Shipping data to ClickHouse \(per GB\)|0.0272 CU|


New subscription resource plans are classified into yearly subscription resource plans and monthly subscription resource plans:

-   Yearly subscription resource plans

    A yearly subscription resource plan expires when the quota is reached or the one-year validity period expires. You can purchase multiple yearly subscription resource plans based on your business requirements. A resource plan with a higher quota provides a higher discount.

    For example, if you purchase a yearly subscription resource plan of 1,000 CUs, you can use the available resources in the plan until the quota is reached or the one-year validity period expires.

    |Yearly subscription resource plan \(CU\)|Price \(USD\)|Discount \(%\)|
    |----------------------------------------|-------------|--------------|
    |100|95|5|
    |1,000|900|10|
    |10,000|8,500|15|
    |100,000|75,000|25|
    |500,000|360,000|28|
    |2,000,000|1,400,000|30|

-   Monthly subscription resource plan

    A monthly subscription resource plan provides the same quota on resource usage for each month within the validity period. The available resources in a monthly subscription resource plan are cleared at the end of the month and are resumed at the start of the next month. A monthly subscription resource plan has a validity period of one year, two years, three years, four years, or five years. You can purchase multiple monthly subscription resource plans based on your business requirements. A resource plan with a higher quota or a longer validity period provides a higher discount.

    For example, if you purchase a monthly subscription resource plan of 10,000 CUs with a validity period of three years, you have 10,000 CUs of free resources to use each month within the three-year validity period.

    The following table lists the prices of monthly subscription resource plans that have different quotas and validity periods.

    |Monthly subscription plan \(CU\)|One-year validity period \(USD\)|Two-year validity period \(USD\)|Three-year validity period \(USD\)|Four-year validity period \(USD\)|Five-year validity period \(USD\)|
    |--------------------------------|--------------------------------|--------------------------------|----------------------------------|---------------------------------|---------------------------------|
    |100|900 \(25% discount\)|1,680 \(30% discount\)|2,160 \(45% discount\)|2,640 \(50% discount\)|2,700 \(55% discount\)|
    |1,000|8,550 \(29% discount\)|15,960 \(33% discount\)|20,520 \(48% discount\)|20,520 \(52% discount\)|25,650 \(57% discount\)|
    |10,000|82,800 \(31% discount\)|154,560 \(36% discount\)|198,720 \(49% discount\)|242,880 \(54% discount\)|248,400 \(59% discount\)|
    |100,000|792,000 \(34% discount\)|1,478,400 \(38% discount\)|1,900,800 \(52% discount\)|2,323,200\(56% discount\)|2,376,000 \(60% discount\)|
    |500,000|3,825,000 \(36% discount\)|7,140,000 \(40% discount\)|9,180,000 \(53% discount\)|11,220,000 \(57% discount\)|11,475,000 \(62% discount\)|
    |1,000,000|7,380,000 \(38% discount\)|13,776,000 \(43% discount\)|17,712,000 \(55% discount\)|21,648,000 \(59% discount\)|22,140,000 \(63% discount\)|


## Examples

The following examples use Alibaba Cloud to describe the billing for using Log Service resources.

-   Example 1: real-time computing and offline computing

    A website sends 100 million API requests to Log Service per day. The size of the logs written to Log Service is 200 bytes in each request and therefore is 20 GB per day. The data traffic during business peak hours is five times of the average data traffic, reaching 1.16 MB/s \(lower than 5 MB/s\). The logs have a lifecycle of two days and are read once a day for real-time computing. Then they are exported to OSS for offline computing.

    The total fee incurred is USD 3.3375 per day. This fee is calculated based on the following formula: USD 0.01 + USD 0.09 + USD 0.09 + USD 0.0115 + USD 0.136 + USD 3 = USD 3.3375. The following table describes the billing items.

    |Billing item|Description|
    |------------|-----------|
    |Shard|One shard is used to store the logs. The fee incurred for using the shard is USD 0.01.|
    |Read/write traffic|The size of raw data is 20 GB. The compression ratio is 10%. Therefore, the size of compressed data written to Log Service is 2 GB. The fee incurred for data writes is USD 0.09. This fee is calculated based on the following formula: 2 GB × USD 0.045/GB = USD 0.09.|
    |The size of data read for real-time computing is the same as the size of data writes. The fee incurred for data reads is USD 0.09.|
    |The fee incurred for data storage is USD 0.0115. This fee is calculated based on the following formula: 2 GB × 2 days × USD 0.002875/GB per day = USD 0.0115.|
    |The fee incurred for data export to OSS in the CSV format is USD 0.136. This fee is calculated based on the following formula: 20GB × USD 0.0068/GB = USD 0.136.|
    |API requests|The fee incurred for making 100 million API requests is USD 3. This fee is calculated based on the following formula: USD 0.00000003 × 100,000,000 = USD 3.|

-   Example 2: log indexing and online queries

    A service sends one million API requests to Log Service per day. The size of the logs written to Log Service is 200 bytes in each request and therefore is 200 MB per day. The logs are stored for 30 days.

    The total fee incurred is USD 0.0690625 per day. This fee is calculated based on the following formula: USD 0.0175 + USD 0.0215625 + USD 0.03 = USD 0.0690625. The following table describes the billing items.

    |Billing item|Description|
    |------------|-----------|
    |Indexing|Indexes are created for all log fields. The fee incurred for indexing is USD 0.0175. This fee is calculated based on the following formula: 2 GB × USD 0.0875/GB = USD 0.0175.|
    |Storage|The size of indexes is 50 MB. Therefore, the size of stored data is 250 MB. The fee incurred for data storage is USD 0.0215625 per day. This fee is calculated based on the following formula: 0.25 GB × 30 days × USD 0.002875 = USD 0.0215625. Indexes are compressed from raw logs. The compression ratio is 25%. Therefore, the size of indexes is 50 MB. |
    |API requests|The fee incurred for making 100 API requests is USD 0.03. This fee is calculated based on the following formula: USD 0.00000003 × 1,000,000 = USD 0.03.|

-   Example 3: time series data storage

    A service writes 400 MB of time series data to Log Service per day. The data is stored for 15 days.

    The total fee incurred is USD 0.103625 per day. This fee is calculated based on the following formula: USD 0.010884 + USD 0.00324 = USD 0.014124. The following table describes the billing items.

    |Billing item|Description|
    |------------|-----------|
    |Index|Indexes are created for all log fields. The fee incurred for indexing is USD 0.010884. This fee is calculated based on the following formula: 0.4 GB × USD 0.02721/GB = USD 0.010884.|
    |Storage|The size of indexes is 50 MB. Therefore, the size of stored data is 450 MB. The fee incurred for data storage is USD 0.00324 per day. This fee is calculated based on the following formula: 0.45 GB × 15 days × USD 0.00048 = USD 0.00324. Indexes are compressed from raw time series data. The compression ratio is 12.5%. Therefore, the size of indexes is 50 MB. |

-   Example 4: resource plans

    A customer needs to write 10 GB of data to and read 10 GB of data from Log Service per day. The customer also needs to index 200 GB of data and store 2,000 GB of data per day.

    -   Based on the pay-as-you-go billing method, the total fee incurred per day is USD 23.7. This fee is calculated based on the following formula: USD 0.45 + USD 17.5 + USD 5.75 = USD 23.7. The total fee incurred per month is USD 711. The specific details are shown in the following table.

        |Billing item|Description|
        |------------|-----------|
        |Read/write traffic|The fee incurred for data reads and writes is USD 0.45. This fee is calculated based on the following formula: 10 GB × USD 0.045/GB = USD 0.45.|
        |Indexing|The fee incurred for indexing is USD 17.5. This fee is calculated based on the following formula: 200 GB × USD 0.0875/GB = USD 17.5.|
        |Storage|The fee incurred for data storage is USD 5.75 per day. This fee is calculated based on the following formula: 2,000 GB × USD 0.002875 = USD 5.75.|

    -   If the customer purchases eight monthly subscription resource plans of 100 CUs with a validity period of three years, the fee is 480 CUs per month, reducing by 230 CUs. For three years, the fee is 6,480 CUs, a reduction of 8,280 CUs or USD 8,280.

