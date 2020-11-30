# 将日志服务数据投递到AnalyticDB MySQL

日志服务采集到日志后，支持将日志投递至云原生数据仓库AnalyticDB MySQL进行存储与分析。本文档介绍将日志投递至AnalyticDB MySQL的操作步骤。

## 前提条件

-   已采集日志到目标Logstore。更多信息，请参见[数据采集](/intl.zh-CN/数据采集/采集方式.md)。
-   在AnalyticDB MySQL中已完成以下准备工作：
    1.  在日志服务Project所在地域，创建AnalyticDB MySQL集群。更多信息，请参见[t1854366.dita\#task1307]()。

        **说明：** 目前日志服务仅支持同地域投递。

    2.  创建数据库账号。更多信息，请参见[创建数据库账号]()。
    3.  创建数据库。更多信息，请参见[创建数据库]()。
    4.  如果您需要通过外网地址连接AnalyticDB MySQL集群，还需要申请外网地址。更多信息，请参见[申请和释放公网地址]()。
    5.  创建数据库表。更多信息，请参见[CREATE TABLE]()。

## 创建投递任务

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在Project列表区域，单击目标Project。

3.  在**日志存储** \> **日志库**页签中，依次展开目标Logstore下的**数据处理** \> **导出**，单击**AnalyticDB**右侧的**+**。

4.  在投递提示对话框中，单击**直接投递**。

5.  首次创建投递任务到AnalyticDB MySQL时，需创建服务关联角色AliyunServiceRoleForAnalyticDBForMySQL。

    1.  在创建服务关联角色对话框中，单击AliyunServiceRoleForAnalyticDBForMySQL。

        ![创建服务关联角色](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0248744061/p179192.png)

    2.  在创建服务关联角色对话框中，单击**确定**。

6.  在LogHub —— 数据投递页面，配置相关参数，然后单击**确定**。

    ![数据投递](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0248744061/p179204.png)

    |参数|说明|
    |--|--|
    |**投递名称**|配置投递任务的名称，便于后续管理。|
    |**投递描述**|配置投递任务的描述，便于后续管理。|
    |**集群版本**|选择AnalyticDB MySQL集群版本，此处以选择3.0为例。|
    |**集群名称**|选择您已创建的AnalyticDB MySQL集群。|
    |**数据库名称**|选择您已创建的AnalyticDB MySQL集群中的数据库。|
    |**表名**|选择您已创建的AnalyticDB MySQL集群中的数据库表。|
    |**账号名称**|配置为您在AnalyticDB MySQL集群中创建的数据库账号名称。|
    |**账号密码**|配置为您在AnalyticDB MySQL集群中创建的数据库账号密码。|
    |**字段映射**|系统自动提取日志服务中最近10分钟的日志字段，同时自动映射对应的AnalyticDB MySQL数据表中的字段。左边文本框为日志字段名称，右边为AnalyticDB MySQL数据库表中的字段。![投递任务2](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6449276061/p76519.jpg) |
    |**投递开始时间**|配置投递开始时间。 当日志服务采集到日志后，日志服务可以实时将数据投递到AnalyticDB MySQL。 |
    |**是否过滤脏数据**|脏数据包括数据类型转化失败和非空字段为空等数据。    -   如果打开**是否过滤脏数据**开关，则遇到脏数据时，过滤掉脏数据。
    -   如果关闭**是否过滤脏数据**开关，则遇到脏数据时，投递任务中断。请谨慎选择。 |


创建投递任务后，您可以在日志服务控制台上管理数据投递任务，包括查看任务详情、修改投递规则以及启动、停止、删除任务等操作。更多信息，请参见[管理日志投递任务]()。

## 查看日志数据

将日志投递到AnalyticDB MySQL后，您就可以在数据库中通过[SELECT]()语句查询日志数据。

