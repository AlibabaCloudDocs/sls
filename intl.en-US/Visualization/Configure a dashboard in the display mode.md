# Configure a dashboard in the display mode

This topic describes how to configure a dashboard in the display mode. By default, you can view all charts on a dashboard in the display mode. You can also perform multiple operations on a dashboard in the display mode.

## Set a query time range for a dashboard of a Logstore

By default, all charts on a dashboard use the query time range that is set for the dashboard. For information about how to set a query time range for a single chart, see [Set charts](#section_wwk_b5k_kgb).

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the project in which you want to query and analyze logs.

3.  In the left-side navigation pane, click the ![Create Alert for Monitoring - 001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p104975.png) icon.

4.  In the dashboard list that appears, click the target dashboard.

5.  Click **Time Range**. In the Time dialog box, set a time range for the dashboard.

    You can set one of the following time ranges:

    **Note:** The time range that you set in the display mode is for temporary use. The next time you open the dashboard, the system will display the analysis results in the default query time range.

    -   Relative: queries log data that is obtained in a time range of 1 minute, 5 minutes, 15 minutes, or other time ranges that end with the current time, accurate to the second. For example, if the current time is 19:20:31 and you select 1Hour as the relative time, the charts on the dashboard display the analysis results of the log data that is obtained from 18:20:31 to 19:20:31.
    -   Time Frame: If you select or customize a time range less than one hour \(for example, 1 minute, 5 minutes, and 15 minutes\), log data obtained in the time range that ends with the current time is queried, accurate to the minute. If you select or customize a time range greater than one hour, log data obtained on the hour before the current time is queried. For example, if the current time is 19:20:31 and you select 1Hour as the time frame, the charts on the dashboard display the analysis results of the log data queried from 18:00:00 to 19:00:00.
    -   Custom: queries log data that is obtained in a specified time range.
    After you set a time range, move the pointer over Time Range, and you can see the time range that you set.


## Enter the edit mode

On the dashboard page, click **Edit** to enter the edit mode. In the edit mode, you can perform multiple operations on the dashboard and the charts on the dashboard. For more information, see [Edit mode](/intl.en-US/Visualization/Configure a dashboard in the edit mode.md).

## Set alerts

On the dashboard page, choose **Alerts** \> **Create** to create an alert. For more information, see [Create an alert rule](/intl.en-US/Alerting/Alerting (Old)/Create an alert rule.md).

## Configure a refresh method

You can manually refresh the dashboard, or set an automatic refresh interval for the dashboard.

-   To manually refresh the dashboard, choose **Refresh** \> **Once**.
-   To set an automatic refresh interval for the dashboard, choose **Refresh** \> **Auto Refresh**, and then select an interval.

    The auto refresh interval can be 15 seconds, 60 seconds, 5 minutes, or 15 minutes.

    **Note:** If your browser is inactive, the automatic refresh interval may be inaccurate.


## Share a dashboard

To share a dashboard with authorized users, click **Share** to copy the link of the dashboard page and then send the link to the users. The shared dashboard page uses the settings of the dashboard at the time of sharing. The settings include the time range of charts and the format of chart titles.

**Note:** Before you share a dashboard with other users, you must grant the relevant permissions to the users.

## Display a dashboard in full screen

In the upper-right corner of the dashboard page, click **Full Screen**. Then the charts on a dashboard are displayed in full screen. This feature is applicable to data display and report scenarios.

## Set the chart title format

In the upper-right corner of the dashboard page, click **Title Configuration**. Available title formats include:

-   Single-line Title and Time Display
-   Title Only
-   Time Only

## Reset the query time range

In the upper-right corner of the dashboard page, click **Reset Time**. The default query time range of all charts on the dashboard are restored. This feature is applicable to scenarios in which you need to restore time settings.

## Set charts

You can select a chart and perform the following operations on the chart:

**Note:** Different types of charts on a dashboard have different shortcut menus. For example, you cannot view the analysis details of a custom chart or Markdown chart because they are not analysis charts.

![Dashboard](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5443866951/p36996.png)

-   View analysis details of a chart

    Find the target chart on the dashboard and choose **![Create an alert to monitor a chart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p104976.png)** \> **View Analysis Details**. On the page that appears, you can view the query statement and properties of the chart.

-   Set a query time range for a chart

    Find the target chart and choose **![Create an alert to monitor a chart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p104976.png)** \> **Select Time Range** to set a query time range for the chart.

-   Set an alert for a chart

    Find the target chart and choose **![Create an alert to monitor a chart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p104976.png)** \> **Create Alert**. In the Create Alert dialog box, create an alert for the chart and set the notification method. For more information, see [Create an alert rule](/intl.en-US/Alerting/Alerting (Old)/Create an alert rule.md).

-   Download chart data

    Find the target chart and choose **![Create an alert to monitor a chart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p104976.png)** \> **Download Chart Data**. The downloaded data is in the CSV format.

-   Download a chart

    Find the target chart and choose **![Create an alert to monitor a chart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p104976.png)** \> **Download Chart**. The downloaded chart is in the PNG format.

-   Check whether a drill-down event is configured for a chart

    Find the target chart and move the pointer over the ![Create an alert to monitor a chart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p104976.png) icon in the upper-right corner of the chart. Check the color of the hand icon at the bottom of the shortcut menu. If the icon is red, a drill-down event is configured for the chart. If the icon is gray, no drill-down event is configured for the chart.

-   Preview the query statement of a chart

    Find the target chart and choose **![Create an alert to monitor a chart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9353749951/p104976.png)** \> **![Preview](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5443866951/p111641.png)**. In the Preview Query Statement dialog box, view the query statement of the chart.


