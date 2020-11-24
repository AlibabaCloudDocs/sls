# Display query results on a single value chart

This topic describes how to configure a single value chart to display query results.

## Background information

The single value chart highlights a single value. Single value charts include the following types:

-   Rectangle Frame: shows a general value.
-   Dial: shows the difference between the current value and the specified threshold value.
-   Compare Numb Chart: shows the SQL query results of interval-valued comparison and periodicity-valued comparison functions. For information about the analytic syntax, see [Interval-valued comparison and periodicity-valued comparison function](/intl.en-US/Index and query/Analysis grammar/Interval-valued comparison and periodicity-valued comparison function.md).

Rectangle Frame is selected by default. A rectangle frame is the most basic method to display data at a specified point. In most cases, it is used to show the key information at a specified time point. To display a proportional metric, you can select Dial.

Each single value chart consists of the following elements:

-   Numeric value
-   Unit \(optional\)
-   Description \(optional\)
-   Chart type

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the name of the project that you want to manage.

3.  Choose **Log Storage** \> **Logstores**, and then click the Logstore that you want to manage.

4.  Enter a query statement in the search box, specify a time range, and then click **Search & Analyze**.

5.  On the Graph tab, click the ![Single value chart - 001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0207895951/p93119.png) icon.

6.  On the **Properties** tab, configure the properties of the single value chart.

    **Note:** Log Service normalizes data in numeric value-based charts. For example, `230000` is processed as `230K`. You can include [Mathematical calculation functions](/intl.en-US/Index and query/Analysis grammar/Mathematical calculation functions.md) in query statements to customize numeric formats.

    -   The following table lists the parameters of a rectangle frame.

        |Parameter|Description|
        |:--------|:----------|
        |**Chart Types**|The type of the single value chart. If you select **Rectangle Frame** for this parameter, query results are displayed in a rectangle frame.|
        |**Value Column**|The value displayed in the chart. By default, data in the first row of the specified column is displayed.|
        |**Unit**|The unit of data.|
        |**Unit Font Size**|The font size of the unit. You can drag the slider to adjust the font size. Valid values: 10 to 100. Unit: pixels.|
        |**Description**|The description of the value.|
        |**Description Font Size**|The font size of the value description. You can drag the slider to adjust the font size. Valid values: 10 to 100. Unit: pixels.|
        |**Format**|The format in which data is displayed.|
        |**Font Size**|The font size of the value. You can drag the slider to adjust the font size. Valid values: 10 to 100. Unit: pixels.|
        |**Font Color**|The color of the value, unit, and description in the chart. You can use the default color or select a color.|
        |**Background Color**|The color of the background. You can use the default color or select a color.|

    -   The following table lists the parameters of a dial.

        |Parameter|Description|
        |:--------|:----------|
        |**Chart Types**|The type of the single value chart. If you select **Dial** for this parameter, query results are displayed on a dial.|
        |**Actual Value**|The actual value in the chart. By default, data in the first row of the specified column is displayed.|
        |**Unit**|The unit of the value on the dial.|
        |**Font Size**|The font size of the value and unit. Valid values: 10 to 100. Unit: pixels.|
        |**Compare Value**|The field value to be compared. The value of the selected field is displayed on the dial.|
        |**Contrast Value Font Size**|The font size of the compare value.|
        |**Unit of Compared Values**|The unit of the compare value.|
        |**Description**|The description of the value.|
        |**Description Font Size**|The font size of the value description. You can drag the slider to adjust the font size. Valid values: 10 to 100. Unit: pixels.|
        |**Dial Maximum**|The maximum value of the scale on the dial. Default value: 100.|
        |**Maximum Value Column**|The maximum value in the specified column. If you turn on the **Use Query Results** switch, **Dial Maximum** is replaced by **Maximum Value Column**. Then, you can select the maximum value from the query results for this parameter.|
        |**Use Query Results**|If you turn on the Use Query Results switch, Dial Maximum is replaced by Maximum Value Column. Then, you can select the maximum value from query results for this parameter.|
        |**Format**|The format in which data is displayed.|
        |**Colored Regions**|The number of segments to divide the dial. Each segment is displayed in a different color. Valid values: 2, 3, 4, and 5. Default value: 3. |
        |**Region Max Value**|The maximum value of the scale in each colored segment of the dial. By default, the maximum value in the last segment is the maximum value on the dial. You do not need to specify this value. If you turn on the **Use Query Results** switch, **Region Max Value** is replaced by **Region Percentage**.**Note:** A dial is evenly divided into three colored segments by default. If you change the value of **Colored Regions**, Region Max Value is not automatically adjusted. You can manually set the maximum value for each colored segment based on your business requirements. |
        |**Font Color**|The color of the value on the dial.|
        |**Region**|The colored segments to divide the dial. A dial is evenly divided into three segments by default. The segments are displayed in blue, yellow, and red. If you set **Colored Regions** to a value greater than 3, the added segments are displayed in blue by default. You can change the color of each segment. |

    -   The following table lists the parameters of a compare numb chart.

        |Parameter|Description|
        |:--------|:----------|
        |**Chart Types**|The type of the single value chart. If you select **Compare Numb Chart** for this parameter, query results are displayed on a compare numb chart.|
        |**Show Value**|The value that is displayed in the center of the compare numb chart. In most cases, this value is set to the statistical result that is calculated by the related comparison function in the specified time range.|
        |**Compare Value**|The value that is compared with the threshold. In most cases, this value is set to the result of comparison between the statistical results calculated by the related comparison function in the specified time range and in the previously specified time range.|
        |**Font Size**|The font size of the show value.Valid values: 10 to 100. Unit: pixels. You can turn on the **Adaptive** switch to enable the adaptive feature of the font size.|
        |**Unit**|The unit of the show value.|
        |**Unit Font Size**|The font size of the unit for the show value. Valid values: 10 to 100. Unit: pixels.|
        |**Compare Unit**|The unit of the compare value.|
        |**Compare Font Size**|The font size of the compare value and unit. Valid values: 10 to 100. Unit: pixels.|
        |**Description**|The description of the show value and its growth trend. The description is displayed under the value.|
        |**Description Font Size**|The font size of the description. Valid values: 10 to 100. Unit: pixels.|
        |**Trend Comparison Threshold**|The value that is used to measure the variation trend of the compare value. For example, the compare value is -1.

        -   If you set **Trend Comparison Threshold** to 0, a down arrow that indicates a value decrease is displayed on the page.
        -   If you set **Trend Comparison Threshold** to -1, this indicates that the value remains unchanged. The system does not display the trend on the page.
        -   If you set **Trend Comparison Threshold** to -2, an up arrow that indicates a value increase is displayed on the page. |
        |**Show Value Format**|The format in which the show value is displayed.|
        |**Compare Value Format**|The format in which the compare value is displayed.|
        |**Font Color**|The color of the show value and its description.|
        |**Growth Font Color**|The font color of the compare value that is greater than the threshold.|
        |**Growth Background Color**|The background color displayed when the compare value is greater than the threshold.|
        |**Decrease Font Color**|The font color of the compare value that is less than the threshold.|
        |**Decrease Background Color**|The background color displayed when the compare value is less than the threshold.|
        |**Equal Background Color**|The background color displayed when the compare value is equal to the threshold.|


## Examples

To view the number of requests, execute the following query statements:

-   Rectangle frame

    ```
    * | select count(1) as pv
    ```

    ![Rectangle frame](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4823359951/p5729.png)

-   Dial

    ```
    * | select count(1) as pv
    ```

    ![Compare numb chart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5823359951/p9590.png)

-   Compare numb chart

    To view and compare the requests for today and yesterday, execute the following query statement:

    ```
    * | select diff[1],diff[2], diff[1]-diff[2] from (select compare( pv , 86400) as diff from (select count(1) as pv from log))
    ```

    ![Dial](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4823359951/p7726.png)


