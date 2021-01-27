# GetContextLogs

Queries the contextual log entries of a specified log entry.

## Description

You can specify a log as the start log. The time range of a contextual query is one day before and after the time at which the start log is generated.

## Request syntax

```
GET /logstores/logstoreName? type=context_log&pack_id=pack\_id&pack_meta=pack\_meta&back_lines=back\_lines&forward_lines=forward\_lines HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature
Date: GMT Date
Host: ProjectName.Endpoint
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

The value of the Host parameter consists of a project name and an endpoint. You must specify a project name for the Host parameter.

## Request parameters

-   Request headers

    The GetContextLogs operation does not have operation-specific request headers. For information about the common request headers of Log Service API operations, see [Common request headers](/intl.en-US/Developer Guide/API Reference/Common request headers.md).

-   Parameters

    |Parameter|Type|Required|Example|Description|
    |---------|----|--------|-------|-----------|
    |projectName|String|Yes|big-game|The name of the project.|
    |logstoreName|String|Yes|app\_log|The name of the Logstore.|
    |type|String|Yes|context\_log|The type of data in the Logstore. This parameter must be set to context\_log in the GetContextLogs operation.|
    |pack\_id|String|Yes|id|The unique ID of the log group to which the start log belongs.|
    |pack\_meta|String|Yes|meta|The unique context identifier of the start log in the log group.|
    |back\_lines|Integer|Yes|100|The number of log entries that are received earlier than the start log. Value range: \(0,100\].|
    |forward\_lines|Integer|Yes|100|The number of log entries that are received later than the start log. Value range: \(0,100\].|


## Response parameters

-   Response headers

    The GetContextLogs operation does not have operation-specific response headers. For information about the common response headers of Log Service API operations, see [Common response headers](/intl.en-US/Developer Guide/API Reference/Common response headers.md).

-   Response elements

    If the request succeeds, the HTTP status code 200 is returned. The response body contains the queried logs. The request body may be empty. The following table describes the parameters of the response body.

    |Parameter|Type|Example|Description|
    |---------|----|-------|-----------|
    |total\_lines|Integer|201|The total number of returned log entries. The logs include the start log that is specified in the request.|
    |back\_lines|Integer|100|The number of log entries that are received earlier than the start log.|
    |forward\_lines|Integer|100|The number of log entries that are received later than the start log.|
    |progress|String|Complete|Indicates whether the query results are complete.    -   If the value is Complete, the query succeeded and the query results are complete.
    -   If the value is Incomplete, the query succeeded but the query results are incomplete. You must repeat the request to obtain complete query results. |
    |logs|Array|None|The retrieved logs in contextual order. This parameter is empty if no contextual logs are found based on the specified start log.|

    Each item in the logs parameter is a key-value pair of a log entry. In addition, the log entry contains the following parameters.

    |Parameter|Type|Example|Description|
    |---------|----|-------|-----------|
    |\_\_index\_number\_\_|String|-100|The relative position of a returned log entry in the query results. A negative value indicates that the log entry is received earlier than the start log. The value 0 indicates that the log entry is the start log. A positive value indicates that the log is received later than the start log. For example, -100 indicates that the log is the 100th log that is received earlier than the start log.|
    |\_\_tag\_\_:\_\_pack\_id\_\_|String|895CEA449A52FE-8c8|The unique ID of the log group to which the log belongs. It can be used as the value of the pack\_id parameter in the request.|
    |\_\_pack\_meta\_\_|String|0\|MTU1OTI4NTExMjg3NTQ2NDU1OA==\|4\|1|The unique context identifier of the log in a log group. It can be used as the value of the pack\_meta parameter in the request.|


## Examples

-   Sample requests

    ```
    GET /logstores/app_log? type=context_log&&pack_id=id&pack_meta=meta&back_lines=100&forward_lines=100 HTTP/1.1
    Header :
    {
    Authorization: LOG yourAccessKeyId:yourSignature
    Date: Wed, 3 Sept. 2014 08:33:46 GMT
    Host: big-game.cn-hangzhou.log.aliyuncs.com
    x-log-bodyrawsize: 0
    x-log-apiversion: 0.4.0
    x-log-signaturemethod: hmac-sha1
    }
    ```

-   Sample success responses

    ```
    {
        "total_lines": 201,
        "back_lines": 100,
        "forward_lines": 100,
        "progress": "Complete", 
        "logs": [
            {
                "__index_number__": "-100",
                "__tag__:__pack_id__": "895CEA449A52FE-8c8",
                "__pack_meta__": "0|MTU1OTI4NTExMjg3NTQ2NDU1OA==|4|1",
                ...
            }
        ]
    }
    ```

-   If you use SDK for Java, you must use SDK for Java 0.6.38 or later.

    If you use Logtail to collect logs, the contextual information is automatically added to logs.

    ```
    package sdksample;
    
    import com.aliyun.openservices.log.Client;
    import com.aliyun.openservices.log.common.LogContent;
    import com.aliyun.openservices.log.common.LogItem;
    import com.aliyun.openservices.log.common.QueriedLog;
    import com.aliyun.openservices.log.common.TagContent;
    import com.aliyun.openservices.log.exception.LogException;
    import com.aliyun.openservices.log.request.PutLogsRequest;
    import com.aliyun.openservices.log.response.GetContextLogsResponse;
    import com.aliyun.openservices.log.response.GetLogsResponse;
    
    import java.util.ArrayList;
    import java.util.Date;
    import java.util.List;
    
    public class GetContextLogsSample {
    
        private static int getCurrentTimestamp() {
            return (int) (new Date().getTime() / 1000);
        }
    
        private static class PackInfo {
            public String packID;
            public String packMeta;
    
            public PackInfo(String id, String meta) {
                this.packID = id;
                this.packMeta = meta;
            }
        }
    
        private static PackInfo extractPackInfo(QueriedLog log) {
            PackInfo ret = new PackInfo("", "");
            ArrayList<LogContent> contents = log.GetLogItem().GetLogContents();
            for (int i = 0; i < contents.size(); ++i) {
                LogContent content = contents.get(i);
                if (content.GetKey().equals("__tag__:__pack_id__")) {
                    ret.packID = content.GetValue();
                } else if (content.GetKey().equals("__pack_meta__")) {
                    ret.packMeta = content.GetValue();
                }
            }
            return ret;
        }
    
        public static void main(String args[]) throws InterruptedException, LogException {
            String endpoint = "EndPoint"; 
            String accessKeyId = "yourAccessKeyId"; // Enter the AccessKey ID of your Alibaba Cloud account.
            String accessKeySecret = "yourSignature"; // Enter the AccessKey secret of your Alibaba Cloud account.
            String project = "ProjectName"; // Specify the name of the project from which you want to query logs.
            String logstore = "logstoreName"; // Specify the name of the Logstore from which you want to query logs.
            // Create a client instance.
            Client client = new Client(endpoint, accessKeyId, accessKeySecret);
    
            System.out.println("Make sure that the indexing feature is enabled for the specified Logstore.") ;
            Thread.sleep(3000);
    
            // Call the GetLogs operation and add | with_pack_meta to the query statement to retrieve the values of the pack_id and pack_meta parameters in the start log.
            // Time range: 15 minutes.
            // Start log: the first log entry in the returned results.
            String query = "*|with_pack_meta";
            GetLogsResponse response = client.GetLogs(project, logstore,
                    (int) getCurrentTimestamp() - 900, (int) getCurrentTimestamp(),"", query);
            ArrayList<QueriedLog> logs = response.GetLogs();
            if (logs.isEmpty()) {
                System.out.println("No logs are found."); ;
                System.exit(1);
            }
    
            // Extract the pack information of the start log.
            PackInfo info = extractPackInfo(logs.get(0));
            if (info.packMeta.isEmpty() || info.packID.isEmpty()) {
                System.out.println("pack ID: " + info.packID + ", pack meta: " + info.packMeta);
                System.out.println("The pack information of the start log is incomplete. Make sure that the start log is written by using Logtail.") ;
                System.exit(1);
            }
    
            // Use the retrieved pack information to perform a contextual query (a two-way query).
            GetContextLogsResponse contextRes = client.getContextLogs(project, logstore,
                    info.packID, info.packMeta, 10, 10);
            System.out.println("Two-way query.");
            System.out.println("pack ID: " + info.packID + ", pack meta: " + info.packMeta);
            System.out.println("is complete: " + contextRes.isCompleted());
            System.out.println("total lines: " + contextRes.getTotalLines());
            System.out.println("back lines: " + contextRes.getBackLines());
            System.out.println("forward lines: " + contextRes.getForwardLines());
            Thread.sleep(1000);
    
            // Use the first log entry in the query results to query earlier log entries (one-way query). You can perform this operation for up to three times.
            List<QueriedLog> contextLogs = contextRes.getLogs();
            for (int i = 0; i < 3 && ! contextLogs.isEmpty(); i++) {
                QueriedLog log = contextLogs.get(0);
                info = extractPackInfo(log);
                GetContextLogsResponse res = client.getContextLogs(project, logstore,
                        info.packID, info.packMeta, 10, 0);
                System.out.println("Query earlier logs.");
                System.out.println("pack ID: " + info.packID + ", pack meta: " + info.packMeta);
                System.out.println("is complete: " + res.isCompleted());
                System.out.println("total lines: " + res.getTotalLines());
                System.out.println("back lines: " + res.getBackLines());
                System.out.println("forward lines: " + res.getForwardLines());
                contextLogs = res.getLogs();
    
                Thread.sleep(1000);
            }
    
            // Use the last log entry in the query results to query subsequent log entries (one-way query). You can perform this operation for up to three times.
            contextLogs = contextRes.getLogs();
            for (int i = 0; i < 3 && ! contextLogs.isEmpty(); i++) {
                QueriedLog log = contextLogs.get(contextLogs.size() - 1);
                info = extractPackInfo(log);
                GetContextLogsResponse res = client.getContextLogs(project, logstore,
                        info.packID, info.packMeta, 0, 10);
                System.out.println("Query subsequent log entries.");
                System.out.println("pack ID: " + info.packID + ", pack meta: " + info.packMeta);
                System.out.println("is complete: " + res.isCompleted());
                System.out.println("total lines: " + res.getTotalLines());
                System.out.println("back lines: " + res.getBackLines());
                System.out.println("forward lines: " + res.getForwardLines());
                contextLogs = res.getLogs();
    
                Thread.sleep(1000);
            }
        }
    }
    ```


## Error codes

|HTTP status code|Error code|Error message|Description|
|----------------|----------|-------------|-----------|
|404|ProjectNotExist|Project ProjectName does not exist.|The error message returned because the specified project does not exist.|
|404|LogStoreNotExist|logstore logstoreName does not exist.|The error message returned because the specified Logstore does not exist.|
|400|InvalidParameter|Invalid pack meta/id.|The error message returned because the value of the pack\_meta or pack\_id parameter in the request is invalid.|
|400|InvalidParameter|back\_lines or forward\_lines must be postive.|The error message returned because the value of the back\_lines or forward\_lines parameter is invalid. You must set one or both of the parameters to positive values.|

For a list of error codes, see [Common error codes](/intl.en-US/Developer Guide/API Reference/Common error codes.md).

