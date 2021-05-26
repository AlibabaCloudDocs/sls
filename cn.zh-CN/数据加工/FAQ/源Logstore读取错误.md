# 源Logstore读取错误

本文介绍数据加工服务读取源Logstore错误的原因以及排查处理方法。

加工引擎启动成功后，开始读取源Logstore的数据。数据加工引擎对源Logstore采用流式读取，在加工过程中会持续不断的读取源Logstore中的数据。

![源Logstore读取错误](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2653749951/p59308.png)

本环节产生错误主要是由于对源Logstore的访问异常。可能原因如下：

-   源Logstore信息配置错误。
-   源Logstore信息发生变化。
-   网络错误。

错误影响：

-   读取源Logstore发生错误时，加工任务会一直重试，直到重试成功或被手动停止。重试成功后，加工任务会继续正常工作。
-   加工任务会保存之前成功的断点并一直重试。重试成功后，从断点处继续读取，不会产生数据的丢失和冗余。

## 常见错误排查

-   为源Logstore配置了非法的AccessKey ID和AccessKey Secret。
    -   错误日志

        ```
        {
          "errorCode": "Unauthorized", 
          "errorMessage": "AccessKeyId not found: LTAIL3gUus8AEu11"
        }
        { 
          "errorCode": "SignatureNotMatch", 
          "errorMessage": "signature uJfAJbc0ji04gb+cXhh0qWtajpM= not match"
        }
        ```

    -   排查方法

        检查您所配置的源Logstore的AccessKey ID和AccessKey Secret是否存在且正确。

-   源Logstore信息发生变化。

    您配置了正确的源Logstore信息，并已进行了部分加工任务，但是在数据加工过程中，源Logstore信息发生了变化，导致原有的配置信息无法访问源Logstore。

    -   错误日志

        源Logstore信息发生变化主要是以下两种情况：

        -   源Logstore被删除。

            ```
            {
              "errorMessage": "Logstore [logstore_name] does not exist."
            }
            ```

        -   源Logstore的AccessKey信息发生变化。

            ```
            {
              "errorCode": "Unauthorized", 
              "errorMessage": "AccessKeyId not found: LTAIL3gUus8AEu11"
            }
            { 
              "errorCode": "SignatureNotMatch", 
              "errorMessage": "signature uJfAJbc0ji04gb+cXhh0qWtajpM= not match"
            }
            ```

    -   排查方法
        -   检查源Logstore是否被删除。
        -   检查源Logstore的AccessKey信息是否发生改变。
-   网络异常。
    -   错误日志

        ```
        {
          "errorCode": "LogRequestError",
          "errorMessage": "HTTPConnectionPool(host='your_host', port=80): Max retries exceeded with url: your_url (Caused by NewConnectionError: Failed to establish a new connection: [Errno 11001] getaddrinfo failed'"
        }
        ```

    -   排查方法

        检查网络连接是否正常。

-   读取不到源Logstore数据。

    该问题无报错信息。更多信息，请参见[通用错误排查](/cn.zh-CN/数据加工/FAQ/错误排查概述.md)。


