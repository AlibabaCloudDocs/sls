# Display query results in a table

This topic describes how to configure a table to display query results.

## Background information

Tables are commonly used to sort and display data for quick reference and analysis. All search and analytic results in Log Service can be visualized by using charts. The results are displayed in a table by default.

Each table consists of the following elements:

-   Table header
-   Row
-   Column

where:

-   The number of columns can be specified by using a `SELECT` statement.
-   The number of rows is calculated based on the number of log entries in a specified time range. The default clause is `LIMIT 100`.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Click the target project in the Projects section.

3.  Choose **Log Management** \> **Logstores**, click the management icon of the target Logstore, and then select **![Management icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9484688951/p52166.png)** \> **Search & Analysis**.

4.  Enter a query statement in the search box, select a time range, and then click **Search & Analyze**.

5.  Click ![Table - 001](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6805006951/p93113.png) to display the query results in a table.

6.  On the **Properties** tab, configure the properties of the table.

    |Parameter|Description|
    |---------|-----------|
    |Items per Page|The number of entries to return on each page.|
    |Transpose Rows and Columns|Specifies whether to transpose rows and columns.|
    |Hide Reserved Fields|Specifies whether to hide reserved fields.|
    |Disable Sorting|Specifies whether to disable the sorting feature.|
    |Disable Search|Specifies whether to disable the search feature.|
    |Highlight Settings|Specifies the rules that are used to highlight compliant rows or columns.|
    |Sparkline|The **Sparkline** feature that you can use to add an **area chart**, a **line chart**, or a **column chart** for columns in the table.|


## Examples

You can filter fields in raw log entries. The following figure shows a raw log entry.

![Raw log entry](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9723359951/p5701.png)

1.  To filter the `request_method`, `request_uri`, and `request_time` fields in the latest 10 log entries, execute the following statement:

    ```
    * | SELECT request_method, request_uri, request_time GROUP BY request_method, request_uri, request_time LIMIT 10
    ```

    ![Table - 01](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9723359951/p5702.png)

2.  To query the average of the `request_time` \(the average request time\) field values in a specified time range and round the average to three decimal places, execute the following statement:

    ```
    * | SELECT round(avg(request_time), 3) as average_request
    ```

    ![Table - 01](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0823359951/p5703.png)

3.  To group the values of the `remote_addr` field in a specified time range and sort the values in descending order, execute the following statement:

    ```
    * | SELECT remote_addr, count(*) as count GROUP BY remote_addr ORDER BY count DESC
    ```

    ![Table - 01](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0823359951/p5704.png)


