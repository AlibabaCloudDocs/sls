# 使用Web Tracking采集日志

日志服务支持通过Web Tracking采集HTML、H5、iOS和Android平台的日志，并支持自定义维度和指标。本文介绍如何使用Web Tracking采集日志。

您可以通过Web Tracking采集各种浏览器、iOS App或Android App的用户信息，例如：

-   用户使用的浏览器、操作系统、分辨率等信息。
-   用户浏览行为记录（例如：用户在网站上的单击行为、购买行为等信息）。
-   用户在App中的停留时间、是否活跃等信息。

## 注意事项

-   使用Web Tracking则表示该Logstore打开互联网匿名写入权限，没有经过有效鉴权，可能产生脏数据。
-   GET请求不支持上传16 KB以上的Body内容。
-   POST请求每次写入的日志数量上限为3 MB或者4096条。更多信息，请参见[PutLogs](/intl.zh-CN/开发指南/API 参考/日志库相关接口/PutLogs.md)。

## 步骤1：开通Web Tracking

-   控制台方式
    1.  登录[日志服务控制台](https://sls.console.aliyun.com)。
    2.  在Project列表区域，单击目标Project。
    3.  在**日志存储** \> **日志库**页签中，选择目标Logstore右侧的**![图标](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1140559951/p65765.png)** \> **修改**。
    4.  在Logstore属性页面，单击右上方的**修改**。
    5.  打开**WebTracking**开关，并单击**保存**。
-   SDK方式

    通过[Log Service Java SDK](https://github.com/aliyun/aliyun-log-java-sdk)开通Web Tracking。

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
              //在已经创建的Logstore上开通Web Tracking功能。
              LogStore logSt = client.GetLogStore(project, logStore).GetLogStore();
              client.UpdateLogStore(project, new LogStore(logStore, logSt.GetTtl(), logSt.GetShardCount(), true));
              //关闭Web Tracking功能。
              //client.UpdateLogStore(project, new LogStore(logStore, logSt.GetTtl(), logSt.GetShardCount(), false));
              //新建支持Web Tracking功能的Logstore。
              //client.UpdateLogStore(project, new LogStore(logStore, 1, 1, true));
          }
          catch (LogException e){
              e.printStackTrace();
          }
      }
    }
    ```


## 步骤2：采集日志

开通Web Tracking后，您可以通过以下四种方法上传日志到Logstore中。

-   通过JavaScript SDK上传日志。
    1.  安装依赖包。

        ```
        npm install --save js-sls-logger
        ```

    2.  导入您的应用程序模块。

        ```
        import SlsWebLogger from 'js-sls-logger'
        ```

    3.  配置opts参数。

        ```
          const opts = {
            host: 'cn-hangzhou.log.aliyuncs.com',      
            project: 'my_project_name',                 
            logstore: 'my_logstore_name',               
            time: 10, 
            count: 10, 
          }
        ```

        |参数名称|是否必填|说明|
        |----|----|--|
        |host|是|日志服务所在地域的Endpoint。更多信息，请参见[服务入口](/intl.zh-CN/开发指南/API 参考/服务入口.md)。此处以杭州为例，其它地域请根据实际情况填写。|
        |project|是|Project名称。|
        |logstore|是|Logstore名称。|
        |time|否|发送日志的时间间隔，默认值为10，单位为秒。|
        |count|否|发送日志的数量大小，默认值为10。|

    4.  创建SlsWebLogger对象。

        ```
        const logger = new SlsWebLogger(opts)
        ```

    5.  上传日志。

        ```
        logger.send({ 
            customer: 'zhangsan',
            product: 'iphone 12',
            price: 7998  
        })
        ```

-   通过HTTP GET请求上传日志。

    参见如下命令上传日志，请根据实际值替换参数。

    ```
    curl --request GET 'http://${project}.${host}/logstores/${logstore}/track?APIVersion=0.6.0&key1=val1&key2=val2'
    ```

    |参数|是否必填|说明|
    |:-|----|:-|
    |$\{project\}|是|Project名称。|
    |$\{host\}|是|日志服务所在地域的Endpoint。更多信息，请参见[服务入口](/intl.zh-CN/开发指南/API 参考/服务入口.md)。|
    |$\{logstore\}|是|Logstore名称。|
    |APIVersion=0.6.0|是|保留字段。|
    |\_\_topic\_\_=yourtopic|否|指定日志主题。|
    |key1=val1&key2=val2|是|您要上传到日志服务的键值对（Key-Value），可以有多个，请确保长度小于16 KB。|

-   通过HTML img标签上传日志。

    ```
    <img src='http://${project}.${host}/logstores/${logstore}/track.gif?APIVersion=0.6.0&key1=val1&key2=val2'/>
    <img src='http://${project}.${host}/logstores/${logstore}/track_ua.gif?APIVersion=0.6.0&key1=val1&key2=val2'/>
    ```

    track\_ua.gif除了上传自定义的参数外，还会将http头中的UserAgent、referer也作为日志中的字段。

    **说明：** 如果您需要采集HTTPS页面的referer，那么上述Web Tracking的链接也必须为HTTPS。

-   通过HTTP POST请求上传日志。

    如果请求的数据量比较大，可以使用POST方法上传数据。更多信息，请参见[PutWebtracking](/intl.zh-CN/开发指南/API 参考/日志库相关接口/PutWebtracking.md)。


