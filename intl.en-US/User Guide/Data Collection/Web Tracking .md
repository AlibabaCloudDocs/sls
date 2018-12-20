# Web Tracking  {#concept_ixm_xl5_vdb .concept}

Log Service supports collecting logs from HTML, H5, iOS, and Android platforms by using Web Tracking, and customizing dimensions and metrics. 

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13028/15381294802604_en-US.png)

As shown in the preceding figure, you can collect user information from various browsers, iOS apps, and Android apps \(apart from [iOS/Android  SDK](../../../../reseller.en-US/SDK Reference/Basic Descriptions /Overview .md) \) by using Web Tracking. For example: 

-   Browsers, operating systems, and resolutions used by users.
-   Browsing behaviors of users, such as the clicking behaviors and purchasing behaviors on the website.
-   The staying time in the app for users and whether the users are active or not. 

**Note:** Using Web Tracking means that this Logstore enables the anonymous write permission of the Internet, and dirty data may be generated.

## Procedure  {#section_kt1_2m5_vdb .section}

## Step 1. Enable Web Tracking {#section_ofw_2m5_vdb .section}

You can enable Web Tracking in the console or by using Java SDK.

-   **Enable Web Tracking in the console**
    1.  On the Logstore List page,  click **Modify** at the right of the Logstore that must enable the Web Tracking function.
    2.  Turn on the Web Tracking switch.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13028/15381294812605_en-US.png)

-   **Enable Web Tracking by using **

    [Java SDK](../../../../reseller.en-US/SDK Reference/Basic Descriptions /Overview .md):

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
              //Enable the Web Tracking function on the created Logstore.
              LogStore logSt = client.GetLogStore(project, logStore). GetLogStore();
              client.UpdateLogStore(project, new LogStore(logStore, logSt.GetTtl(), logSt.GetShardCount(), true));
              //Disable the Web Tracking function.
              //client.UpdateLogStore(project, new LogStore(logStore, logSt.GetTtl(), logSt.GetShardCount(), false));
              //Create a Logstore that supports the Web Tracking function.
              //client.UpdateLogStore(project, new LogStore(logStore, 1, 1, true));
          }
          catch (LogException e){
              e.printStackTrace();
          }
      }
    }
    ```


## Step 2. Collect logs {#section_ktl_vm5_vdb .section}

After the Web Tracking function is enabled for Logstore, you can use any of the following three methods to upload data to the Logstore.

-   **Use HTTP  GET request **

    ```
    curl --request GET 'http://${project}.${host}/logstores/${logstore}/track?APIVersion=0.6.0&key1=val1&key2=val2'
    ```

    The parameter meanings are as follows.

    |Field|Meaning|
    |:----|:------|
    |`${project}`|The name of the project created in Log Service.|
    |`${host}`|The domain name of the region where your Log Service is located.|
    |`${logstore}`|The name of the Logstore with the Web Tracking function enabled under `${project}` .|
    |`APIVersion=0.6.0`|The reserved field, which is required.|
    |`__topic__=yourtopic `|Specify the log topic, reserved fields \(optional\).|
    |`key1=val1`, `key2=val2`|The key-value pairs to be uploaded to Log Service. Multiple key-value pairs are supported, but you must make sure that the URL length is less than 16 KB.|

-   **Use HTML img tag**

    ```
    <img src='http://${project}.${host}/logstores/${logstore}/track.gif?APIVersion=0.6.0&key1=val1&key2=val2'/>
    <img src='http://${project}.${host}/logstores/${logstore}/track_ua.gif?APIVersion=0.6.0&key1=val1&key2=val2'/>
    ```

    The parameter meanings are the same as those in Use HTTP GET request. 

-   **Use JS SDK **
    1.  Copy loghub-tracking.js    to the web directory, and introduce the following script on the page: 

        [Click to download.](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/31752/cn_zh/1462870126706/loghub-tracking.js?file=loghub-tracking.js)

        ```
        <script type="text/javascript" src="loghub-tracking.js" async></script>
        ```

        **Note:** To keep page loading running, the script sends HTTP requests asynchronously. If data must be sent several times in the page loading process, the subsequent request overwrites the preceding  HTTP request, and the browser shows the  tracking request exits.  Sending requests synchronously can help to avoid this problem. To send requests synchronously, replace the statement in the script.

        Original script:

        ```
        this.httpRequest_.open("GET", url, true)
        ```

        Replace the last parameter to send requests synchronously:

        ```
        this.httpRequest_.open("GET", url, false)
        ```

    2.  Create a Tracker object.

        ```
        var logger = new window.Tracker('${host}','${project}','${logstore}'); 
        logger.push('customer', 'zhangsan'); 
        logger.push('product', 'iphone 6s'); 
        logger.push('price', 5500); 
        logger.logger(); 
        logger.push('customer', 'lisi'); 
        logger.push('product', 'ipod'); 
        logger.push('price', 3000); 
        logger.logger(); 
        ```

        The parameter meaning are as follows:

        |Field |Meaning |
        |------|--------|
        |`${host} `|The domain name of the region where your Log Service is located.|
        |`${project} `|The name of the project created in Log Service.|
        |`${logstore} `|The name of the Logstore with the Web Tracking function enabled under `${project}`.|

        After running the preceding commands, you can see the following two logs in Log Service:

        ```
        customer:zhangsan
        product:iphone 6s
        price:5500
        ```

        ```
        customer:lisi
        product:ipod
        price:3000 
        ```


After data is uploaded to Log Service, you can use Log Service to[ship](reseller.en-US/User Guide/Index and query/Overview.md) data to Object Storage Service \(OSS\).  You can also use the Consumer Library provided by Log Service to consume data.

