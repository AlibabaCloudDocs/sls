# PHP SDK

本文介绍安装日志服务PHP SDK及使用PHP SDK完成常见操作的相关步骤。

-   已开通日志服务。更多信息，请参见[开通日志服务](https://www.alibabacloud.com/product/log-service?spm=a2c5t.10695662.1996646101.searchclickresult.536d31bdPTqffd)。
-   已创建并获取AccessKey。更多信息，请参见[访问密钥](/intl.zh-CN/开发指南/API 参考/访问密钥.md)。
-   已安装PHP开发环境。

    日志服务PHP SDK支持PHP 5.2.1及以上版本。您可以执行php -v命令检查您已安装的PHP版本。


## 安装PHP SDK

1.  [下载最新的PHP SDK包](https://github.com/aliyun/aliyun-log-php-sdk)。

2.  解压aliyun-log-php-sdk-master.zip包到指定目录。

    PHP SDK是一个软件开发包，不需要额外的安装操作。PHP SDK包中除了SDK代码外，还包括一组第三方依赖包和一个Autoloader类，用于简化您的调用流程。


## 步骤2：创建日志服务Client

Aliyun\_Log\_Client是日志服务的PHP客户端，用于管理Project、Logstore等日志服务资源。使用PHP SDK发起日志服务请求，您需要初始化一个Client实例。

```
$endpoint = 'cn-hangzhou.log.aliyuncs.com'; //日志服务的域名。更多信息，请参见[服务入口](/intl.zh-CN/开发指南/API 参考/服务入口.md)。此处以杭州为例，其它地域请根据实际情况填写。
$accessKeyId = '11****TY';        //阿里云访问密钥AccessKey ID。更多信息，请参见[访问密钥](/intl.zh-CN/开发指南/API 参考/访问密钥.md)。阿里云主账号AccessKey拥有所有API的访问权限，风险很高。强烈建议您创建并使用RAM账号进行API访问或日常运维。
$accessKey = 'YT****ED';             //阿里云访问密钥AccessKey Secret。
$client = new Aliyun_Log_Client($endpoint, $accessKeyId, $accessKey);  //创建日志服务Client。
```

## PHP SDK示例

日志服务PHP SDK提供丰富的示例程序，方便参考或直接使用，更多信息，请参见[aliyun-log-php-sdk](https://github.com/aliyun/aliyun-log-php-sdk)。此处以创建Logstore为例进行说明，示例代码如下所示：

```
$req2 = new Aliyun_Log_Models_CreateLogstoreRequest(test-project,test-logstore,3,2);  //配置Project名称、Logstore名称、数据保存时长和Shard数量。其中如果数据保存时长配置为3650，表示永久保存。
$res2 = $client -> createLogstore($req2);
```

