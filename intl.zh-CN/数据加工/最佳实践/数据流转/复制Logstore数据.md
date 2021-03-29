# 复制Logstore数据

日志服务支持对每一个源Logstore配置一个加工任务，实现复制Logstore数据。本文介绍复制Logstore数据的典型场景和操作方法。

## 场景说明

某公司访问日志被采集存储在同一阿里云账号中的多个Project。现有需求将project-a中logstore-a访问日志复制到新建project-b的logstore-b中，从而后续在project-b进行统一的查询与分析。对此需求，日志服务提供数据加工的复制功能，可以将logstore-a数据复制到logstore-b。

![copy](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4217311161/p228146.png)

在操作前，确保您已完成如下操作。

-   已创建logstore-a，且已完成数据写入。
-   已完成logstore-b性能评估和规划。例如评估Shard数量。更多信息，请参见[性能指南](/intl.zh-CN/数据加工/性能指南.md)。
-   已创建logstore-b。更多信息，请参见[管理Logstore](/intl.zh-CN/数据采集/准备工作/管理Logstore.md)。

## 操作步骤

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在全部Project区域，单击project-a。

3.  在**日志存储** \> **日志库**页签中，单击logstore-a。

4.  在查询和分析页面的右上角单击**数据加工**，进入数据加工模式。

5.  单击**保存数据加工**。

6.  在创建数据加工规则页面，配置如下参数。

    |参数|说明|
    |--|--|
    |规则名称|数据加工规则的名称。输入test。|
    |授权方式|授予日志服务读取logstore-a中数据的权限。以默认角色为例，选择默认角色。|
    |目标名称|存储目标名称。输入test。|
    |目标Region|目标Project所在地域。选择华东1（杭州）。|
    |目标Project|logstore-b所属的Project名称。输入project-b。|
    |目标库|logstore-b名称。输入logstore-b。|
    |授权方式|授予日志服务读写logstore-b的权限。以默认角色为例，选择默认角色。 |
    |时间范围|加工的时间范围。 对Logstore中的数据从开始位置持续加工，直到加工任务被手动停止。选择所有。|

    更多参数配置，请参见[创建数据加工任务](/intl.zh-CN/数据加工/创建数据加工任务.md)。

7.  单击**确定**。


打开project-b项目，在**日志存储** \> **日志库**页签中选择logstore-b日志库，您可以查看到从logstore-a复制过来的数据。

![result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7200501161/p226660.png)

