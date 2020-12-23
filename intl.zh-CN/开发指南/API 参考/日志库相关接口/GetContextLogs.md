# GetContextLogs

调用GetContextLogs接口查询指定日志前（上文）后（下文）的若干条日志。

## 接口说明

上下文查询的时间范围为起始日志的前后一天。

## 请求语法

```
GET /logstores/logstoreName?type=context_log&pack_id=pack\_id&pack_meta=pack\_meta&back_lines=back\_lines&forward_lines=forward\_lines HTTP/1.1
Authorization: LOG yourAccessKeyId:yourSignature
Date: GMT Date
Host: ProjectName.Endpoint
x-log-bodyrawsize: 0
x-log-apiversion: 0.6.0
x-log-signaturemethod: hmac-sha1
```

其中，Host由Project名称和日志服务Endpoint构成，您需要在Host中指定Project。

## 请求参数

-   请求头

    GetContextLogs接口无特有请求头。关于Log Service API的公共请求头，请参见[公共请求头](/intl.zh-CN/开发指南/API 参考/公共请求头.md)。

-   参数列表

    |参数名称|数据类型|是否必填|示例值|描述|
    |----|----|----|---|--|
    |projectName|String|是|big-game|Project名称。|
    |logstoreName|String|是|app\_log|Logstore名称。|
    |type|String|是|context\_log|Logstore中数据的类型。该接口中该参数固定为context\_log。|
    |pack\_id|String|是|id|起始日志所属的LogGroup的唯一身份标识。|
    |pack\_meta|String|是|meta|起始日志在对应LogGroup内的唯一上下文结构标识。|
    |back\_lines|Integer|是|100|指定起始日志往前（上文）的日志条数，取值范围为\(0,100\]。|
    |forward\_lines|Integer|是|100|指定起始日志往后（下文）的日志条数，取值范围为\(0,100\]。|


## 返回数据

-   响应头

    GetContextLogs接口无特有响应头。关于Log Service API的公共响应头，请参见[公共响应头](/intl.zh-CN/开发指南/API 参考/公共响应头.md)。

-   响应元素

    返回HTTP状态码200，则表示请求成功，其响应Body包括查询到的日志（可能为空），具体如下：

    |参数名称|数据类型|示例值|描述|
    |----|----|---|--|
    |total\_lines|Integer|201|返回的总日志条数，包含请求参数中所指定的起始日志。|
    |back\_lines|Integer|100|向前查询到的日志条数。|
    |forward\_lines|Integer|100|向后查询到的日志条数。|
    |progress|String|Complete|查询的结果是否完整。    -   Complete：查询已经完成，返回结果为完整结果。
    -   Incomplete：查询已经完成，返回结果为不完整结果，需要重复请求以获得完整结果。 |
    |logs|Array|无|获取到的日志，按上下文顺序排列。当根据指定起始日志查询不到上下文日志时，此参数为空。|

    logs中的每一项都是该日志的内容（键值对），除用户日志内容外，还包含三个字段，具体如下：

    |参数名称|数据类型|示例值|描述|
    |----|----|---|--|
    |\_\_index\_number\_\_|String|-100|该日志在本次查询结果中相对上下文的位置，负数表示上文，0表示起始日志，正数表示下文。例如：-100表示起始日志往前的第100条日志。|
    |\_\_tag\_\_:\_\_pack\_id\_\_|String|895CEA449A52FE-8c8|该日志所属的LogGroup的唯一身份标识，可作为请求参数中的pack\_id进行查询。|
    |\_\_pack\_meta\_\_|String|0\|MTU1OTI4NTExMjg3NTQ2NDU1OA==\|4\|1|该日志在所属LogGroup内的唯一上下文结构标识，可作为请求参数中的pack\_meta进行查询。|


## 示例

-   请求示例

    ```
    GET /logstores/app_log?type=context_log&&pack_id=id&pack_meta=meta&back_lines=100&forward_lines=100 HTTP/1.1
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

-   正常返回示例

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

-   此处以Java SDK为例，请使用Java SDK 0.6.38及以上的版本。

    如果您通过Logtail采集方式采集日志，该方式将自动为日志附加上下文信息。

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
            String accessKeyId = "yourAccessKeyId"; // 使用您的阿里云访问密钥AccessKey ID。
            String accessKeySecret = "yourSignature"; // 使用您的阿里云访问密钥AccessKey Secret。
            String project = "ProjectName"; // 要查询的Project。
            String logstore = "logstoreName"; // 要查询的Logstore。
            // 构建一个客户端实例。
            Client client = new Client(endpoint, accessKeyId, accessKeySecret);
    
            System.out.println("请确保指定的logstore已开启索引。");
            Thread.sleep(3000);
    
            // 使用GetLogs并在查询语句中加上|with_pack_meta来获取起始日志的pack_id和pack_meta。
            // 查询时间范围：最近15分钟。
            // 起始日志：返回结果的第一条。
            String query = "*|with_pack_meta";
            GetLogsResponse response = client.GetLogs(project, logstore,
                    (int) getCurrentTimestamp() - 900, (int) getCurrentTimestamp(),"", query);
            ArrayList<QueriedLog> logs = response.GetLogs();
            if (logs.isEmpty()) {
                System.out.println("未查询到任何日志。");
                System.exit(1);
            }
    
            // 提取第一条日志的pack信息。
            PackInfo info = extractPackInfo(logs.get(0));
            if (info.packMeta.isEmpty() || info.packID.isEmpty()) {
                System.out.println("pack ID: " + info.packID + ", pack meta: " + info.packMeta);
                System.out.println("起始日志的pack信息不完整，请确保该日志是通过logtail写入。");
                System.exit(1);
            }
    
            // 使用得到的pack信息进行上下文查询（双向查询）。
            GetContextLogsResponse contextRes = client.getContextLogs(project, logstore,
                    info.packID, info.packMeta, 10, 10);
            System.out.println("双向查询");
            System.out.println("pack ID: " + info.packID + ", pack meta: " + info.packMeta);
            System.out.println("is complete: " + contextRes.isCompleted());
            System.out.println("total lines: " + contextRes.getTotalLines());
            System.out.println("back lines: " + contextRes.getBackLines());
            System.out.println("forward lines: " + contextRes.getForwardLines());
            Thread.sleep(1000);
    
            // 使用查询结果中的第一条日志，向前查询上文（单向），至多三次。
            List<QueriedLog> contextLogs = contextRes.getLogs();
            for (int i = 0; i < 3 && !contextLogs.isEmpty(); i++) {
                QueriedLog log = contextLogs.get(0);
                info = extractPackInfo(log);
                GetContextLogsResponse res = client.getContextLogs(project, logstore,
                        info.packID, info.packMeta, 10, 0);
                System.out.println("向前查询上文");
                System.out.println("pack ID: " + info.packID + ", pack meta: " + info.packMeta);
                System.out.println("is complete: " + res.isCompleted());
                System.out.println("total lines: " + res.getTotalLines());
                System.out.println("back lines: " + res.getBackLines());
                System.out.println("forward lines: " + res.getForwardLines());
                contextLogs = res.getLogs();
    
                Thread.sleep(1000);
            }
    
            // 使用查询结果中的最后一条日志，向后查询下文（单向），至多三次。
            contextLogs = contextRes.getLogs();
            for (int i = 0; i < 3 && !contextLogs.isEmpty(); i++) {
                QueriedLog log = contextLogs.get(contextLogs.size() - 1);
                info = extractPackInfo(log);
                GetContextLogsResponse res = client.getContextLogs(project, logstore,
                        info.packID, info.packMeta, 0, 10);
                System.out.println("向后查询下文");
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


## 错误码

|HTTP状态码|错误码|错误信息|描述|
|-------|---|----|--|
|404|ProjectNotExist|Project ProjectName does not exist.|Project不存在。|
|404|LogStoreNotExist|logstore logstoreName does not exist.|Logstore不存在。|
|400|InvalidParameter|Invalid pack meta/id.|请求参数中的pack\_meta或pack\_id为非法值。|
|400|InvalidParameter|back\_lines or forward\_lines must be postive.|请求参数中的back\_lines或forward\_lines中有非法值，其中至少有一个参数应为正数。|

更多错误码，请参见[通用错误码](/intl.zh-CN/开发指南/API 参考/通用错误码.md)。

