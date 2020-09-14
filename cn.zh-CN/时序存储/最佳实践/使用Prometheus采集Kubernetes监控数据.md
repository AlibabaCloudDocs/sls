# 使用Prometheus采集Kubernetes监控数据

本文介绍如何在Kubernetes上部署Prometheus，将监控数据采集到日志服务MetricStore中，并将日志服务MetricStore对接到Grafana实现监控数据可视化展示。

-   已拥有Kubernetes集群，集群版本在1.10以上。
-   已创建MetricStore，详情请参见[创建MetricStore](/cn.zh-CN/时序存储/管理MetricStore.md)。
-   已安装Grafana，详情请参见[安装Grafana](http://docs.grafana.org/installation/)。

Prometheus作为面向云原生的监控软件，对Kubernetes提供了友好的支持。在Kubernetes中，几乎所有的组件都提供了Prometheus的指标接口，因此Prometheus基本成为Kubernetes监控的实施标准。

Grafana是一个开源的度量分析与可视化套件，兼容所有的Prometheus仪表盘模板。日志服务支持Grafana访问时序数据，您可直接将日志服务MetricStore作为Grafana的Prometheus数据源进行接入，实现时序数据可视化展示。

## 在自建Kubernetes上安装Prometheus

如果您使用自建Kubernetes，推荐以[注册集群](https://help.aliyun.com/document_detail/155208.html)的方式接入到阿里云，注册好后按照[阿里云Kubernetes安装方式](#section_10t_rym_hng)安装Prometheus。如果您不使用注册集群方式，可通过[Helm安装包](https://github.com/helm/charts/blob/master/stable/prometheus-operator)安装Prometheus，安装前需先[创建保密字典](#step_61m_xab_8rs)并调整[默认配置](https://github.com/helm/charts/blob/master/stable/prometheus-operator/values.yaml)。

## 在阿里云Kubernetes上安装Prometheus

如果您使用阿里云Kubernetes，可直接在应用目录中安装并配置Prometheus将数据存储到日志服务。

1.  登录[容器服务管理控制台](https://cs.console.aliyun.com)。

2.  创建命名空间。

    1.  在左侧导航栏中，单击**命名空间**。

    2.  单击**创建**。

    3.  配置名称为monitoring，并单击**确定**。

3.  创建保密字典。

    1.  在左侧导航栏中，选择**应用配置** \> **保密字典**。

    2.  单击**创建**。

    3.  配置如下参数，单击**确定**。

        ![安装Prometheus](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9683129951/p128239.png)

        |参数|说明|
        |--|--|
        |**名称**|配置为**sls-ak**。|
        |**命名空间**|选中您在[步骤2](#step_321_yuw_7pi)中创建的命名空间，即**monitoring**。|
        |**类型**|选中**Opaque**，并添加如下两个键值对。         -   **名称**为**username**，**值**为您的RAM用户的AccessKeyID。
        -   **名称**为**password**，**值**为您的RAM用户的AccessKeySecret。
建议您使用只具备日志服务Project写入权限的RAM用户的AccessKey，详情请参见[授予指定Project写入权限](/cn.zh-CN/开发指南/访问控制RAM/RAM自定义授权场景.md)。 |

4.  创建PrometheusOperator。

    1.  在左侧导航中，选择**市场** \> **应用目录**。

    2.  单击**ack-prometheus-operator**。

    3.  在**参数**页签下，修改其中的配置项。

        -   调整prometheusSpec下的retention，建议修改为1d或12h。
        -   将tsdb下的enable设置为true，并增加remoteWrite配置。

            remoteWrite配置中的url为日志服务Metricstore的URL，请根据实际值替换。格式为https://\{project\}.\{sls-enpoint\}/prometheus/\{project\}/\{metricstore\}/api/v1/write。其中\{sls-enpoint\}为日志服务的Endpoint，详情请参见[服务入口](/cn.zh-CN/开发指南/API 参考/服务入口.md)，\{project\}和\{metricstore\}为您已创建的日志服务的Project和Metricstore。

        -   如果Prometheus数据量较大，可修改queue\_config配置，建议修改为：

            ```
            batchSendDeadline: 20s
            capacity: 20480
            maxBackoff: 5s
            maxSamplesPerSend: 2048
            minBackoff: 100ms
            minShards: 100
            ```

        ```
              remoteWrite:
              - basicAuth:
                  username:
                    name: sls-ak
                    key: username
                  password:
                    name: sls-ak
                    key: password
                queueConfig:
                  batchSendDeadline: 20s
                  maxBackoff: 5s
                  minBackoff: 100ms
                ### url格式为https://{project}.{sls-enpoint}/prometheus/{project}/{metricstore}/api/v1/write
                ### \{sls-enpoint\}为日志服务的Endpoint，详情请参见[服务入口](/cn.zh-CN/开发指南/API 参考/服务入口.md)。
                ### \{project\}和\{metricstore\}替换为您已创建的日志服务的Project和Metricstore。
                url: https://sls-prometheus-test.cn-beijing.log.aliyuncs.com/prometheus/sls-prometheus-test/prometheus-raw/api/v1/write
                                        
        ```


## 使用Grafana访问Prometheus数据

1.  登录Grafana。

2.  在左侧导航栏，单击**![G1](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7664559951/p112522.png)** \> **Data Sources**。

3.  在Data Sources页签，单击**Add data source**。

4.  单击**Prometheus**区域中的**Select**。

5.  在Settings页签中，配置如下参数。

    |参数|说明|
    |:-|:-|
    |Name|配置数据源名称，例如Prometheus-01。|
    |HTTP|    -   **URL**：日志服务MetricStore的URL，格式为https://\{project\}.\{sls-enpoint\}/prometheus/\{project\}/\{metricstore\}。其中\{sls-enpoint\}为日志服务的Endpoint，详情请参见[服务入口](/cn.zh-CN/开发指南/API 参考/服务入口.md)，\{project\}和\{metricstore\}为您已创建的日志服务的Project和Metricstore，请根据实际值替换。例如https://sls-prometheus-test.cn-hangzhou.log.aliyuncs.com/prometheus/sls-prometheus-test/prometheus。

**说明：** 为保证传输安全性，请务必使用https。

    -   **Whitelisted Cookies**：添加访问白名单，可选。 |
    |Auth|只需打开**Basic auth**开关。|
    |Basic Auth Details|    -   **User**为阿里云账号的AccessKeyID。
    -   **Password**为阿里云账号的AccessKeySecret。
建议您使用仅具备日志服务Project只读权限的RAM用户的AccessKey，详情请参见[授予指定Project只读权限](/cn.zh-CN/开发指南/访问控制RAM/RAM自定义授权场景.md)。 |

6.  单击**Save&Test**。

    配置完成后，您可以在Grafana上查看数据仪表盘。

    ![grafana](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9683129951/p128468.png)


