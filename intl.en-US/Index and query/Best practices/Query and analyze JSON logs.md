# Query and analyze JSON logs

This topic describes how to query and analyze JSON logs in the Log Service console.

JSON logs are collected. For more information, see [Collect logs in simple mode](/intl.en-US/Data Collection/Logtail collection/Text logs/Collect logs in simple mode.md).

## Usage notes

When you query and analyze JSON logs, make sure that the following requirements are met:

-   The format of a query statement in Log Service is `Search statement | Analytic statement`. In an analytic statement, you must use double quotation marks \(""\) to enclose a field name and use single quotation marks \(''\) to enclose a string.
-   You must add all parent paths to a specified field in the KEY1.KEY2.KEY3 format, for example, concent.request.request\_length.
-   Log Service allows you to query and analyze leaf nodes in JSON objects. However, you cannot query or analyze child nodes that contain leaf nodes.
-   You cannot query or analyze fields whose values are JSON arrays. In addition, you cannot query or analyze the fields in a JSON array.

## Step 1: Configure indexes

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the project in which you want to query and analyze logs.

3.  Choose **Log Storage** \> **Logstores**. On the Logstores tab, click the Logstore where logs are stored.

4.  On the Search & Analysis page of the Logstore, choose **Index Attributes** \> **Attributes**.

    If the indexing feature is not enabled, click **Enable**.

5.  Configure field indexes.

    You can configure indexes one by one. You can also click **Automatic Index Generation**. Then, Log Service automatically configures indexes based on the first log entry in the Preview Data section.

    **Note:**

    -   If you want to use the analysis feature, you must turn on the Enable Analytics switch for the related fields when you configure indexes. For more information, see [Configure indexes](/intl.en-US/Index and query/Configure indexes.md).
    -   By default, indexes are automatically configured for some reserved fields in Log Service. For more information, see [Reserved fields](/intl.en-US/Product Introduction/Limits/Reserved fields.md).
    -   Log Service allows you to configure indexes for leaf nodes in JSON objects. However, you cannot configure indexes for child nodes that contain leaf nodes. For example, you can create an index for the request\_time field, but you cannot create an index for the time field.
    -   You cannot configure indexes for fields whose values are JSON arrays. In addition, you cannot configure indexes for the fields in a JSON array. For example, the value of the body\_bytes\_sent field is a JSON array. In this case, you cannot create an index for the field.
    -   When you configure indexes for the fields in JSON objects, you must add the related parent path in the KEY1.KEY2 format. Example: time.request\_time.
    ![Configure indexes](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6146998161/p211906.png)

6.  Click **OK**.

    **Note:** The indexing feature is applicable only to the log data that is written to the current Logstore after you configure indexes. If you want to query historical data, you can use the reindexing feature. For more information, see [Reindex logs for a Logstore](/intl.en-US/Index and query/Query/Reindex logs for a Logstore.md).


## Step 2: Query logs

On the Search & Analysis page of the Logstore, enter a search statement in the search box and select a time range. Then, click **Search & Analyze**.

-   Query the log entries whose response status code is 200.

    ```
    content.status:200
    ```

-   Query the log entries whose request length is greater than 70.

    ```
    content.request.request_length > 70
    ```

-   Query the log entries that contain GET requests.

    ```
    content.request.request_method:GET
    ```


## Step 3: Analyze logs

On the Search & Analysis page of the Logstore, enter a query statement in the search box and select a time range. Then, click **Search & Analyze**.

-   Calculate the number of log entries for each request status.

    ```
    * | SELECT "content.status", COUNT(*) AS PV GROUP BY "content.status"
    ```

    ![PV](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6146998161/p232778.png)

-   Calculate the number of requests for each request duration and sort analysis results by request duration in ascending order.

    ```
    * | SELECT "content.time.request_time", COUNT(*) AS count GROUP BY "content.time.request_time" ORDER BY "content.time.request_time"
    ```

    ![Request duration](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6146998161/p232779.png)

-   Calculate the average request duration for each request method.

    ```
    * | SELECT avg("content.time.request_time") AS avg_time,"content.request.request_method"  GROUP BY "content.request.request_method"
    ```

    ![Average request duration](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6146998161/p232780.png)


## Sample logs

The following figure shows sample JSON logs.

![Sample logs](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4293938161/p211913.png)

