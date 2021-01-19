# 将日志服务数据投递到OSS

日志服务采集到日志后，支持将日志投递至OSS进行存储与分析。本文档介绍将日志投递至OSS的操作步骤。

-   已创建Project和Logstore。更多信息，请参见[创建Project和Logstore](/cn.zh-CN/快速入门/快速入门.md)。
-   已采集到日志。更多信息，请参见[数据采集](/cn.zh-CN/数据采集/采集方式.md)。
-   已开通OSS服务，且在日志服务Project所在的地域创建Bucket。更多信息，请参见[开通OSS服务](/cn.zh-CN/控制台用户指南/开通OSS服务.md)。
-   已完成[云资源访问授权](https://ram.console.aliyun.com/#/role/authorize?request=%7B%22Requests%22%3A%20%7B%22request1%22%3A%20%7B%22RoleName%22%3A%20%22AliyunLogDefaultRole%22%2C%20%22TemplateId%22%3A%20%22DefaultRole%22%7D%7D%2C%20%22ReturnUrl%22%3A%20%22https%3A//sls.console.aliyun.com/%22%2C%20%22Service%22%3A%20%22Log%22%7D)。

    如果您要跨阿里云账号或使用RAM用户配置投递规则，请参见[RAM授权](/cn.zh-CN/消费与投递/数据投递/投递日志到OSS/RAM授权.md)。

    ![云资源访问授权](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0877688951/p112344.png)


日志服务支持将Logstore中的数据自动归档到OSS，以发挥更多的日志价值。

-   OSS支持自由设置生命周期，可以长期存储日志。
-   您可以通过数据处理平台（例如E-MapReduce和DLA）或自建程序消费OSS数据。

## 投递数据

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在Project列表区域，单击目标Project。

3.  在**日志存储** \> **日志库**页签中，单击目标Logstore左侧的**\>**，选择**数据处理** \> **导出** \> **OSS（对象存储）**。

4.  单击**开启投递**。

5.  在投递提示对话框中，单击**直接投递**。

6.  配置OSS投递规则。

    重要参数配置如下所示。

    |参数|说明|
    |:-|:-|
    |OSS投递名称|投递规则的名称。 只能包含小写字母、数字、短划线（-）和下划线（\_），必须以小写字母或数字作为开头和结尾，且名称长度为2~128字符。 |
    |OSS Bucket|OSS Bucket名称。 **说明：** 必须是已存在的OSS Bucket名称，且该OSS Bucket与日志服务Project位于相同地域。 |
    |OSS Prefix|OSS Bucket中的目录，将日志服务中的日志投递到该OSS Bucket目录中。|
    |分区格式|按照投递任务的创建时间动态生成OSS Bucket目录，不能以正斜线（/）开头，默认值为%Y/%m/%d/%H/%M，相关示例请参见[分区格式](#section_gzu_5la_tzu)，参数详情请参见[strptime API](http://man7.org/linux/man-pages/man3/strptime.3.html)。|
    |RAM角色|OSS Bucket拥有者创建的角色标识（ARN），用于访问权限控制。例如：acs:ram::45643:role/aliyunlogdefaultrole。获取ARN的方法请参见[如何获取RAM角色标识](#section_6y4_v2h_1n4)。|
    |投递大小|通过投递大小，控制OSS Object大小（以未压缩计算），取值范围为5~256，单位为MB。 当投递数据量达到此处设置的大小时，会自动创建一个新的投递任务。 |
    |存储格式|日志数据投递到OSS后，支持存储为不同的文件格式，包括JSON格式、CSV格式和Parquet格式。更多信息，请参见[CSV格式](/cn.zh-CN/消费与投递/数据投递/投递日志到OSS/CSV格式.md)。|
    |是否压缩|OSS数据存储的压缩方式。     -   不压缩：表示原始数据不压缩。
    -   压缩（snappy）：表示使用[snappy](https://google.github.io/snappy/)算法压缩数据，可减少OSS Bucket存储空间。 |
    |是否投递tag|是否投递日志标签。|
    |投递时间|投递任务的时长。取值范围为300~900，默认值为300，单位为秒。 当投递任务的时长达到此处设置的大小时，会自动创建一个新的投递任务。 |

7.  单击**确定**。

    **说明：**

    -   开启日志投递后，日志服务并发执行投递任务，每个Shard都会根据投递大小、投递时间决定任务生成的频率，当任一条件满足时，即会创建投递任务。
    -   创建投递任务后，您可以通过投递任务的状态和投递到OSS的数据确认投递规则是否符合预期结果。

## 查看OSS数据

将日志投递到OSS成功后，您可以通过OSS控制台、API、SDK或其它方式访问OSS数据。更多信息，请参见[文件管理](/cn.zh-CN/控制台用户指南/文件管理/文件概览.md)。

OSS Object地址格式如下所示：

```
oss://OSS-BUCKET/OSS-PREFIX/PARTITION-FORMAT_RANDOM-ID
```

OSS-BUCKET为OSS Bucket名称，OSS-PREFIX为目录前缀，PARTITION-FORMAT为分区格式（由投递任务的创建时间通过[strptime API](http://man7.org/linux/man-pages/man3/strptime.3.html)计算得到），RANDOM-ID是投递任务的唯一标识。

**说明：** OSS Bucket目录是按照投递任务创建时间设置的。例如：2016-06-23 00:00:00创建投递任务，投递的是2016-06-22 23:55后写入日志服务的数据，假设5分钟投递一次数据到OSS。如果您要分析2016-06-22全天日志，除了查看2016/06/22目录下的全部object以外，还需要检查2016/06/23/00/目录下前十分钟的Object是否包含2016-06-22的日志。

## 分区格式

一个投递任务对应一个OSS Bucket目录，目录格式为oss://OSS-BUCKET/OSS-PREFIX/PARTITION-FORMAT\_RANDOM-ID。PARTITION-FORMAT是根据投递任务的创建时间格式化而得到的，以创建时间为2017/01/20 19:50:43的投递任务为例，介绍分区格式，如下表所示。

|OSS Bucket|OSS Prefix|分区格式|OSS文件路径|
|:---------|:---------|:---|:------|
|test-bucket|test-table|%Y/%m/%d/%H/%M|oss://test-bucket/test-table/2017/01/20/19/50\_1484913043351525351\_2850008|
|test-bucket|log\_ship\_oss\_example|year=%Y/mon=%m/day=%d/log\_%H%M%s|oss://test-bucket/log\_ship\_oss\_example/year=2017/mon=01/day=20/log\_195043\_1484913043351525351\_2850008.parquet|
|test-bucket|log\_ship\_oss\_example|ds=%Y%m%d/%H|oss://test-bucket/log\_ship\_oss\_example/ds=20170120/19\_1484913043351525351\_2850008.snappy|
|test-bucket|log\_ship\_oss\_example|%Y%m%d/|oss://test-bucket/log\_ship\_oss\_example/20170120/\_1484913043351525351\_2850008|
|test-bucket|log\_ship\_oss\_example|%Y%m%d%H|oss://test-bucket/log\_ship\_oss\_example/2017012019\_1484913043351525351\_2850008|

使用Hive、MaxCompute等大数据平台或阿里云DLA产品分析OSS数据时，如果您希望使用Partition信息，可将PARTITION-FORMAT设置为key=value格式。例如：oss://test-bucket/log\_ship\_oss\_example/year=2017/mon=01/day=20/log\_195043\_1484913043351525351\_2850008.parquet，设置为三层分区列，分别为：year、mon、day。

## 相关操作

创建投递任务后，您可以在OSS投递管理页面，执行修改投递规则、关闭投递、查看投递任务状态及错误信息、重试投递任务等操作。

-   修改投递规则

    单击**投递配置**，修改投递规则，参数详情请参见本文中的[投递数据](#section_gz3_etl_ezq)。

-   关闭投递

    单击**关闭投递**，即可关闭投递。

-   查看投递任务状态及错误信息

    日志服务支持查看过去两天内的所有日志投递任务及其投递状态。

    -   任务状态

        |状态|说明|
        |:-|:-|
        |成功|投递任务正常运行。|
        |进行中|投递任务进行中，请稍后查看是否投递成功。|
        |失败|因外部原因而无法重试的错误导致投递任务失败，请根据错误信息进行排查并重试。|

    -   错误信息

        如果投递任务出现错误，控制台上会显示相应的错误信息。

        |错误信息|错误原因|处理方法|
        |:---|:---|:---|
        |UnAuthorized|没有权限。|请确认以下配置：         -   OSS Bucket拥有者是否已创建AliyunLogDefaultRole角色。
        -   角色描述中配置的阿里云账号ID是否正确。
        -   AliyunLogDefaultRole角色是否被授予OSS Bucket写权限。
        -   RAM角色标识是否配置正确。 |
        |ConfigNotExist|配置不存在。|一般是由于关闭投递导致的。请在重新开启投递并配置投递规则后，通过重试解决。|
        |InvalidOssBucket|OSS Bucket不存在。|请确认以下配置：         -   OSS Bucket所在地域与日志服务Project所在地域是否相同。
        -   Bucket名称是否配置正确。 |
        |InternalServerError|日志服务内部错误。|通过重试解决。|

    -   重试任务

        日志服务会按照策略默认为您重试，您也可以手动重试。日志服务默认重试最近两天之内的所有任务，重试等待的最小间隔是15分钟。当任务执行失败时，第一次失败需要等待15分钟再进行重试，第二次失败需要等待30分钟再进行重试，第三次失败需要等待60分钟再进行重试，以此类推。

        如果您需要立即重试失败任务，请单击**重试全部失败任务**、目标任务右侧的**重试**或通过API、SDK指定任务进行重试。


## 常见问题

如何获取RAM角色标识？

1.  登录[RAM 控制台](https://ram.console.aliyun.com/)。

2.  在左侧导航栏中，单击RAM 角色管理。

3.  在RAM角色列表中，单击AliyunLogDefaultRole。

4.  在基本信息区域，获取ARN。

    ![获取角色ARN](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8777688951/p113301.png)


