# Log collection methods

LogHub supports multiple methods to collect logs, such as by using clients, Web pages, protocols, SDKs, and APIs. All collection methods are implemented based on Restful APIs. You can also implement new collection methods by using APIs and SDKs.

## Data sources

The following table describes the data sources from which Log Service can collect logs.

|Category|Source|Collection method|Reference|
|:-------|:-----|:----------------|:--------|
|Application|Program output|[Logtail](/intl.en-US/Data Collection/Logtail collection/Overview/Logtail overview.md)| |
|Access log|[Logtail](/intl.en-US/Data Collection/Logtail collection/Overview/Logtail overview.md)|[Collect and analyze NGINX access logs](/intl.en-US/Practices/Best Practices/Query and analysis/Collect and analyze NGINX access logs.md)|
|Link tracking|Jaeger Collector and [Logtail](/intl.en-US/Data Collection/Logtail collection/Overview/Logtail overview.md)|-|
|Language|Java|[Log Service Java SDK](https://github.com/aliyun/aliyun-log-java-sdk) and [Java Producer Library](/intl.en-US/Data Collection/Other collection methods/SDK collection/Producer Library.md)|-|
|Log4j appender|[1.x](https://github.com/aliyun/aliyun-log-log4j-appender) and [2.x](https://github.com/aliyun/aliyun-log-log4j2-appender)|-|
|Logback appender|[Logback](https://github.com/aliyun/aliyun-log-logback-appender)|-|
|C|[Log Service C SDK](https://github.com/aliyun/aliyun-log-c-sdk)|-|
|Python|[Log Service Python SDK](https://github.com/aliyun/aliyun-log-python-sdk)|-|
|Python logging|[Python logging handler](https://aliyun-log-python-sdk.readthedocs.io/tutorials/tutorial_logging_handler.html)|-|
|PHP|[Log Service PHP SDK](https://github.com/aliyun/aliyun-log-php-sdk)|-|
|.NET|[Log Service csharp SDK](https://github.com/aliyun/aliyun-log-chsarp-sdk)|-|
|C++|[Log Service C++ SDK](https://github.com/aliyun/aliyun-log-cpp-sdk)|-|
|Go|[Log Service Go SDK](https://github.com/aliyun/aliyun-log-go-sdk) and [Golang Producer Library](/intl.en-US/Data Collection/Other collection methods/SDK collection/Golang Producer Library.md)|-|
|Node.js|[Node.js](https://github.com/aliyun-UED/aliyun-sdk-js)|-|
|JavaScript|[JavaScript/Web tracking](/intl.en-US/Data Collection/Other collection methods/Use web tracking to collect logs.md)|-|
|Operating system \(OS\)|Linux|[Logtail](/intl.en-US/Data Collection/Logtail collection/Overview/Logtail overview.md)|-|
|Windows|[Logtail](/intl.en-US/Data Collection/Logtail collection/Overview/Logtail overview.md)|-|
|Mac OS or Unix|[Native C](/intl.en-US/Developer Guide/SDK Reference/Overview.md)|-|
|Docker files|[Use Logtail to collect Docker files](/intl.en-US/Data Collection/Logtail collection/Container log collection/Use the console to collect Kubernetes text logs in the DaemonSet mode.md)|-|
|Docker output|[Use Logtail to collect container logs](/intl.en-US/Data Collection/Logtail collection/Container log collection/Use the console to collect Kubernetes stdout and stderr logs in the DaemonSet mode.md)|-|
|Mobile client|iOS/Android|[Log Service Android SDK](https://github.com/aliyun/aliyun-log-android-sdk)，[Log Service iOS SDK](https://github.com/aliyun/aliyun-log-ios-sdk)|-|
|Web page|[JavaScript/Web tracking](/intl.en-US/Data Collection/Other collection methods/Use web tracking to collect logs.md)|-|
|Intelligent IoT|[C Producer Library](https://github.com/aliyun/aliyun-log-c-sdk)| |
|Alibaba Cloud services|ECS, OSS, and other Alibaba Cloud services. For more information, see [Alibaba Cloud service logs](/intl.en-US/Data Collection/Cloud product collection/Cloud service logs.md).|Activate Log Service in the Alibaba Cloud console|[Alibaba Cloud service logs](/intl.en-US/Data Collection/Cloud product collection/Cloud service logs.md)|
|MaxCompute import|Use DataWorks to export MaxCompute data|-|
|Third-party software|Logstash|[Logstash](/intl.en-US/Data Collection/Other collection methods/Logstash/Create Logstash configurations for log collection and processing.md)|-|
|Flume|[Use Flume to consume log data](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Use Flume to consume log data.md)|-|

## Select a network

Log Service provides service endpoints for different Alibaba Cloud regions. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md). Each region allows access from the following networks:

-   Internal network \(classic network\) or private network \(VPC\): Log Service can access other Alibaba Cloud services in the same region, offering optimal link bandwidth. We recommend that you select this option.
-   Public network \(classic network\): accessible without any limits. The transmission speed depends on the link quality. We recommend that you use HTTPS to ensure secure transmission of data.

## FAQ

-   Q: Which network do I select for private line access?

    A: Select the internal network or private network.

-   Q: Can I collect public IP addresses when collecting public network data?

    A: You need to enable Log Service to record public IP addresses. For more information, see [Manage a Logstore](/intl.en-US/Data Collection/Preparation/Manage a Logstore.md).

-   Q: Which network do I select if I want to collect ECS logs from Region A and write these logs into the Log Service project in Region B?

    A: Select the public network. You can install Logtail on the ECS instance in Region A for Internet transmission and specify the service endpoint that is associated with Region B. For more information about how to select a network, see [Select a network type](/intl.en-US/Data Collection/Logtail collection/Select a network type.md).

-   Q: How can I determine whether a service endpoint is accessible?

    A: You can run the following command. The service endpoint is accessible if any information is returned.

    ```
     curl $myproject.cn-hangzhou.log.aliyuncs.com
    ```

    `$myproject` specifies the project name and `cn-hangzhou.log.aliuncs.com` specifies the service endpoint.


