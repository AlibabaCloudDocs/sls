# Overview

Log Service provides software development kits \(SDKs\) for multiple programming languages, such as Java, .NET, Python, PHP, and C. You can select an SDK based on your business requirements.

The implementation of Log Service SDKs varies depending on programming languages. However, Log Service SDKs are APIs that are encapsulated in different programming languages. The SDKs provide the following common features:

-   Log Service SDKs encapsulate APIs. They implement the underlying API request creation and response parsing. APIs that are developed in different languages are called by using similar methods. This allows you to use different programming languages. For more information, see [Procedure](/intl.en-US/Developer Guide/SDK Reference/Procedure.md).
-   Log Service SDKs implement digital signature of API operations. This simplifies API calls. For more information, see [Request signature](/intl.en-US/Developer Guide/API Reference/Request signatures.md).
-   Log Service SDKs convert the format of logs into the Protocol Buffers \(Protobuf\) format. You do not need to convert the log format when you write logs to Log Service. For more information, see [Protobuf format](/intl.en-US/Developer Guide/API Reference/Common resources/Data encoding.md).
-   Log Service SDKs use the method that is defined in Log Service APIs to compress logs. Several SDKs allow you to specify whether to write logs in the compression mode. The compression mode is used by default.
-   Log Service SDKs provide a unified exception handling mechanism. You can use a method to handle request exceptions based on the SDK language. For more information, see [Exception handling mechanism](/intl.en-US/Developer Guide/SDK Reference/Exception handling.md).
-   Log Service SDKs support only synchronous requests.

## Supported SDKs

|SDK language|Source code|
|:-----------|:----------|
|Java|[Log Service Java SDK](https://github.com/aliyun/aliyun-log-java-sdk) and [Log Service SDK for Java 0.6.0 API](http://log-java-docs.oss-cn-hangzhou.aliyuncs.com/)|
|.NET Core|[aliyun-log-dotnetcore-sdk](https://github.com/aliyun/aliyun-log-dotnetcore-sdk)|
|.NET|[Log Service csharp SDK](https://github.com/aliyun/aliyun-log-chsarp-sdk)|
|PHP|[Log Service PHP SDK](https://github.com/aliyun/aliyun-log-php-sdk)|
|Python|[Log Service Python SDK](https://github.com/aliyun/aliyun-log-python-sdk) and [User Guide](http://aliyun-log-python-sdk.readthedocs.io/README_CN.html)|
|Node.js|[Log Service Node.js SDK](https://github.com/aliyun-UED/aliyun-sdk-js/tree/master/samples/sls)|
|C|[Log Service C SDK](https://github.com/aliyun/aliyun-log-c-sdk)|
|GO|[Log Service Go SDK](https://github.com/aliyun/aliyun-log-go-sdk)|
|iOS|[Log Service iOS SDK](https://github.com/aliyun/aliyun-log-ios-sdk)|
|Android|[Log Service Android SDK](https://github.com/aliyun/aliyun-log-android-sdk)|
|C++|[Log Service C++ SDK](https://github.com/aliyun/aliyun-log-cpp-sdk)|
|JavaScript SDK|[JavaScript SDK](/intl.en-US/Data Collection/Other collection methods/Use web tracking to collect logs.md)|

