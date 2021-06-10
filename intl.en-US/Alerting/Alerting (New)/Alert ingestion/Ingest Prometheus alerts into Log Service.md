# Ingest Prometheus alerts into Log Service

You can configure the alert ingestion system of Log Service as an Alertmanager component in Prometheus. After you complete the configuration, Prometheus sends alerts to the alerting system of Log Service. Then, the alerting system denoises the alerts and sends alert notifications.

An alert ingestion service and an alert ingestion application are created. For more information, see [Configure webhook URLs for alert ingestion](/intl.en-US/Alerting/Alerting (New)/Alert ingestion/Configure webhook URLs for alert ingestion.md).

## Configure Prometheus

In the Prometheus configuration file, add the settings of the alertmanagers parameter. The following code shows the settings:

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

|Parameter|Description|Example|
|---------|-----------|-------|
|path\_prefix|The path. Set this parameter to the subpath of the webhook URL that is generated after you create an alert ingestion service and an alert ingestion application in the alert ingestion system of Log Service. For more information, see [Obtain webhook URLs](/intl.en-US/Alerting/Alerting (New)/Alert ingestion/Configure webhook URLs for alert ingestion.mdsection_098_gul_glw).|```
-path_prefix: event/webhook/RAMAK_WEDC***YEBD/Prometheus-alert01_k8s
``` |
|targets|The receiving end of alerts. Set the value to the endpoint of Log Service, for example, `cn-heyuan-intranet.log.aliyuncs.com`. For more information, see [Obtain webhook URLs](/intl.en-US/Alerting/Alerting (New)/Alert ingestion/Configure webhook URLs for alert ingestion.mdsection_098_gul_glw). **Note:** If your Prometheus server is deployed on an Elastic Compute Service \(ECS\) instance, we recommend that you select the region where the ECS instance resides and use the related LAN or virtual private cloud \(VPC\) endpoint. You can also use the Internet webhook URL of a region.

|```
-targets:
        -cn-heyuan-intranet.log.aliyuncs.com
``` |

## Prometheus alerts

The following code provides an example of a Prometheus alert.

**Note:** If the severity field exists in a Prometheus alert, Log Service maps the severity of the alert to the corresponding severity after the alert is ingested into Log Service. If the severity field does not exist, the default severity of the Prometheus alert is medium. For more information, see [Severity levels](/intl.en-US/Alerting/Alerting (New)/Alert ingestion/Overview.md).

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

## Field mapping

After a Prometheus alert is ingested into Log Service, the alert is converted to an alert that is supported by Log Service by using field mapping. The following code provides an example of a Prometheus alert:

```
{
    "aliuid": "{The ID of the Alibaba Cloud account to which the alert ingestion application belongs}",
    "alert_instance_id": "{The alert instance ID that is automatically generated}",
    "project": "{The project to which Alert Center belongs}",
    "region": "{The region of the endpoint to which the alert is sent}",
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
        "__pub_alert_region__": "{The region of the endpoint to which the alert is sent}",
        "__config_app__": "sls_pub_alert",
        "__pub_alert_service__": "{The ID of the alert ingestion service}",
        "__pub_alert_app__": "{The ID of the alert ingestion application}",
        "__pub_alert_protocol__": "prometheus",
        "severity": "page"
    },
    "severity": 2,
    "policy": {
        "alert_policy_id": "{The alert policy that is specified for the alert ingestion application}",
        "action_policy_id": "{The action policy that is specified for the alert ingestion application}",
        "repeat_interval": "{The cycle that is specified for the alert ingestion application}"
    },
    "drill_down_query": "http: //127.0.0.1:9090/graph?g0.expr=go_threads%7Binstance%3D%22localhost%3A9090%22%2Cjob%3D%22prometheus%22%7D+%3E+0\\u0026g0.tab=1"
}
```

The following table describes the mappings between the alert attributes of Log Service and Prometheus fields.

|Alert attribute|Prometheus field|Description|
|---------------|----------------|-----------|
|aliuid|None|The ID of the Alibaba Cloud account to which the alert ingestion application belongs.|
|alert\_id|alertname|The ID of the alert monitoring rule.|
|alert\_type|None|The alert type. Valid value: sls\_pub.|
|alert\_name|alertname|The name of the alert monitoring rule.|
|status|None|The alert status. Valid values: firing and resolved. -   If the value of the endsAt field in the Prometheus alert is greater than the value of the alert\_time field, the status is firing.
-   If the value of the endsAt field in the Prometheus alert is less than the value of the alert\_time field, the status is resolved. |
|next\_eval\_interval|None|The interval at which the alert is evaluated. If the current alert status is firing, the value of this parameter is calculated by using the following formula: Evaluation interval = \(endsAt - alert\_time\)/4. |
|alert\_time|None|The time when Log Service receives the Prometheus alert.|
|fire\_time|startsAt|The time when Log Service receives the Prometheus alert.|
|resolve\_time|None|The time when the alert is cleared.-   If the value of the endsAt field in the Prometheus alert is greater than the value of the alert\_time field, the value of the resolve\_time is 0.
-   If the value of the endsAt field in the Prometheus alert is less than the value of the alert\_time field, the value of the resolve\_time field is a timestamp. The timestamp is the same as the timestamp of the endsAt field. |
|labels|labels|The labels of the alert. -   If the severity field exists in the labels field of the Prometheus alert, the severity field is added to the annotations field after the alert is ingested into Log Service.
-   If you add a label on the **Enrichment** tab when you create the alert ingestion application, the specified label is added to the labels field.

**Note:** If the key of the specified label on the **Enrichment** tab is the same as a subfield in the labels field of the Prometheus alert, the label on the **Enrichment** tab prevails. |
|annotations|annotations|After the Prometheus alert is ingested into Log Service, the following fields are added to the annotations field of the corresponding Log Service alert: -   \_\_config\_app\_\_: "sls\_pub\_alert"
-   \_\_pub\_alert\_service\_\_: \{The ID of the alert ingestion service\}
-   \_\_pub\_alert\_app\_\_: \{The ID of the alert ingestion application\}
-   \_\_pub\_alert\_protocol\_\_: "prometheus"
-   \_\_pub\_alert\_region\_\_: \{The region of the endpoint to which the alert is sent\}

If you add an annotation on the **Enrichment** tab when you create the alert ingestion application, the specified annotation is added to the annotations field.

**Note:** If the key of the specified annotation on the **Enrichment** tab is the same as a subfield in the labels field of the Prometheus alert, the annotation on the **Enrichment** tab prevails. |
|severity|severity|The alert severity. For more information, see [Severity levels](/intl.en-US/Alerting/Alerting (New)/Alert ingestion/Overview.md).|
|policy|None|The alert policy that is specified for the alert ingestion application. For more information, see [Data structure of the policy variable](/intl.en-US/Alerting/Alerting (New)/Notification management/Manage notification methods/Template variables.md).|
|project|None|The project to which Alert Center belongs. For more information, see [Projects](/intl.en-US/Product Introduction/Basic concepts/Projects.md).|
|drill\_down\_query|generatorURL|The value of the generatorURL field in the Prometheus alert is displayed.|

