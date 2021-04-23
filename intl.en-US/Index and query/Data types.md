# Data types

When you configure indexes, you can set the data type of a field to text, long, double, or JSON. This topic describes the data types of fields and provides some examples.

## Text type

To query and analyze log data of the string type, you must set the data type of the related fields to text when you configure indexes. You must also turn on the Enable Analytics switch for these fields.

**Note:** By default, after you turn on the Full Text Index switch, the data type of all fields except the \_\_time\_\_ field is set to text.

-   Sample log entry

    ![Text log entry](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4361719161/p237753.png)

-   Index configurations

    ![Text index](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4361719161/p237450.png)

-   Query statements
    -   To query the log entries that do not contain GET requests, execute the following search statement:

        ```
        not request_method : GET
        ```

    -   To query the log entries that start with cn, execute the following search statement:

        ```
        cn*
        ```

    -   To collect the statistics on the distribution of clients, execute the following query statement:

        ```
        * | SELECT ip_to_province(client_ip) as province, count(*) AS pv GROUP BY province ORDER BY pv
        ```


## Long and double types

You can query the value of a field by using a numeric range only after you set the data type of the field to long or double.

-   If the value of a log field is an integer, we recommend that you set the data type of the field to long when you configure indexes.
-   If the value of a log field is a floating-point number, we recommend that you set the data type of the field to double when you configure indexes.

**Note:**

-   If you set the data type of a field to long and the actual field value is a floating-point number, the field cannot be queried.
-   If you set the data type of a field to long or double and the actual field value is a string, the field cannot be queried.
-   If you set the data type of a field to long or double, you cannot use asterisks \(\*\) or question marks \(?\) to perform fuzzy searches.
-   If the value of a field is an invalid numeric value, you can query data by using the not key \> -1000000 search statement. This not key \> -1000000 search statement returns the log entries whose value of the field is an invalid numeric value. -100000 can be replaced by a valid value that is smaller or equal to the smallest valid value of the field in your log entries.

-   Sample log entry

    ![Text log entry](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4361719161/p237753.png)

-   Index configurations

    ![Text index](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4361719161/p237450.png)

-   Query statements
    -   To query the log entries whose request duration is greater than 60 seconds, execute the following search statement:

        ```
        request_time > 60
        ```

    -   To query the log entries whose request duration is greater than or equal to 60 seconds and less than 200 seconds, execute one of the following search statements:
        -   ```
request_time in [60 200)
```

        -   ```
request_time >= 60 and request_time < 200
```

    -   To query the log entries whose response status code is 200, execute the following search statement:

        ```
        status = 200
        ```


## JSON type

If the value of a field is in the JSON format, you can set the data type of the field to JSON when you configure indexes.

-   You can set the data type of a field in JSON objects to long, double, or text based on the field value, and turn on the Enable Analytics switch. After you turn on the switch, Log Service allows you to query and analyze fields in JSON objects.
-   For partially valid JSON-formatted data, only the valid parts can be parsed in Log Service.

    The following example shows an incomplete JSON log entry. Log Service can parse the conctent.remote\_addr, content.request.request\_length, and content.request.request\_method fields.

    ```
    content: {
         remote_addr:"192.0.2.0"
         request: {
                 request_length:"73"
                 request_method:"GE
    ```


**Note:**

-   Log Service allows you to configure indexes for leaf nodes in JSON objects. However, you cannot configure indexes for child nodes that contain leaf nodes.
-   You cannot configure indexes for fields whose values are JSON arrays. In addition, you cannot configure indexes for fields in a JSON array.
-   If the value of a field is of the Boolean type, you can set the data type of the field to text when you configure indexes.
-   The format of a query statement is `Search statement | Analytic statement`. In an analytic statement, you must enclose a field name by using double quotation marks \(""\) and enclose a string by using single quotation marks \(''\).

-   Sample log entry

    The following figure provides an example of a JSON log entry. This log entry contains the reserved fields of Log Service. This log entry also contains the class, latency, status, and info fields. The value of the info field is a JSON object that contains multiple layers.

    ![Sample JSON log entry](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4361719161/p237714.png)

-   Index configurations

    ![Index configurations](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4361719161/p5522.png)

    In the preceding figure, you must take note of the following limits:

    -   The values of IP and data fields are JSON arrays. Therefore, you cannot configure indexes for the IP and data fields. You cannot query and analyze data by using these two fields.
    -   The region and CreateTime fields are in a JSON array. Therefore, you cannot configure indexes for the region and CreateTime fields. You cannot query and analyze data by using these two fields.
-   Query statements
    -   To query the log entries whose value of the usedTime field is greater than 60 seconds, execute the following search statement:

        ```
        info.usedTime > 60
        ```

    -   To query the log entries whose value of the success field is true, execute the following search statement:

        ```
        info.success : true
        ```

    -   To query the log entries whose usedTime field value is greater than 60 seconds and the projectName field value is not project01, execute the following search statement:

        ```
        info.usedTime > 60 not info.param.projectName : project01
        ```

    -   To calculate the average duration that is used to obtain the details of the current project, execute the following query statement:

        ```
        methodName = getProjectInfo | SELECT avg("info.usedTime") AS avg_time
        ```


