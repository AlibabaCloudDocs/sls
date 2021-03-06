# Log Service SDK for .NET Core

This topic describes how to install and use Log Service SDK for .NET Core.

-   Log Service is activated. For more information, see [Activate Log Service](https://www.alibabacloud.com/product/log-service?spm=a2c5t.10695662.1996646101.searchclickresult.536d31bdPTqffd).
-   An AccessKey pair is created and obtained. For more information, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).
-   A project is created. For more information, see [Create a project](/intl.en-US/Data Collection/Preparation/Manage a project.md).
-   A .NET Core development environment is installed.

    Log Service SDK for .NET Core supports the following versions:

    -   .NET Core 2.0
    -   .NET Framework \(with .NET Core 1.x SDK\) 4.6.2
    -   .NET Framework \(with .NET Core 2.0 SDK\) 4.6.1
    -   Mono 5.4
    -   Xamarin.iOS 10.14
    -   Xamarin.Mac 3.8
    -   Xamarin.Android 8.0
    -   Universal Windows Platform 10.0.16299

## Step 1: Install Log Service SDK for .NET Core

You can create a project to install Log Service SDK for .NET Core. The following example shows how to install Log Service SDK for .NET Core by using Visual Studio 2019:

1.  [Download Log Service SDK for .NET Core](https://github.com/aliyun/aliyun-log-dotnetcore-sdk).

2.  Decompress the installation package aliyun-log-dotnetcore-sdk-master.zip to a specified directory.

3.  Use Visual Studio 2019 to open the aliyun-log-dotnetcore-sdk-master\\Aliyun.Api.LogService.Examples\\Aliyun.Api.LogService.Examples.csproj file, and then debug and install the SDK.


## Step 2: Create a Log Service client

ILogServiceClient is the .NET Core client that you can use to manage Log Service resources, such as projects and Logstores. To use Log Service SDK for .NET Core to initiate a service request, you must create a client instance.

```
public static ILogServiceClient BuildSimpleClient()
{
    return LogServiceClientBuilders.HttpBuilder
        .Endpoint("endpoint", "projectName") // The endpoint of Log Service and the name of the project. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).
        .Credential("accessKeyId", "accessKey") // The AccessKey ID and the AccessKey secret of your Alibaba Cloud account. For more information, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md). High security risks may arise if you use the AccessKey pair of your Alibaba Cloud account because the account has permissions to call all API operations. We recommend that you create and use a RAM user to call API operations or perform routine O&M.
        .Build();
}
```

## Sample code

Log Service SDK for .NET Core provides multiple sample programs. For more information, see [aliyun-log-dotnetcore-sdk](https://github.com/aliyun/aliyun-log-dotnetcore-sdk). The following sample code shows how to throw an exception:

```
public async Task Caller()
{
    try
    {
        return await Wrapper();
    } catch (LogServiceException e)
    {
        // After an exception is captured, the following information is obtained: 
        Console.WriteLine($@"
            RequestId (The ID of the request): {e.RequestId}
            ErrorCode (The error code): {e.ErrorCode}
            ErrorMessage (The error message): {e.ErrorMessage}");
        throw;
    }
}

private async Task<IResponse> Wrapper()
{
    var response = await client.GetLogsAsync(...);

    return response
        // If a failure response is returned, the exception is thrown.
        .EnsureSuccess()
        .Result;
}
```

