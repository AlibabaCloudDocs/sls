# Consume metering and billing logs

The biggest advantage of cloud services is the pay-as-you-go billing method, with no need to reserve resources. Therefore, all cloud products have metering and billing demands. This topic describes a metering and billing solution that is developed based on Log Service and used by many cloud products. You can use this solution to process hundreds of billions of metering logs every day.

## Bill generation from metering logs

Metering logs record billing items. The back-end billing module computes the expenses based on the billing items and rules to generate the final bill. For example, the following raw access log records the usage of a project:

```
microtime:1457517269818107 Method:PostLogStoreLogs Status:200 Source:10.145.6.81 ClientIP:112.124.143.241 Latency:1968 InFlow:1409 NetFlow:474 OutFlow:0 UserId:44 AliUid:1264425845****** ProjectName:app-myapplication ProjectId:573 LogStore:perf UserAgent:ali-sls-logtail APIVersion:0.5.0 RequestId:56DFF2D58B3D939D691323C7
            
```

The metering and billing programs read the raw log and generate use data from various dimensions based on relevant rules. The following figure shows the generated use data, including the inbound traffic, number of use times, and outbound traffic.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/1401649651/p32415.png)

## Typical scenarios of billing by using metering logs

-   An electric power company receives a log every 10 seconds. The log records the power consumption, peak power consumption, and average power consumption for each user ID within the 10 seconds. Based on such logs, the company generates bills for users on an hourly, daily, or monthly basis.
-   A carrier receives a log from a base station every 10 seconds. The log records the services used by a mobile number within the 10 seconds, such as Internet access, voice calls, SMS messages, and voice over Internet Protocol \(VoIP\) calls. The log also records the data usage or duration of each service. Based on this log, the back-end billing service computes the expenses incurred during this period.
-   A weather forecast API service charges user requests based on the type of the requested API operation, city, query type, and size of the query result.

## Requirements and challenges

A metering and billing solution must meet the following basic requirements:

-   Accuracy and reliability: Computation results must be accurate.
-   Flexibility: Data can be supplemented. For example, if some data is not pushed in time, data can be supplemented and corrected to re-compute the expenses.
-   Timeliness: Services can be billed in seconds. Services are immediately stopped if an account has any overdue payments.

Other requirements:

-   Bill correction: If real-time billing fails, bills can be generated from metering logs.
-   Details query: Users can view their consumption details.

Two challenges in reality:

-   Increasing data size: The data size keeps increasing as the number of users and calls grows. One challenge is to maintain auto scaling of the architecture.
-   Fault tolerance: The billing program may have bugs. The other challenge is to keep the metering data independent of the billing program.

The metering and billing solution described in this topic is developed by Alibaba Cloud based on Log Service. The solution has been in stable operation online for many years without any error or latency.

## System architecture

In the system architecture of the metering and billing solution, LogHub of Log Service works as follows:

1.  Collects metering logs in real time and writes metering logs to the metering program. LogHub supports more than 30 API operations and access methods to help you collect and write metering logs.
2.  Allows the metering program to consume the LogHub data of a step size at regular intervals, and then compute the expenses in the memory to generate billing data.
3.  Creates indexes for metering logs to meet details query requirements.
4.  Pushes metering logs to Object Storage Service \(OSS\) for offline storage. Allows you to check accounts and take statistics of data on a T+1 basis.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/1401649651/p32417.png)

The internal logic of the real-time metering program is as follows:

1.  Calls the GetCursor operation to obtain the cursor of a log in a specified period, for example, 10:00 to 11:00, from LogHub.
2.  Calls the PullLogs operation to consume data in this period.
3.  Takes statistics of data and computes data in the memory, obtains the result, and then generates billing data.

    Similarly, the specified period can be 1 minute or 10 seconds.


![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/1401649651/p32418.png)

The performance of the metering and billing solution is analyzed as follows:

-   Assume that 1 billion metering logs are generated every day, each of which is 200 bytes. The total data size is 200 GB.
-   In LogHub, the SDK or agent can compress data by default. Therefore, the size of stored data is 40 GB after compression, which is at most one-fifth of the original size. The size of data generated in an hour is calculated as follows: 40/24 = 1.6 GB.
-   LogHub allows you to read a maximum of 1,000 log groups each time. The maximum size of each log group is 5 MB. On a GE network, all data generated in an hour can be read within 2 seconds.
-   After the metering program takes statistics of data and computes data in the memory, metering logs generated in an hour can be consumed within 5 seconds.

## Solution to a large data size

In some billing scenarios of carriers and the Internet of Things \(IoT\), a large number of metering logs are generated, for example, 10 trillion logs with a data size of 2 PB per day. After compression, the size of data generated in an hour is 16 TB. On a 10-GE network, it takes 1,600 seconds to read all data generated in an hour, which cannot meet the quick billing requirements.

## 1. Control the size of generated billing data

Modify the program that generates metering logs, such as Nginx, to aggregate metering logs in the memory first and dump the aggregated metering logs once every minute. In this way, the data size is associated with the total number of users. Assume that Nginx processes data for 1,000 users per minute. The size of metering logs generated in an hour is calculated as follows: 1,000 × 200 × 60 = 12 GB. The data size is only 240 MB after compression.

## 2. Parallelize metering log processing

In LogHub, each Logstore can have several shards. For example, a Logstore contains three shards, and three metering programs are available. To ensure that the metering data of a user is always processed by the same metering program, use the hash function to map users to shards by user ID. Then, the metering data of the same user can be allocated to a fixed shard. For example, the metering data of users in the Xihu District of Hangzhou is written to shard 1, and the metering data of users in the Shangcheng District of Hangzhou is written to shard 2. In this case, the back-end metering programs can process the metering data in two shards in parallel.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/1401649651/p32419.png)

## FAQ

-   How do I supplement data?

    In LogHub, you can set the lifecycle of each Logstore in the range of 1 to 365 days. If the billing program needs to consume data again, it can compute data by period in the lifecycle of a Logstore.

-   What can I do if metering logs are scattered on many front-end servers?
    1.  Use a Logtail agent to collect the logs in real time from each server.
    2.  Use the machine IDs of servers to define a dynamic machine group for auto scaling.
-   How do I query details?

    You can create indexes for LogHub data to support real-time query and statistical analysis. For example, if you want to query metering logs with a large data size, you can use the following query statement:

    ```
    Inflow>300000 and Method=Post* and Status in [200 300]
    ```

    After enabling the indexing feature for LogHub data, you can query and analyze data in real time.

    You can also add a statistical analysis statement to the end of the query statement as follows:

    ```
    Inflow>300000 and Method=Post* and Status in [200 300] | select max(Inflow) as s, ProjectName group by ProjectName order by s desc            
    ```

-   How do I store logs and check accounts on a T+1 basis?

    Log Service can ship LogHub data to other systems. You can customize shards and the storage format to store logs in OSS. Then, you can use E-MapReduce, HybridDB, Hadoop, Hive, Presto, and Spark to compute log data.


