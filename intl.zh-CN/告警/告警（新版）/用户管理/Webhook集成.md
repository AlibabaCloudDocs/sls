# Webhook集成

Webhook集成用于管理Webhook通知渠道，您可以在行动策略中直接使用已创建的Webhook。目前，日志服务支持钉钉、企业微信、飞书、Slack以及自定义的通用Webhook。

## 操作步骤

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  进入Webhook集成页面。

    1.  在Project列表区域，单击目标Project。

    2.  在左侧导航栏中，单击![告警](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9461784261/p289822.png)图标。

    3.  单击**打开告警中心**，选择**告警管理** \> **Webhook集成**。

3.  创建Webhook。

    1.  在Webhook集成页面，单击**新建**。

    2.  在新建Webhook对话框中，配置如下参数，然后单击**确认**。

        |参数|描述|
        |--|--|
        |ID|Webhook的唯一标识，不可重复。|
        |名称|Webhook名称。|
        |类型|Webhook类型，目前支持：        -   钉钉
        -   企业微信
        -   飞书
        -   Slack
        -   通用Webhook |
        |请求方法|调用该Webhook的请求方法。仅**类型**为通用Webhook时需要配置，其余类型默认为POST。 |
        |请求地址|Webhook URL地址。您需要在各个Webhook通知渠道侧完成相关配置，并获取Webhook URL地址。相关说明如下：

        -   钉钉

在钉钉侧创建自定义机器人，并获取Webhook URL地址。更多信息，请参见[钉钉](/intl.zh-CN/告警/告警（新版）/通知管理/通知渠道/通知渠道说明.md)。

        -   企业微信

在企业微信侧创建自定义机器人，并获取Webhook URL地址。

        -   飞书

在飞书侧创建自定义机器人，并获取Webhook URL地址。更多信息，请参见[Use bots in groups](https://www.feishu.cn/hc/en-US/articles/360024984973)。

        -   Slack

在Slack侧创建Webhook，获取Webhook URL地址。更多信息，请参见[Sending messages using Incoming Webhooks](https://api.slack.com/messaging/webhooks)。 |
        |Headers|调用该Webhook的自定义Headers。仅**类型**为通用Webhook时需要配置。 |


创建Webhook后，可以在行动策略中使用Webhook方式进行告警通知。更多信息，请参见[创建行动策略](/intl.zh-CN/告警/告警（新版）/通知管理/创建行动策略.md)。

