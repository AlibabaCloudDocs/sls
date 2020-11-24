# Display query results in a table

This topic describes how to configure a table to display query results.

## Background information

Tables are used to sort and display data for quick reference and analysis. All query results that match specified query statements can be rendered into visualized charts. By default, the query results are displayed in a table.

Each table consists of the following elements:

-   Table header
-   Row
-   Column

where:

-   The number of columns can be specified by using a `SELECT` statement.
-   The number of rows is calculated based on the number of log entries in a specified time range. The default clause is `LIMIT 100`.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the name of the project that you want to manage.

3.  Choose **Log Storage** \> **Logstores**, and then click the Logstore that you want to manage.

4.  Enter a query statement in the search box, specify a time range, and then click **Search & Analyze**.

5.  On the Graph tab, click the ![Table - 001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6805006951/p93113.png) icon.

6.  On the **Properties** tab, configure the properties of the table.

    |Parameter|Description|
    |---------|-----------|
    |Items per Page|The number of entries to return on each page.|
    |Transpose Rows and Columns|Specifies whether to transpose rows and columns.|
    |Hide Field|The field to be hidden. If you select a field, the field is not displayed in the table.|
    |Hide All Reserved fields|Specifies whether to hide all reserved fields of Log Service. For information about reserved fields, see [Reserved fields](/intl.en-US/Product Introduction/Limits/Reserved fields.md).|
    |AutoFit Column Width|Specifies whether you can manually adjust the column width in the corresponding dashboard.|
    |Metric Filter|If you turn on the Metric Filter switch, you can filter metrics to view specific types of data.|
    |Disable Sorting|Specifies whether to disable the sorting feature.|
    |Disable Search|Specifies whether to disable the search feature.|
    |Format|The format in which data is displayed.|
    |Font Size|The font size of the value. You can drag the slider to adjust the font size. Valid values: 12 to 24. Unit: pixels.|
    |Highlight Settings|If you turn on the Highlight Settings switch, you can create rules to highlight matched rows or columns.|
    |Sparkline|If you turn on the **Sparkline** switch, you can add an area chart, a line chart, or a column chart for columns in the table.|


## Examples

You can filter fields in raw log entries. The following figure shows a raw log entry.

![Raw logs](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9723359951/p5701.png)

-   To filter the request\_method, request\_uri, and request\_time fields in the latest 10 log entries, execute the following query statement:

    ```
    * | SELECT request_method, request_uri, request_time GROUP BY request_method, request_uri, request_time LIMIT 10
    ```

    ![Table - 01](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9723359951/p5702.png)

-   To query the average of the request\_time field values \(the average request time\) in a specified time range and round the average to three decimal places, execute the following query statement:

    ```
    * | SELECT round(avg(request_time), 3) as average_request
    ```

    ![Table - 01](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0823359951/p5703.png)

-   To group the values of the remote\_addr field in a specified time range and sort the values in descending order, execute the following query statement:

    ```
    * | SELECT remote_addr, count(*) as count GROUP BY remote_addr ORDER BY count DESC
    ```

    ![Table - 01](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0823359951/p5704.png)


