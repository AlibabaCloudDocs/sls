# Query and analyze application logs

This topic describes how to use Log Service to query and analyze application logs.

## Features of AppLogs

-   Comprehensive information

    AppLogs are provided by programmers, covering key locations, variables, and exceptions. Over 90% of bugs in the production environment can be located based on AppLogs.

-   Various formats

    One piece of code is often developed by multiple programmers who have their preferred formats. Therefore, the formats of AppLogs are difficult to be unified. Style inconsistency also exists in logs introduced from third-party databases.

-   Similarity among AppLogs

    Despite of the arbitrary formats, different AppLogs share elements in common. For example, the following information is shared by Log4J logs:

    -   Date
    -   Level
    -   File or class
    -   Line number
    -   Thread ID

## Challenges in processing AppLogs

-   Large data volume

    The size of AppLogs is generally one order of magnitude larger than that of access logs. Assume that a website has one million independent PVs each day, each PV involves about 20 logic modules, and 10 major logic points in each logic module need to be logged.

    The total number of the log entries is calculated by using the following formula:

    ```
    1,000,000 × 20 × 10 = 2 × 10^8 entries
    ```

    Assume that the length of each entry is 200 bytes. The total storage size is calculated based on the following formula:

    ```
    2 × 10^8 × 200 = 4 × 10^10 Bytes = 40 GB
    ```

    The data size grows as the business system becomes increasingly complex. It is common for a medium-sized website to generate 100 to 200 GB of log data every day.

-   Multiple distributed applications

    Most applications are stateless and run in different frameworks, such as:

    -   Server
    -   Docker \(container\)
    -   Function Compute \(Container Service\)
    The numbers of corresponding instances vary from a few to thousands. Multiple instances require a cross-server log collection scheme.

-   Complex runtime environments

    Programs are running in different environments, such as:

    -   Application logs are stored in Docker containers.
    -   API logs are stored in Function Compute.
    -   Old system logs are stored in traditional IDCs.
    -   Mobile-side logs are stored in users' mobile devices.
    -   Mobile web logs are stored in browsers.
    To have a comprehensive view of the log data, you must unify and store the logs in a single place.


## Unified data storage

Purpose: to collect data from different sources into a central place to facilitate subsequent operations.

You can create a project in [Log Service](https://www.aliyun.com/product/sls/) to store AppLogs. Log Service supports over 30 collection methods, such as tracking in physical severs, entering JavaScript code on the mobile web side, and exporting logs on servers.

![Unified data storage](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8784688951/p32435.png)

Apart from writing logs by using methods such as SDKs, Log Service provides a convenient, stable, and high-performance agent, namely, Logtail, for server logs. Logtail provides two versions for Windows and Linux. After you have defined the machine group and completed the log collection configuration, server logs can be collected in real time.

After the log collection configuration is complete, you can operate on logs in the project.

Compared with other log collection agents, such as Logstash, Flume, FluentD, and Beats, Logtail has following advantages:

-   Easy to use: provides APIs and remote management and monitoring capabilities. Logtail is designed with the rich experience of Alibaba Group in million-level server log collection and management. This enables you to configure a collection point for hundreds of thousands of devices within seconds.
-   Adaptive to different environments: Logtail supports the Internet, VPCs, and user-created IDCs. The HTTPS and resumable upload features make it possible to integrate with data on the public network.
-   High performance with a few consumed resources: With years of refinement, Logtail is superior to its open-source competitors in performance and resource consumption.

## Quick fault locating

Purpose: to ensure that the time used to locate problems is constant, regardless of how the data volume increases and how servers are deployed.

For example, you can quickly locate an order error and a long latency issue out of terabytes of data every week. The process also filters and investigates data based on multiple conditions.

1.  For example, you can investigate AppLogs with latency details for the requests with a latency of over 1 second and methods starting with Post.

    ```
    Latency > 1000000 and Method=Post*
    ```

2.  Search for logs that contain the keyword "error" and do not contain keyword "merge".
    -   Result of a day

        ![Result of a day](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8784688951/p32437.png)

    -   Result of a week

        ![Result of a week](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8784688951/p32438.png)

    -   Result of a longer time period

        ![Result of a longer time period](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8784688951/p32439.png)

        The results of any time span can be returned within a second.


## Association analysis

There are two types of association: intra-process association and inter-process association. The difference between the two types is described as follows:

-   Intra-process association: This type of association is simple because the old and new logs of a function are stored in the same file. In a multi-thread process, you can filter logs by thread ID.
-   Cross-process association: It is hard to find clear clues to deal with logs from different processes. The association is generally performed by passing the TracerId parameter to Remote Procedure Call \(RPC\).

![Association analysis](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8784688951/p32440.png)

-   Context association

    Locate an error log by running a keyword-based query in the Log Service console.

    ![Context view](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6779990061/p32441.png)

    Click Context View. View the preceding and following results.

    -   You can click **Old** or **New** for more results.
    -   You can also highlight the results by entering keywords.
    ![Context association](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6779990061/p32442.png)

-   Cross-process association

    Cross-process association is also known as tracing. Currently, the following versions are well-known:

    -   Dapper \(Google\): the foundation for all tracers.
    -   StackDriver Trace \(Google\): compatible with Zipkin.
    -   Zipkin: an open-source tracing system by Twitter.
    -   Appdash: a tracer for Go.
    -   EagleEye: developed by the Middleware Technology Department of Alibaba Group.
    -   X-Ray: introduced at AWS re:Invent 2016.
    Applying a tracer from scratch is easier than in an existing system because of the costs and challenges in adapting it to your system.

    Based on Log Service, you can implement a basic tracing feature. To obtain all required logs, you can record associative fields such as Request\_id and OrderId in logs of different modules and search for the fields in various Logstores.

    ![Association logs](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8784688951/p32443.png)

    For example, you can search logs of frontend servers, backend servers, payment systems, and order systems by using SDKs. After the results have been returned, you can create a page on the frontend to associate the cross-process calls. The following figure shows the tracing system built based on Log Service.

    ![Tracing system](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1270470061/p32444.png)


## Statistical analysis

After the specific logs are located, you can analyze the logs, such as calculating the types of online error logs.

1.  You can query logs by `__level__`. For example, 20 errors are found within one day.

    ```
    __level__:error
    ```

    ![Error number](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8784688951/p32445.png)

2.  To determine the unique log type, you can analyze and aggregate data based on the file and line fields.

    ```
    __level__:error | select __file__, __line__, count(*) as c group by __file__, __line__ order by c desc
    ```

    Then, the results of all types and locations of errors can be returned.

    ![Error types and locations](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1270470061/p32446.png)


You can locate and analyze IP addresses with certain error codes or high latency.

## Other features

-   Log backup for auditing

    You can back up the logs to OSS as Infrequent Access \(IA\) objects which has a lower storage cost, or to MaxCompute.

-   Keyword alerting

    [Cloud Monitor](/intl.en-US/Developer Guide/Monitor Log Servic/Cloud Monitor.md)

-   Log query permission management

    You can grant permissions to RAM users or a user group to isolate the development environment and the production environment.

    Price and cost: In this scheme, AppLogs use the LogHub and LogSearch features of Log Service. Compared with an open source scheme, this scheme is easy-to-use and only has 25% of the costs of an open source scheme, improving the development efficiency.


