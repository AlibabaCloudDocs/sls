# Saved search

If you need to frequently view the result of a query statement, you can save the query statement as a saved search. Log Service provides the saved search feature to save the required data query and analysis operations. You can use a saved search to quickly perform query and analysis operations.

The indexing feature is enabled and indexes are configured. For more information, see [Configure indexes](/intl.en-US/Index and query/Configure indexes.md).

## Create a saved search

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the project in which you want to query and analyze logs.

3.  On the **Log Storage** \> **Logstores** tab, click the Logstore where logs are stored.

4.  Enter a query statement in the search box, specify a time range, and then click **Search & Analyze**.

    A query statement consists of a search statement and an analytic statement in the format of Search statement\|Analytic statement. For more information, see [Search syntax](/intl.en-US/Index and query/Query/Search syntax.md) and [SQL syntax and functions](/intl.en-US/Index and query/Analysis grammar/General aggregate functions.md).

5.  Click **Save Search** in the upper-right corner of the page.

6.  In the Saved Search Details panel, set the required parameters. The following table describes the parameters.

    ![Generate variables](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8523359951/p10770.png)

    |Parameter|Description|
    |---------|-----------|
    |**Saved Search Name**|The name of the saved search.|
    |**Variable Config**|To perform drill-down analysis, select the required content of the query statement in the **Query** field, and click **Generate Variables** to generate a placeholder variable.     -   **Variable Name**: the name of the placeholder variable.
    -   **Default Value**: the content that you select from the **Query** field.
    -   **Matching Mode**: the match mode. You can use the match mode to replace the default value by triggering a drill-down event. Valid values: Global Match and Exact Match.
        -   Global Match: matches all words that are the same as the selected keyword in the query statement as variables.
        -   Exact Match: matches the selected keyword in the query statement as a variable.
For example, when you configure drill-down analysis for a chart, you set **Event Action** to **Open Saved Search** for the chart, and specify the saved search. The **Variable** of the chart is the same as the **Variable Name** of the saved search. When you click the chart value, you are redirected to the saved search. The **Default Value** of the placeholder variable is replaced by the chart value that triggers the drill-down event. Then, the new query statement is executed. For more information, see [Configure a drill-down event for a chart](/intl.en-US/Visualization/Configure a drill-down event for a chart.md).

**Note:** Before you set **Event Action** to **Open Saved Search**, you must create a saved search and configure variables. |

7.  Click **OK**.

    After you create a saved search, click the ![Saved search - 002](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5705813061/p103650.png) icon next to the search box on the Search & Analysis page of the Logstore, and click the name of the saved search to quickly perform query and analysis operations.


## Modify a saved search

1.  In the left-side navigation pane, choose **Resources** \> **Saved Search**.

2.  In the Saved Search list, click the saved search that you want to modify.

3.  Enter a query statement and click **Search & Analyze**.

    A query statement consists of a search statement and an analytic statement in the format of Search statement\|Analytic statement. For more information, see [Search syntax](/intl.en-US/Index and query/Query/Search syntax.md) and [SQL syntax and functions](/intl.en-US/Index and query/Analysis grammar/General aggregate functions.md).

4.  Click **Modify Saved Search** in the upper-right corner of the page.

5.  In the Saved Search Details panel, modify the required settings and click **OK**.


## Obtain the ID of a saved search

After you create a saved search, you can use the ID of the saved search to embed the saved search page to a self-managed web page. For more information, see [Customize the UI of embedded pages](/intl.en-US/Developer Guide/Other visualization methods/Customize the UI of embedded pages.md).

1.  In the left-side navigation pane, choose **Resources** \> **Saved Search**.

2.  In the Saved Search list, click the saved search from which you want to obtain the ID.

3.  Obtain the ID of the saved search in the URL.

    ![Saved search ID](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0778506261/p277471.png)


