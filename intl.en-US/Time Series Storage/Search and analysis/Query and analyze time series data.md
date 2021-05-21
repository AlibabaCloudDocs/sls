# Query and analyze time series data

This topic describes how to query and analyze time series data in a Metricstore and how to set the legend format for a time series chart.

Time series data is collected.

## Procedure

**Note:** Only the combination of SQL and PromQL is supported on the query page of a Metricstore. If you need to use the standard SQL syntax, click the **Search & Analyze** button in the upper-right corner to go to the Search & Analysis page of the Metricstore.

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the name of the project in which the time series data is stored.

3.  On the **Time Series Storage** \> **Metricstore** tab, click the name of the MetricStore where the time series data is stored.

4.  In the upper-right corner of the page, click **15 Minutes\(Relative\)** to set a time range for query and analysis.

    You can select a relative time, set a time frame, or customize a time range.

    **Note:** The query results may contain time series data that is generated one minute earlier or later than the specified time range.

5.  On the Query Statements tab, query and analyze the time series data.

    You can use the following methods:

    -   Enter a query statement in the field next to the Metricstore name and click **Preview**.

        You can click the **Add Query Statement** button or the ![Copy the query statement](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9435541061/p128502.png) icon to add or copy a query statement. Then, enter the query statement and click **Preview**.

        The results of multiple query statements are displayed in the same time series chart.

    -   Select a metric from the **Metrics** drop-down list. A query statement is automatically generated. Then, click **Preview**.

        You can modify the generated query statement.

    ![Query and analysis in a Metricstore](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9435541061/p128490.png)


## Set the legend format for the time series chart

After query statements are specified, you can set the legend format for the time series chart on the Query Statements tab.

The default legend for each time series consists of the metric name and labels. You can change the legend to a label value by using a magic variable. The format is \{\{Label key\}\}. For example, the label of a time series is \{ip="123.0.0.1"\}. If you enter \{\{ip\}\} in the **Legend Format** field, the legend of the time series changes to 123.0.0.1.

![Set the legend format](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9435541061/p128687.png)

## Set a placeholder variable

Log Service allows you to set a placeholder variable in a query statement. For example, assume that you configure a drill-down event for Chart A to redirect to the dashboard where Chart B is located. The placeholder variable that you configured for Chart B is the same as the variable that you click to trigger the drill-down event. Then, the placeholder variable is replaced by the variable that you click to trigger the drill-down event and the query statement of Chart B is executed. The format for a placeholder variable is `${{the variable name|the default value}}`, for example, `host=~"^.*"` can be set to `host=~"${{host|^.*}}"`.

Example: In the following query statement, set the values of the host, url, method, status, and proxy\_upstream\_name fields to placeholder variables. The following figure shows the result.

```
* | select promql_query_range('sum(sum_over_time(pv:host:status:method:upstream_name:upstream_status:url{host=~"^.*", url=~".*$", method=~".*", status=~".*", proxy_upstream_name=~".*"}[1m]))') from metrics limit 10000
```

![Placeholder variables](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2529751261/p174559.png)

## Related operations

|Operation|Description|
|---------|-----------|
|Create a saved search for the specified query statement|On the Query Statements tab, click the ![Create a saved search for the specified query statement](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9435541061/p128501.png) icon to create a saved search for the specified query statement. For more information, see [Saved search](/intl.en-US/Index and query/Query/Saved search.md).|
|Copy the specified query statement|On the Query Statements tab, click the ![Copy the specified query statement](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9435541061/p128502.png) icon to copy the specified query statement.|
|View the raw time series data|On the Query Statements tab, click the ![View the raw time series data](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9435541061/p128504.png) icon to view the raw time series data.|
|Set the properties of the time series chart|On the Properties tab, set the properties of the time series chart. For more information, see [Configure a time series chart](/intl.en-US/Time Series Storage/Visualization/Configure a time series chart.md).|
|Configure a drill-down event|On the Interactive Behavior tab, configure a drill-down event. For more information, see [Configure a drill-down event for a chart](/intl.en-US/Query and visualization/Dashboard/Configure a drill-down event for a chart.md).|
|Add the query results to a dashboard|Click **Add to New Dashboard** to add the query results to a dashboard.|
|Create an alert rule based on a query statement|Click **Save as Alert** to create the alert rule. For more information, see [Create an alert rule](/intl.en-US/Query and visualization/Alarm/Create an alert rule.md).|
|Refresh the time series data|You can manually refresh the time series data in the Metricstore or enable the automatic refresh feature. -   In the upper-right corner of the query page, choose **Refresh** \> **Once** to refresh the time series data in the Metricstore.
-   In the upper-right corner of the query page, choose **Refresh** \> **Auto Refresh** and select a refresh interval. Then, the time series data in the Metricstore is automatically refreshed at the selected interval.

The interval can be 15 seconds, 60 seconds, 5 minutes, or 15 minutes. |
|Share the query results|In the upper-right corner of the query page, click **Share** to copy the URL of the page. You can then send the URL to other users who have the permissions to query the time series data in the Metricstore. The query results that are displayed when you copy the URL are also displayed when a user visits the URL.|
|Go to the Search & Analysis page|In the upper-right corner of the Search & Analysis page, click **Search & Analyze**.|

