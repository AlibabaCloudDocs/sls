# 接入Grafana告警

Grafana提供丰富的可视化界面，同时具备告警功能。您可以在Grafana中，添加Notification Channel配置。添加完成后，Grafana会将告警消息发送到日志服务告警系统中。由日志服务告警系统完成告警降噪、通知等处理。

已创建开放告警应用。更多信息，请参见[配置开放告警对外接口](/cn.zh-CN/告警/告警（新版）/开放告警/配置开放告警对外接口.md)。

## Grafana配置

在New notification channel页面中，配置如下参数。

![Grafana配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4399869161/p267572.png)

|参数|说明|
|--|--|
|**Name**|自定义Notification channel名称。|
|**Type**|Notification channel类型，此处配置为**webhook**。|
|**Url**|Notification channel的URL，此处配置为您在日志服务中创建开放告警服务和应用后生成的接口信息（完整URL）。如何获取，请参见[获取接口信息](/cn.zh-CN/告警/告警（新版）/开放告警/配置开放告警对外接口.mdsection_098_gul_glw)。**说明：** 如果您的Grafana运行在阿里云ECS上，则建议您在选择Grafana告警消息接入地域时，选择ECS所在地域，并使用局域网或VPC域名。否则您选择任一地域的公网接口即可。 |

## Grafana告警消息

Grafana告警消息内容示例如下：

**说明：** 如果Grafana告警消息中存在severity字段，则将Grafana告警消息发送到日志服务后，日志服务会根据该字段映射告警严重度。如果没有，则默认映射为中等。更多信息，请参见[告警严重程度](/cn.zh-CN/告警/告警（新版）/开放告警/概述.md)。

```
{
    "dashboardId": 1,
    "evalMatches": [
        {
            "value": 173.14285714285714,
            "metric": "go_gc_duration_seconds_count{instance=\"localhost: 9090\", job=\"prometheus\"}",
            "tags": {
                "__name__": "go_gc_duration_seconds_count",
                "instance": "localhost:9090",
                "job": "prometheus"
            }
        }
    ],
    "message": "sadfasdf",
    "orgId": 1,
    "panelId": 4,
    "ruleId": 2,
    "ruleName": "fuxasdfasd",
    "ruleUrl": "http://localhost:3000/d/biSKHC8Mz/new-dashboard-copy?tab=alert&viewPanel=4&orgId=1",
    "state": "alerting",
    "tags": {
        "severity" : "crit",
        "xasdfasdf": "mveonasdf"
    },
    "title": "[Alerting] fuxasdfasd"
}
```

## 字段映射

Grafana告警消息被接入到日志服务后，映射为日志服务告警内容。示例如下：

```
{
    "aliuid": "{开放告警应用所属的阿里云账号ID}",
    "alert_instance_id": "{自动生成}",
    "project": "{告警中心所属的Project}",
    "region": "{发送告警消息的网络接口对应的地域}",
    "alert_id": "2",
    "alert_type": "sls_pub",
    "alert_name": "sadfasdf",
    "next_eval_interval": 0,
    "alert_time": 1603859020,
    "fire_time": 1603859020,
    "resolve_time": 0,
    "status": "firing",
    "labels": {
        "xasdfasdf": "mveonasdf"
    },
    "annotations": {
        "__pub_alert_region__": "{发送告警消息的网络接口对应的地域}",
        "__config_app__": "sls_pub_alert",
        "__pub_alert_service__": "{开放告警服务ID}",
        "__pub_alert_app__": "{开放告警应用ID}",
        "__pub_alert_protocol__": "grafana",
        "severity" : "crit",
        "orgId": "1",
        "dashboardId": "1",
        "panelId": "4",
        "ruleUrl": "http://localhost:3000/d/biSKHC8Mz/new-dashboard-copy?tab=alert&viewPanel=4&orgId=1",
        "imageUrl": "",
        "desc": "sadfasdf",
        "title": "[Alerting] fuxasdfasd"
    },
    "severity": 10,
    "policy": {
        "alert_policy_id": "{开放告警应用中配置的告警策略}",
        "action_policy_id": "{开放告警应用中配置的行动策略}",
        "repeat_interval": "{开放告警应用中配置的重复等待时间}"
    },
    "drill_down_query": "http://localhost:3000/d/biSKHC8Mz/new-dashboard-copy?tab=alert&viewPanel=4&orgId=1",
    "results": [{
        "query": "go_gc_duration_seconds_count{instance=\"localhost: 9090\", job=\"prometheus\"}",
        "fire_result": {
            "__name__": "go_gc_duration_seconds_count",
            "instance": "localhost:9090",
            "job": "prometheus",
            "value": "173.142",
        }
    }]
}
```

日志服务告警属性与Grafana字段的映射关系如下：

|告警属性|Grafana字段|说明|
|----|---------|--|
|aliuid|无|用于接入告警的开放告警应用所属的阿里云账号ID|
|alert\_id|ruleId|告警监控规则ID|
|alert\_type|无|告警类型，固定为sls\_pub。|
|alert\_name|ruleName|告警监控规则名称|
|status|state|告警状态。-   如果Grafana告警消息中的state值为ok，则对应日志服务中的status值为resolved。
-   如果Grafana告警消息中的state值为其他值（例如alerting），则对应日志服务中的status值为firing。 |
|next\_eval\_interval|无|告警评估时间间隔，固定为0。|
|alert\_time|无|日志服务接收到Grafana告警消息的时间|
|fire\_time|无|日志服务接收到Grafana告警消息的时间|
|resolve\_time|无|告警恢复时间，固定为0。|
|labels|tags|告警标签信息。-   如果Grafana告警消息的tags字段中存在severity字段，则被接入到日志服务后，severity字段将被添加日志服务告警的annotations字段中。
-   如果您在创建开放告警应用时 ，在**信息加工**中添加了标签信息，则此标签信息将被添加到labels字段中。

**说明：** 当您在**信息加工**中配置的标签的Key与Grafana告警消息的tags字段中的子字段重复时，映射结果以您在**信息加工**中配置的为准。 |
|annotations|无|Grafana告警被接入到日志服务后，日志服务告警的annotations字段中将添加如下额外字段。-   \_\_config\_app\_\_: "sls\_pub\_alert"
-   \_\_pub\_alert\_service\_\_: \{开放告警服务id\}
-   \_\_pub\_alert\_app\_\_: \{开放告警应用id\}
-   \_\_pub\_alert\_protocol\_\_: "grafana"
-   \_\_pub\_alert\_region\_\_: \{告警消息发送的接入点对应的Region\}
-   orgId : \{grafana消息的orgId字段\}
-   dashboardId: \{grafana消息的dashboardId字段\}
-   panelId: \{grafana消息的panelId字段\}
-   ruleUrl: \{grafana消息的ruleUrl字段\}
-   imageUrl: \{grafana消息的imageUrl字段\}
-   desc: \{grafana消息的message字段\}
-   title: \{grafana消息的title字段\}

如果您在创建开放告警应用时 ，在**信息加工**中添加了标注信息，则此标注信息将被添加到annotations字段中。

**说明：** 当您在**信息加工**中配置的标注的Key与Grafana告警消息的tags字段中的子字段重复时，映射结果以您在**信息加工**中配置的为准。 |
|severity|severity|告警严重度。更多信息，请参见[告警严重程度](/cn.zh-CN/告警/告警（新版）/开放告警/概述.md)。|
|policy|无|您在开放告警应用中配置的告警策略。更多信息，请参见[Policy结构](/cn.zh-CN/告警/告警（新版）/通知管理/通知渠道/内容模板变量说明.md)。|
|project|无|告警中心所属的Project。更多信息，请参见[项目（Project）](/cn.zh-CN/产品简介/基本概念/项目（Project）.md)。|
|drill\_down\_query|ruleUrl|展示Grafana告警消息中ruleUrl字段的值。|
|results|evalMatches|结果集数据，evalMatches数组中的每一个对象，分别对应results中的一个QueryData结构。具体映射关系请参见[表 1](#table_6sd_hqs_8pd)。results字段的更多信息，请参见[QueryData结构](/cn.zh-CN/告警/告警（新版）/通知管理/通知渠道/内容模板变量说明.md)。|

|QueryData结构|EvalMatch字段|说明|
|-----------|-----------|--|
|query|metric|查询语句|
|fire\_result|tags、value|Grafana告警中tags字段的内容展开为键值对格式，存入fire\_result字段中。value字段直接存入fire\_result字段中。value字段的值只保留3位小数。 |

