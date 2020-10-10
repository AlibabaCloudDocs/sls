# 采集Prometheus监控数据

Prometheus是一款面向云原生的监控软件，支持众多软件、系统的数据采集与监控。本文介绍如何将Prometheus监控数据采集到日志服务。

-   已创建MetricStore，详情请参见[创建MetricStore](/cn.zh-CN/时序存储/管理MetricStore.md)。
-   已安装Prometheus，详情请参见[GETTING STARTED](https://prometheus.io/docs/prometheus/latest/getting_started/)。
-   已在Prometheus上配置数据采集规则，详情请参见[scrape\_config](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#scrape_config)。

## 操作步骤

日志服务支持Prometheus的Remote Write协议，只需要在Prometheus中启动Remote Write功能即可采集数据到日志服务，相关操作如下所示。

1.  登录Prometheus所在服务器。

2.  打开配置文件，并根据实际情况替换如下参数，具体操作请参见[remote\_write](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#remote_write)。

    ```
    url: https://sls-prometheus-test.cn-beijing.log.aliyuncs.com/prometheus/sls-prometheus-test/prometheus-raw/api/v1/write
    basic_auth:
      username: access-key-id
      password: access-key-secret
    
    queue_config:
      batch_send_deadline: 20s
      capacity: 20480
      max_backoff: 5s
      max_samples_per_send: 2048
      min_backoff: 100ms
      min_shards: 100                      
    ```

    |参数|说明|
    |--|--|
    |url|日志服务MetricStore的URL，格式为https://\{project\}.\{sls-enpoint\}/prometheus/\{project\}/\{metricstore\}/api/v1/write。其中\{sls-enpoint\}请参见[服务入口](/cn.zh-CN/开发指南/API 参考/服务入口.md)，\{project\}和\{metricstore\}替换为您对应的Project和MetricStore。 **说明：**

    -   如果您是在阿里云内网，请优先使用内网域名。
    -   为保证传输安全性，请务必使用https。 |
    |basic\_auth|鉴权信息，以Remote Write协议写入数据到日志服务需要BasicAuth鉴权，其中username为您的阿里云账号AccessKeyID，password为您的阿里云AccessKeySecret。建议您使用只具备日志服务Project写入权限的RAM用户AccessKey，详情请参见[授予指定Project写入权限](/cn.zh-CN/开发指南/访问控制RAM/RAM自定义授权场景.md)。|
    |queue\_config|queue\_config用于设置写入的缓存、重试等策略。 为避免过多无效网络请求，建议min\_backoff不低于100ms，max\_backoff不低于5s。

如果Prometheus数据量较大，可修改queue\_config配置，建议修改为：

    ```
batch_send_deadline: 20s
capacity: 20480
max_backoff: 5s
max_samples_per_send: 2048
min_backoff: 100ms
min_shards: 100
    ``` |

3.  验证是否已上传数据到日志服务。

    配置好Prometheus后，您可通过预览方式查看数据是否已上传到日志服务。

    1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

    2.  在Project列表区域，单击目标Project。

    3.  在**时序存储** \> **时序库**页签中，单击目标MetricStore右侧的**![修改日志库](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0478559951/p52318.png)图标** \> **消费预览**。

        在消费预览页面，如果有数据，则表示配置成功。

        ![Prometheus-数据消费](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3683129951/p128310.png)


采集到Prometheus监控数据后，您可以使用日志服务查询分析Prometheus监控数据，详情请参见[查询分析时序数据](/cn.zh-CN/时序存储/查询与分析/查询分析时序数据.md)。也可以使用Grafana访问Prometheus数据，详情请参见[时序数据对接Grafana](/cn.zh-CN/时序存储/可视化/时序数据对接Grafana.md)。

