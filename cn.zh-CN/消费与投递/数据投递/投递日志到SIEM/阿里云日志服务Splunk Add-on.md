# 阿里云日志服务Splunk Add-on

本文介绍如何通过阿里云日志服务Splunk Add-on将日志服务上的日志投递到Splunk。

## 架构

Splunk Add-on架构：

-   通过Splunk Data Input创建日志服务消费组，并从日志服务进行实时日志消费。
-   将采集到的日志通过Splunk私有协议（Private Protocol）或者HTTP Event Collector（HEC）投递到Splunk indexer。

**说明：** 此Add-on仅用于采集数据，只需要在Splunk Heavy Forwarder上安装，不需要在Indexer和Search Head上安装。

![Splunk-001](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7031649951/p95069.png)

## 机制

![Splunk-002](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7031649951/p95070.png)

-   一个Data Input创建一个消费者进行日志消费。
-   一个消费组由多个消费者构成，同一个消费组下面的消费者共同消费一个Logstore中的数据，消费者之间不会重复消费数据。
-   一个Logstore下面会有多个Shard。
    -   每个Shard只会分配到一个消费者。
    -   一个消费者可以同时拥有多个Shard。
-   所创建的消费者名字是由消费组名、host名、进程名、event协议类型组合而成，保证同一消费组内的消费者不重名。

更多关于消费组的信息，请参见[通过消费组消费日志数据](/cn.zh-CN/消费与投递/实时消费/消费组消费/通过消费组消费日志数据.md)。

## 准备工作

-   获取日志服务的AccessKey。

    您可以通过阿里云RAM获取日志服务Project的AccessKey，详情请参见[访问密钥](/cn.zh-CN/开发指南/API 参考/访问密钥.md)和[配置访问密钥](/cn.zh-CN/数据加工/配置访问授权/配置访问密钥.md)。

    您可以通过权限助手配置RAM权限，详情请参见[配置权限助手](/cn.zh-CN/开发指南/访问控制RAM/配置权限助手.md)。常用的RAM配置如下：

    **说明：** <Project name\>为您的日志服务Project名称，<Logstore name\>为您的日志服务Logstore名称，请根据实际情况替换，名字替换支持通配符\*。

    ```
    {
      "Version": "1",
      "Statement": [
        {
          "Action": [
            "log:ListShards",
            "log:GetCursorOrData",
            "log:GetConsumerGroupCheckPoint",
            "log:UpdateConsumerGroup",
            "log:ConsumerGroupHeartBeat",
            "log:ConsumerGroupUpdateCheckPoint",
            "log:ListConsumerGroup",
            "log:CreateConsumerGroup"
          ],
          "Resource": [
            "acs:log:*:*:project/<Project name>/logstore/<Logstore name>",
            "acs:log:*:*:project/<Project name>/logstore/<Logstore name>/*"
          ],
          "Effect": "Allow"
        }
      ]
    }
    ```

-   检查Splunk版本及运行环境。
    -   确保使用最新的Add-on版本。
    -   操作系统：Linux、Mac OS、Windows。
    -   Splunk版本：Splunk heavy forwarder 8.0及以上版本、Splunk indexer 7.0及以上版本。
-   配置Splunk HTTP Event Collector，详情请参见[Configure HTTP Event Collector on Splunk Enterprise](https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/UsetheHTTPEventCollector#Configure_HTTP_Event_Collector_on_Splunk_Enterprise)。

    如果需要使用HEC来发送event，请确保HEC配置成功。如果选择Splunk私有协议，则可以跳过该步骤。

    **说明：** 目前创建Event Collector token时，不支持开启indexer acknowledgment功能。


## 安装Add-on

您可以登录Splunk web UI页面，通过以下两种方法安装Splunk Add-on。

**说明：** 此Add-on仅用于采集数据，只需要在Splunk Heavy Forwarder上安装，不需要在Indexer和Search Head上安装。

-   方法一
    1.  单击**![Splunk-004](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8031649951/p95176.png)**图标。
    2.  在应用页面，单击**浏览更多应用**。
    3.  搜索**Alibaba Cloud Log Service Add-on for Splunk**，单击**安装**。
    4.  安装完成后，根据页面提示重启 Splunk服务。
-   方法二
    1.  单击**![Splunk-004](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8031649951/p95176.png)**图标。
    2.  在应用页面，单击**从文件安装应用**。
    3.  选择目标.tgz文件，单击**上载**。

        您可以[App Search Results](https://splunkbase.splunk.com/app/4934/)上下载目标.tgz文件。

    4.  单击**安装**。
    5.  安装完成后，根据页面提示重启 Splunk服务。

## 配置Add-on

1.  在Splunk Web UI页面中，单击Alibaba Cloud Log Service Add-on for Splunk。

2.  配置全局账号。

    选择**配置** \> **Account**，在Account页签中，配置日志服务的AccessKey信息。

    **说明：** 此处配置的用户名、密码分别对应日志服务的AccessKey ID、AccessKey Secret。

3.  配置Add-on的运行日志级别。

    选择**配置** \> **Logging**，在Logging页签中，配置Add-on的运行日志级别。

4.  创建Data Input。

    1.  单击**输入**，进入输入页面。

    2.  单击**Create New Input**，创建新的Data Input。

        |参数|必选项 & 格式|描述|取值举例|
        |--|--------|--|----|
        |名字|是，String|全局唯一的Data Input名。|无|
        |间隔|是，Integer|Splunk Data Input退出后的重启时间，单位：s。|默认值：10s|
        |索引|是，String|Splunk索引。|无|
        |SLS AccessKey|是，String|阿里云访问密钥由AccessKey ID、AccessKey Secret组成。**说明：** 此处配置的用户名、密码分别对应日志服务的AccessKey ID、AccessKey Secret。

|全局账号配置中配置的用户名和密码。|
        |SLS endpoint|是，String|阿里云日志服务入口，详情请参见[服务入口](/cn.zh-CN/开发指南/API 参考/服务入口.md)。|        -   cn-huhehaote.log.aliyuncs.com
        -   https://cn-huhehaote.log.aliyuncs.com |
        |SLS project|是，String|日志服务Project，详情请参见[管理Project](/cn.zh-CN/数据采集/准备工作/管理Project.md)。|无|
        |SLS logstore|是，String|日志服务Logstore，详情请参见[管理Logstore](/cn.zh-CN/数据采集/准备工作/管理Logstore.md)。|无|
        |SLS consumer group|是，String|日志服务消费组。扩容时，多个Data Input需要配置相同的消费组名称，详情请参见[通过消费组消费日志数据](/cn.zh-CN/消费与投递/实时消费/消费组消费/通过消费组消费日志数据.md)。|无|
        |SLS cursor start time|是，String|消费起始时间。该参数只有消费组首次创建时有效。非首次创建日志都是从上次的保存点开始消费。**说明：** 这里的时间是日志到达时间。

|取值：begin、end、ISO格式的时间（例如2018-12-26 0:0:0+8:00\)。|
        |SLS heartbeat interval|是，Integer|SLS消费者与Sever间的心跳间隔。单位：s。|默认值：60s|
        |SLS data fetch interval|是，Integer|日志拉取间隔，如果日志频率较低，建议不要设的太小。单位：s。|默认值：1s|
        |Topic filter|否，String|Topic过滤字符串，以；间隔区分多个过滤的Topic。如果日志的topic被命中，则忽略该日志，从而不能投递到Splunk。|“TopicA;TopicB”意味着topic为TopicA、TopicB的日志将被忽略。|
        |Unfolded fields|否，JSON|Json格式的topic到字段列表的映射关系。\{"topicA": \["field\_nameA1", "field\_nameA2", ...\], "topicB": \["field\_nameB1", "field\_nameB2", ...\], ...\}|\{"actiontrail\_audit\_event": \["event"\] \} 表示topic为 actiontrail\_audit\_event的日志，该日志的event字段将把字符串展开为JSON格式。|
        |Event source|否，String|Splunk event数据源。|无|
        |Event source type|否，String|Splunk event数据源类型。|无|
        |Event retry times|否，Integer|0表示无限重传。|默认值：0次|
        |Event protocol|是|Splunk event发送协议。如果选择私有协议，后续参数可以忽略。|        -   HTTP for HEC
        -   HTTPS for HEC
        -   Private protocol |
        |HEC host|是，只有Event protocol选择HEC时有效，String。|HEC host，详情请参见[Set up and use HTTP Event Collector in Splunk Web](https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/UsetheHTTPEventCollector)。|无|
        |HEC port|是，只有Event protocol选择HEC时有效，Integer。|HEC端口。|无|
        |HEC token|是，只有Event protocol选择HEC时有效，String。|HEC token，详情请参见[HEC token](https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/UsetheHTTPEventCollector#About_Event_Collector_tokens)。|无|
        |HEC timeout|是，只有Event protocol选择HEC时有效，Integer。|HEC超时时间，单位：s。|默认：120s|


## 开始工作

-   查询数据

    首先确保创建的Data Input是启动状态。然后您可以在Splunk web UI首页单击Search & Reporting，进入App: Search & Reporting页面，查询采集到的审计日志。

    ![Splunk-003](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8031649951/p95102.png)

-   内部日志
    -   使用`index="_internal" | search "SLS info"`查询日志服务相关的信息。
    -   使用`index="_internal" | search "error"`查询运行时的错误信息。

## 性能与安全

-   性能规格

    日志服务Splunk Add-on性能及数据传输的吞吐量受到如下因素的影响：

    -   服务入口：可以使用公网、经典网络、VPC网络或者全球加速服务入口。一般情况下，建议采用经典网络或VPC网络的服务入口，详情请参见[服务入口](/cn.zh-CN/开发指南/API 参考/服务入口.md)。
    -   带宽：包括日志服务与Splunk heavy forwarder间的带宽、Splunk heavy forwarder与indexer之间的带宽。
    -   Splunk indexer处理能力：indexer的数据接收能力。
    -   Shard数量：对于单个日志服务Logstore，配置的Shard数越多，数据传输能力越强。您需要根据原始日志的生成速率确认Shard数量，详情请参见[管理Shard](/cn.zh-CN/数据采集/准备工作/管理Shard.md)。
    -   Splunk Data Input的配置数量：同一个Logstore，配置越多的Data Input（相同的消费组），吞吐量越大。

        **说明：** 消费者的并发受到日志服务Logstore Shard数量的影响。

    -   Splunk heavy forwarder占用的CPU core数和内存：一般情况下一个Splunk Data Input需要消耗1～2G内存，占用1个CPU core。
    当上述条件满足的情况下，一个Splunk Data Input可以提供1M/s～2M/s的日志消费能力。实际使用时，您需要根据原始日志的生成速率确认Shard数量。

    例如，一个Logstore有10M/s的日志生产能力，需要至少拆分出10个split，并且在Add-on中配置10个Data Input。如果是单机部署，需要具备10个空闲CPU core及12G的内存。

-   高可用性

    消费组将检测点（check-point）保存在服务器端，当一个消费者停止，另外一个消费者将自动接管并从断点继续消费。可以在不同机器上创建Splunk Data Input，这样在一台机器停止或者损坏的情况下，其他机器上的Splunk Data Input创建的消费者可以自动接管并从断点进行消费。理论上，为了备用，也可以在不同机器上启动大于Shard数量的Splunk Data Input。

-   使用HTTPS
    -   日志服务

        如果服务入口（endpoint）配置为https://前缀，例如https://cn-beijing.log.aliyuncs.com，程序与日志服务的连接将自动使用HTTPS加密。

        服务器证书\*.aliyuncs.com是GlobalSign签发，默认大多数Linux、Windows的机器会自动信任此证书。如果某些特殊情况，机器不信任此证书，请参见[Install a trusted root CA or self-signed certificate](https://success.outsystems.com/Support/Enterprise_Customers/Installation/Install_a_trusted_root_CA__or_self-signed_certificate)。

    -   Splunk

        为了使用HTTPS的HEC能力，需要在Splunk HEC全局配置界面上打开SSL功能，详情请参见[Configure HTTP Event Collector on Splunk Enterprise](https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/UsetheHTTPEventCollector#Configure_HTTP_Event_Collector_on_Splunk_Enterprise)。

-   AccessKey存储保护

    日志服务的AccessKey和Splunk的HEC token等关键信息，都保存在Splunk保密存储中，不用担心泄漏风险。


## 常见问题

-   配置错误
    -   当创建或修改Data Input时，会有基本配置信息检查，配置参数说明请参见[表 1](#table_3m7_ks5_n9x)。
    -   日志服务配置错误，例如创建消费组失败。
        -   命令：`index="_internal" | search "error"`
        -   异常日志：

            ```
            aliyun.log.consumer.exceptions.ClientWorkerException: 
            error occour when create consumer group, 
            errorCode: LogStoreNotExist, 
            errorMessage: logstore xxxx does not exist
            ```

        -   ConsumerGroupQuotaExceed错误

            在日志服务中，一个Logstore最多配置20个消费组。请在日志服务控制台查看消费组信息，如果空闲数量不足，建议删除不需要的消费组。如果超过20个消费组，系统会报ConsumerGroupQuotaExceed错误。

-   权限错误
    -   无权限访问阿里云日志服务
        -   命令：`index="_internal" | search "error"`
        -   异常日志：

            ```
            aliyun.log.consumer.exceptions.ClientWorkerException: 
            error occour when create consumer group, 
            errorCode: SignatureNotMatch, 
            errorMessage: signature J70VwxYH0+W/AciA4BdkuWxK6W8= not match
            ```

    -   无权限访问HEC
        -   命令：`index="_internal" | search "error"`
        -   异常日志：

            ```
            ERROR HttpInputDataHandler - Failed processing http input, token name=n/a, channel=n/a, source_IP=127.0.0.1, reply=4, events_processed=0, http_input_body_size=369
            
            WARNING pid=48412 tid=ThreadPoolExecutor-0_1 file=base_modinput.py:log_warning:302 | 
            SLS info: Failed to write [{"event": "{\"__topic__\": \"topic_test0\", \"__source__\": \"127.0.0.1\", \"__tag__:__client_ip__\": \"10.10.10.10\", \"__tag__:__receive_time__\": \"1584945639\", \"content\": \"goroutine id [0, 1584945637]\", \"content2\": \"num[9], time[2020-03-23 14:40:37|1584945637]\"}", "index": "main", "source": "sls log", "sourcetype": "http of hec", "time": "1584945637"}] remote Splunk server (http://127.0.0.1:8088/services/collector) using hec. 
            Exception: 403 Client Error: Forbidden for url: http://127.0.0.1:8088/services/collector, times: 3
            ```

        -   可能原因
            -   HEC没有配置或启动。
            -   Data Input中的HEC基本信息配置错误。例如：如果使用HTTPS，需要开启SSL配置。
            -   检查Event Collector token的indexer acknowledgment功能是否为关闭状态。
-   消费时延

    -   如果您开启了[服务日志](/cn.zh-CN/开发指南/监控日志服务/服务日志/简介.md)功能，可以进行如下操作。
        -   通过消费组监控仪表盘查看消费组状态，详情请参见[服务日志仪表盘](/cn.zh-CN/开发指南/监控日志服务/服务日志/服务日志仪表盘.md)。
        -   对消费者监控仪表盘中的图表设置告警，详情请参见[设置告警](/cn.zh-CN/可视化与告警/告警/设置告警.md)。
    -   如果您未开启服务日志功能，可通过日志服务控制台查看消费组状态，详情请参见[查看消费组状态](/cn.zh-CN/消费与投递/实时消费/消费组消费/通过消费组消费日志数据.md)。
    建议参见[性能规格](#section_0p1_mj7_a9l)，采用扩充Shard数或者创建具有相同消费组的Data Input的方式解决消费时延问题。

-   网络抖动

    -   命令：`index="_internal" | search "SLS info: Failed to write"`
    -   异常日志：

        ```
        WARNING pid=58837 tid=ThreadPoolExecutor-0_0 file=base_modinput.py:log_warning:302 |
        SLS info: Failed to write [{"event": "{\"__topic__\": \"topic_test0\", \"__source__\": \"127.0.0.1\", \"__tag__:__client_ip__\": \"10.10.10.10\", \"__tag__:__receive_time__\": \"1584951417\", \"content2\": \"num[999], time[2020-03-23 16:16:57|1584951417]\", \"content\": \"goroutine id [0, 1584951315]\"}", "index": "main", "source": "sls log", "sourcetype": "http of hec", "time": "1584951417"}] remote Splunk server (http://127.0.0.1:8088/services/collector) using hec. 
        Exception: ('Connection aborted.', ConnectionResetError(54, 'Connection reset by peer')), times: 3
        ```

    正常情况下，event会自动重传。但是如果问题持续存在，请联系网络管理员排查网络状况。

-   修改消费起始时间

    **说明：** Data Input的消费起始时间参数，只有消费组首次创建时有效。非首次创建日志都是从上次的保存点开始消费。

    1.  在Splunk Web UI的输入页面，禁用目标Data Input。
    2.  登录[日志服务控制台](https://sls.console.aliyun.com)，在所消费的目标Logstore下的**数据消费**栏中，删除对应的消费组。
    3.  在Splunk Web UI的输入页面，单击目标Data Input右侧的**操作** \> **编辑**，修改SLS cursor start time参数，并重新启动Data Input。

