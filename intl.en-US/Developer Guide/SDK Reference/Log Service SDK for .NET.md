# Log Service SDK for .NET

This topic describes how to install and use Log Service SDK for .NET.

-   Log Service is activated. For more information, see [Activate Log Service](https://www.alibabacloud.com/product/log-service?spm=a2c5t.10695662.1996646101.searchclickresult.536d31bdPTqffd).
-   An AccessKey pair is created and obtained. For more information, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).
-   A .NET development environment is installed.

    Log Service SDK for .NET supports .NET Framework 3.5, .NET Framework 4.0, and .NET Framework 4.5. We recommend that you install the following versions:

    -   Microsoft .NET Framework 3.5, Microsoft .NET Framework 4.0, or Microsoft .NET Framework 4.5.
    -   Visual Studio 2010 or later.

## Step 1: Install Log Service SDK for .NET

You can create a project to install Log Service SDK for .NET. The following example shows how to install Log Service SDK for .NET by using Visual Studio 2019:

1.  [Download Log Service SDK for .NET](https://github.com/aliyun/aliyun-log-csharp-sdk).

2.  Decompress the installation package aliyun-log-csharp-sdk-master.zip to a specified directory.

3.  Use Visual Studio 2019 to open the aliyun-log-csharp-sdk-master\\LOGSDKSample\\LOGSDKSample.csproj file, and then debug and install the SDK.


## Step 2: Create a Log Service client

LogClient is the C\# client that you can use to manage Log Service resources, such as projects and Logstores. To use Log Service SDK for .NET to initiate a service request, you must create a client instance.

```
String endpoint = "cn-hangzhou.log.aliyuncs.com", // The endpoint of Log Service. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md). The endpoint of the China (Hangzhou) region is used as an example. Replace it with the actual endpoint.
accesskeyId = "your accesskey id", // The AccessKey ID of your Alibaba Cloud account. For more information, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md). High security risks may arise if you use the AccessKey pair of your Alibaba Cloud account because the account has permissions to call all API operations. We recommend that you create and use a RAM user to call API operations or perform routine O&M.
accessKey = "your access key", // The AccessKey secret of your Alibaba Cloud account.
LogClient client = new LogClient(endpoint, accesskeyId, accessKey);
```

## Sample code

Log Service SDK for .NET provides multiple sample programs. For more information, see [aliyun-log-csharp-sdk](https://github.com/aliyun/aliyun-log-csharp-sdk). The following sample code shows how to write logs to Log Service:

```
PutLogsRequest putLogsReqError = new PutLogsRequest();
            putLogsReqError.Project = my-project; // The name of the project.
            putLogsReqError.Topic = "topic"; // The topic of logs.
            putLogsReqError.Logstore = my-logstore; // The name of the Logstore.
            putLogsReqError.LogItems = new List<LogItem>();
            for (int i = 1; i <= 10; ++i)
            {
                LogItem logItem = new LogItem();
                logItem.Time = DateUtils.TimeSpan();
                for (int k = 0; k < 10; ++k)
                    logItem.PushBack("error_" + i.ToString(), "invalid operation");
                putLogsReqError.LogItems.Add(logItem);
            }
            PutLogsResponse putLogRespError = client.PutLogs(putLogsReqError);

            Thread.Sleep(5000);
```

