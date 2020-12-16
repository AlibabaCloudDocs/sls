# Log Service SDK for PHP

This topic describes how to install and use Log Service SDK for PHP.

-   Log Service is activated. For more information, see [Activate Log Service](https://www.aliyun.com/product/sls?spm=5176.7933691.J_8058803260.20.3eeb2a665LA0eU).
-   An AccessKey pair is created and obtained. For more information, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).
-   A PHP development environment is installed.

    Log service SDK for PHP supports PHP version 5.2.1 and later. You can run the php -V command to check the version of PHP that you have installed.


## Install Log Service SDK for PHP

1.  [Download the latest Log Service SDK for PHP](https://github.com/aliyun/aliyun-log-php-sdk).

2.  Decompress the aliyun-log-php-sdk-master.zip package to a specified directory.

    The SDK for PHP does not require installation. The SDK for PHP contains SDK code, a set of third-party dependencies, and an Autoloader class. This simplifies the process of SDK calls.


## Step 2: Create Aliyun\_Log\_Client

Aliyun\_Log\_Client is the PHP client of Log Service. You can use Aliyun\_Log\_Client to manage Log Service resources, such as projects and Logstores. To use Log Service SDK for PHP to initiate a service request, you must create a client instance.

```
$endpoint = 'cn-hangzhou.sls.aliyuncs.com '; // The endpoint of Log Service. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md). The endpoint of the China (Hangzhou) region is used as an example. Replace it with the actual endpoint.
$accessKeyId = '11****TY'; // The AccessKey ID of your Alibaba Cloud account. For more information, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md). High security risks may arise if you use the AccessKey pair of your Alibaba Cloud account because the account has permissions to call all API operations. We recommend that you create and use a RAM user to call API operations or perform routine O&M.
$accessKey = 'YT****ED'; // The AccessKey secret of your Alibaba Cloud account.
$client = new Aliyun_Log_Client($endpoint, $accessKeyId, $accessKey); // Create Aliyun_Log_Client.
```

## Sample

Log Service SDK for PHP provides multiple sample programs. For more information, see [aliyun-log-php-sdk](https://github.com/aliyun/aliyun-log-php-sdk). The following sample code shows how to create a Logstore.

```
$req2 = new Aliyun_Log_Models_CreateLogstoreRequest(test-project,test-logstore,3,2); // Enter the project name, Logstore name, data retention period, and the number of shards. If you set ttl to 3650, data is permanently stored.
$res2 = $client -> createLogstore($req2);
```

