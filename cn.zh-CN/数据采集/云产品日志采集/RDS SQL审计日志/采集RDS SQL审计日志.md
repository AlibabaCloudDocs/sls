# 采集RDS SQL审计日志

本文介绍如何通过日志服务控制台采集RDS SQL审计日志。

-   已创建RDS实例，且已开启SQL审计（RDS SQL Server实例、RDS PostgreSQL实例）或付费版的SQL洞察（RDS MySQL实例）功能。
    -   如果是RDS MySQL实例，请参见[创建RDS MySQL实例](/cn.zh-CN/RDS MySQL 数据库/快速入门/创建RDS MySQL实例.md)、[SQL洞察](/cn.zh-CN/RDS MySQL 数据库/日志/审计/历史事件/SQL洞察.md)。
    -   如果是RDS SQL Server实例，请参见[创建RDS SQL Server实例](/cn.zh-CN/RDS SQL Server 数据库/快速入门/创建RDS SQL Server实例.md)、[SQL审计（数据库审计）](/cn.zh-CN/RDS SQL Server 数据库/日志/审计/历史事件/SQL审计（数据库审计）.md)。
    -   如果是RDS PostgreSQL实例，请参见[创建RDS PostgreSQL实例](/cn.zh-CN/RDS PostgreSQL 数据库/快速入门/创建RDS PostgreSQL实例.md)、[SQL审计（数据库审计）](/cn.zh-CN/RDS PostgreSQL 数据库/日志/审计/历史事件/SQL审计（数据库审计）.md)。
-   在RDS实例所在地域，已创建日志服务Project和Logstore，详情请参见[步骤1：创建Project和Logstore](/cn.zh-CN/快速入门/快速入门.md)。

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在接入数据区域，选择**RDS 审计-云产品**。

3.  在选择日志空间页签中，选择您已创建的目标Project和Logstore，单击**下一步**。

4.  在数据源配置页签中，完成RAM授权及开通投递，单击**下一步**。

    **说明：**

    -   如果您还未授权日志服务分发日志，请单击**RAM授权**右侧的**授权**，根据页面提示完成授权。
    -   如果页面中未显示目标RDS实例或开启投递失败，可能是因为您的RDS实例不符合条件。您可以参见本文中的前提条件检查您的RDS实例。
    ![数据源配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7930559951/p47510.png)

5.  在查询分析配置页签中，单击**下一步**。

    日志服务默认为RDS SQL审计日志对应的Logstore开启并配置索引。如果您要修改索引，详情请参见[开启并配置索引](/cn.zh-CN/查询与分析/开启并配置索引.md)。


日志服务采集到RDS SQL审计日志后，您可以执行查询分析、下载、投递、加工、创建告警等操作，详情请参见[云产品日志通用操作](/cn.zh-CN/数据采集/云产品日志采集/云产品日志通用操作.md)。

