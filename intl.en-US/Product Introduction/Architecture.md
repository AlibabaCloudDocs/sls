# Architecture

This topic describes the architecture of Log Service.

The following figure shows the architecture of Log Service.

![sls_architect](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0620125261/p284432.png)

-   Data sources

    Log Service allows you to collect data from open source software, servers and applications, Alibaba Cloud services, mobile terminals, and IoT devices. You can also collect data that is transferred over multiple protocols.

-   Log Service
    -   Data types

        Log Service provides a platform on which you can use various features to process large amounts of data at low cost and in real time. The types of data that you can process include logs, metrics, and traces. For more information, see [Log](/intl.en-US/Product Introduction/Basic concepts/Log.md), [Time series data](/intl.en-US/Product Introduction/Basic concepts/Time series.md), and [Trace](/intl.en-US/Product Introduction/Basic concepts/Trace.md).

    -   Features
        -   Data collection: Log Service allows you to collect data by using Logtail, SDKs, and protocols. For more information, see [Log collection methods](/intl.en-US/Data Collection/Log collection methods.md).
        -   Data transformation: Data transformation is a fully managed feature that provides high availability and scalability in Log Service. You can use the data transformation feature to standardize, enrich, dispatch, mask, and filter data. For more information, see [Overview](/intl.en-US/Data Transformation/Overview.md).
        -   Data query and analysis: Log Service allows you to query and analyze petabytes of data in real time. This feature supports more than 10 operators, more than 10 machine learning functions, and more than 100 SQL functions. This features also supports scheduled SQL jobs and dedicated SQL instances. For more information, see [Log search](/intl.en-US/Index and query/Log search.md) and [Log analysis](/intl.en-US/Index and query/Log analysis.md).
        -   Visualization: Log Service allows you to visualize query and analysis results. You can customize dashboards based on charts. For more information, see [Visualization overview](/intl.en-US/Visualization/Overview.md).
        -   Alerting: Log Service provides the integrated alerting feature. This feature includes the alert monitoring, alert management, and notification management systems. The alerting feature is applicable to multiple scenarios such as DevOps, ITOps, AIOps, SecOps, and BizOps. For more information, see [The alerting feature of Log Service](/intl.en-US/Alerting/Alerting (New)/Feature overview/The alerting feature of Log Service.md).
        -   Data consumption and shipping: Log Service allows you to consume data in real time by using Storm, Flume, or Flink. You can also ship data to Object Storage Service \(OSS\), MaxCompute, or Time Series Database \(TSDB\) in real time. For more information, see [Data shipping overview](/intl.en-US/Log consumption and shipping/Data shipping/Overview.md) and [Data consumption overview](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Overview.md).
        -   Log audit: Log Service provides an automated and centralized method to collect and audit the logs of cloud services across Alibaba Cloud accounts in real time. For more information, see [Log Audit Service](/intl.en-US/Application/Log Audit Service/Background information.md).
    -   Methods

        You can manage your resources in Log Service by using the Log Service console, API, SDKs, or CLI.

    -   Scenarios

        Log Service is applicable to multiple scenarios such as operation, O&M, R&D, and security. For more information, see [Scenarios](/intl.en-US/Product Introduction/Scenarios.md).

-   Data destinations

    Log Service allows you to export data to cloud services or third-party software by using the data consumption or data shipping feature.


