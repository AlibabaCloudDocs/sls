# Display query and analysis results on multiple pages

If a large amount of data is returned when you query logs, the query results will be displayed at a decreased speed. Log Service allows you to read raw logs by page. This limits the amount of data that is returned each time. This topic describes how to display the query and analysis results on multiple pages.

## Paging methods

Log Service allows you to query and analyze logs. You can use SQL statements to query logs by keyword and analyze the query result. You can also query logs by calling the GetLogstoreLogs API operation. You can query raw logs by keyword, analyze logs by executing SQL statements, and obtain the analysis result. Each query statement contains a search statement and an analytic statement. The search statement and analytic statement use different paging methods.

-   [Search statement](/intl.en-US/Index and query/Query/Search syntax.md): queries raw logs by keyword. To display the query result on multiple pages, you can call the GetLogtoreLogs API operation and specify the offset and lines parameters.
-   [Analytic statement](/intl.en-US/Index and query/Real-time log analysis.md): analyzes logs and obtain the analysis result. To display the analysis result on multiple pages, you can use the LIMIT clause. For more information, see [LIMIT syntax](/intl.en-US/Index and query/Analysis grammar/LIMIT syntax.md).

## Display the query result on multiple pages

You can specify the offset and lines parameters of the GetLogStoreLogs API operation to obtain the paged query result.

-   offset: the line from which logs are read.
-   lines: the number of lines to read for the current request. A maximum of 100 logs can be read at a time. If you set a value greater than 100, only 100 lines are returned.

When you query logs by page, the value of the offset parameter increases until all logs are read. When the value reaches a specific number, zero is returned and the progress is completed. In this case, all data has been read.

-   Sample pseudocode

    ```
    offset = 0 // Read logs from line 0.
    lines = 100 // Read 100 lines at a time.
    query = "status:200" // Only logs whose value of the status field contains 200 are queried.
    while True:
         response = get_logstore_logs(query, offset, lines) // The response to the read request.
         process (response)                                 // Use the custom logic to process the query result.
         If response.get_count() == 0 && response.is_complete()   
             Stop reading logs and exit the current loop.
         else
            offset += 100                        // The value of the offset parameter increases to 100. The next 100 lines will be read.
    ```

-   Python code example

    For more information, see [Log Service SDK for Python](/intl.en-US/Developer Guide/SDK Reference/Log Service SDK for Python.md).

    ```
        endpoint = ''// The endpoint of Log Service. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).
        accessKeyId = ''// The AccessKey ID of your Alibaba Cloud account. For more information, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md). High security risks may arise if you use the AccessKey pair of your Alibaba Cloud account because the account has permissions to call all API operations. We recommend that you create and use a RAM user to call API operations or perform routine O&M.
        accessKey = ''// The AccessKey secret of your Alibaba Cloud account.
        project = ''// The name of the project.
        logstore = ''// The name of the Logstore.
        client = LogClient(endpoint, accessKeyId, accessKey)
        topic = ""
        query = "index"
        From = int(time.time()) - 600
        To = int(time.time())
        log_line = 100
        offset = 0
        while True:
            res4 = Nonefor retry_time in range(0, 3):
                req4 = GetLogsRequest(project, logstore, From, To, topic, query, log_line, offset, False)
                res4 = client.get_logs(req4)
                if res4 isnotNoneand res4.is_completed():
                    break
                time.sleep(1)
            offset += 100if res4.is_completed() && res4.get_count() == 0:
                  break;
            if res4 isnotNone:
                res4. log_print() // Display the execution result.
    ```

-   Java code example

    For more information, see [Log Service SDK for Java](/intl.en-US/Developer Guide/SDK Reference/Log Service SDK for Java.md).

    ```
            int log_offset = 0;
            int log_line = 100; // Read a maximum of 100 lines at a time. If you need to read more than 100 lines at a time, use the offset parameter. The offset and lines parameters are available for keyword-based queries, but unavailable for SQL-based queries. To read more than 100 lines at a time in an SQL-based query, use the LIMIT clause.
            while (true) {
                GetLogsResponse res4 = null;
                // For each log offset, 100 lines are read at a time. If a read operation fails, a maximum of three retries are allowed.
                for (int retry_time = 0; retry_time < 3; retry_time++) {
                    GetLogsRequest req4 = new GetLogsRequest(project, logstore, from, to, topic, query, log_offset,
                            log_line, false);
                    res4 = client.GetLogs(req4);
    
                    if (res4 ! = null && res4.IsCompleted()) {
                        break;
                    }
                    Thread.sleep(200);
                }
                System.out.println("Read log count:" + String.valueOf(res4.GetCount()));
                log_offset += log_line;
                if (res4.IsCompleted() && res4.GetCount() == 0) {
                            break;
                }
    
            }
                        
    ```


## Display the analysis result on multiple pages

You can use the LIMIT clause to display the analysis result on multiple pages. The following example shows the syntax of a LIMIT clause:

```
limit Offset, Line
```

The offset and lines parameters are used:

-   offset: the line from which the result is read.
-   lines: the number of lines to read. The value of the lines parameter does not have an upper limit. However, if you read a large number of lines at a time, the network latency increases, and the processing speed of the client decreases.

Assume that 2,000 lines are returned by the analytic statement `* | selectcount(1) , url group by url`. You can use the following statements to display the result on 4 pages. Each page displays 500 lines.

```
* | selectcount(1) , url  group by url  limit 0, 500
* | selectcount(1) , url  group by url  limit 500, 500
* | selectcount(1) , url  group by url  limit 1000, 500
* | selectcount(1) , url  group by url  limit 1500, 500
```

-   Sample pseudocode

    ```
        offset = 0 // Read logs from line 0.
        lines = 500// Read 500 lines at a time.
        query = "* | select count(1) , url  group by url  limit "whileTrue:
        real_query = query + offset + "," +  lines
        response = get_logstore_logs(real_query) // The response to the read request.
        process (response)                       // Use the custom logic to process the analysis result.
        If response.get_count() == 0   
             Stop reading logs and exit the current loop.
        else
            offset += 500                        // The value of the offset parameter increases to 500. The next 500 lines will be read.
    ```

-   Python code example

    For more information, see [Log Service SDK for Python](/intl.en-US/Developer Guide/SDK Reference/Log Service SDK for Python.md).

    ```
        endpoint = ''// The endpoint of Log Service. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md). 
        accessKeyId = ''// The AccessKey ID of your Alibaba Cloud account. For more information, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md). High security risks may arise if you use the AccessKey pair of your Alibaba Cloud account because the account has permissions to call all API operations. We recommend that you create and use a RAM user to call API operations or perform routine O&M.
        accessKey = ''// The AccessKey secret of your Alibaba Cloud account.
        project = ''// The name of the project.
        logstore = ''// The name of the Logstore.
        client = LogClient(endpoint, accessKeyId, accessKey)
        topic = ""
        origin_query = "* | select count(1) , url  group by url  limit "
        From = int(time.time()) - 600
        To = int(time.time())
        log_line = 100
        offset = 0
        while True:
            res4 = None
            query = origin_query + str(offset) + " , " + str(log_line)
            for retry_time in range(0, 3):
                req4 = GetLogsRequest(project, logstore, From, To, topic, query)
                res4 = client.get_logs(req4)
                if res4 isnotNoneand res4.is_completed():
                    break
                time.sleep(1)
            offset += 100if res4.is_completed() && res4.get_count() == 0:
                  break;
            if res4 isnotNone:
                res4.log_print() // Display the execution result.
    ```

-   Java code example

    For more information, see [Log Service SDK for Java](/intl.en-US/Developer Guide/SDK Reference/Log Service SDK for Java.md).

    ```
            int log_offset = 0;
            int log_line = 500;
            String origin_query = "* | select count(1) , url  group by url  limit "while (true) {
                GetLogsResponse res4 = null;
                // For each log offset, 500 lines are read at a time. If a read operation fails, a maximum of three retries are allowed.
                query = origin_query + log_offset + "," + log_line;
                for (int retry_time = 0; retry_time < 3; retry_time++) {
                    GetLogsRequest req4 = new GetLogsRequest(project, logstore, from, to, topic, query);
                    res4 = client.GetLogs(req4);
    
                    if (res4 ! = null && res4.IsCompleted()) {
                        break;
                    }
                    Thread.sleep(200);
                }
                System.out.println("Read log count:" + String.valueOf(res4.GetCount()));
                log_offset += log_line;
                if (res4.GetCount() == 0) {
                            break;
                }
    
            }
    ```


