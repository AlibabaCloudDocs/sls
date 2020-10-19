# New subscription resource plans

This topic describes the new subscription resource plans of Log Service.

## New subscription resource plans

A subscription resource plan specifies a quota on resource usage. If you purchase a subscription resource plan, the resources that you use are deducted from the resource plan. If the quota is exceeded, you are charged for the excess resources based on the pay-as-you-go method.

**Note:** New subscription resource plans are released on October 25, 2020.

Log Service provides multiple new subscription resource plans. You can reduce costs by purchasing a resource plan that has a higher quota or a longer validity period. You can use a new subscription resource plan to deduct the resource usage of multiple billing items such as occupied storage space, read/write requests, data transformation, and data shipping.

-   Types: New subscription resource plans are classified into yearly subscription resource plans and monthly subscription resource plans. A yearly subscription resource plan expires when the quota is reached or the one-year validity period expires. A monthly subscription resource plan provides the same quota on resource usage for each month within the validity period.
-   Discounts: A new type of subscription resource plan is provided at a discount. You can use this subscription resource plan to deduct all types of resources. The most competitive plan reduces your costs by 52%.

    **Note:** The prices of pay-as-you-go resource plans remain unchanged. Only new subscription resource plans are provided at a discount.

-   Quota on resource usage: New subscription resource plans specify the quotas on resource usage in charge units \(CUs\). The price of each CU is USD 1.
-   The following table lists the price of each billing item in CUs.

    |Billing item|Alibaba Cloud|
    |------------|-------------|
    |Storage space occupied by log data \(per GB per day\)|0.002875 CU|
    |Storage space occupied by time series data \(per GB per day\)|0.00048 CU|
    |Read/write data volume over Alibaba Cloud internal network \(per GB\)|0.045 CU|
    |Indexes created or recreated for log data \(per GB\)|0.0875 CU|
    |Indexes created for time series data \(per GB\)|0.02721 CU|
    |Read traffic over the Internet \(per GB\)|0.2 CU|
    |Active shards \(per shard per day\)|0.01 CU|
    |Number of read/write requests \(per million requests\)|0.03 CU|
    |Data transformation \(per GB\)|0.0204 CU|
    |Shipping data to OSS \(in the JSON or CSV format, per GB\)|0.0068 CU|
    |Shipping data to OSS \(in the Parquet format, per GB\)|0.0272 CU|
    |Shipping data to MaxCompute \(per GB\)|0.0272 CU|
    |Shipping data to AnalyticDB for MySQL \(per GB\)|0.0272 CU|
    |Shipping data to TSDB \(per GB\)|0.0272 CU|
    |Shipping data to ClickHouse \(per GB\)|0.0272 CU|


New subscription resource plans are classified into yearly subscription resource plans and monthly subscription resource plans:

-   Yearly subscription resource plans

    A yearly subscription resource plan expires when the quota is reached or the one-year validity period expires. You can purchase multiple yearly subscription resource plans based on your business requirements. A resource plan with a higher quota provides a higher discount.

    For example, if you purchase a yearly subscription resource plan of 1,000 CUs, you can use the available resources in the plan until the quota is reached or the one-year validity period expires.

    |Yearly subscription resource plan \(CU\)|Price \(USD\)|Discount \(%\)|
    |----------------------------------------|-------------|--------------|
    |100|95|9.5|
    |1,000|900|9.0|
    |10,000|8,500|8.5|
    |100,000|75,000|7.5|
    |500,000|360,000|7.2|
    |2,000,000|1,400,000|7.0|

-   Monthly subscription resource plan

    A monthly subscription resource plan provides the same quota on resource usage for each month within the validity period. The resource usage quota in a monthly subscription resource plan is cleared at the end of the month and is resumed at the start of the next month. A monthly subscription resource plan has a validity period of one year, two years, three years, four years, or five years. You can purchase multiple monthly subscription resource plans based on your business requirements. A resource plan with a higher quota or a longer validity period provides a higher discount.

    For example, if you purchase a monthly subscription resource plan of 10,000 CUs with a validity period of three years, you have 10,000 CUs of free resources to use each month within the three-year validity period.

    The following table lists the prices of monthly subscription resource plans that have different quotas and validity periods.

    |Monthly subscription plan \(CU\)|One-year validity period \(USD\)|Two-year validity period \(USD\)|Three-year validity period \(USD\)|
    |--------------------------------|--------------------------------|--------------------------------|----------------------------------|
    |100|900 \(25% discount\)|1,680 \(30% discount\)|2,160 \(45% discount\)|
    |1,000|8,550 \(29% discount\)|15,960 \(33% discount\)|20,520 \(48% discount\)|
    |10,000|82,800 \(31% discount\)|154,560 \(36% discount\)|198,720 \(49% discount\)|
    |100,000|792,000 \(34% discount\)|1,478,400 \(38% discount\)|1,900,800 \(52% discount\)|


## Examples

The following examples show how you are billed for using the resource plans of Alibaba Cloud Log Service.

You need to write and read 10 GB of data to and from Log Service per day. You also need to index 200 GB of data and store 2,000 GB of data per day.

-   Based on the pay-as-you-go billing method, the total fee incurred for one day is USD 23.7\(USD 0.45 + USD 17.5 + USD 5.75 = USD 23.7\). The total fee incurred per month is USD 711. The following table shows the billing details.

    |Billing item|Description|
    |------------|-----------|
    |Read/write traffic|The fee incurred for data reads and writes is USD 0.45 \(10 GB × USD 0.045/GB = USD 0.45\).|
    |Indexing|The fee incurred for indexing is USD 17.5 \(200 GB × USD 0.0875/GB = USD 17.5\).|
    |Storage|The fee incurred for data storage is USD 5.75 per day \(2000 GB × 0.002875 USD/GB/day = USD 5.75\).|

-   If you purchase eight monthly subscription resource plans of 100 CUs with a validity period of three years, the fee is 480 CUs per month, reduced by 230 CUs. For three years, the fee is 6,480 CUs, reduced by 8,280 CUs or USD 8,280.

