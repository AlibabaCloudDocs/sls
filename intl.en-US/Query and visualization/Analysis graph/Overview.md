# Overview

All query results that match specified query statements can be rendered as visualized charts.

## Prerequisites

Indexes are configured and related analytics switches are turned on. To do this, click **Index Attributes**. In the Search & Analysis dialog box, turn on the **Enable Analytics** switch for a field. For more information, see [Enable and configure the index feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).

**Note:**

-   Before you display query results on charts, you must be familiar with [Real-time log analysis](/intl.en-US/Index and query/Real-time log analysis.md).
-   If you do not include an analytic statement in a query statement, the query results cannot be displayed.

## Precautions

When multiple query statements are executed in sequence, the **Value Column**, **X Axis**, or **Y Axis** information cannot automatically change based on the current query statement. The X-axis and Y-axis information may remain the same as that of the last query statement. In this case, the query results of the current query statement cannot be automatically displayed on a chart. If the following messages are returned, you must configure the properties of a chart on the **Properties** tab based on the current query statement:

-   The selected dimensions are not in the query results. Check and configure the attributes.
-   X-Axis or Y-Axis is unavailable. Check and configure the attributes.

-   [Display query results in a table](/intl.en-US/Query and visualization/Analysis graph/Display query results in a table.md)
-   [Display query results on a line chart](/intl.en-US/Query and visualization/Analysis graph/Display query results on a line chart.md)
-   [Display query results on a column chart](/intl.en-US/Query and visualization/Analysis graph/Display query results on a column chart.md)
-   [Display query results on a bar chart](/intl.en-US/Query and visualization/Analysis graph/Display query results on a bar chart.md)
-   [Display query results on a pie chart](/intl.en-US/Query and visualization/Analysis graph/Display query results on a pie chart.md)
-   [Display query results on a single value chart](/intl.en-US/Query and visualization/Analysis graph/Individual value plot.md)
-   [Display query results on an area chart](/intl.en-US/Query and visualization/Analysis graph/Display query results on an area chart.md)
-   [Display query results on a map](/intl.en-US/Query and visualization/Analysis graph/Display query results on a map.md)
-   [Display query results on a flow chart](/intl.en-US/Query and visualization/Analysis graph/Display query results on a flow chart.md)
-   [Display query results in a sankey diagram](/intl.en-US/Query and visualization/Analysis graph/Sankey diagram.md)
-   [Display query results on a word cloud](/intl.en-US/Query and visualization/Analysis graph/Display query results on a word cloud.md)

## Chart configurations

On the Graph tab, multiple charts are provided to display query results. You can select a type of chart from the chart bar based on your business requirements.

-   On the Graph tab, query results are displayed in the **Chart Preview** and **Data Preview** sections. **Chart Preview** shows the query results that are displayed in the specified type of chart. **Data Preview** shows the query results that are displayed in a table.
-   On the Graph tab, you can also configure the following settings:
    -   On the Data Source tab, you can set placeholder variables. For example, you configure the drill-down event of Chart A to redirect to the dashboard where Chart B is located. The placeholder variable that you configured for Chart B is the same as the variable that you click to trigger the drill-down event. Then, the placeholder variable is replaced by the variable that you click to trigger the drill-down event and the query statement of Chart B is executed. For more information, see [Configure a drill-down event for a chart](/intl.en-US/Query and visualization/Dashboard/Configure a drill-down event for a chart.md).

        This feature is applicable to scenarios where you configure drill-down events to redirect to destination dashboards.

    -   On the **Properties** tab, you can configure the properties of a chart to be displayed. You can set the X-axis, left Y-axis and right Y-axis, margins, font size, and other parameters. The properties vary based on different types of chart.

        This feature is applicable to all query scenarios.

    -   On the **Interactive Behavior** tab, you can configure drill-down events for a chart. Then, you can click the variable value in the chart to trigger the specified drill-down event. For more information, see [Configure a drill-down event for a chart](/intl.en-US/Query and visualization/Dashboard/Configure a drill-down event for a chart.md).

        This feature applies when you trigger drill-down events for charts.


