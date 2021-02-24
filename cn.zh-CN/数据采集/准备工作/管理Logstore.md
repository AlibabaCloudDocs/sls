# 管理Logstore

本文介绍如何在日志服务控制台上创建、修改、删除Logstore。

## 创建Logstore

**说明：** 一个Project中，最多创建200个Logstore。

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在Project列表区域，单击目标Project。

3.  在**日志存储** \> **日志库**页签中，单击**+**。

4.  在创建Logstore页面中，配置如下参数。

    |参数|描述|
    |--|--|
    |Logstore名称|Logstore的名称，在其所属Project内必须唯一。创建Logstore成功后，无法更改其名称。|
    |WebTracking|打开**WebTracking**开关后，您可以通过WebTracking从HTML、H5、iOS或Android上采集数据到日志服务。|
    |永久保存|打开**永久保存**开关后，日志服务将永久保存采集到的日志。**说明：** 通过API方式获取数据保存时间时，如果对应值为3650则表示永久保存。 |
    |数据保存时间|日志在Logstore中的保存时间。单位为天，取值范围：1~3000。超过该时间后，日志会被删除。仅在未打开**永久保存**开关时，需设置**数据保存时间**。

**说明：** 缩短数据保存时间后，日志服务将在1小时后开始删除数据。但日志服务控制台首页的**存储量（日志）**将于次日更新。例如您原本的数据保存时间为5天，现修改为1天，则日志服务将在1小时候后开始删除前4天的日志。 |
    |Shard数目|日志服务使用Shard读写数据。一个Shard提供的写入能力为5 MB/s、500次/s，读取能力为10 MB/s、100次/s。每个Logstore中最多创建10个Shard，每个Project中最多创建200个Shard。更多信息，请参见[分区](/cn.zh-CN/产品简介/基本概念/分区.md)。|
    |自动分裂Shard|打开**自动分裂Shard**开关后，如果您写入的数据量超过已有Shard服务能力，日志服务会自动根据数据量增加Shard数量。更多信息，请参见[管理Shard](/cn.zh-CN/数据采集/准备工作/管理Shard.md)。|
    |最大分裂数|打开**自动分裂shard**开关后，Shard自动分裂的最大数目，最大值为64。|
    |记录外网IP|打开**记录外网IP**开关后，日志服务自动把以下信息添加到日志的Tag字段中。    -   \_\_client\_ip\_\_：日志来源设备的公网IP地址。
    -   \_\_receive\_time\_\_：日志到达服务端的时间，格式为Unix时间戳，表示从1970-1-1 00:00:00 UTC计算起的秒数。 |

5.  单击**确定**。


## 修改Logstore配置

1.  在**日志存储** \> **日志库**页签中，选择目标Logstore右侧的**![修改日志库](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0478559951/p52318.png)** \> **修改**。

2.  在**Logstore属性**页面中，单击**修改**。

    具体的参数说明请参见[创建Logstore](#section_v52_2jx_ndb)。

3.  单击**保存**，完成修改。


## 删除Logstore

**说明：**

-   删除Logstore前需先删除其对应的所有Logtail配置。
-   如果该Logstore上还启用了日志投递，建议删除前停止向该Logstore写入新数据，并确认Logstore中已有的数据已经全部投递成功。
-   如果您使用阿里云账号删除Logstore时，控制台提示权限不足，请提交[工单](https://selfservice.console.aliyun.com/ticket/category/sls/today)进行删除。

1.  在**日志存储** \> **日志库**页签中，选择目标Logstore右侧的**![修改日志库](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0478559951/p52318.png)** \> **删除**。

    **警告：** Logstore一旦删除，其存储的日志数据将会被永久删除，不可恢复，请谨慎操作。

2.  在删除对话框中，单击**确认**。


