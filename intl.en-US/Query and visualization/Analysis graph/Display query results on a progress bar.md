# Display query results on a progress bar

This topic describes how to configure a progress bar to display query results.

## Background information

The progress bar shows the percentage of the actual value of a field to the maximum value of the field. You can configure the properties of the progress bar to adjust its style and set display rules.

Each progress bar consists of the following elements:

-   Actual value
-   Unit \(optional\)
-   Total value

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the name of the project that you want to manage.

3.  Choose **Log Storage** \> **Logstores**, and then click the Logstore that you want to manage.

4.  Enter a query statement in the search box, specify a time range, and then click **Search & Analyze**.

5.  On the Graph tab, click the ![Progress bar - 001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2447895951/p93120.png) icon.

6.  On the **Properties** tab, configure the properties of the progress bar.

    |Parameter|Description|
    |---------|-----------|
    |Actual Value|The actual value in the chart. By default, data in the first row of the specified column is displayed.|
    |Unit|The unit of the value in the progress bar.|
    |Total Value|The maximum value indicated by the progress bar. Default value: 100. After you set the value, the ratio of the actual value to the maximum value is displayed on the progress bar.|
    |Maximum Value Column|The maximum value in the specified column. If you turn on the **Use Query Results** switch, **Total Value** is replaced by **Maximum Value Column**. Then, you can select the maximum value from the query results for this parameter.|
    |Use Query Results|If you turn on the Use Query Results switch, Total Value is replaced by Maximum Value Column. Then, you can select the maximum value from the query results for this parameter.|
    |Edge Shape|The edge shape of the progress bar.|
    |Vertical Display|Specifies whether to display the progress bar in vertical display mode.|
    |Font Size|The font size of the value in the progress bar.|
    |Thickness|The thickness of the progress bar.|
    |Background Color|The background color of the progress bar.|
    |Font color|The font color of the value in the progress bar.|
    |Default Color|The default color of the progress bar.|
    |Color Display Mode|The display mode of the progress bar. Valid values: Default Color, Gradient, and Display by Rule.|
    |Start Color|The start color of the progress bar. This parameter is available if **Gradient** is selected for **Color Display Mode**.|
    |End Color|The end color of the progress bar. This parameter is available if **Gradient** is selected for **Color Display Mode**.|
    |Display Color|The display color of the progress bar. This parameter is available if **Display by Rule** is selected for **Color Display Mode**. **Note:** The value of **Actual Value** is compared with that of **Threshold** based on the condition specified by **Operator**. If the actual value matches the condition specified by **Operator**, the progress bar is displayed in the color specified by Display Color. Otherwise, the progress bar is displayed in the default color. |
    |Operator|The condition that is used to specify the color of the progress bar. This parameter is available if **Display by Rule** is selected for **Color Display Mode**.|
    |Threshold|The threshold that is used to specify the color of the progress bar. This parameter is available if **Display by Rule** is selected for **Color Display Mode**.|


## Examples

To view the percentage of a metric or the proportion of data, you can execute the following query statement:

```
* | select diff[1],diff[2] from (select compare( pv , 86400) as diff from (select count(1) as pv from log))
```

If you select **Display by Rule** for Color Display Mode, colors are dynamically displayed based on the specified rule. If the conditions of a rule are not matched, the default color is displayed.

![256](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8789916061/p129909.png)

