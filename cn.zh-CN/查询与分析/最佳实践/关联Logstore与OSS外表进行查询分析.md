# 关联Logstore与OSS外表进行查询分析

在进行日志数据查询分析时，经常需要结合外部表格对日志数据进行分析。本文介绍如何在日志服务中联合OSS外表进行数据分析。

-   已采集日志，详情请参见[数据采集](/cn.zh-CN/数据采集/采集方式.md)。
-   已开启并配置索引，详情请参见[开启并配置索引](/cn.zh-CN/查询与分析/开启并配置索引.md)。
-   已创建OSS Bucket，详情请参见[创建存储空间](/cn.zh-CN/快速入门/创建存储空间.md)。

某支付公司，想要分析用户年龄、地域、性别等因素对支付习惯的影响。该公司已通过日志服务实时采集用户支付行为（支付方式、支付费用等）日志，并将用户属性（地域、年龄、性别等）信息保存在OSS中。针对该场景，日志服务查询分析引擎提供Logstore和外部数据源（ExternalStore，例如MySQL数据库、OSS等）联合查询分析功能。您可以使用SQL的JOIN语法把用户属性数据和行为数据进行联合，分析与用户属性相关的指标。

与OSS进行关联查询分析，具有如下优势：

-   节省费用：将更新频率低的数据保存在OSS上，只需要支付少量的存储费用，并且可以通过内网读数据，免去流量费用。
-   降低运维工作：在轻量级的联合分析平台中，不需要搬迁数据到同一个存储系统中。
-   节省时间：使用SQL分析数据，分析结果秒级可见，并可以将常用的分析结果定义为报表，打开即可看到结果。

1.  创建CSV文件并上传到OSS。

    1.  创建名为user.csv的文件。

        ![属性文件](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0540559951/p41528.png)

    2.  上传user.csv文件到OSS，详情请参见[上传文件](/cn.zh-CN/快速入门/上传文件.md)。

2.  登录[日志服务控制台](https://sls.console.aliyun.com)。

3.  在Project列表区域，单击目标Project。

4.  在**日志存储** \> **日志库**页签中，单击目标Logstore。

5.  输入查询分析语句，单击**查询/分析**。

    通过SQL定义虚拟外部表（此处以user\_meta1为例），映射到OSS文件，如果执行结果中的**result**为**true**，表示执行成功。

    ```
    * | create table user_meta1 ( userid bigint, nick varchar, gender varchar, province varchar, gender varchar,age bigint) with ( endpoint='example.com',accessid='<youraccessid>',accesskey='<accesskey>',bucket='testossconnector',objects=ARRAY['user.csv'],type='oss')
    ```

    ![外部存储](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0540559951/p8538.png)

    在查询分析语句中定义外部存储名称、表的Schema等信息，并通过WITH语法指定OSS访问信息及文件信息，详细信息如下表所示。

    |配置项|说明|
    |:--|:-|
    |外部存储名称|外部存储名称，即虚拟表的名称，例如user\_meta1。|
    |表的Schema|定义表的属性，包括表的列名及格式，例如\(userid bigint, nick varchar, gender varchar, province varchar, gender varchar,age bigint\)。|
    |endpoint|OSS Bucket访问域名，详情请参见[访问域名和数据中心](/cn.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md)。|
    |accessid|您的AccessKey ID。|
    |accesskey|您的AccessKey Secret。|
    |bucket|CSV文件所在的OSS Bucket名称。|
    |objects|CSV文件路径。 **说明：** objects为array类型，可以包含多个OSS文件。 |
    |type|固定为oss，表示外部存储类型为OSS。|

6.  验证是否成功定义外部表。

    执行如下语句，如果返回结果为您之前定义的表内容，则表示定义外部表成功。

    ```
    select * from user_meta1
    ```

    ![验证结果](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0540559951/p8539.png)

7.  通过JOIN语法完成Logstore和OSS外表的联合查询。

    执行如下查询分析语句关联日志服务中日志的ID和OSS文件中的userid，补全日志信息。其中，test\_accesslog为Logstore名称，l为Logstore别名，user\_meta1为您定义的外部存储表，请根据实际情况替换。

    ```
    * | select * from test_accesslog l join user_meta1 u on l.userid = u.userid
    ```

    联合查询示例：

    -   统计不同性别用户的访问情况

        ```
        * | select u.gender, count(1) from test_accesslog l join user_meta1 u on l.userid = u.userid group by u.gender
        ```

        ![性别访问](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0540559951/p41547.png)

    -   统计不同年龄用户的访问情况

        ```
        * | select u.age, count(1) from test_accesslog l join user_meta1 u on l.userid = u.userid group by u.age
        ```

        ![年龄访问](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0540559951/p41550.png)

    -   统计不同年龄段在时间维度上的访问趋势

        ```
        * | select date_trunc('minute',__time__) as minute, count(1) ,u.age from test_accesslog l join user_meta1 u on l.userid = u.userid group by u.age,minute
        ```

        ![时间访问](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0540559951/p41551.png)


