# 接入SkyWalking Trace数据

本文介绍通过Logtail将SkyWalking平台上的Trace数据转发至日志服务的操作步骤。

已创建Trace实例。更多信息，请参见[创建Trace实例]()。

## 使用限制

仅支持SkyWalking V3版本的GRPC协议，对应的SkyWalking发行版本为8.0.0及以上。

## 步骤一：配置数据接入

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在接入数据区域，选择**SkyWalking**。

3.  选择您Trace实例所在的Project以及$\{instance\}-traces Logstore。

4.  创建机器组。

    -   如果您已有可用的机器组，请单击**使用现有机器组**。
    -   如果您还没有可用的机器组，请执行以下操作（以ECS为例）。
        1.  在ECS页签中，选中目标ECS实例，单击**立即执行**。

            更多信息，请参见[安装Logtail（ECS实例）](/intl.zh-CN/数据采集/Logtail采集/安装/安装Logtail（ECS实例）.md)。

            **说明：** 如果是自建集群、其他云厂商服务器，需要手动安装Logtail。更多信息，请参见[安装Logtail（Linux系统）](/intl.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md)或[安装Logtail（Windows系统）](/intl.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Windows系统）.md)。

        2.  安装完成后，单击**确认安装完毕**。
        3.  创建机器组，单击**下一步**。

            日志服务支持创建IP地址机器组和用户自定义标识机器组，详细参数说明请参见[创建IP地址机器组](/intl.zh-CN/数据采集/Logtail采集/机器组/创建IP地址机器组.md)和[创建用户自定义标识机器组](/intl.zh-CN/数据采集/Logtail采集/机器组/创建用户自定义标识机器组.md)。

5.  选中目标机器组，将该机器组从**源机器组**移动到**应用机器组**，单击**下一步**。

    **说明：** 如果创建机器组后立刻应用，可能因为连接未生效，导致心跳为**FAIL**，您可单击**自动重试**。如果还未解决，请参见[Logtail机器组无心跳]()进行排查。

6.  在数据源设置页签中，添加如下配置，单击**下一步**。

    其中，$\{instance\}为您的Trace实例ID，请根据实际情况替换。

    **说明：** 如果您的Logtail本地11800端口被占用，可替换为其他可用端口。

    ```
    {
        "inputs" : [
            {
                "detail" : {
                    "Address" : "0.0.0.0:11800"
                },
                "type" : "service_skywalking_agent_v3"
            }
        ],
        "aggregators" : [
            {
                "detail" : {
                    "MetricsLogstore" : "$\{instance\}-metrics",
                    "TraceLogstore" : "$\{instance\}-traces"
                },
                "type" : "aggregator_skywalking"
            }
        ],
        "global" : {
            "AlwaysOnline" : true,
            "DelayStopSec" : 300
        }
    }
    ```

7.  预览数据及设置索引，单击**下一步**。

    日志服务默认开启全文索引。您也可以根据采集到的日志，手动或者自动设置字段索引。更多信息，请参见[配置索引](/intl.zh-CN/查询与分析/配置索引.md)。

    **说明：**

    -   如果您要查询分析日志，那么全文索引和字段索引属性必须至少启用一种。同时启用时，以字段索引为准。
    -   索引类型为long、double时，大小写敏感和分词符属性无效。

## 步骤2：配置SkyWalking客户端

配置SkyWalking客户端，将数据发送到Logtail监听的地址，详细说明如下：

-   如果您使用的是Java Agent，则替换其中的collector.backend\_service参数。具体操作，请参见[Agent配置](https://github.com/apache/skywalking/blob/master/docs/en/setup/service-agent/java-agent/README.md)。
-   如果您使用的是.net core Agent，则使用`dotnet skyapm config $\{service\}$\{endpoint\}`命令生成配置文件。其中，$\{service\}需替换为实际的服务名，$\{endpoint\}需替换为步骤[5](#step_fn9_9yq_89g)中配置的机器组IP地址及对应的端口号。具体操作，请参见[SkyAPM-donet](https://github.com/SkyAPM/SkyAPM-dotnet)。
-   如果您使用的是其他Agent或SDK发送数据，需将后端地址替换为步骤[5](#step_fn9_9yq_89g)中配置的机器组IP地址及对应的端口号。

-   [查看Trace实例详情]()
-   [查询和分析Trace数据]()

