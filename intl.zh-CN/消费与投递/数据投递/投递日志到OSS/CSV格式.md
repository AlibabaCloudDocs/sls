# CSV格式

日志服务将日志投递到OSS后，支持存储为不同的文件格式。本文介绍CSV格式。

## 参数配置

在[配置投递规则](/intl.zh-CN/消费与投递/数据投递/投递日志到OSS/将日志服务数据投递到OSS.md)时，如果选择**存储格式**为**csv**，对应的参数配置如下所示。

![CSV字段配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9777688951/p5813.png)

参数说明如下所示。更多信息，请参见[CSV标准](https://tools.ietf.org/html/rfc4180)和[postgresql CSV说明](https://www.postgresql.org/docs/9.4/static/sql-copy.html)。

|参数|说明|
|:-|:-|
|CSV字段|您可以在日志服务的原始日志页签中查看日志字段的Key，将您需要投递到OSS的字段名（Key）有序填入。 除了日志内容的Key和Value外，日志服务还提供保留字段\_\_time\_\_、\_\_topic\_\_、\_\_source\_\_，保留字段更多信息，请参见[保留字段](/intl.zh-CN/产品简介/使用限制/保留字段.md)。 **说明：** 同一个Key在**CSV字段**中只能配置一次，不支持多次使用。 |
|分隔符|支持半角逗号（,）、竖线（\|）、空格和制表符，用于分割不同字段。|
|转义符|当字段内出现分隔符时，需要使用转义符包裹该字段，避免读数据时造成字段错误分割。|
|无效字段内容|当**CSV字段**中指定的Key不存在时，投递此参数配置的内容。|
|投递字段名称|打开**投递字段名称**开关后，将字段名称写入到CSV文件。|

## OSS文件地址

投递到OSS后，OSS文件地址样例如下所示。

|压缩类型|文件后缀|OSS文件地址举例|说明|
|:---|:---|:--------|--|
|无压缩|.csv|oss://oss-shipper-shenzhen/ecs\_test/2016/01/26/20/54\_1453812893059571256\_937.csv|未压缩的CSV文件可下载到本地，以文本方式打开查看。|
|Snappy|.snappy.csv|oss://oss-shipper-shenzhen/ecs\_test/2016/01/26/20/54\_1453812893059571256\_937.snappy.csv|Snappy压缩文件的打开方式，请参见[打开Snappy压缩文件](/intl.zh-CN/消费与投递/数据投递/投递日志到OSS/解压snappy压缩文件.md)。|

## 示例

如果您通过HybridDB for PostgreSQL来消费OSS的CSV文件数据，相关投递参数配置如下所示。

-   分隔符：半角逗号（,）。
-   转义符：半角双引号（“）。
-   无效字段内容：不填写。
-   投递字段名称：不勾选（HybridDB for PostgreSQL默认CSV文件行首无字段说明）。

