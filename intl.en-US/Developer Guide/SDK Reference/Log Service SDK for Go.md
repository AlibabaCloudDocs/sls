# Log Service SDK for Go

This topic describes how to install and use Log Service SDK for Go.

-   Log Service is activated. For more information, see [Activate Log Service](https://www.aliyun.com/product/sls?spm=5176.7933691.J_8058803260.20.3eeb2a665LA0eU).
-   An AccessKey pair is created and obtained. For more information, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).
-   A Golang development environment is installed.

## Step 1: Install Log Service SDK for Go

Run the following command to install Log Service SDK for Go:

```
go get -u github.com/aliyun/aliyun-log-go-sdk
```

**Note:** No messages are displayed during the installation. The installation may take some time to complete. If the installation times out, run the preceding command again.

## Step 2: Create a Log Service client

A Log Service client is a Go client that you can use to manage Log Service resources, such as projects and Logstores. To use Log Service SDK for Go to initiate a service request, you must create a client instance.

```
AccessKeyID = "your_accesskey_id" // The AccessKey ID of your Alibaba Cloud account. For more information, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md). High security risks may arise if you use the AccessKey pair of your Alibaba Cloud account because the account has permissions to call all API operations. We recommend that you create and use a RAM user to call API operations or perform routine O&M.
AccessKeySecret = "your_accesskey_secret" // The AccessKey secret of your Alibaba Cloud account.
Endpoint = "cn-hangzhou.log.aliyuncs.com" // The endpoint of Log Service. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md). The endpoint of the China (Hangzhou) region is used as an example. Replace it with the actual endpoint. 
Client = sls.CreateNormalInterface(Endpoint,AccessKeyID,AccessKeySecret,"") // Create the Log Service client.
```

## Sample code

Log Service SDK for Go provides multiple sample programs. For more information, see [aliyun-log-go-sdk](https://github.com/aliyun/aliyun-log-go-sdk). The following sample code shows how to create a project and a Logstore:

```
project, err := client.CreateProject("project_name","description") // Enter the project name and the description.
    if err ! = nil {
        fmt.Println(err)
    }
    fmt.Println(project)
    err = client.CreateLogStore("project_name","logstore_name",2,2,true,16) // Enter the project name, Logstore name, data retention period, and number of shards. Specify whether you want to enable automatic sharding and specify the maximum number of shards. If you set ttl to 3650, the data is permanently stored.
    if err ! = nil {
        fmt.Println(err)
    }
```

