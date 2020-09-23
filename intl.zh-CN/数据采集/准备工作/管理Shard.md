# 管理Shard

本文介绍如何在日志服务控制台上分裂、合并、删除Shard。

您在创建Logstore时已设置Shard数量，后续还可以分裂或合并Shard，实现增加或减少Shard。

-   每个Shard支持5MB/s的数据写入和10MB/s的数据读取，当数据流量超过Shard服务能力时，建议您分裂Shard。

    您可以在**Logstore属性**页面进行Shard分裂操作。

-   当数据流量远达不到Shard的最大读写能力时，建议您合并Shard。

## 分裂Shard

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  单击目标Project。

3.  在**日志管理** \> **日志库**页签中，单击目标Logstore右侧的**![修改日志库](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0478559951/p52318.png)** \> **修改**。

4.  在**Logstore属性**页面中，单击**修改**。

5.  选择待分裂的Shard，单击**分裂**。

    **说明：** 分裂Shard时，需要选择一个处于readwrite状态的Shard。

    ![分裂分区](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0167745951/p2594.png)

6.  选择分裂数量，单击**确定**。

7.  单击**保存**，完成分裂操作。

    分裂完成后，原Shard状态由readwrite变为readonly。在readonly状态下，Shard中的数据仍可以被消费，但不可写入新数据。新Shard状态为readwrite，位于原Shard后面，且新Shard的MD5范围覆盖原Shard的范围。


## 自动分裂Shard

日志服务支持自动分裂Shard。开启自动分裂Shard功能后，当满足以下两个条件时Shard会自动分裂。

-   当写入数据量超出当前Shard的[写入服务能力](/intl.zh-CN/产品简介/限制说明/数据读写.md)且持续5分钟以上。
-   Logstore中readwrite状态的Shard数目未超过设定的最大Shard总数。

**说明：** 最近15分钟内分裂出来的新Shard不会自动分裂。

您可以在创建或修改Logstore时开启自动分裂Shard，并设定Shard的最大分裂数。

-   自动分裂Shard

    开启自动分裂Shard功能，当写入数据量超出当前Shard的[写入服务能力](/intl.zh-CN/产品简介/限制说明/数据读写.md)且持续5分钟以上，可自动根据数据量增加分区数量。

-   最大分裂数

    Shard自动分裂的最大数目，开启自动分裂Shard功能后，最大可支持自动分裂至64个Shard。


## 合并Shard

您可以通过合并操作缩容Shard，日志服务会自动找到指定Shard右侧相邻的Shard，并将两个Shard合并。

**说明：**

-   合并Shard时，必须指定一个处于readwrite状态的Shard，且不能是最后一个readwrite状态的Shard。
-   合并完成后，所指定的Shard和其右侧相邻Shard的状态变成readonly。同时新生成一个readwrite状态的Shard，新Shard的MD5范围覆盖原来两个Shard的范围。

1.  在**日志管理** \> **日志库**页签中，单击目标Logstore右侧的**![修改日志库](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0478559951/p52318.png)** \> **修改**。

2.  在**Logstore属性**页面中，单击**修改**。

3.  选择待合并的Shard，单击**合并**。

    ![合并分区](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/1167745951/p2596.png)

4.  单击**保存**，完成合并操作。


## 删除Shard

-   自动删除

    如果您在创建Logstore时设置了数据保存时间，那么Shard及Shard中的数据超出保存时间后会被自动删除。

-   手动删除

    如果您在创建Logstore时开启了永久保存，建议您通过删除Logstore的方式删除Logstore中的Shard和数据，详情请参见[删除Logstore](/intl.zh-CN/数据采集/准备工作/管理Logstore.md)。


