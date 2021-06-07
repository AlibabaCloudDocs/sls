# 接入Prometheus告警

您可以在Prometheus中，将日志服务开放告警系统配置为一个Alertmanager组件。配置完成后，Prometheus会将告警消息发送到日志服务告警系统中，由日志服务告警系统完成告警降噪、通知等处理。

已创建开放告警服务和应用。具体操作，请参见[配置开放告警对外接口](/intl.zh-CN/告警/告警（新版）/开放告警/配置开放告警对外接口.md)。

## Prometheus配置

在Prometheus配置文件中，添加alertmanagers配置，如下所示：

```
# Alertmanager configuration
alerting:
  alertmanagers:
  - path_prefix: /event/webhook/RAMAK_\{ACCESS\_KEY\_ID\}/\{WEBHOOK\_APP\_ID\}
    api_version: v2
    static_configs:
    - targets:
      - \{ALIYUN\_SLS\_ENDPOINT\}
```

|参数|说明|示例|
|--|--|--|
|path\_prefix|配置路径信息，此处配置为您在日志服务中创建开放告警服务和应用后生成的接口信息（子路径部分）。如何获取，请参见[获取接口信息](/intl.zh-CN/告警/告警（新版）/开放告警/配置开放告警对外接口.mdsection_098_gul_glw)。|```
-path_prefix: event/webhook/RAMAK_WEDC***YEBD/Prometheus-alert01_k8s
``` |
|targets|告警消息的接收端，此处配置为日志服务的访问域名，例如`cn-heyuan-intranet.log.aliyuncs.com`。如何获取，请参见[获取接口信息](/intl.zh-CN/告警/告警（新版）/开放告警/配置开放告警对外接口.mdsection_098_gul_glw)。**说明：** 如果您的Prometheus运行在阿里云ECS上，则建议您在选择Prometheus告警消息接入地域时，选择ECS所在地域，并使用局域网或VPC域名。否则您选择任一地域的公网接口即可。

|```
-targets:
        -cn-heyuan-intranet.log.aliyuncs.com
``` |

## Prometheus告警消息

Prometheus告警消息内容示例如下：

**说明：** 如果Prometheus告警消息中存在severity字段，则将Prometheus告警消息发送到日志服务后，日志服务会根据该字段映射告警严重度。如果没有，则默认映射为中等。更多信息，请参见[告警严重程度](/intl.zh-CN/告警/告警（新版）/开放告警/概述.md)。

```
[
    {
        "annotations": {
            "description": "description info",
            "summary": "High request latency"
        },
        "endsAt": "2020-10-28T12:28:52.710Z",
        "startsAt": "2020-10-28T12:23:37.710Z",
        "generatorURL": "http: //127.0.0.1:9090/graph?g0.expr=go_threads%7Binstance%3D%22localhost%3A9090%22%2Cjob%3D%22prometheus%22%7D+%3E+0\\u0026g0.tab=1",
        "labels": {
            "alertname": "HighErrorRate",
            "instance": "localhost:9090",
            "job": "prometheus",
            "severity": "page"
        }
    }
]
```

## 字段映射

Prometheus告警消息被接入到日志服务后，映射为日志服务告警内容。示例如下：

```
{
    "aliuid": "{开放告警应用所属的阿里云账号ID}",
    "alert_instance_id": "{自动生成}",
    "project": "{告警中心所属的Project}",
    "region": "{告警消息发送的网络接口对应的地域}",
    "alert_id": "HighErrorRate",
    "alert_type": "sls_pub",
    "alert_name": "HighErrorRate",
    "next_eval_interval": 78,
    "alert_time": 1603859020,
    "fire_time": 1603859017,
    "resolve_time": 0,
    "status": "firing",
    "labels": {
        "alertname": "HighErrorRate",
        "instance": "localhost:9090",
        "job": "prometheus"
    },
    "annotations": {
        "__pub_alert_region__": "{告警消息发送的网络接口对应的地域}",
        "__config_app__": "sls_pub_alert",
        "__pub_alert_service__": "{开放告警服务ID}",
        "__pub_alert_app__": "{开放告警应用ID}",
        "__pub_alert_protocol__": "prometheus",
        "severity": "page"
    },
    "severity": 2,
    "policy": {
        "alert_policy_id": "{开放告警应用中配置的告警策略}",
        "action_policy_id": "{开放告警应用中配置的行动策略}",
        "repeat_interval": "{开放告警应用中配置的重复等待时间}"
    },
    "drill_down_query": "http: //127.0.0.1:9090/graph?g0.expr=go_threads%7Binstance%3D%22localhost%3A9090%22%2Cjob%3D%22prometheus%22%7D+%3E+0\\u0026g0.tab=1"
}
```

日志服务告警属性与Prometheus字段的映射关系如下：

|告警属性|Prometheus字段|说明|
|----|------------|--|
|aliuid|无|用于接入告警的开放告警应用所属的阿里云账号ID|
|alert\_id|alertname|告警监控规则ID|
|alert\_type|无|告警类型，固定为sls\_pub。|
|alert\_name|alertname|告警监控规则名称|
|status|无|告警状态，包括firing和resolved。-   如果Prometheus告警消息中的endsAt大于alert\_time，则为firing。
-   如果Prometheus告警消息中的endsAt小于alert\_time，则为resolved。 |
|next\_eval\_interval|无|告警评估时间间隔。如果当前告警状态为firing，则其值为 \(endsAt-alert\_time\) / 4。 |
|alert\_time|无|日志服务接收到Prometheus告警消息的时间|
|fire\_time|startsAt|日志服务接收到Prometheus告警消息的时间|
|resolve\_time|无|告警恢复时间，详细说明如下：-   如果Prometheus告警消息中的endsAt大于alert\_time，则resolve\_time的值为0。
-   如果Prometheus告警消息中的endsAt小于alert\_time，则resolve\_time的值为endsAt对应的时间戳。 |
|labels|labels|告警标签信息。-   如果Prometheus告警消息的labels字段中存在severity字段，则被接入到日志服务后，severity字段将被添加日志服务告警的annotations字段中。
-   如果您在创建开放告警应用时 ，在**信息加工**中添加了标签信息，则此标签信息将被添加到labels字段中。

**说明：** 当您在**信息加工**中配置的标签的Key与Prometheus告警消息的labels字段中的子字段重复时，映射结果以您在**信息加工**中配置的为准。 |
|annotations|annotations|Prometheus告警被接入到日志服务后，日志服务告警的annotations字段中将添加如下额外字段。-   \_\_config\_app\_\_: "sls\_pub\_alert"
-   \_\_pub\_alert\_service\_\_: \{开放告警服务id\}
-   \_\_pub\_alert\_app\_\_: \{开放告警应用id\}
-   \_\_pub\_alert\_protocol\_\_: "prometheus"
-   \_\_pub\_alert\_region\_\_: \{告警消息发送的endpoint对应的region\}

如果您在创建开放告警应用时 ，在**信息加工**中添加了标注信息，则此标注信息将被添加到annotations字段中。

**说明：** 当您在**信息加工**中配置的标注的Key与Prometheus告警消息的labels字段中的子字段重复时，映射结果以您在**信息加工**中配置的为准。 |
|severity|severity|告警严重度。更多信息，请参见[告警严重程度](/intl.zh-CN/告警/告警（新版）/开放告警/概述.md)。|
|policy|无|您在开放告警应用中配置的告警策略。更多信息，请参见[Policy结构](/intl.zh-CN/告警/告警（新版）/通知管理/通知渠道/内容模板变量说明.md)。|
|project|无|告警中心所属的Project。更多信息，请参见[项目（Project）](/intl.zh-CN/产品简介/基本概念/项目（Project）.md)。|
|drill\_down\_query|generatorURL|展示Prometheus告警消息中generatorURL字段的值。|

