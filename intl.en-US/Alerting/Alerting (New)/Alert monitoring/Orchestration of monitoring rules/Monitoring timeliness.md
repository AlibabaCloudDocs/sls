# Monitoring timeliness

A specified query statement is executed at the specified check frequency based on the specified time range to query data. The query result is used and calculated as a parameter in the trigger condition. If the calculation result of the trigger condition is true, an alert is triggered. This topic describes the timeliness of alert monitoring.

The timeliness of alert monitoring has the following features:

-   When you create an alert monitoring rule, we recommend that you do not set the time range for a query to the same relative time that is specified for the **Check Frequency** parameter.

    For example, the time range is set to 1 Minute\(Relative\), and the **Check Frequency** parameter is set to Fixed Interval 1 Minutes.

    In this example, the **Check Frequency** parameter is set to Fixed Interval 1 Minutes. After data is written to Log Service, latency exists before the data can be queried. Even if the latency is low, you may still fail to query all of the data For example, the alert monitoring rule is executed at 12:03:30 and the time range is set to 1 Minute\(Relative\), this indicates that the time range is \[12:02:30, 12:03:30\). Logs that are written to Log Service at 12:03:29 may not be queried at 12:03:30.

    -   If you have high requirements on alert accuracy, such as no duplicate alerts and no missing alerts, you can set the time range to an earlier period. You can set a new time range whose start time and end time are 10 to 70 seconds earlier than the original ones. For example, the alert monitoring rule is executed at 12:03:30. In this case, you can set the time range to \[12:02:20,12:03:20\) that is 10 seconds earlier. This prevents missing alerts that are caused by indexing latency.
    -   If you have high requirements on real-time performance, you can set the time range to an earlier period. For example, you want to receive alert notifications at once alerts are triggered regardless of duplicate alerts. In this case, you can set a new time range whose start time is 0 to 70 seconds earlier than the original one. For example, the alert monitoring rule is executed at 12:03:30. You can set the time range to 70Seconds\(Relative\), this indicates that the time range is \[12:02:20,12:03:30\).
-   When the logs that are generated at different points in time within the same minute are written at the same time, an issue may occur. In this case, the indexes of the later logs may be written to the point in time of the earlier logs. This issue occurs due to the method that Log Service uses to build indexes.

    For example, the alert monitoring rule is executed at 12:03:30 and the time range is set to 1 Minute\(Relative\), this indicates that the time range is \[12:02:30, 12:03:30\). Multiple log entries are written at 12:02:50 and these log entries are generated at different points in time within the same minute, for example, 12:02:20 and 12:02:50. In this case, the indexes of these log entries may be written to the point in time 12:02:20. However, this may result in an issue that no log entries can be queried in the time range \[12:02:30,12:03:30\).

    -   If you have high requirements on alert accuracy, such as no duplicate alerts and no missing alerts, you can set the time range to a time frame. You can set the time range to 1 Minute\(Time Frame\), 5 Minutes\(Time Frame\), or 1 Hour\(Time Frame\). Then, you can set the **Check Frequency** parameter to the same time period, for example, 1 Minutes, 5 Minutes, or 1 Hours.
    -   If you have high requirements on real-time performance, we recommend that you set the time range to a specific period. The specific period includes at least the 1 minute that is earlier than the current time. This allows you to receive alert notifications at once alerts are triggered regardless of duplicate alerts. For example, the alert monitoring rule is executed at 12:03:30. If you set the time range to 90Minutes\(Relative\), this indicates that the time range is \[12:02:00,12:03:30\). Then, you can set the **Check Frequency** parameter to 1 Minutes.

