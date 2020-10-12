# 管理Logstore

本文介绍如何在日志服务控制台上创建、修改、删除Logstore。

## 创建Logstore

**说明：** 一个Project中，最多创建200个Logstore。

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在Project列表区域，单击目标Project。

3.  在**日志存储** \> **日志库**页签中，单击**+**。

4.  在创建Logstore页面中，配置如下参数。

    |参数|说明|
    |--|--|
    |Logstore名称|Logstore名称在其所属Project内必须唯一，创建后不能修改。|
    |WebTracking|WebTracking支持从HTML、H5、iOS或Android平台采集数据到日志服务。默认为关闭状态。|
    |永久保存|开启该功能后，日志服务将永久保存采集到的日志，默认为关闭状态。**说明：** 通过API方式获取数据保存时间时，如果对应值为3650则表示永久保存。 |
    |数据保存时间|未开启**永久保存**时，需设置**数据保存时间**。 日志服务采集的日志在Logstore中的保存时间，单位为天，取值范围：1~3000。超过该时间后，日志会被删除。 |
    |Shard数目|日志库的分区数量，每个Logstore可以创建1~10个分区。每个Project中最多创建200个分区。|
    |自动分裂shard|当数据量超过已有Shard的服务能力后，开启自动分裂功能可自动根据数据量增加分区数量，默认为开启状态。详情请参见[管理Shard](/cn.zh-CN/数据采集/准备工作/管理Shard.md)。|
    |最大分裂数|开启**自动分裂shard**后，Shard自动分裂的最大数目，最大为64。|
    |记录外网IP|开启该功能后，日志服务采集到日志后，自动把以下信息添加到日志的Tag字段中。     -   \_\_client\_ip\_\_：日志来源设备的公网IP地址。
    -   \_\_receive\_time\_\_：日志到达服务端的时间，格式为Unix时间戳。 |

5.  单击**确定**，完成创建。


## 修改Logstore配置

1.  在**日志存储** \> **日志库**页签中，单击目标Logstore右侧的**![修改日志库](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0478559951/p52318.png)** \> **修改**。

2.  在**Logstore属性**页面中，单击**修改**。

    具体的参数说明请参见[创建Logstore](#section_v52_2jx_ndb)。

3.  单击**保存**，完成修改。


## 删除Logstore

**说明：**

-   删除Logstore前需先删除其对应的所有Logtail配置。
-   如果该Logstore上还启用了日志投递，建议删除前停止向该Logstore写入新数据，并确认Logstore中已有的数据已经全部投递成功。
-   如果您使用主账号删除Logstore提示权限不足，请单击[工单系统](https://selfservice.console.aliyun.com/ticket/category/sls/today)提交工单进行删除。

1.  在**日志存储** \> **日志库**页签中，单击目标Logstore右侧的**![修改日志库](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0478559951/p52318.png)** \> **删除**。

    **警告：** Logstore一旦删除，其存储的日志数据将会被永久删除，不可恢复，请谨慎操作。

2.  在确认对话框中，单击**确认**。


