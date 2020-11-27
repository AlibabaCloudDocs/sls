# 管理Project

本文介绍如何在日志服务控制台上创建、修改、删除Project等操作。

已开通日志服务。

首次登录[日志服务控制台](https://sls.console.aliyun.com)时，根据页面提示开通日志服务。

## 创建Project

**说明：** 一个阿里云账户最多可创建50个Project。

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在**Project列表**区域，单击**创建Project**。

3.  在创建Project对话框中，配置如下参数。

    |参数|说明|
    |--|--|
    |Project名称|Project名称在阿里云地域内全局唯一，创建后不能修改。|
    |注释|Project的简单注释。|
    |所属地域|请根据日志来源等信息选择合适的阿里云地域。 如果您要采集ECS日志，建议选择与ECS相同的地域，即可使用阿里云内网采集日志，加快日志采集速度。

创建Project后，无法修改其所属地域，且日志服务不支持Project跨地域迁移，请谨慎选择Project所属地域。不同地域对应的Endpoint请参见[服务入口](/intl.zh-CN/开发指南/API 参考/服务入口.md)。 |

4.  单击**确定**，完成创建。


## 修改Project

创建Project成功后，只允许修改Project的注释信息。

1.  在**Project列表**中，找到目标Project，单击**编辑**。

2.  在编辑注释对话框中，修改注释信息。

3.  单击**确定**，完成修改。


## 删除Project

**警告：**

-   日志服务内置Project（例如通过日志审计服务默认创建的Project）及相关云产品Project，无法自行删除。您可以提交[工单](https://workorder-intl.console.aliyun.com/console.htm?spm=a2796.7919406.0.dcontactus3.676a2d23RjosdV#/ticket/add/?productId=1210)进行删除。
-   删除Project后，其管理的所有日志数据及配置信息都会被永久释放，不可恢复。在删除Project前请慎重确认，避免数据丢失。

1.  在**Project列表**中，找到目标Project，单击**删除**。

2.  在删除Project对话框，选择删除原因，并单击**确定**。


