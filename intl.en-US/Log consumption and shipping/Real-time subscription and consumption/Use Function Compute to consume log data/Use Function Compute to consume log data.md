# Use Function Compute to consume log data

Log Service allows you to use Function Compute to transform streaming data. You can configure an extract-transform-load \(ETL\) job to detect data updates and call functions. Then the incremental data in a Logstore is consumed and transformed. You can use template functions or custom functions to transform data.

-   Log Service is authorized to call functions. To authorize Log Service, go to the [Cloud Resource Access Authorization](https://ram.console.aliyun.com/#/role/authorize?request=%7B%22Requests%22%3A%20%7B%22request1%22%3A%20%7B%22RoleName%22%3A%20%22AliyunLogETLRole%22%2C%20%22TemplateId%22%3A%20%22ETL%22%7D%7D%2C%20%22ReturnUrl%22%3A%20%22https%3A//sls.console.aliyun.com/%22%2C%20%22Service%22%3A%20%22Log%22%7D) page.
-   A project and a Logstore are created. For more information, see [Create a project and a Logstore](/intl.en-US/Quick Start/Quick start.md).

## Scenarios

-   Data cleansing and transformation

    You can use Log Service to collect, transform, and query logs.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13169/15584256735804_en-US.png)

-   Data shipping

    You can use Log Service to ship data to various destinations. In this scenario, Log Service serves as a data channel for big data services in the cloud.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13169/15584256735805_en-US.png)


## ETL functions

-   Function types
    -   Template functions

        For more information, see [aliyun-log-fc-functions](https://github.com/aliyun/aliyun-log-fc-functions).

    -   User-defined functions

        You can define functions based on your business scenarios. For more information, see [Create a custom function](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Use Function Compute to consume log data/Development guide.md).

-   Function calling mechanism

    An ETL job is used to call functions. After you create an ETL job for a Logstore in Log Service, a timer is started to poll data from the shards of the Logstore based on the job configurations. If new data is written to the Logstore, a triple data record in the `<shard_id,begin_cursor,end_cursor >` format is generated as a function event. Then the ETL function is called.

    **Note:** If no new data is written to the Logstore and the storage system is updated, the cursor information will change. The ETL function is called for each shard but no data is transformed. In this case, you can use the cursor information to obtain data from the shards. If no data is obtained, the ETL function is called but no data is transformed. You can ignore the function callings. For more information, see [Development guide](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Use Function Compute to consume log data/Development guide.md#).

    An ETL job calls functions based on the time mechanism. For example, you set the calling interval in an ETL job to 60 seconds for a Logstore. If data is continuously written to Shard 0, the ETL function is called every 60 seconds to transform data that is located in the cursor range of the last 60 seconds.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13169/15584256735806_en-US.png)


## Procedure

1.  Create a service. For more information, see [Create a service]().

    **Note:** The service must reside in the same region as the Log Service project.

2.  Create a function. For more information, see [Create a function]().

3.  Create a trigger. For more information, see [Create a trigger]().


## References

-   Query trigger logs

    You can create an index for trigger logs and view statistics about tasks. For more information, see [Enable and configure the index feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).

-   View the logs of function callings

    You can use the command line tool to view detailed information about function callings. For more information, see [log entry](https://www.alibabacloud.com/help/zh/doc-detail/52704.htm).


## FAQ

If you create a trigger but the ETL function is not called, you can troubleshoot the issue by using one of the following methods:

-   Check whether new data is written to the Logstore for which an ETL job is configured. If new data is written to the Logstore, the function will be called.
-   Check trigger logs and logs of function callings.

