# Overview

Data transformation is a fully managed feature that provides high availability and scalability in Log Service. This feature can be used in scenarios such as data standardization, enrichment, distribution, aggregation, and re-indexing.

## Transformation process

**Note:** The data transformation feature is available in all regions except the China \(Qingdao\) region.

Log Service transforms log data by using the following process:

1.  A consumer group reads log data from the source Logstore.
2.  Log Service transforms every log entry based on the transformation rule.
3.  Log Service writes transformed log data to the destination Logstore.

After the data is transformed, you can view the data in the destination Logstore.

## Scenarios

-   Data standardization: Log data is read from a Logstore, transformed, and written to another Logstore.

    In this scenario, data is standardized, enriched, and re-indexed.

    ![Data standardization](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5943749951/p51410.png)

-   Data distribution: Log data is read from a Logstore, transformed, and written to multiple Logstores.

    ![Data distribution](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5943749951/p51411.png)

-   Multi-source data aggregation: Log data is read from multiple Logstores, transformed, and written to a specified Logstore.

    ![Multi-source data aggregation](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5943749951/p51412.png)

-   Common data transformation scenarios.

    These scenarios include data filtering, splitting, transformation, and enrichment.

    ![Data transformation](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5943749951/p51413.png)

    Log Service provides more than 200 built-in functions and more than 400 regular expressions. You can also use the domain-specific language \(DSL\) for Log Service to create user-defined functions \(UDFs\) based on your business requirements. The following scenarios are common scenarios:

    -   Filters out specified log data.
    -   Splits a log entry into multiple log entries.
    -   Extracts, deletes, modifies fields, and transforms the field values.
    -   Maps fields from external resources and enriches raw log data with the fields.

## Benefits

-   Allows you to use the DSL for Log Service to orchestrate functions as needed. You can use the orchestrated functions to filter, extract, split, transform, enrich, and distribute data.
-   Processes data in real time and allows you to view data in seconds. The feature extends or shrinks the computation capability based on the data size and provides a high throughput.
-   Applies to log analysis scenarios and provides out-of-the-box functions.
-   Provides charts and dashboards for real-time data analysis and provides alerts for exception logs.
-   Offers a fully managed and maintenance-free service that can be integrated with the big data services of Alibaba Cloud and open source ecosystems.

## Billing

-   You are charged for the server and network resources consumed when you use the data transformation feature. For more information, see [Billing overview](/intl.en-US/Pricing/Billing overview.md).
-   You can disable the indexing feature for the source Logstore and set a short data retention period to reduce costs. For more information about how to reduce costs, see [Performance guide](/intl.en-US/Data Transformation/Performance guide.md) and [Cost optimization guide](/intl.en-US/Data Transformation/Cost optimization guide.md).

