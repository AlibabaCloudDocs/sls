# Query logs

This topic describes how to query logs in a Logstore.

-   Log data is collected. For more information, see [Data collection](/intl.en-US/Data Collection/Log collection methods.md).
-   The indexing feature is enabled and indexes are configured for the Logstore. For more information, see [Enable and configure the indexing feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).

## Log search and analytics

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click a project.

3.  On the **Log Management** \> **Logstores** tab, choose **![Management icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9484688951/p52166.png)** \> **Search & Analysis** to the right of a Logstore.

4.  On the Search & Analysis page, select **15 Minutes** in the Relative section to set the time range for the query.

    You can select a relative time or time frame, or customize a time range.

    **Note:** The query results may contain logs that are generated 1 minute earlier or later than the specified time period.

5.  Enter a query statement in the search box.

    A query statement consists of a search statement and an analytic statement in the format of search statement\|analytic statement. For more information, see [Statement format](/intl.en-US/Index and query/Overview.md).

6.  Click **Search & Analyze** to view the query results.


## Manage the query results

You can view the query results in a log distribution histogram, on the Raw Logs tab, or by using a chart. You can also configure alerts and saved searches.

**Note:** By default, 100 results are returned. For information about how to obtain more than 100 results, see [LIMIT syntax](/intl.en-US/Index and query/Analysis grammar/LIMIT syntax.md).

-   Log distribution histogram

    The log distribution histogram shows the distribution of query results in different time ranges.

    -   Move the pointer over a green rectangle to view the time range that is represented by the rectangle. You can also view the number of log entries that are obtained within the time range.
    -   Click a rectangle to view a more fine-grained log distribution. You can also view the query results on the Raw Logs tab.

        ![Log distribution histogram](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1523359951/p12708.png)

-   Raw Logs tab

    On the Raw Logs tab, you can view the query results. You can perform the following operations:

    -   Quick analysis: analyzes the distribution of a field within a period of time. For more information, see [Quick analysis](/intl.en-US/Index and query/Query/Quick analysis.md).
    -   Contextual query: queries the contextual data of the specified log entries in the raw log file. Choose **![Query Logs - 004](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0484688951/p103516.png)** \> **Context View**. A contextual query is performed. For more information, see [Context query](/intl.en-US/Index and query/Query/Context query.md).
    -   LiveTail: monitors log data in real time and extracts key information. Choose **![Query Logs - 004](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0484688951/p103516.png)** \> **LiveTail**. Log monitoring and extraction are performed. For more information, see [LiveTail](/intl.en-US/Index and query/Query/LiveTail.md).

        **Note:** LiveTail can monitor and extract the log data that is collected by Logtail.

    -   Key-value pair arrangement: displays log entries in key-value pairs. Choose **![Query Logs - 004](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0484688951/p103516.png)** \> **Warp/Unwarp Key-value Pairs**. Log entries are displayed in key-value pairs.
    -   Log download: downloads logs. In the upper-right corner of the Raw Logs tab, click the ![Download logs](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0484688951/p103211.png) icon. In the Log Download dialog box, select a download range and tool, and then click OK. Logs are downloaded. For more information, see [Download logs](/intl.en-US/Index and query/Download logs.md).
    -   Column settings: sets fields. In the upper-right corner of the Raw Logs tab, click **Column Settings**. Select fields from the section on the left. Click **Add** to add the fields to the section on the right. The columns that correspond to the added fields appear on the Raw Logs tab. The field names are column names. The columns list the field values.

        **Note:** To view the log content on the **Raw Logs** tab, you must select Content.

        ![Column settings](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0484688951/p12709.png)

    -   Content column settings: If the content of a field exceeds 3,000 characters, the excess characters are hidden. In this case, the message **The character string is too long and has been truncated** is displayed in front of the key value. You can click **Display Content Column** to modify the configurations.

        **Note:** If the content display limit is set to 10,000 characters, excess characters are not delimited.

        ![Content column settings](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0484688951/p21106.png)

        The following table describes the parameters in the Display Content Column dialog box.

        |Parameter|Description|
        |:--------|:----------|
        |**Key-Value Pair Arrangement**|Valid values: **New Line** and **Full Line**.|
        |**Hide Default Key-value Pairs**|If you turn on this switch, the reserved fields of Log Service are hidden.|
        |**Default JSON Data Level**|The level of JSON expansion.|
        |**Truncate Character String**|**Key**|The key of the truncated value. By default, a field value is truncated if it contains more than 3,000 characters. The value of this parameter is null if no field values exceed 3,000 characters.|
        |**Status**|Specifies whether to enable the value truncation feature. By default, the feature is enabled.         -   **Enable**: If the value length exceeds the specified **truncate step**, the excess characters are truncated.
        -   **Disable**: If the value length exceeds the specified **truncate step**, the excess characters are not truncated. |
        |**Truncate Step**|Specifies the maximum number of characters that can be displayed for a value. This parameter also specifies the number of incremental characters that are displayed each time you click Show. Valid values: 500 to 10000. Default value: 3000. |

-   Charts

    If you enable analytics when you configure indexes for fields and use query statements to query logs, you can view the analysis results on the Graph tab.

    -   Multiple chart types are provided in Log Service, including tables, line charts, and bar charts. You can select a chart type to display the analysis results. For more information, see [Chart overview](/intl.en-US/Query and visualization/Analysis graph/Overview.md).
    -   Log Service allows you to create dashboards for real-time data analysis. You can click **Add to New Dashboard** to save your query statements as a chart to a specified dashboard. For more information, see [Create and delete a dashboard](/intl.en-US/Query and visualization/Dashboard/Create a dashboard.md).
    -   Drill-down analysis allows you to view deeper analysis results, which reveal more details. You can set the drill-down parameters and add the chart to a dashboard. Click a chart value to trigger a drill-down event. You can view deeper analysis results. For more information, see [Configure a drill-down event for a chart](/intl.en-US/Query and visualization/Dashboard/Configure a drill-down event for a chart.md).
-   Alert

    You can click **Save as Alert** on the Search & Analysis page to create an alert for the query results. For more information, see [Create an alert rule](/intl.en-US/Query and visualization/Alarm/Create an alert rule.md).

-   Saved search

    You can also click **Save Search** on the Search & Analysis page to create a saved search. For more information, see [Saved search](/intl.en-US/Index and query/Query/Saved search.md).


