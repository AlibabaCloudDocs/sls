# Add a filter

You can add a filter to a dashboard and use the filter to refine search results or replace placeholder variables with specified values. This topic describes how to add a filter to a dashboard. The topic also provides two examples.

-   The indexing feature of the Logstore is enabled and indexes are configured. For more information, see [Configure indexes](/intl.en-US/Index and query/Configure indexes.md).
-   Analysis charts are added to the dashboard. For more information, see [Add an analysis chart to a dashboard](/intl.en-US/Visualization/Add an analysis chart to a dashboard.md).

    If the filter type is set to Replace Variable, placeholder variables must be specified for the charts on the dashboard.


A filter modifies the search statements or replace placeholder variables for all charts on a dashboard. Each analysis chart represents the query results of a query statement that is in the format of \[search query\] \| \[sql query\]. The filtering condition is appended to the original query statement to filter data.

-   Filter: uses key-value pairs as a filtering condition. The condition is appended to the original query statement by using the `AND` or `NOT` operator. By using the AND operator, the new query statement is in the Key: Value AND \[search query\] \| \[sql query\] format. This statement means to search the query results of the original query statement for log entries that contain Key:Value. For the Filter type, you can select or enter multiple key-value pairs. When you select multiple key-value pairs as the filtering condition, the logical OR operator is used between the pairs.
-   Replace Variable: specifies a variable and the variable value. If the dashboard contains a chart for which the same variable is configured, the variable in the query statement of the chart is replaced with the selected value when a filtering operation is performed. This applies to all charts for which the same variable is configured.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the project in which you want to query and analyze logs.

3.  In the left-side navigation pane, click the ![Create Alert for Monitoring - 001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p104975.png) icon.

4.  In the dashboard list that appears, click the target dashboard.

5.  In the upper-right corner of the dashboard page that appears, click **Edit**.

6.  Click the ![Filter icon](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4923359951/p36998.png) icon in the menu bar.

7.  In the Add Filter dialog box, set the parameters, and then click **OK**. The following table describes the parameters.

    |Parameter|Description|
    |:--------|:----------|
    |**Filter Name**|The name of the filter.|
    |**Display Settings**|Valid values:     -   **Title**: specifies whether to add a title for the filter. You can turn on the Title switch to add a title for the filter.
    -   **Border**: specifies whether to add borders to the filter. You can turn on the Border switch to add borders to the filter.
    -   **Background**: specifies whether to add a white background to the filter. You can turn on the Background switch to add a white background to the filter. |
    |**Type**|The type of the filter.     -   **Filter**: filters data by key-value pairs. When a filtering operation is performed, the filtering condition is appended to the original query statement by using the `AND` or `NOT` operator.

        -   By using the `AND` operator, the new statement is in the Value AND \[search query\] \| \[sql query\] format.
        -   By using the `NOT` operator, the new statement is in the Value NOT \[search query\] \| \[sql query\] format.
A static list item is a value of the specified key that is used to filter the query results of a query statement. You can set multiple values in the **Static List Items** field.

    -   **Replace Variable**: specifies a variable and the variable value. If the dashboard contains a chart for which the same variable is configured, the variable in the query statement of the chart is replaced with the selected value when a filtering operation is performed. A static list item is a value of the specified variable that is used to filter the query results of a query statement. You can set multiple values in the **Static List Items** field. |
    |**Key**|    -   If you select **Filter**, enter the key that you use to filter data in the **Key** field.
    -   If you select **Replace Variable**, enter the variable that you use to filter data in the **Key** field.

**Note:** If you select **Replace Variable**, you must configure a placeholder variable for analysis charts. The placeholder variable must be the same as the variable that you specify in the Key field. |
    |**Alias**|The alias of the key. This parameter is available only when you select **Filter**.|
    |**Global filter**|The parameter is available only when you select **Filter**.     -   To filter the specified values in all fields, turn on the **Global filter** switch.
    -   To filter the specified values in specified keys, turn off the **Global filter** switch. |
    |**Static List Items**|The values of the **key** that are used to filter data. You can click the plus sign \(**+**\) multiple times to enter multiple values for the specified key. If you turn on the **Select by Default** switch for a value, this value is used to filter data every time you open the dashboard. |
    |**Add Dynamic List Item**|If you turn on the **Add Dynamic List Item** switch, dynamic values can be retrieved for the specified **key**. Dynamic list items are dynamic values that are retrieved by executing the specified query statement. The values vary in different time ranges. If you turn on the **Add Dynamic List Item** switch, you must set the following parameters:

    -   **Select Logstore**: Select a Logstore from which data is queried.
    -   **Inherit Filtering Conditions**: If you turn on the switch, the filtering condition on the dashboard is inherited.
    -   Query statement: Enter a query statement and set a time range.
    -   **Dynamic List Item Preview**: Preview query results. |


## Example 1: Use different time granularities to analyze logs

After you [collect NGINX access logs](/intl.en-US/Index and query/Best practices/Collect and analyze NGINX access logs.md), you need to query these logs in real time. You can use a query statement to view the number of page views \(PVs\) per minute. However, if you need to view the number of PVs per second, you must modify the value of \_\_time\_\_ - \_\_time\_\_ % 60 in the query statement. To simplify this operation, you can use a filter to replace variables in the query statement.

1.  Add an analysis chart to a dashboard.

    Before you add a filter to a dashboard, you must add an analysis chart to the dashboard. For more information, see [Add an analysis chart to a dashboard](/intl.en-US/Visualization/Add an analysis chart to a dashboard.md).

    -   Set the query statement.

        Execute the following query statement to view the PVs per minute:

        ```
        * | SELECT date_format(__time__ - __time__ % 60, '%H:%i:%s') as time, count(1) as count GROUP BY time ORDER BY time
        ```

    -   Set a placeholder variable.

        Select 60 in the query statement. Click Generate Variable to generate a variable named interval.

        ![Set a placeholder variable.](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4923359951/p13757.png)

2.  Add a filter.

    Set the Type, Key, and Static List Items parameters based on the following settings. For information about how to set other parameters, see [Procedure](#section_gdp_tpc_mfb).

    -   **Type**: Select Replace Variable.
    -   **Key**: Enter interval.
    -   **Static List Items**: Add 1 and 120 as values of the key. The default unit is seconds.
3.  In the filter, select 1 from the Interval drop-down list, and click **Search**.

    The following statement shows the query statement after the placeholder variable is replaced:

    ```
    * | SELECT date_format(__time__ - __time__ % 1, '%H:%i:%s') as time, count(1) as count GROUP BY time ORDER BY time 
    ```

    ![Filter](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4923359951/p13759.png)


## Example 2: Switch between request methods

After you [collect NGINX access logs](/intl.en-US/Index and query/Best practices/Collect and analyze NGINX access logs.md), you can add dynamic values to a filter to dynamically switch between request methods.

**Note:** You must configure an index for the method field.

1.  Add an analysis chart to a dashboard.

    Before you add a filter to a dashboard, you must add an analysis chart to the dashboard. For more information, see [Add an analysis chart to a dashboard](/intl.en-US/Visualization/Add an analysis chart to a dashboard.md).

    Execute the following statement to view the PVs per minute. The query statement starts with an asterisk \(`*`\), which means no condition is set to filter the query results. You can add another filter to view the PV data of different request methods.

    ```
    * | SELECT date_format(__time__ - __time__ % 60, '%H:%i:%s') as time, count(1) as count GROUP BY time ORDER BY time
    ```

2.  Add a filter.

    Set the Type, Key, Select Logstore, and Add Dynamic List Item parameters based on the following settings. For information about how to set other parameters, see [Procedure](#section_gdp_tpc_mfb).

    -   **Type**: Select Filter.
    -   **Key**: Enter method.
    -   **Add Dynamic List Item**: Turn on the switch.
    -   **Select Logstore**: Select the Logstore to which the dashboard belongs.
    -   Enter the query statement \* \|select distinct method in the search box.
3.  In the filter, select **GET** from the method drop-down list, and click Search.

    The PV data whose method is GET is displayed in the chart. The following statement is the new query statement:

    ```
    (*) and (method: GET) | SELECT date_format(__time__ - __time__ % 60, '%H:%i:%s') as time, count(1) as count GROUP BY time ORDER BY time 
    ```

    ![Dynamic values](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5923359951/p13761.png)


