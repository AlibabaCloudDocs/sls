# Parquet格式

日志服务将日志投递到OSS后，支持存储为不同的文件格式，本文介绍Parquet格式。

## 参数配置

在[配置投递规则](/cn.zh-CN/消费与投递/数据投递/投递日志到OSS/将日志服务数据投递到OSS.md)时，如果选择**存储格式**为**Parquet**，对应的参数配置如下所示。

![Parquet字段配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9777688951/p5812.png)

相关参数说明如下表所示。

|参数|说明|
|--|--|
|Key名称|您可以在日志服务的原始日志页签中查看日志字段的Key，将您需要投递到OSS的字段名（Key）有序填入，在投递时将按照此顺序组织Parquet数据，并使用该key作为Parquet数据列名。除了日志内容的Key外，日志服务还提供保留字段\_\_time\_\_、\_\_topic\_\_、\_\_source\_\_，保留字段详情请参见[保留字段](/cn.zh-CN/产品简介/限制说明/保留字段.md)。如果遇到如下两种情况时，Parquet数据列的值为null。 -   此处配置的key在日志服务的日志中不存在。
-   字段由string类型转换非string类型（如double、int64等）失败。

**说明：** 同一个Key在**Parquet字段**中只能配置一次，不支持多次使用。 |
|类型|Parquet存储支持6种类型：string、boolean、int32、int64、float、double。 日志投递过程中，会将日志服务中的日志字段由string类型转换为Parquet目标类型。如果转换到非string类型失败，则该列数据为null。 |

## OSS文件地址

投递到OSS后，OSS文件地址示例如下表所示。

|压缩类型|文件后缀|OSS文件地址示例|说明|
|:---|:---|:--------|--|
|不压缩|.parquet|oss://oss-shipper-shenzhen/ecs\_test/2016/01/26/20/54\_1453812893059571256\_937.parquet|下载到本地，使用[parquet-tools](https://github.com/apache/parquet-mr/tree/master/parquet-tools-deprecated)工具打开。|
|压缩（snappy）|.snappy.parquet|oss://oss-shipper-shenzhen/ecs\_test/2016/01/26/20/54\_1453812893059571256\_937.snappy.parquet|下载到本地，使用[parquet-tools](https://github.com/apache/parquet-mr/tree/master/parquet-tools-deprecated)工具打开。|

## 数据消费

-   通过E-MapReduce、Spark 、Hive消费数据，详情请参见[社区文档](https://cwiki.apache.org//confluence/display/Hive/LanguageManual+DDL)。
-   通过单机校验工具消费数据

    开源社区提供的[parquet-tools](https://github.com/apache/parquet-mr/tree/master/parquet-tools-deprecated)可以用来文件级别验证Parquet格式、查看schema、读取数据内容。您可以自行编译该工具或者单击[parquet-tools-1.6.0rc3-SNAPSHOT](https://logservice-resource.oss-cn-shanghai.aliyuncs.com/tools/parquet-tools-1.6.0rc3-SNAPSHOT.jar)，下载日志服务提供的版本。

    -   查看Parquet文件schema

        ```
        $ java -jar parquet-tools-1.6.0rc3-SNAPSHOT.jar schema -d 00_1490803532136470439_124353.snappy.parquet | head -n 30
        message schema {
          optional int32 __time__;
          optional binary ip;
          optional binary __source__;
          optional binary method;
          optional binary __topic__;
          optional double seq;
          optional int64 status;
          optional binary time;
          optional binary url;
          optional boolean ua;
        }
        creator: parquet-cpp version 1.0.0
        file schema: schema
        --------------------------------------------------------------------------------
        __time__: OPTIONAL INT32 R:0 D:1
        ip: OPTIONAL BINARY R:0 D:1
        .......
        ```

    -   查看Parquet文件全部内容

        ```
        $ java -jar parquet-tools-1.6.0rc3-SNAPSHOT.jar head -n 2 00_1490803532136470439_124353.snappy.parquet
        __time__ = 1490803230
        ip = 10.200.98.220
        __source__ = *.*.*.*
        method = POST
        __topic__ =
        seq = 1667821.0
        status = 200
        time = 30/Mar/2017:00:00:30 +0800
        url = /PutData?Category=YunOsAccountOpLog&AccessKeyId=*************&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=********************************* HTTP/1.1
        __time__ = 1490803230
        ip = 10.200.98.220
        __source__ = *.*.*.*
        method = POST
        __topic__ =
        seq = 1667822.0
        status = 200
        time = 30/Mar/2017:00:00:30 +0800
        url = /PutData?Category=YunOsAccountOpLog&AccessKeyId=*************&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=********************************* HTTP/1.1
        ```

    更多用法请执行java -jar parquet-tools-1.6.0rc3-SNAPSHOT.jar -h命令查看帮助。


