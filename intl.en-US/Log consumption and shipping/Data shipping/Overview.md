# Overview

Log Service provides the data shipping and consumption features. You can ship log data to Alibaba Cloud services such as OSS and MaxCompute in real time by using the Log Service console. You can also use an SDK or the API of Log Service to consume log data in real time. This topic describes the benefits, scenarios, and shipping destinations of the data shipping feature.

In the Log Service console, you can ship log data to other Alibaba Cloud services. Then, you can store or consume the log data by using other systems such as E-MapReduce. After you enable the log shipping feature, Log Service ships the collected log data to the specified cloud service at regular intervals.

## Scenarios

The log shipping feature can be used to connect Log Service with data warehouses.

## Benefits

Using the Log Service console to ship logs has the following benefits:

-   Ease of use

    You can specify the log shipping settings in the Log Service console. Then you can use the settings to ship log data from Logstores to other Alibaba Cloud services such as OSS.

-   High efficiency

    Log Service stores log data that is collected from multiple servers. This improves efficiency when you ship log data to other Alibaba Cloud services such as OSS.

-   Effective management

    You can ship log data from different projects or Logstores to different OSS buckets or MaxCompute tables. This facilitates log data management.


## Log shipping destinations

-   OSS

    For information about how to ship log data to OSS, see [Ship log data from Log Service to OSS](/intl.en-US/Log consumption and shipping/Data shipping/Ship logs to OSS/Ship log data from Log Service to OSS.md).

    -   We recommend that you use E-MapReduce to convert the data type of the shipped log data into the data type that OSS supports.
    -   After you ship log data to OSS, you can use Data Lake Analytics \(DLA\) to analyze the data.
-   MaxCompute

    For information about how to ship data to MaxCompute by using the data integration feature of DataWorks, see [Use Data Integration to ship data collected by LogHub to destinations]().

-   SIEM

    For information about how to ship data to a security information and event management \(SIEM\) system by using the log shipping feature of Log Service, see [Ship data from Log Service to SIEM](/intl.en-US/Log consumption and shipping/Data shipping/Send logs to an SIEM system/Introduction.md).

-   Table Store

    For information about how to ship data to Table Store by using the transfer feature of Table Store, see [The transfer feature of Table Store]().


