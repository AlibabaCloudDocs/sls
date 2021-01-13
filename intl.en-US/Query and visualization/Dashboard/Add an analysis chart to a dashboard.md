# Add an analysis chart to a dashboard

Log Service allows you save query results as charts to a dashboard. This topic describes how to add an analysis chart to a dashboard.

-   Log data is collected. For more information, see [Data collection](/intl.en-US/Data Collection/Log collection methods.md).
-   The indexing feature of the Logstore is enabled and indexes are configured. For more information, see [Enable and configure the indexing feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).

## Limits

Each dashboard can contain a maximum of 50 analysis charts.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the destination project.

3.  Query logs.

    1.  Choose **Log Storage** \> **Logstores**. Find the target Logstore, and then choose **![Management icon](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9484688951/p52166.png)** \> **Search & Analysis**.

    2.  On the page that appears, enter a query statement in the search box, and click **Search & Analyze**.

4.  On the Graph tab, select a chart type, such as the line chart.

5.  On the Properties tab, configure the properties of the chart.

    The properties vary depending on the chart types. For more information, see [Charts](/intl.en-US/Query and visualization/Analysis graph/Overview.md).

6.  Set a placeholder variable.

    Set a placeholder variable for the chart. Then you can perform drill-down analysis and filter data. For example, you configure a placeholder variable for the current chart and then save the chart to Dashboard A. Then you configure a drill-down event for another chart. The variable name that you configure for the chart is the same as the placeholder variable. When you click a value on the chart, you are redirected to Dashboard A. At the same time, the placeholder variable is replaced with the chart value that you click, and the new query statement of the current chart is executed. For more information, see [Configure a drill-down event for a chart](/intl.en-US/Query and visualization/Dashboard/Configure a drill-down event for a chart.md).

    1.  On the **Data Source** tab, select the keyword in the query statement and click **Generate Variable**.

        You can configure multiple variables.

    2.  Set the parameters. The following table describes the parameters.

        ![Generate variables](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1923359951/p10583.png)

        |Parameter|Description|
        |:--------|:----------|
        |**Variable Name**|The name of the placeholder variable.|
        |**Default Value**|The default value of the placeholder variable in the current chart.|
        |**Matching Mode**|Valid values:         -   Global Match: matches all words that are the same as the selected keyword in the query statement as variables.
        -   Exact Match: matches the selected keyword in the query statement as a variable. |
        |**Result**|The query statement that contains the specified variable.|

7.  Configure a drill-down event.

    After you configure a drill-down event for the chart, you can click a value on the chart to implement a more fine-grained analysis.

    On the **Interactive Behavior** tab, specify the fields for which you need to enable drill-down analysis. For more information, see [Configure a drill-down event for a chart](/intl.en-US/Query and visualization/Dashboard/Configure a drill-down event for a chart.md).

    ![Event Action](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1923359951/p10243.png)

8.  Click **Add to New Dashboard**.

9.  In the dialog box that appears, specify the dashboard and chart name, and then click **OK**.

    |Parameter|Description|
    |:--------|:----------|
    |**Operation**|Valid values:     -   **Add to Existing Dashboard**: adds the chart to an existing dashboard.
    -   **Create Dashboard**: creates a dashboard and then add the chart to the dashboard. |
    |**Dashboards**|Select an existing dashboard. This parameter is required only when you set the **Operation** parameter to **Add to Existing Dashboard**. |
    |**Dashboard Name**|Enter a name for the new dashboard. This parameter is required only when you set the **Operation** parameter to **Create Dashboard**. |
    |**Chart Name**|The name of the chart. The chart name is displayed as the chart title on the dashboard.|


You can add up to 50 analysis charts to the dashboard.

