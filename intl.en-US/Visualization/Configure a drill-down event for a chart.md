# Configure a drill-down event for a chart

Log Service allows you to configure a drill-down event for a chart to obtain deeper analysis results. This topic describes how to configure a drill-down event for a chart of a Logstore.

-   The indexing feature of a Logstore is enabled and indexes are configured. For more information, see [Configure indexes](/intl.en-US/Index and query/Configure indexes.md).
-   If the drill-down event that you configure is to open a Logstore, a Logstore must be created. For more information, see [Create a Logstore](/intl.en-US/Preparation/Manage a Logstore.md).
-   If the drill-down event that you configure is to open a saved search, a saved search must be created. For more information, see [Saved search](/intl.en-US/Index and query/Query/Saved search.md).

    If you need to set a variable for a chart, a placeholder variable must be set in the target saved search. For more information, see [Set a placeholder variable](/intl.en-US/Visualization/Add an analysis chart to a dashboard.md).

-   If the drill-down event that you configure is to open a dashboard, a dashboard must be created. For more information. see [Create a dashboard](/intl.en-US/Visualization/Create a dashboard.md).

    If you need to set a variable for a chart, a placeholder variable must be set for a chart on the destination dashboard. For more information, see [Set a placeholder variable](/intl.en-US/Visualization/Add an analysis chart to a dashboard.md).

-   If the drill-down event that you configure is to open a custom HTTP link, an HTTP link must be created.

Drilling is essential for data analysis. This feature allows you to analyze data in a fine-grained or coarse-grained way. Drilling includes rolling up and drilling down. Drilling down allows you to obtain deeper analysis results and make better decisions.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the project in which you want to query and analyze logs.

3.  On the **Log Storage** \> **Logstores** tab, click the Logstore where logs are stored.

4.  Enter a query statement in the search box, set a time range, and then click **Search & Analyze**.

5.  On the **Graph** tab, select a chart type, and set the parameters on the Properties tab of the chart.

    For more information about the properties of a chart, see [Analysis chart](/intl.en-US/Visualization/Analysis graph/Overview.md).

6.  Click the **Interactive Behavior** tab. On this tab, configure a drill-down event for the chart.

    You can set Event Action to Disable, Open Logstore, Open Saved Search, Open Dashboard, Open Dashboard, or Custom HTTP Link.

    -   **Disable**: disables the drill-down event.
    -   **Open Logstore**: sets the drill-down event to open a Logstore. If you set Event Action to Open Logstore, you must set the parameters. The following table describes the parameters.

        |Parameter|Description|
        |---------|-----------|
        |**Open in New Tab**|If you turn on this switch, the specified Logstore is opened on a new tab when the drill-down event is triggered.|
        |**Select Logstore**|The name of the Logstore to which you want to be redirected. When a drill-down event is triggered, you are redirected to the Logstore page.|
        |**Time Range**|The query time range of the data in the Logstore to which you are redirected. Valid values:         -   Default: The query time range of the data in the Logstore to which you are redirected is the default time range of a Logstore. The default time range is 15 minutes, accurate to the second.
        -   Inherit table time: The query time range of the data in the redirected Logstore is the time range that is specified for the chart when the drill-down event is triggered.
        -   Relative: The query time range of the data in the Logstore to which you are redirected is accurate to the second.
        -   Time Frame: The query time range of the data in the Logstore to which you are redirected is accurate to the minute, hour, or day. |
        |**Inherit Filtering Conditions**|If you turn on the **Inherit Filtering Conditions** switch, the filtering conditions that are added to the dashboard are synchronized to the Search & Analyze search box of the Logstore page to which you are redirected when the drill-down event is triggered.|
        |**Filter**|Enter a filter statement on the **Filter** tab. When the drill-down event is triggered, the filter statement is synchronized to the Search & Analyze search box of the redirected Logstore page.The filter statement can contain fields that you specify in the **Optional Parameter Fields** field. |

    -   **Open Saved Search**: sets the drill-down event to execute a saved search.

        |Notes|Notes|
        |:----|:----|
        |**Open in New Tab**|If you turn on this switch, the specified saved search is opened on a new tab when the drill-down event is triggered.|
        |**Select Saved Search**|The name of the saved search to which you want to be redirected.|
        |**Time Range**|The time range of the data that the saved search queries. Valid values:         -   Default: The time range of the data that the saved search queries is the default time range of a Logstore. The default time range is 15 minutes, accurate to the second.
        -   Inherit table time: The time range of the data that the saved search queries is the time range that is specified for the chart when the drill-down event is triggered.
        -   Relative: The time range of the data that the saved search queries is accurate to the second.
        -   Time Frame: The time range of the data that the saved search queries is accurate to the minute, hour, or day. |
        |**Inherit Filtering Conditions**|If you turn on the **Inherit Filtering Conditions** switch, the filtering conditions that are added to the dashboard are synchronized to the Search & Analyze search box of the Logstore page to which you are redirected when the drill-down event is triggered. The filtering conditions are appended to the saved search by using the `AND` operator.|
        |**Inherit Variables**|If you turn on the **Inherit Variables** switch, and the variable that you configure on the dashboard is the same as the variable in the saved search, the variable value that you click to trigger the drill-down event will replace the variable in the saved search. **Note:** You must configure a variable in the saved search before you can use this feature. |
        |**Filter**|Click the **Filter** tab, and then enter a filter statement in the Filter Statement field. When the drill-down event is triggered, the filter statement is appended to the redirected saved search by using the `AND` operator. The filter statement can contain fields that you specify in the **Optional Parameter Fields** field. |
        |**Variable**|Log Service allows you to modify a saved search by using variables. If you configure a variable that is the same as the variable in the saved search, the variable value that you click to trigger the drill-down event replaces the variable in the saved search. Click the **Variable** tab, and then add variables. **Note:**

        -   If you configure a variable for the chart, you must first configure a variable for the saved search to which you want to be redirected.
        -   You can add up to five dynamic and five static variables.
        -   Dynamic variables
            -   **Variable**: the name of the variable.
            -   **Variable Value Column**: the column where the variable value is located.
        -   Static variables
            -   **Variable**: the name of the variable.
            -   **Static Value**: the static value of the variable. |

    -   **Open Dashboard**: sets the drill-down event to open a dashboard. If you set Event Action to Open Dashboard, you must set the parameters. The following table describes the parameters.

        |Parameter|Description|
        |:--------|:----------|
        |**Open in New Tab**|If you turn on this switch, the specified dashboard is opened on a new tab when the drill-down event is triggered.|
        |**Select Dashboard**|The name of the dashboard to which you want to be redirected.|
        |**Time Range**|The query time range of the dashboard to which you are redirected. Valid values:         -   Default: The query time range of the dashboard to which you are redirected is the default time range of a dashboard. The default time range is 15 minutes, accurate to the second.
        -   Inherit table time: The time range of the chart on the dashboard to which you are redirected is the time range of the chart that is specified on the source dashboard when the drill-down event is triggered.
        -   Relative: The query time range of the dashboard to which you are redirected is accurate to the second.
        -   Time Frame: The query time range of the dashboard to which you are redirected is accurate to the minute, hour, or day. |
        |**Inherit Filtering Conditions**|If you turn on the **Inherit Filtering Conditions** switch, the filtering conditions that are added to the source dashboard are synchronized to the redirected dashboard when the drill-down event is triggered.|
        |**Inherit Variables**|If you turn on the **Inherit Variables** switch, and the variables that you configure on the source dashboard are synchronized to the redirected dashboard.|
        |**Filter**|Click the **Filter** tab, and then enter a filter statement in the Filter Statement field. When you click a chart value on the source dashboard, the filter statement is synchronized to the redirected dashboard. The filter statement can contain fields that you specify in the **Optional Parameter Fields** field. |
        |**Variable**|The variables that you configure for the chart are synchronized to the redirected dashboard when the drill-down event is triggered. Click the **Variable** tab, and then add variables. **Note:**

        -   If you configure a variable for the chart, you must first configure a variable for the chart on the dashboard to which you are redirected.
        -   You can add up to five dynamic and five static variables.
        -   Dynamic variables
            -   **Variable**: the name of the variable.
            -   **Variable Value Column**: the column where the variable value is located.
        -   Static variables
            -   **Variable**: the name of the variable.
            -   **Static Value**: the static value of the variable. |

    -   **Custom HTTP Link**: sets the drill-down event to open a custom HTTP link.

        The path in the HTTP link is the path of the destination file in the server. If you add an optional parameter to the path, when you click a chart value to trigger the drill-down event, the added parameter is replaced with the chart value. At the same time, you are redirected to the new HTTP link.

        |Parameter|Description|
        |:--------|:----------|
        |**Enter Link**|The address to which you want to be redirected.|
        |**Use System Variables**|If you turn on the **Use System Variables** switch, you can insert variables that are provided by Log Service into an HTTP link. The variables are $\{sls\_project\}, $\{sls\_dashboard\_title\}, $\{sls\_chart\_name\}, $\{sls\_chart\_title\}, $\{sls\_region\}, $\{sls\_start\_time\}, $\{sls\_end\_time\}, $\{sls\_realUid\}, and $\{sls\_aliUid\}.|
        |**Transcoding**|If you turn on the **Transcoding** switch, the HTTP link is encoded.|
        |**Optional Parameter Fields**|If you add an optional parameter to the path, when you click a chart value to trigger the drill-down event, the added parameter is replaced with the chart value.|

7.  Click **Add to New Dashboard**.

8.  In the dialog box that appears, set the dashboard name and chart name, and then click **OK**.


## Examples

You need to store NGINX access logs in a Logstore named accesslog and create a dashboard named RequestMethod and a dashboard named destination\_drilldown. Then you need to add a table of request methods to the dashboard named RequestMethod, and set the drill-down event for the table to open the dashboard named destination\_drilldown. You also need to add a line chart to the dashboard named destination\_drilldown. The line chart displays the change of PVs over time. When you click a request method on the table on the RequestMethod dashboard, you are redirected to the destination\_drilldown dashboard. Then you can view the change of PVs over time on the dashboard.

1.  Create a dashboard named destination\_drilldown.

    Before you configure a drill-down event for the table of request methods, you must create a dashboard to which you want to be redirected and add a line chart to the dashboard. The line chart displays the change of PVs over time. For more information, see [Add an analysis chart to a dashboard](/intl.en-US/Visualization/Add an analysis chart to a dashboard.md).

    -   Set the query statement.

        The query statement queries logs by request type and view the change of PVs over time.

        ```
        request_method: * | SELECT date_format(date_trunc('minute', __time__), '%H:%i:%s') AS time, COUNT(1) AS PV GROUP BY time ORDER BY time
        ```

    -   Set a placeholder variable.

        Specify the asterisk \(\*\) to generate a placeholder variable and set the variable name to method.

        ![Placeholder variable](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3923359951/p10732.png)

2.  Configure a drill-down event for the table of request methods, and add the table to the dashboard named RequestMethod.

    For more information, see [procedure](#section_cnd_wl3_y2b).

    -   Set the query statement.

        This query statement queries the number of log entries by request method in the NGINX access logs.

        ```
        *|SELECT request_method, COUNT(1) AS c GROUP BY request_method ORDER BY c DESC LIMIT 10
        ```

    -   Select a chart type.

        In this example, select the table.

    -   Configure a drill-down event for the table.

        -   Configure a drill-down event for the request\_method column in the table.
        -   Set **Select Dashboard** to **destination\_drilldown**.
        -   Set **Variable** to **method**.
        ![Event action](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0325866951/p10708.png)

3.  View drill-down analysis results.

    On the RequestMethod dashboard, click **GET**, and you are redirected to the destination\_drilldown dashboard. The asterisks \(\*\) in the original query statement is replaced with the value **GET**. The change of PVs of GET requests over time is displayed in a line chart.

    ![Request method](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4923359951/p10714.png)

    ![Dashboard to which you are redirected](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4923359951/p10739.png)


