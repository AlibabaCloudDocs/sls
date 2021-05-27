# Saved search

The saved search feature allows you to save a query statement as a saved search. The feature can simplify data queries. This topic describes how to create a saved search in the Log Service console.

The indexing feature is enabled and indexes are configured. For more information, see [Configure indexes](/intl.en-US/Index and query/Configure indexes.md).

If you need to frequently view the result of a query statement, you can save the query statement as a saved search. You can also use this saved search in alert rules. Log Service periodically executes the saved search and sends alert notifications if the query result meets the specified conditions.

## Create a saved search

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the project in which you want to query and analyze logs.

3.  On the **Log Storage** \> **Logstores** tab, click the Logstore where logs are stored.

4.  Enter a query statement in the search box, set a time range, and then click **Search & Analyze**.

5.  Click **Save Search** in the upper-right corner of the page.

6.  In the Saved Search Details dialog box, set the parameters. The following table describes the parameters.

    ![Generate variables](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8523359951/p10770.png)

    |Parameter|Description|
    |---------|-----------|
    |**Saved Search Name**|The name of the saved search. The name must meet the following requirements:     -   The name can contain lowercase letters, digits, hyphens \(-\), and underscores \(\_\).
    -   The name must start and end with a lowercase letter or digit.
    -   The name must be 3 to 63 characters in length. |
    |**Variable Config**|Select part of the **Query** statement, and click **Generate Variable**. The generated variable is a placeholder variable.     -   **Variable Name**: the name of the placeholder variable.
    -   **Default Value**: the content that you select from the **Query** statement.
    -   **Matching Mode**: the match mode. You can use the match mode to replace the default value by triggering a drill-down event. Valid values: Global Match and Exact Match.
For example, when you configure drill-down analysis for a chart, you set **Event Action** to **Open Saved Search** for the chart, and specify the saved search. The **Variable** of the chart is the same as the **Variable Name** of the saved search. When you click the chart value, you are redirected to the saved search. At the same time, the **Default Value** of the placeholder variable is replaced by the chart value that triggers the drill-down event. Then, the new query statement is executed. For more information, see [Configure a drill-down event for a chart](/intl.en-US/Query and visualization/Dashboard/Configure a drill-down event for a chart.md).

**Note:** Before you can set **Event Action** to **Open Saved Search** when you configure drill-down analysis for a chart, you must create a saved search and configure variables. |

7.  Click **OK**.


## Modify a saved search

1.  Click the ![Saved search - 002](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5705813061/p103650.png) icon on the left side of the search box. In the Saved Search dialog box, click the destination saved search.

2.  Enter a new query statement and click **Search & Analyze**.

3.  Click **Modify Saved Search**.


