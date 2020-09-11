# Overview

Log Service provides a fully managed, highly available, and scalable data transformation feature. This feature is widely used in scenarios such as data standardization, enrichment, distribution, aggregation, and re-indexing.

## Transformation procedure

The data transformation feature transforms data by performing the following steps:

1.  Uses a consumer group to read log data from the source Logstore.
2.  Transforms every log entry based on the transformation rule.
3.  Writes transformed log data to the destination Logstore.

After the data is transformed, you can view the data in the destination Logstore.

**Note:** The data transformation feature is available in all regions except the China \(Qingdao\) region.

## Scenarios

-   Data standardization: Log data is read from a Logstore, transformed, and written to another Logstore.

    In this scenario, data is standardized, enriched, and re-indexed.

    ![Data standardization](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5943749951/p51410.png)

-   Data distribution: Log data is read from a Logstore, transformed, and written to multiple Logstores.

    ![Data distribution](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5943749951/p51411.png)

-   Multi-source data aggregation \(many-to-one\): Log data is read from multiple Logstores, transformed, and written to a specified Logstore.

    ![Multi-source data aggregation](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5943749951/p51412.png)

-   Common data transformation scenarios.

    Common data transformation scenarios include data filtering, splitting, transformation, and enrichment.

    ![Data transformation](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5943749951/p51413.png)

    Log Service provides more than 200 built-in functions and more than 400 regular expressions. You can also use the domain specific language \(DSL\) for Log Service to create user-defined functions \(UDFs\) based on your business requirements. The following scenarios are common scenarios that you may encounter:

    -   Filters out specified log data.
    -   Splits a log entry into multiple log entries.
    -   Extracts, deletes, and modifies fields and transforms the field values.
    -   Maps fields from external resources and enriches raw log data with the fields.

## Benefits

-   Provides more than 200 built-in functions, including text processing functions, text search functions, and enrichment functions, and more than 400 grok patterns.
-   Allows you to use the DSL for Log Service to orchestrate functions as needed. You can use the orchestrated functions to filter, extract, split, transform, enrich, and distribute data.
-   Processes data in real time and allows you to view data in seconds. Automatically extends or shrinks the computation capability based on the data size. Provides a high throughput.
-   Applies to log analysis scenarios and provides out-of-the-box functions.
-   Provides charts and dashboards for real-time data analysis, and alerts for exception logs.
-   Offers a fully managed and maintenance-free service that can be integrated with Alibaba Cloud big data services and open-source ecosystems.

## Billing

-   You are charged for reading data from the source Logstore and writing data to destination Logstores. For more information, see [Pay-as-you-go](/intl.en-US/Pricing/Pay-as-you-go.md). The server and network resources that are consumed when you use the data transformation feature are free of charge during the public preview.
-   You can delete the indexes that you create in the source Logstore and set a relatively short retention period for the data. This reduces your costs. For more information about how to reduce costs, see [Performance guide](/intl.en-US/Data Transformation/Performance guide.md) or [Cost optimization guide](/intl.en-US/Data Transformation/Cost optimization guide.md).

