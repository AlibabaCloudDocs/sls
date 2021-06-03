# 日志（Log）

日志（Log）是系统运行过程中变化的一种抽象数据，其内容为指定对象的操作和其操作结果按时间的有序集合。

## 格式

文本日志（LogFile）、事件（Event）、数据库日志（BinLog）、时序数据（Metric）数据等都是日志的不同载体。日志服务采用半结构化的数据模式定义一条日志，包含日志主题（Topic）、时间（Time）、内容（Content）、来源（Source）和标签（Tags）五个数据域。日志服务对各个数据域的格式要求不同，详细说明如下：

|数据域|说明|格式|
|:--|:-|:-|
|日志主题（Topic）|自定义字段，用于标识日志的主题。例如您可以根据日志类型为网站相关日志设置不同的日志主题（access\_log、operation\_log）。更多信息，请参见[日志主题（Topic）](/intl.zh-CN/产品简介/基本概念/日志主题.md)。|包括空字符串在内的任意字符串，大小为0~128字节。该字段为空字符串时，表示未设置日志主题。 |
|时间（Time）|日志服务保留字段，一般为日志中的时间信息（日志生成时间）或采集日志时Logtail所在主机的系统时间。|Unix时间戳，即从1970-1-1 00:00:00 UTC开始所经过的秒数。|
|内容（Content）|记录日志的具体内容，由一个或多个内容项组成，每一个内容项为一个键值对（Key:Value）。|Key:Value格式，详细说明如下：-   Key为UTF-8编码字符串，可以为字母、下划线和数字但不以数字开头。字符串大小为1~128字节。不可使用如下字段：
    -   \_\_time\_\_
    -   \_\_source\_\_
    -   \_\_topic\_\_
    -   \_\_partition\_time\_\_
    -   \_extract\_others\_
    -   \_\_extract\_others\_\_
-   Value为任意字符串，大小不超过1 MB。 |
|来源（Source）|日志来源，例如产生日志的服务器IP地址。|任意字符串，大小为0~128字节。|
|标签（Tags）|日志标签。包括： -   自定义标签：通过[PutLogs](/intl.zh-CN/开发指南/API参考/日志库相关接口/PutLogs.md)接口，在写入数据时添加标签。
-   系统标签：日志服务为您添加的标签，包括\_\_client\_ip\_\_和\_\_receive\_time\_\_。

|字典格式，Key和Value均为字符串类型。在日志中以\_\_tag\_\_:为前缀展示。|

## 示例

以下以一条网站访问日志为例，说明原始日志与日志服务中数据模型的映射关系。

-   原始日志

    ```
    127.0.0.1 - - [01/Mar/2021:12:36:49  0800] "GET /index.html HTTP/1.1" 200 612 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36
    ```

-   通过极简模式采集到日志服务后的日志样例

    ![日志样例](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7782012261/p272301.png)

-   通过完整正则模式采集到日志服务后的日志样例

    ![日志样例](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7782012261/p272326.png)


