# 管理MetricStore

本文介绍如何在日志服务控制台上创建、修改、删除MetricStore。

已创建Project，详情请参见[创建Project](/cn.zh-CN/数据采集/准备工作/管理Project.md)。

## 创建MetricStore

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在Project列表区域，单击目标Project。

3.  在**时序存储** \> **时序库**页签中，单击**+**图标。

4.  在创建MetricStore页面中，配置如下参数。

    |参数|说明|
    |--|--|
    |MetricStore名称|MetricStore名称在其所属Project内必须唯一，创建后不能修改。|
    |永久保存|开启该功能后，日志服务将永久保存采集到的时序数据，默认为关闭状态。|
    |数据保存时间|未开启**永久保存**时，需设置**数据保存时间**。 日志服务采集的时序数据在MetricStore中的保存时间，单位为天，取值范围：1~3000。超过该时间后，时序数据会被删除。 |

5.  单击**确定**，完成创建。


## 修改MetricsStore配置

1.  在**时序存储** \> **时序库**页签中，单击目标MetricStore右侧的**![修改日志库](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0478559951/p52318.png) 图标** \> **修改**。

2.  在**MetricStore属性**页面中，单击**修改**。

    -   修改保存时间，参数说明请参见[创建MetricStore](#section_8ju_apk_egg)。
    -   管理Shard

        创建MetricStore时，默认为MetricStore创建2个Shard。在后续使用中，您可以根据业务需求分裂、合并及删除Shard。MetricStore的Shard操作与Logstore相同，详情请参见[管理Shard](/cn.zh-CN/数据采集/准备工作/管理Shard.md)。

3.  单击**保存**，完成修改。


## 删除MetricStore

**说明：**

-   删除MetricStore前必须删除其对应的所有Logtail采集配置。
-   如果该MetricStore上还启用了数据投递，建议删除前停止向该MetricStore写入新数据，并确认MetricStore中已有的数据已经全部投递成功。
-   如果您使用主账号删除MetricStore提示权限不足，请提[工单](https://selfservice.console.aliyun.com/ticket/category/sls/today)进行删除。

1.  在**时序存储** \> **时序库**页签中，单击目标MetricStore右侧的**![修改日志库](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0478559951/p52318.png)图标** \> **删除**。

    **警告：** MetricStore一旦删除，其存储的时序数据将会被永久删除，不可恢复，请谨慎操作。

2.  在确认对话框中，单击**确认**。


