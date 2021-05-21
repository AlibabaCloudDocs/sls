# Send time series data from Log Service to Grafana

Metricstores of Log Service are compatible with the query API of Prometheus. This topic describes how to connect Log Service as a Prometheus data source to Grafana. You can then send data from Log Service to Grafana and visualize the data in Grafana.

-   Grafana is installed. For more information, see [Install Grafana](http://docs.grafana.org/installation/).
-   Time series data is imported to Log Service. For more information, see [Collect metric data from Prometheus](/intl.en-US/Time Series Storage/Data collection/Collect metric data from Prometheus.md).

## Configure Log Service as a Prometheus data source in Grafana

1.  Log on to Grafana.

2.  In the left-side navigation pane, choose **Configuration** \> **Data Sources**.

3.  On the Data Sources tab, click **Add data source**.

4.  Move the pointer over the **Prometheus** card and click **Select**.

5.  On the Settings tab, set the following parameters.

    |Parameter or section|Description|
    |:-------------------|:----------|
    |Name|Set a name for the data source, for example, Prometheus-01.|
    |HTTP|    -   **URL**: Enter the URL of the Metricstore. Format: https://\{project\}.\{sls-enpoint\}/prometheus/\{project\}/\{metricstore\}. Replace \{sls-enpoint\} with the Log Service endpoint in the region where the project resides. For the list of Log Service endpoints in different regions, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md). Replace \{project\} and \{metricstore\} with the project name and Metricstore name. Example: https://sls-prometheus-test.cn-hangzhou.log.aliyuncs.com/prometheus/sls-prometheus-test/prometheus.

**Note:** To ensure secure transmission, use HTTPS.

    -   **Whitelisted Cookies**: Specify a whitelist for access. This parameter is optional. |
    |Auth|Select the **Basic Auth** check box.|
    |Basic Auth Details|    -   **User**: Enter an AccessKey ID.
    -   **Password**: Enter an AccessKey secret that corresponds to the AccessKey ID.
We recommend that you use an AccessKey pair of a RAM user that has only the permissions to read data from the Log Service project. For information about how to grant a RAM user only the permissions to read data from a specified Log Service project, see [The read-only permission on projects](/intl.en-US/Developer Guide/Access control RAM/Use custom policies to grant permissions to a RAM user.md).|

6.  Click **Save & Test**.


## Import a Log Service dashboard template to Grafana

Perform the following steps to import a Log Service dashboard template to Grafana:

1.  Copy the template ID.

    1.  Log on to the [dashboard market of Grafana](https://grafana.com/grafana/dashboards?search=SLS).

    2.  Click the template.

    3.  On the right side of the page, click **Copy ID to Clipboard**.

2.  Log on to Grafana.

3.  In the left-side navigation pane, choose **Create** \> **Import**.

4.  In the Grafana.com Dashboard field, paste the template ID that you copied in [Step 1](#step_a91_11u_9af).

    Then, click a blank area to go to the page for data source configuration.

5.  Configure the data source.

    In this step, set the parameters based on the data source that you added when you [configure Log Service as a Prometheus data source in Grafana](#section_h0f_4wl_pjo). The parameters vary depending on the dashboard template. The parameters may include telegraf and host.

6.  Click **Import**.


## Access Log Service by using the query API of Prometheus

Log Service is compatible with the query API of Prometheus. You can configure Log Service as a Prometheus data source in Grafana or use the Prometheus API to access Log Service. The following table provides examples of the supported API operations.

|Operation|Example|
|---------|-------|
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

