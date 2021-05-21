# Collect metric data from Prometheus

Prometheus is a cloud native application that is used to monitor metrics of various software and systems. This topic describes how to collect metric data from Prometheus to Log Service.

-   A Metricstore is created. For more information, see [Create a Metricstore](/intl.en-US/Time Series Storage/Manage Metricstores.md).
-   Prometheus is installed. For more information, see [GETTING STARTED](https://prometheus.io/docs/prometheus/latest/getting_started/).
-   Prometheus rules for data collection are configured. For more information, see the [<scrape\_config\>](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#scrape_config) section in the Prometheus documentation.

## Procedure

You can use the remote write feature of Prometheus to collect metric data to Log Service. To do this, you must perform the following steps to enable the feature in Prometheus:

1.  Log on to the server on which Prometheus is installed.

2.  Open the Prometheus configuration file and set the following fields based on your business requirements. For more information, see the [<remote\_write\>](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#remote_write) section in the Prometheus documentation.

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

    |Log field|Description|
    |---------|-----------|
    |url|The URL of the Metricstore. The format is https://\{project\}.\{sls-enpoint\}/prometheus/\{project\}/\{metricstore\}/api/v1/write. Replace the value of the \{project\} variable with the Log Service project name, and replace the value of the \{metricstore\} variable with the Metricstore name. For information about how to set the \{sls-enpoint\} variable, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md). **Note:**

    -   If you use the Alibaba Cloud internal network, we recommend that you use the domain name of the internal network.
    -   To ensure secure transmission, you must use HTTPS. |
    |basic\_auth|The authentication information. Authentication based on this field is required when the remote write feature is used to write data to Log Service. Set the value of the username subfield to an AccessKey ID of your Alibaba Cloud account and set the value of the password subfield to the AccessKey secret that corresponds to the AccessKey ID. We recommend that you use an AccessKey pair of a RAM user that has only the write permission on the Log Service project. For information about how to grant only the write permission on a specified Log Service project to a RAM user, see [Use custom policies to grant permissions to a RAM user](/intl.en-US/Developer Guide/Access control RAM/Use custom policies to grant permissions to a RAM user.md).|
    |queue\_config|The cache and retry policies for data writes.To minimize invalid network requests, set the min\_backoff subfield to a value that is greater than or equal to 100ms and set the max\_backoff subfield to a value that is greater than or equal to 5s.

If you need to collect a large amount of metric data from Prometheus, set the queue\_config field to the following value:

    ```
batch_send_deadline: 20s
capacity: 20480
max_backoff: 5s
max_samples_per_send: 2048
min_backoff: 100ms
min_shards: 100
    ``` |

3.  Check whether data is collected to Log Service.

    After you configure Prometheus, you can use the preview feature in the Log Service console to check whether data is collected to Log Service.

    1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

    2.  In the Projects section, click the project name.

    3.  On the **Metricstore Storage** \> **Metricstore** tab, choose **![Modify the configurations of a Logstore](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5023359951/p52318.png)** \> **Consumption Preview** next to the Metricstore.

        If data is displayed in the Consumption Preview pane, it indicates that the Prometheus configurations are correct.

        ![Prometheus data consumption](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6019441061/p128310.png)


After metric data is collected from Prometheus to Log Service, you can use the search and analysis feature of Log Service to query the metric data. For more information, see [Query and analyze time series data](/intl.en-US/Time Series Storage/Search and analysis/Query and analyze time series data.md). You can also use Grafana to visualize the Prometheus metric data. For more information, see [Send time series data from Log Service to Grafana]().

