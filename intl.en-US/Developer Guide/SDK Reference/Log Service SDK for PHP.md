# Log Service SDK for PHP

This topic describes how to install and use Log Service SDK for PHP.

-   Log Service is activated. For more information, see [Activate Log Service](https://www.alibabacloud.com/product/log-service?spm=a2c5t.10695662.1996646101.searchclickresult.536d31bdPTqffd).
-   An AccessKey pair is created and obtained. For more information, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).
-   A PHP development environment is installed.

    Log service SDK for PHP supports PHP version 5.2.1 and later. You can run the php -v command to check the version of PHP that is installed.

    **Note:** Log Service SDK for PHP does not support Composer, which is a tool for dependency management.


## Step 1: Install Log Service SDK for PHP

1.  Click [here](https://github.com/aliyun/aliyun-log-php-sdk) to download the latest version of Log Service SDK for PHP.

2.  Decompress the aliyun-log-php-sdk-master.zip package to a specified directory.

    Log Service SDK for PHP does not require additional installation steps. Log Service SDK for PHP contains SDK code, a set of third-party dependencies, and an Autoloader class. This simplifies the process of SDK calls.


## Step 2: Create Aliyun\_Log\_Client

Aliyun\_Log\_Client is the PHP client of Log Service. You can use Aliyun\_Log\_Client to manage Log Service resources, such as projects and Logstores. To use Log Service SDK for PHP to initiate a service request, you must create a client instance.

```
$endpoint = 'cn-hangzhou.log.aliyuncs.com'; // The endpoint of Log Service. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md). In this example, the endpoint of the China (Hangzhou) region is used. Replace the parameter value with the actual endpoint. 
$accessKeyId = '11****TY';        // The AccessKey ID of your Alibaba Cloud account. For more information, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md). Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account because the account has permissions to call all API operations. We recommend that you create and use a RAM user to call API operations or perform routine O&M. 
$accessKey = 'YT****ED';             // The AccessKey secret of your Alibaba Cloud account. 
$client = new Aliyun_Log_Client($endpoint, $accessKeyId, $accessKey);  // Create Aliyun_Log_Client. 
```

## Sample code

Log Service SDK for PHP provides various sample programs. For more information, see [aliyun-log-php-sdk](https://github.com/aliyun/aliyun-log-php-sdk). The following sample code shows how to create a Logstore.

```
$req2 = new Aliyun_Log_Models_CreateLogstoreRequest(test-project,test-logstore,3,2);  // Enter the project name, Logstore name, data retention period, and the number of shards. If you set the data retention period to 3650, data is permanently stored. 
$res2 = $client -> createLogstore($req2);
```

