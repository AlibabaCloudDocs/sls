# 采集Open-Falcon数据

Open-Falcon是一款企业级、高可用、可扩展的开源监控解决方案，用于监控服务器的状态，例如磁盘空间、端口存活、网络流量等。本文介绍如何通过Logtail和Transfer将Open-Falcon数据上传至日志服务。

已在服务器上安装Logtail（Linux Logtail 0.16.44及以上版本），详情请参见[安装Logtail（Linux系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md)。

出于性能和可靠性考虑，推荐将Logtail和Open-Falcon的transfer模块安装在相同机器上。

## 使用限制

您所使用的Open-Falcon版本需包含[Influxdb support](https://github.com/open-falcon/falcon-plus/commit/df7a2f80e27902a7e081c595bd1a24080cc624e7)功能。

## 步骤1：创建Logtail采集配置

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在Project列表区域，单击目标Project。

3.  在**时序存储** \> **时序库**页签中，单击目标Logstore下面**数据接入** \> **logtail配置**右侧的加号。

4.  在接入数据页面中，单击**自定义数据插件**。

5.  在创建机器组页签中，创建机器组。

    -   如果您已有可用的机器组，请单击**使用现有机器组**。
    -   如果您还没有可用的机器组，请执行以下操作（以ECS为例）：
        1.  选择ECS实例安装Logtail。更多信息，请参见[安装Logtail（ECS实例）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（ECS实例）.md)。

            如果已在ECS上安装Logtail，请单击**确认安装完毕**。

            **说明：** 如果是自建集群、其他云厂商服务器，需要手动安装Logtail。更多信息，请参见[安装Logtail（Linux系统）](/cn.zh-CN/数据采集/Logtail采集/安装/安装Logtail（Linux系统）.md#)。

        2.  安装完成后，单击**确认安装完毕**。
        3.  创建机器组。

            如何创建机器组，请参见[创建IP地址机器组](/cn.zh-CN/数据采集/Logtail采集/机器组/创建IP地址机器组.md)或[创建用户自定义标识机器组](/cn.zh-CN/数据采集/Logtail采集/机器组/创建用户自定义标识机器组.md)。

6.  在机器组配置页签中，应用机器组。

    选择一个机器组，将该机器组从**源机器组**移动到**应用机器组**。

7.  在Logtail配置页签中，配置**配置名称**和**插件配置**。

    inputs为Logtail采集配置，必选项，请根据您的数据源配置。

    **说明：** 一个inputs中只允许配置一个类型的数据源。

    ```
    {
        "inputs": [
            {
                "detail": {
                    "Format": "influx",
                    "Address": ":8476"
                },
                "type": "service_http_server"
            }
        ],
        "global": {
            "AlwaysOnline": true,
            "DelayStopSec": 500
        }
    }
    ```

    |参数|类型|是否必选|参数说明|
    |--|--|----|----|
    |type|string|是|数据源类型，固定为service\_http\_server。|
    |Format|string|是|数据类型，固定为influx。|
    |Address|string|是|监听地址与端口，格式为ip:port。|

8.  单击**下一步**，完成配置。


## 步骤2：修改Open-Falcon配置

1.  登录Open-Falcon所在服务器。

2.  添加transfer配置。

    1.  打开配置文件。

        配置文件默认为cfg.json。

    2.  将如下脚本添加到配置文件中。

        address中配置的端口号要与您在[步骤7](#step_ek0_cfi_esy)中配置的Address地址中的端口号一致，详情参数说明请参见[Transfer](https://book.open-falcon.org/zh/install_from_src/transfer.html)。

        ```
            "influxdb": {
                "enabled": true,
                "batch": 200,
                "retry": 3,
                "maxConns": 32,
                "precision": "s",
                "address": "http://127.0.0.1:8478",
                "timeout": 5000
            }
        ```


配置完成后，日志服务将Open-Falcon数据通过Logtail上传到日志服务MetricStore中。您可以在MetricStore查询分析页面进行查询分析操作，详情请参见[查询分析时序数据](/cn.zh-CN/时序存储/查询与分析/查询分析时序数据.md)。

