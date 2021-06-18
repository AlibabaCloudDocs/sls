# Markdown chart

This topic describes the common syntax of Markdown charts and how to create a Markdown chart on a dashboard.

-   Log data is collected. For more information, see [Data collection](/intl.en-US/Data Collection/Log collection methods.md).
-   A dashboard is created. For more information, see [Create a dashboard](/intl.en-US/Visualization/Create a dashboard.md).

You can add multiple analysis charts to a dashboard. This way, you can view multiple analysis results and monitor the status of multiple applications on a single dashboard. You can also add Markdown charts to a dashboard. A Markdown chart is edited by using the Markdown syntax.

You can create different Markdown charts based on your business requirements. Markdown charts can make a dashboard more intuitive. You can insert text such as background information, chart description, notes, extension information, and custom images in a Markdown chart. You can also insert saved searches or dashboard links of other projects to redirect to other query pages.

You can insert links in a Markdown chart to redirect to other dashboard pages of the current project. You can also insert an image that corresponds to each link. In addition, you can use a Markdown chart to describe the parameters of analysis charts.

![Display effect](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0872966951/p7247.png)

## Add a Markdown chart

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the project in which you want to query and analyze logs.

3.  In the left-side navigation pane, click the ![Create an alert to monitor a chart - 001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p104975.png) icon.

4.  In the dashboard list that appears, click the target dashboard.

5.  In the upper-right corner of the dashboard page that appears, click **Edit**.

6.  In the edit mode, drag the ![markdown](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5923359951/p36999.png) icon from the menu bar and drop the icon on a specified location to create a Markdown chart.

7.  Double-click the Markdown chart.

8.  In the Markdown Edit dialog box, set the parameters, and then click **OK**.

    ![Create a markdown chart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1872966951/p32307.png)

    |Parameter|Description|
    |:--------|:----------|
    |Chart Name|The name of the Markdown chart.|
    |Show Border|Specifies whether to show the borders of the Markdown chart. You can turn on the **Show Border** switch to show the borders of the Markdown chart.|
    |Show Title|Specifies whether to show the title of the Markdown chart. You can turn on the **Show Title** switch to show the title of the Markdown chart.|
    |Show Background|Specifies whether to show the background of the Markdown chart. You can turn on the **Show Background** switch to show the background of the Markdown chart.|
    |Query Binding|Specifies whether to associate a query statement with a Markdown chart. You can turn on the **Query Binding** switch and associate a query statement with a Markdown chart. Then, dynamic query results are displayed in the Markdown chart.     1.  Select a Logstore from which data is to be queried.
    2.  Enter a query statement in the search box, set the time range, and then click **Search**.

For more information, see [Log search](/intl.en-US/Index and query/Log search.md).

**Note:** The query results may contain logs that are generated 1 minute earlier or later than the specified time period.

The first returned log entry is displayed.

    3.  Click the plus sign next to a field to insert the corresponding query result to the **Markdown Content** column. |
    |**Markdown Content**|Enter Markdown content in the **Markdown content** column on the left. Data preview is displayed in real time in the **Show Chart** column on the right. You can modify the Markdown content based on the data preview. For more information about the Markdown syntax, see [Common Markdown syntax](#section_zhh_y2w_m2b).|

9.  Click **Save**.


## Modify a Markdown chart

1.  In the upper-right corner of the dashboard page, click **Edit**.

2.  Modify the location and size of a Markdown chart

    Drag the Markdown icon and drop the icon on a specified location on the dashboard and drag the lower-right corner of the chart to adjust its size.

3.  Modify the properties of a Markdown chart

    1.  Double-click the target Markdown chart.

    2.  In the Markdown Edit dialog box, modify the parameters, and then click **OK**.

        You can modify the chart name, display settings, query settings, and Markdown content. For more information, see [Add a Markdown chart](#section_wg3_1tv_m2b).


## Delete a chart

1.  In the upper-right corner of the dashboard page, click **Edit**.

2.  Find the target Markdown chart, and then choose **![Create an alert to monitor a chart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p104976.png)** \> **Delete**.

3.  In the upper-right corner of the dashboard page, click **Save**.


## Common Markdown syntax

-   Title
    -   Markdown syntax

        ```
        # Level 1 heading
        ## Level 2 heading
        ### Level 3 heading
        ```

    -   Result

        ![Title](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1872966951/p7249.png)

-   Link
    -   Markdown syntax

        ```
        ### Contents
        
        [Chart description](https://www.alibabacloud.com/help/doc-detail/69313.htm)
        
        [Dashboard](https://www.alibabacloud.com/help/doc-detail/59324.htm)
        ```

    -   Result

        ![Link](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5923359951/p7250.png)

-   Image
    -   Markdown syntax

        ```
        <div align=center>
        
        ![ Alt txt][id]
        
        With a reference later in the document defining the URL location
        
        [id]: https://octodex.github.com/images/dojocat.jpg  "The Dojocat"
        ```

    -   Preview

        ![Image](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1872966951/p7251.png)

-   Special tag
    -   Markdown syntax

        ```
        ---
        
        __Advertisement :)__
        
        ==some mark==   `some code`
        > Classic markup: :wink: :crush: :cry: :tear: :laughing: :yum:
        >> Shortcuts (emoticons): :-)  8-) ;)
        
        __This is bold text__
        
        *This is italic text*
        
        ---
        ```

    -   Result

        ![Special tag](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1872966951/p7252.png)


