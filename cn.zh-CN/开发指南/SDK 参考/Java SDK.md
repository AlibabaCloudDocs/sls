# Java SDK

本文介绍安装日志服务Java SDK及使用Java SDK完成常见操作的相关步骤。

-   已开通日志服务。更多信息，请参见[开通日志服务](https://www.aliyun.com/product/sls?spm=5176.7933691.J_8058803260.20.3eeb2a665LA0eU)。
-   已创建并获取AccessKey。更多信息，请参见[访问密钥](/cn.zh-CN/开发指南/API 参考/访问密钥.md)。
-   已安装Java开发环境。

    日志服务Java SDK支持J2SE 6.0及以上的Java运行环境，您可以执行java -version命令检查您已安装的Java版本。如果未安装，可以从[Java官方网站](http://developers.sun.com/downloads/)下载安装包并完成安装。


## 步骤1：安装Java SDK

您可以通过以下两种方式安装日志服务Java SDK：

-   方式一：在Maven项目中加入依赖项（推荐方式）。

    在Maven工程中使用日志服务Java SDK，只需在pom.xml中加入相应依赖即可。以0.6.60版本为例，在<dependencies\>中加入如下内容：

    ```
    <dependency>
        <groupId>com.aliyun.openservices</groupId>
        <artifactId>aliyun-log</artifactId>
        <version>0.6.60</version>
        </dependency>
    ```

-   方式二：在Eclipse中导入JAR包。

    以0.6.60版本为例，步骤如下：

    1.  下载Java SDK，下载链接请参见[Aliyun LOG Java SDK](https://mvnrepository.com/artifact/com.aliyun.openservices/aliyun-log)。
    2.  将aliyun-log-0.6.60.jar拷贝到您的项目中。
    3.  在Eclipse中选择您的工程，右击选择**Properties** \> **Java Build Path** \> **Add JARs**。
    4.  选中步骤2中拷贝的JAR文件。

## 步骤2：创建日志服务Client

日志服务Client是日志服务的Java客户端，用于管理Project、Logstore等日志服务资源。使用Java SDK发起日志服务请求，您需要初始化一个Client实例。

```
String accessId = "your_access_id";    //阿里云访问密钥AccessKey ID。更多信息，请参见[访问密钥](/cn.zh-CN/开发指南/API 参考/访问密钥.md)。阿里云主账号AccessKey拥有所有API的访问权限，风险很高。强烈建议您创建并使用RAM账号进行API访问或日常运维。
String accessKey = "your_access_key";  //阿里云访问密钥AccessKey Secret。
String host = "cn-hangzhou-intranet.log.aliyuncs.com";  //日志服务的域名。更多信息，请参见[服务入口](/cn.zh-CN/开发指南/API 参考/服务入口.md)。此处以杭州为例，其它地域请根据实际情况填写。
Client client = new Client(host, accessId, accessKey);   //创建日志服务Client。
```

## Java SDK示例

日志服务Java SDK提供丰富的示例程序，方便参考或直接使用，更多信息，请参见[aliyun-log-java-sdk](https://github.com/aliyun/aliyun-log-java-sdk)。此处以创建Project和Logstore为例进行说明，示例代码如下所示：

```
//创建Project和Logstore。
String project = "my-project";  //待创建的Project的名称。
String logstore = "my-logstore";     //待创建的Logstore的名称。
int ttl_in_day = 3;     //数据保存时间。如果配置为3650，表示永久保存。
int shard_count = 10;   //Shard数量。
LogStore store = new LogStore(logstore, ttl_in_day, shard_count);
CreateLogStoreResponse res = client.CreateLogStore(project, store);
```

**说明：**

-   为了提高您系统的IO效率，请尽量不要直接使用SDK写数据到日志服务。如何写数据，请参见[Producer Library](/cn.zh-CN/数据采集/其他采集方式/SDK采集/Producer Library.md)。
-   如果您要消费日志服务中的数据，请尽量不要直接使用Java SDK中拉取数据的接口。您可以通过消费组消费日志数据，更多信息，请参见[通过消费组消费日志数据](/cn.zh-CN/消费与投递/实时消费/消费组消费/通过消费组消费日志数据.md)。该方式屏蔽了日志服务的实现细节，并且提供了负载均衡、按序消费等高级功能。

