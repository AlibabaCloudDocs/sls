# Overview

Log Service provides software development kits \(SDKs\) for various programming languages, such as .NET, Java, Python, PHP, and C. You can select an SDK based on your business requirements.

## Usage notes

The implementation of Log Service SDKs varies based on the programming language. However, Log Service SDKs are APIs that are encapsulated in different programming languages. The SDKs provide the following common features:

-   Log Service SDKs are APIs that are encapsulated in different programming languages. Log Service SDKs implement the underlying API request creation and response parsing. The APIs in different languages are similar. This simplifies the switchover between different languages. For more information, see [Interface regulations](/intl.en-US/Developer Guide/SDK Reference/Procedure.md).
-   Automatic digital signatures. You do not need to specify the digital signature logic of Log Service APIs. This simplifies the use of Log Service APIs. For more information, see [Request signatures](/intl.en-US/Developer Guide/API Reference/Request signatures.md).
-   Protocol Buffer-formatted encapsulation. The logs collected by Log Service are encapsulated in the Protocol Buffer format. You do not need to specify the format. For more information, see [Protocol Buffer format](/intl.en-US/Developer Guide/API Reference/Common resources/Data encoding.md).
-   Log Service SDKs use the method that is defined in Log Service APIs to compress logs. SDKs for some languages allow you to specify whether logs can be written to Log Service in the compression mode. By default, the compression mode is used.
-   Unified exception handling mechanism. You can use SDKs to handle exceptions based on the related programming language. For more information, see [Exception handling](/intl.en-US/Developer Guide/SDK Reference/Exception handling.md).
-   Log Service SDKs support only synchronous requests.

## Supported SDKs

The following table provides links to the reference documents and GitHub source codes of Log Service SDKs for different languages.

|SDK language|Documentation|GitHub source code|
|:-----------|-------------|:-----------------|
|Java|[Log Service SDK for Java](/intl.en-US/Developer Guide/SDK Reference/Log Service SDK for Java.md)|[Log Service SDK for Java](https://github.com/aliyun/aliyun-log-java-sdk) and [Log Service SDK for Java 0.6.0 API](http://log-java-docs.oss-cn-hangzhou.aliyuncs.com/)|
|.NET Core|[Log Service SDK for .NET Core](/intl.en-US/Developer Guide/SDK Reference/Log Service SDK for .NET Core.md)|[Log Service .NET Core SDK](https://github.com/aliyun/aliyun-log-dotnetcore-sdk)|
|.NET|[Log Service SDK for .NET](/intl.en-US/Developer Guide/SDK Reference/Log Service SDK for .NET.md)|[Log Service .NET SDK](https://github.com/aliyun/aliyun-log-chsarp-sdk)|
|PHP|[Log Service SDK for PHP](/intl.en-US/Developer Guide/SDK Reference/Log Service SDK for PHP.md)|[Log Service PHP SDK](https://github.com/aliyun/aliyun-log-php-sdk)|
|Python|[Log Service SDK for Python](/intl.en-US/Developer Guide/SDK Reference/Log Service SDK for Python.md)|[Log Service SDK for Python](https://github.com/aliyun/aliyun-log-python-sdk) and [User Guide](http://aliyun-log-python-sdk.readthedocs.io/README_CN.html)|
|Node.js|[Log Service SDK for Node.js](/intl.en-US/Developer Guide/SDK Reference/Log Service SDK for Node.js.md)|[Log Service Node.js SDK](https://github.com/aliyun-UED/aliyun-sdk-js/tree/master/samples/sls)|
|C|[Log Service SDK for C](/intl.en-US/Developer Guide/SDK Reference/Log Service SDK for C.md)|[Log Service C SDK](https://github.com/aliyun/aliyun-log-c-sdk)|
|GO|[Log Service SDK for Go](/intl.en-US/Developer Guide/SDK Reference/Log Service SDK for Go.md)|[Log Service Go SDK](https://github.com/aliyun/aliyun-log-go-sdk)|
|iOS|[Log Service SDK for IOS](/intl.en-US/Developer Guide/SDK Reference/Log Service SDK for IOS.md)|[Log Service SDK for iOS](https://github.com/aliyun/aliyun-log-ios-sdk) and [Objective-C SDK](https://github.com/lujiajing1126/AliyunLogObjc)|
|Android|[Log Service SDK for Android](/intl.en-US/Developer Guide/SDK Reference/Log Service SDK for Android.md)|[Log Service Android SDK](https://github.com/aliyun/aliyun-log-android-sdk)|
|C++|[Log Service SDK for C++](/intl.en-US/Developer Guide/SDK Reference/Log Service SDK for C++.md)|[Log Service C++ SDK](https://github.com/aliyun/aliyun-log-cpp-sdk)|
|JavaScript SDK|[JavaScript SDK](/intl.en-US/Data Collection/Other collection methods/Use web tracking to collect logs.md)|None|

