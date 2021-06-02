# View the import results of trace data

After you import trace data to Log Service, you can query the related logs to check whether trace data is imported to Log Service.

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Open the **Custom Query** page.

    1.  In the Log Application section, click **Trade Service**.

    2.  In the trace instance list, click the specific instance.

    3.  In the left-side navigation pane, click **Custom Query**.

3.  Enter a query statement.

    For example, to check whether trace data that is related to the user service is imported, execute the following query statement:

    ```
    service:user
    ```

4.  View the query result.

    If the query result contains logs of the user service, you have imported the trace data that is related to the user service.

    ![Query results](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6823262261/p254447.png)


