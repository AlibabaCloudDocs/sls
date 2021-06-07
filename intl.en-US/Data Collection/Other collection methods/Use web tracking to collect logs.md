# Use web tracking to collect logs

Log Service allows you to collect logs from the HTML, HTML5, iOS, and Android platforms by using web tracking. Log Service also allows you to customize dimensions and metrics to collect logs. This topic describes how to collect logs by using web tracking.

You can use web tracking to collect user information from browsers, iOS apps, or Android apps. The information includes:

-   Browsers, operating systems, and resolutions that are used by users.
-   User browsing behavior, such as the number of clicks and purchases on a website.
-   The amount of time that users spend on an app and whether users are active users.

## Usage notes

-   After you enable web tracking for a Logstore, the write permissions on the Logstore are granted to anonymous users from the Internet. This may generate dirty data.
-   The HTTP body of each GET request cannot exceed 16 KB.
-   You can call the PutLogs API operation by using the POST method to write a maximum of 3 MB or 4,096 log entries to Log Service. For more information, see [PutLogs](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/PutLogs.md).

## Step 1: Enable web tracking

-   Enable web tracking in the Log Service console.
    1.  Log on to the [Log Service console](https://sls.console.aliyun.com).
    2.  In the Projects section, click the project in which you want to enable web tracking for a Logstore.
    3.  On the **Log Storage** \> **Logstores** tab, find the Logstore and choose **![Icon](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0423359951/p65765.png)** \> **Modify**.
    4.  On the Logstore Attributes page, click **Modify** in the upper-right corner of the page.
    5.  Turn on the **WebTracking** switch and click **Save**.
-   Enable web tracking by using an SDK.

    The following script shows how to enable web tracking by using [Log Service SDK for Java](https://github.com/aliyun/aliyun-log-java-sdk).

    ```
    import com.aliyun.openservices.log.Client;
    import com.aliyun.openservices.log.common.LogStore;
    import com.aliyun.openservices.log.exception.LogException;
    public class WebTracking {
      static private String accessId = "your accesskey id";
      static private String accessKey = "your accesskey";
      static private String project = "your project";
      static private String host = "log service data address";
      static private String logStore = "your logstore";
      static private Client client = new Client(host, accessId, accessKey);
      public static void main(String[] args) {
          try {
              // Enable web tracking for an existing Logstore.
              LogStore logSt = client.GetLogStore(project, logStore).GetLogStore();
              client.UpdateLogStore(project, new LogStore(logStore, logSt.GetTtl(), logSt.GetShardCount(), true));
              // Disable web tracking.
              //client.UpdateLogStore(project, new LogStore(logStore, logSt.GetTtl(), logSt.GetShardCount(), false));
              // Create a Logstore for which you want to enable web tracking.
              //client.UpdateLogStore(project, new LogStore(logStore, 1, 1, true));
          }
          catch (LogException e){
              e.printStackTrace();
          }
      }
    }
    ```


## Step 2: Collect log data

After you enable web tracking for a Logstore, you can upload logs to a Logstore by using the following methods:

-   Use SDK for JavaScript to upload logs.
    1.  Install the dependency.

        ```
        npm install --save js-sls-logger
        ```

    2.  Import the application module.

        ```
        import SlsWebLogger from 'js-sls-logger'
        ```

    3.  Set the opts parameter.

        ```
          const opts = {
            host: 'cn-hangzhou.log.aliyuncs.com',      
            project: 'my_project_name',                 
            logstore: 'my_logstore_name',               
            time: 10, 
            count: 10, 
          }
        ```

        |Parameter|Required|Description|
        |---------|--------|-----------|
        |host|Yes|The endpoint of Log Service in the region where your Log Service project resides. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md). The China \(Hangzhou\) region is used as an example. For other regions, configure the endpoint based on the project name and the region where the project resides.|
        |project|Yes|The name of the project.|
        |logstore|Yes|The name of the Logstore.|
        |time|No|The time interval at which messages are sent. Default value: 10. Unit: seconds.|
        |count|No|The number of sent messages. Default value: 10.|

    4.  Create SlsWebLogger.

        ```
        const logger = new SlsWebLogger(opts)
        ```

    5.  Upload logs.

        ```
        logger.send({ 
            customer: 'zhangsan',
            product: 'iphone 12',
            price: 7998  
        })
        ```

-   Use the GET method to upload logs.

    Run the following command to upload logs. Replace the parameters as needed.

    ```
    curl --request GET 'http://${project}.${host}/logstores/${logstore}/track? APIVersion=0.6.0&key1=val1&key2=val2'
    ```

    |Parameter|Required|Description|
    |:--------|--------|:----------|
    |$\{project\}|Yes|The name of the project.|
    |$\{host\}|Yes|The endpoint of Log Service in the region where your project resides. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).|
    |$\{logstore\}|Yes|The name of the Logstore.|
    |APIVersion=0.6.0|Yes|A reserved parameter.|
    |\_\_topic\_\_=yourtopic|No|The log topic.|
    |key1=val1&key2=val2|Yes|The key-value pairs that you want to upload to Log Service. Make sure that the data size is less than 16 KB.|

-   Use HTML <img\> tags to upload logs.

    ```
    <img src='http://${project}.${host}/logstores/${logstore}/track.gif?APIVersion=0.6.0&key1=val1&key2=val2'/>
    <img src='http://${project}.${host}/logstores/${logstore}/track_ua.gif?APIVersion=0.6.0&key1=val1&key2=val2'/>
    ```

    The track\_ua.gif file contains custom parameters that you want to upload to Log Service. If you use this method to upload logs, Log Service records both the custom parameters and the UserAgent and referer HTTPS headers as log fields.

    **Note:** To collect the referer header, make sure that the URL in the second <img\> tag uses the HTTPS protocol.

-   Use the POST method to upload logs.

    You can send an HTTP POST request to upload a large amount of data. For more information, see [PutWebtracking](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/PutWebtracking.md).


