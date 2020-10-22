# Log collection methods

Log Service allows you to collect logs by using clients, web pages, protocols, SDKs, and APIs. These collection methods are implemented based on RESTful APIs. This topic describes the collection methods that are supported by Log Service.

## Data sources

The following table describes the data sources from which Log Service can collect logs.

|Type|Source|Collection method|Reference|
|:---|:-----|:----------------|:--------|
|Application|Program output|[Logtail](/intl.en-US/Data Collection/Logtail collection/Overview/Logtail overview.md)|None|
|Access log|[Logtail](/intl.en-US/Data Collection/Logtail collection/Overview/Logtail overview.md)|[Collect and analyze NGINX access logs](/intl.en-US/Practices/Best Practices/Query and analysis/Collect and analyze NGINX access logs.md)|
|Link tracking|Jaeger Collector and [Logtail](/intl.en-US/Data Collection/Logtail collection/Overview/Logtail overview.md)|None|
|Language|Java|[Log Service SDK for Java](https://github.com/aliyun/aliyun-log-java-sdk) and [Java Producer Library](/intl.en-US/Data Collection/Other collection methods/SDK collection/Producer Library.md)|None|
|Log4J Appender|[1.x](https://github.com/aliyun/aliyun-log-log4j-appender) and [2.x](https://github.com/aliyun/aliyun-log-log4j2-appender)|None|
|LogBack Appender|[LogBack](https://github.com/aliyun/aliyun-log-logback-appender)|None|
|C|[Log Service C SDK](https://github.com/aliyun/aliyun-log-c-sdk)|None|
|Python|[Log Service Python SDK](https://github.com/aliyun/aliyun-log-python-sdk)|None|
|Python Logging|[Python Logging Handler](https://aliyun-log-python-sdk.readthedocs.io/tutorials/tutorial_logging_handler.html)|None|
|PHP|[Log Service PHP SDK](https://github.com/aliyun/aliyun-log-php-sdk)|None|
|.Net|[Log Service csharp SDK](https://github.com/aliyun/aliyun-log-chsarp-sdk)|None|
|C++|[Log Service C++ SDK](https://github.com/aliyun/aliyun-log-cpp-sdk)|None|
|Go|[Log Service Go SDK](https://github.com/aliyun/aliyun-log-go-sdk) and [Golang Producer Library](/intl.en-US/Data Collection/Other collection methods/SDK collection/Golang Producer Library.md)|None|
|NodeJS|[NodeJs](https://github.com/aliyun-UED/aliyun-sdk-js)|None|
|JS|[JS/Web Tracking](/intl.en-US/Data Collection/Other collection methods/Use web tracking to collect logs.md)|None|
|OS|Linux|[Logtail](/intl.en-US/Data Collection/Logtail collection/Overview/Logtail overview.md)|None|
|Windows|[Logtail](/intl.en-US/Data Collection/Logtail collection/Overview/Logtail overview.md)|None|
|Mac/Unix|[Native C](/intl.en-US/Developer Guide/SDK Reference/Overview.md)|None|
|Docker file|[Use Logtail to collect Docker files](/intl.en-US/Data Collection/Logtail collection/Container log collection/Use the console to collect Kubernetes text logs in DaemonSet mode.md).|None|
|Docker output|[Use Logtail to collect container logs](/intl.en-US/Data Collection/Logtail collection/Container log collection/Use the console to collect Kubernetes stdout and stderr logs in DaemonSet mode.md).|None|
|Database|MySQL Binlog|[Collect MySQL binary logs](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to collect data/Collect MySQL binary logs.md)|None|
|JDBC Select|[Collect MySQL query results](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to collect data/Collect MySQL query results.md)|None|
|Mobile client|iOS/Android|[Log Service SDK for Android](https://github.com/aliyun/aliyun-log-android-sdk) and [Log Service SDK for iOS](https://github.com/aliyun/aliyun-log-ios-sdk)|None|
|Web page|[JS/Web Tracking](/intl.en-US/Data Collection/Other collection methods/Use web tracking to collect logs.md)|None|
|Intelligent IoT|[C Producer Library](https://github.com/aliyun/aliyun-log-c-sdk)|None|
|Others|HTTP polling|[Logtail HTTP](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to collect data/Collect HTTP data.md)|[Collect and analyze NGINX monitoring logs](/intl.en-US/Practices/Best Practices/Query and analysis/Collect and analyze NGINX monitoring logs.md)|
|Syslog|[Use Logtail to collect syslog logs](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to collect data/Collect syslog logs.md).|None|
|Data import|MaxCompute|[Import data from MaxCompute to Log Service](/intl.en-US/Data Collection/Data import/Import data from MaxCompute to Log Service.md)|None|
|Object Storage Service \(OSS\)|[Import data from OSS to Log Service](/intl.en-US/Data Collection/Data import/Import data from OSS to Log Service.md)|None|
|Flink|Use Flink to write data to Log Service.|[Register Log Service resources](/intl.en-US/Exclusive Mode/Flink SQL Development Guide/Data storage/Data storage resource registration/Register Log Service resources.md)|
|Third-party software|Logstash|[Logstash](/intl.en-US/Data Collection/Other collection methods/Logstash/Create Logstash configurations for log collection and processing.md)|None|
|Flume|[Use Flume to consume log data](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Use Flume to consume log data.md)|None|
|Alibaba Cloud services|Elastic Compute Service \(ECS\) and OSS logs|For more information, see [Alibaba Cloud service logs](/intl.en-US/Data Collection/Cloud product collection/Cloud service logs.md).|None|

## Select a network

Log Service provides endpoints for different Alibaba Cloud regions. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md). Each region allows access from the following networks:

-   Internal network \(classic network or VPC\): Recommended. Log Service can access other Alibaba Cloud services in the same region over a reliable link.
-   Internet: Internet can be accessed without limits. The transmission speed depends on the link quality. We recommend that you use HTTPS to ensure a secure data transmission.

## FAQ

-   Which network do I select for private line access?

    We recommend that you select the internal network \(classic network or VPC\).

-   Can I collect public IP addresses when I collect Internet data?

    Yes, you can enable Log Service to record public IP addresses. For more information, see [Create a Logstore](/intl.en-US/Data Collection/Preparation/Manage a Logstore.md).

-   Which network do I select if I want to collect ECS logs from Region A and write these logs to a Log Service project in Region B?

    You can install Logtail on the ECS instance in Region A for Internet transmission and specify the endpoint that is associated with Region B. For more information about how to select a network, see [Select a network type](/intl.en-US/Data Collection/Logtail collection/Select a network type.md).

-   How can I determine whether an endpoint is accessible?

    You can run the following command. The endpoint is accessible if a message is returned.

    ```
     curl $myproject.cn-hangzhou.log.aliyuncs.com
    ```

    `$myproject` specifies the project name and `cn-hangzhou.log.aliuncs.com` specifies the endpoint.


