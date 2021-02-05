# Parquet-formatted data

This topic describes how to ship data from Log Service to OSS and store the data in the Parquet format.

## Configuration parameters

You can set the storage format of data that is shipped to OSS. The following figure shows how to set the **storage format** to **Parquet**. For more information, see [Configure a data shipping rule](/intl.en-US/Log consumption and shipping/Data shipping/Ship logs to OSS/Ship log data from Log Service to OSS.md).

![Parquet keys](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7257549951/p5812.png)

The following table describes the configuration parameters in the preceding figure.

|Parameter|Description|
|---------|-----------|
|Key Name|The name of the log field that you want to ship to OSS. You can view log fields on the Raw Logs tab of a Logstore. You can also enter the names of the fields that you want to ship to OSS in the Key Name column. When the fields are shipped to OSS, they are stored in the Parquet format in the order that the field names are entered. The names of the fields are the column names in OSS. The log fields that you can ship to OSS include the fields in the log content and the reserved fields such as \_\_time\_\_, \_topic\_\_, and \_\_source\_\_. For more information about reserved fields, see [Reserved fields](/intl.en-US/Product Introduction/Limits/Reserved fields.md). The value of a field in the Parquet format is null in the following two scenarios: -   The field does not exist in logs.
-   The value of the field fails to be converted from the string type to a non-string type, for example, double or Int64.

**Note:** The keys that you enter in the **Parquet Keys** field must be unique. |
|Type|The Parquet storage format supports six data types: string, Boolean, Int32, Int64, float, and double. Log fields are converted from the string type to a data type that the Parquet storage format supports. If the data type of a log field fails to be converted, the value of the log field is null. |

## Directories in OSS buckets

The following table lists the directories in OSS buckets that store data shipped from Log Service.

|Compressed|File extension|Example|Description|
|:---------|:-------------|:------|-----------|
|No|.parquet|oss://oss-shipper-shenzhen/ecs\_test/2016/01/26/20/54\_1453812893059571256\_937.parquet|After you download the OSS buckets to the local server, you can use the parquet-tools utility to open the buckets. For more information about the parquet-tools utility, visit [parquet-tools](https://github.com/apache/parquet-mr/tree/master/parquet-tools-deprecated).|
|Yes \(compressed by using Snappy\)|.snappy.parquet|oss://oss-shipper-shenzhen/ecs\_test/2016/01/26/20/54\_1453812893059571256\_937.snappy.parquet|After you download the OSS buckets to the local server, you can use the parquet-tools utility to open the buckets. For more information about the parquet-tools utility, visit [parquet-tools](https://github.com/apache/parquet-mr/tree/master/parquet-tools-deprecated).|

## Data consumption

-   You can use E-MapReduce, Spark, and Hive to consume data. For more information, visit [LanguageManual DDL](https://cwiki.apache.org//confluence/display/Hive/LanguageManual+DDL).
-   You can use inspection tools to consume data.

    The [parquet-tools](https://github.com/apache/parquet-mr/tree/master/parquet-tools-deprecated) utility can be used to inspect Parquet files, view the schema of the data stored in the files, and read the data. You can compile the utility or download the [parquet-tools-1.6.0rc3-SNAPSHOT](https://logservice-resource.oss-cn-shanghai.aliyuncs.com/tools/parquet-tools-1.6.0rc3-SNAPSHOT.jar) utility that Log Service provides to consume data.

    -   To view the schema of the data stored in a Parquet file, use the following sample code:

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

    -   To view the data stored in a Parquet file, use the following sample code:

        ```
        $ java -jar parquet-tools-1.6.0rc3-SNAPSHOT.jar head -n 2 00_1490803532136470439_124353.snappy.parquet
        __time__ = 1490803230
        ip = 10.200.98.220
        __source__ = *. *. *.*
        method = POST
        __topic__ =
        seq = 1667821.0
        status = 200
        time = 30/Mar/2017:00:00:30 +0800
        url = /PutData? Category=YunOsAccountOpLog&AccessKeyId=*************&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=********************************* HTTP/1.1
        __time__ = 1490803230
        ip = 10.200.98.220
        __source__ = *. *. *.*
        method = POST
        __topic__ =
        seq = 1667822.0
        status = 200
        time = 30/Mar/2017:00:00:30 +0800
        url = /PutData? Category=YunOsAccountOpLog&AccessKeyId=*************&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=********************************* HTTP/1.1
        ```

    For more information, run the java -jar parquet-tools-1.6.0rc3-SNAPSHOT.jar -h command.


