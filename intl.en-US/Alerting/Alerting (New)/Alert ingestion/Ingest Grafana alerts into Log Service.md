# Ingest Grafana alerts into Log Service

Grafana provides a GUI that allows you to use the alerting feature. You can add a notification channel in Grafana. After you add a notification channel, Grafana sends alerts to the alerting system of Log Service. Then, the alerting system denoises the alerts and sends alert notifications.

An alert ingestion application is created. For more information, see [Configure webhook URLs for alert ingestion](/intl.en-US/Alerting/Alerting (New)/Alert ingestion/Configure webhook URLs for alert ingestion.md).

## Configure Grafana

On the New notification channel page, set the required parameters. The following figure shows the parameters.

![Configure Grafana](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9527133261/p267572.png)

|Parameter|Description|
|---------|-----------|
|**Name**|The custom name of the notification channel.|
|**Type**|The type of the notification channel. Select **webhook**.|
|**Url**|The URL of the notification channel. Set this parameter to the full URL of the webhook URL that is generated after you create an alert ingestion service and an alert ingestion application in the alert ingestion system of Log Service. For more information, see [Obtain webhook URLs](/intl.en-US/Alerting/Alerting (New)/Alert ingestion/Configure webhook URLs for alert ingestion.mdsection_098_gul_glw). **Note:** If your Grafana server is deployed on an Elastic Compute Service \(ECS\) instance, we recommend that you select the region where the ECS instance resides and use the related LAN or virtual private cloud \(VPC\) endpoint. You can also use the Internet webhook URL of a region. |

## Grafana alerts

The following code provides an example of a Grafana alert.

**Note:** If the severity field exists in a Grafana alert, Log Service maps the severity of the alert to the corresponding severity after the alert is ingested into Log Service. If the severity field does not exist, the default severity of the Grafana alert is medium. For more information, see [Severity levels](/intl.en-US/Alerting/Alerting (New)/Alert ingestion/Overview.md).

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

## Field mapping

After a Grafana alert is ingested into Log Service, the alert is converted to an alert that is supported by Log Service by using field mapping. The following code provides an example of a Grafana alert:

```
{
    "aliuid": "{The ID of the Alibaba Cloud account to which the alert ingestion application belongs}",
    "alert_instance_id": "{The alert instance ID that is automatically generated}",
    "project": "{The project to which Alert Center belongs}",
    "region": "{The region of the endpoint to which the alert is sent}",
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
        "__pub_alert_region__": "{The region of the endpoint to which the alert is sent}",
        "__config_app__": "sls_pub_alert",
        "__pub_alert_service__": "{The ID of the alert ingestion service}",
        "__pub_alert_app__": "{The ID of the alert ingestion application}",
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
        "alert_policy_id": "{The alert policy that is specified for the alert ingestion application}",
        "action_policy_id": "{The action policy that is specified for the alert ingestion application}",
        "repeat_interval": "{The cycle that is specified for the alert ingestion application}"
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

The following table describes the mappings between the alert attributes of Log Service and Grafana fields.

|Alert attribute|Grafana field|Description|
|---------------|-------------|-----------|
|aliuid|None|The ID of the Alibaba Cloud account to which the alert ingestion application belongs.|
|alert\_id|ruleId|The ID of the alert monitoring rule.|
|alert\_type|None|The alert type. Valid value: sls\_pub.|
|alert\_name|ruleName|The name of the alert monitoring rule.|
|status|state|The status of the alert. -   If the value of the state field in the Grafana alert is ok, the value of the status field in Log Service is resolved.
-   If the value of the state field in the Grafana alert is not ok, for example, alerting, the value of the status field in Log Service is firing. |
|next\_eval\_interval|None|The interval at which the alert is evaluated. Valid value: 0.|
|alert\_time|None|The time when Log Service receives the Grafana alert.|
|fire\_time|None|The time when Log Service receives the Grafana alert.|
|resolve\_time|None|The time when the alert is cleared.|
|labels|tags|The labels of the alert. -   If the severity field exists in the tags field of the Grafana alert, the severity field is added to the annotations field after the alert is ingested into Log Service.
-   If you add a label on the **Enrichment** tab when you create the alert ingestion application, the specified label is added to the labels field.

**Note:** If the key of the specified label on the **Enrichment** tab is the same as a subfield in the tags field of the Grafana alert, the label on the **Enrichment** tab prevails. |
|annotations|None|After the Grafana alert is ingested into Log Service, the following fields are added to the annotations field of the corresponding Log Service alert: -   \_\_config\_app\_\_: "sls\_pub\_alert"
-   \_\_pub\_alert\_service\_\_: \{The ID of the alert ingestion service\}
-   \_\_pub\_alert\_app\_\_: \{The ID of the alert ingestion application\}
-   \_\_pub\_alert\_protocol\_\_: "grafana"
-   \_\_pub\_alert\_region\_\_: \{The region of the endpoint to which the alert is sent\}
-   orgId : \{The orgld field of the Grafana alert\}
-   dashboardId: \{The dashboardId field of the Grafana alert\}
-   panelId: \{The panelId field of the Grafana alert\}
-   ruleUrl: \{The ruleUrl field of the Grafana alert\}
-   imageUrl: \{The imageUrl field of the Grafana alert\}
-   desc: \{The message field of the Grafana alert\}
-   title: \{The title field of the Grafana alert\}

If you add an annotation on the **Enrichment** tab when you create the alert ingestion application, the specified annotation is added to the annotations field.

**Note:** If the key of the specified annotation on the **Enrichment** tab is the same as a subfield in the tags field of the Grafana alert, the annotation on the **Enrichment** tab prevails. |
|severity|severity|The alert severity. For more information, see [Severity levels](/intl.en-US/Alerting/Alerting (New)/Alert ingestion/Overview.md).|
|policy|None|The alert policy that is specified for the alert ingestion application. For more information, see [Data structure of the policy variable](/intl.en-US/Alerting/Alerting (New)/Notification management/Manage notification methods/Template variables.md).|
|project|None|The project to which Alert Center belongs. For more information, see [Projects](/intl.en-US/Product Introduction/Basic concepts/Projects.md).|
|drill\_down\_query|ruleUrl|The value of the ruleURL field in the Grafana alert is displayed.|
|results|evalMatches|The data of the result set. Each object in the evalMatches array is mapped to a QueryData structure in the results field. For more information about the mappings, see [Table 1](#table_6sd_hqs_8pd). For more information about the results field, see [Data structure of the results variable](/intl.en-US/Alerting/Alerting (New)/Notification management/Manage notification methods/Template variables.md).|

|QueryData structure|EvalMatch field|Description|
|-------------------|---------------|-----------|
|query|metric|A query statement.|
|fire\_result|tags and value|The content of the tags field in the Grafana alert is expanded into key-value pairs and stored in the fire\_result field. The value field is stored in the fire\_result field. The value of the value field is rounded to 3 decimal places. |

