# 时序数据对接Grafana

日志服务MetricStore提供了兼容Prometheus的查询接口，您可以直接通过Prometheus数据源方式对接到Grafana进行可视化演示。本文介绍配置Prometheus监控数据为Grafana数据源的操作步骤。

-   已安装Grafana。详情请参见[安装Grafana](http://docs.grafana.org/installation/)。
-   已接入时序数据。详情请参见[采集Prometheus监控数据](/intl.zh-CN/时序存储/数据接入/采集Prometheus监控数据.md)。

## 对接Grafana

1.  登录Grafana。

2.  在左侧导航栏，单击**Configuration** \> **Data Sources**。

3.  在Data Sources页签，单击**Add data source**。

4.  选择**Prometheus**，单击**Select**。

5.  在Settings页签，请您参考如下说明配置数据源。

    |参数|说明|
    |:-|:-|
    |Name|请您自定义一个数据源的名称，例如Prometheus-01。|
    |HTTP|    -   **URL**：日志服务MetricStore的URL，格式为https://\{project\}.\{sls-enpoint\}/prometheus/\{project\}/\{metricstore\}。其中\{sls-enpoint\}为Project所在地域的Endpoint，详情请参见[服务入口](/intl.zh-CN/开发指南/API 参考/服务入口.md)，\{project\}和\{metricstore\}为您已创建的日志服务的Project和Metricstore，请根据实际值替换。例如：https://sls-prometheus-test.cn-hangzhou.log.aliyuncs.com/prometheus/sls-prometheus-test/prometheus。

**说明：** 为保证传输安全性，请务必设置为`https`。

    -   **Whitelisted Cookies**：添加访问白名单，可选。 |
    |Auth|打开**Basic auth**开关。|
    |Basic Auth Details|    -   **User**为阿里云账号AccessKeyID。
    -   **Password**为阿里云账号AccessKeySecret。
建议您使用仅具备指定Project只读权限的RAM用户账号，详情请参见[指定Project只读授权策略](/intl.zh-CN/开发指南/访问控制RAM/RAM自定义授权场景.md)。|

6.  单击**Save & Test**。


## 导入日志服务Grafana模板

您可以在Grafana模板市场中查找日志服务提供的可视化模板并一键导入到您的Grafana中，进行可视化展示。

1.  复制Grafana模板ID。

    1.  登录[Grafana模板市场](https://grafana.com/grafana/dashboards?search=SLS)。

    2.  单击您要导入的模板。

    3.  在页面右侧，单击**Copy ID to Clipboard**。

2.  登录Grafana。

3.  在左侧导航栏中，单击**Create** \> **Import**。

4.  在Grafana.com Dashboard文本框中输入您在[步骤1](#step_a91_11u_9af)中复制的Grafana模板ID。

    配置完成后，单击空白处，即可进入配置页面，配置数据源。

5.  配置数据源。

    此处需配置为您在[对接Grafana](#section_h0f_4wl_pjo)中添加的数据源。不同仪表盘对应的数据源参数不同，可能为telegraf、host等。

6.  单击**Import**。


## Prometheus查询API

日志服务提供了兼容Prometheus的查询API，可直接配置日志服务作为Grafana的Prometheus数据源，同时也支持用各类Prometheus API直接访问。支持的API如下：

|API名称|示例|
|-----|--|
|[Instant queries](https://prometheus.io/docs/prometheus/latest/querying/api/#instant-queries)|```
GET /api/v1/query
POST /api/v1/query
``` |
|[Range queries](https://prometheus.io/docs/prometheus/latest/querying/api/#range-queries)|```
GET /api/v1/query_range
POST /api/v1/query_range
``` |
|[Getting label names](https://prometheus.io/docs/prometheus/latest/querying/api/#getting-label-names)|```
GET /api/v1/labels
POST /api/v1/labels
``` |
|[Querying label values](https://prometheus.io/docs/prometheus/latest/querying/api/#querying-label-values)|```
GET /api/v1/label/<label_name>/values
``` |
|[Finding series by label matchers](https://prometheus.io/docs/prometheus/latest/querying/api/#finding-series-by-label-matchers)|```
GET /api/v1/series
POST /api/v1/series
``` |

