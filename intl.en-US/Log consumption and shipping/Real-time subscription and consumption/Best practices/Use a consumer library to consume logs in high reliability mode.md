# Use a consumer library to consume logs in high reliability mode

Log processing covers real-time computing, data warehousing, and offline computing. This topic describes how to process logs in order without data loss or repetition in real-time computing scenarios, even when upstream and downstream business systems are faulty or data traffic fluctuates.

This topic uses a business day of a bank as an example to illustrate how to process logs. The topic also describes how to use LogHub of Log Service together with Spark Streaming and Storm Spouts to process logs.

## What is log data?

Jay Kreps, a former LinkedIn employee, defined a log as `an append-only, totally-ordered sequence of records ordered by time` in [The Log: What every software engineer should know about real-time data's unifying abstraction](https://engineering.linkedin.com/distributed-systems/log-what-every-software-engineer-should-know-about-real-time-datas-unifying).

![What is log data](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5348344851/p32423.png)

-   Append only: Log entries are appended to the end of the log. They cannot be modified after they are generated.
-   Totally ordered by time: Log entries are strictly ordered. Each log entry is assigned a unique sequential log entry number to indicate its timestamp. Different log entries may be generated at the same timestamp in one second. For example, a GET operation and a SET operation are performed in the same second. However, the two operations are still performed in order on a computer.

## What kind of data can be abstracted into logs?

Half a century ago, captains and operators kept logs in thick notebooks. Today, computers enable logs to be generated and consumed everywhere. Servers, routers, sensors, GPS, orders, and various devices record our lives from different perspectives. In addition to a timestamp used to record the time of a log, captains kept anything they wanted in logs, such as text, an image, weather conditions, and sailing directions. After half a century, logs are generated in a variety of scenarios, such as an order, a payment record, a user access, and a database operation.

In the computer field, common logs include metrics, binary logs for relational and NoSQL databases, events, audit logs, and access logs.

In this topic, a user operation in the bank is regarded as a log entry, which contains the name, account, operation time, operation type, and transaction amount of the user.

For example:

```
2016-06-28 08:00:00 Alice Deposit RMB 1,000
2016-06-27 09:00:00 Bob Withdrawal RMB 20,000
```

## LogHub data model

This section uses the LogHub data model of Alibaba Cloud Log Service for demonstration. For more information, see [Overview](/intl.en-US/Product Introduction/Basic concepts/Overview.md).

-   A log consists of a time and a group of key-value pairs.
-   A log group is a collection of logs that have the same metadata such as the IP address and source.

The following figure shows the relationships between the log and log group.

![LogHub data model](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5348344851/p32424.png)

-   A shard is the basic read/write unit of a log group. It can be regarded as a 48-hour first in, first out \(FIFO\) queue. Each shard allows you to write data at 5 MBit/s and read data at 10 MBit/s. The logical range of a shard is specified by the BeginKey and EndKey. This range enables the shard to contain a type of data different from other shards.
-   A Logstore stores log data of the same type. Each Logstore is a carrier that consists of`` one or more shards whose range is \[0000, FFFF...\).
-   A project is a container for Logstores.

The following figure shows the relationships among the log, log group, shard, Logstore, and project.

![Concept relationships](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5348344851/p32425.png)

## A business day of a bank

For example, one day in the nineteenth century, several users in a city went to a bank to deposit or withdraw money. Several clerks were working in the bank. At that time, transaction data could not be synchronized in real time because computers had not been invented. Each clerk recorded transaction data in an account book and used the account book to check the transaction data every night in the bank. In this example, users are producers of data, money deposit and withdrawal are user operations, and clerks are consumers of data.

In a distributed log processing system, clerks act as standalone servers that have fixed memory and computing capabilities. Users can be regarded as requests from various data sources. The bank hall serves as a Logstore where users can read and write data.

![A business day of a bank](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6348344851/p32426.png)

-   Logs or log group: the user operations such as money deposit and withdrawal.
-   User: the producer of operations.
-   Clerk: the employee who handles user requests in the bank.
-   Bank hall \(Logstore\): the place where user requests are received and then assigned to clerks for handling.
-   Shard: the way in which the bank manager sorts user requests in the bank hall.

## Issue 1: Ordering

Two clerks \(clerks A and B\) were working in the bank. Alice visited the bank and deposited USD 1,000 at counter A. Clerk A recorded the transaction amount in account book A. In the afternoon, Alice went to counter B to withdraw the money. However, clerk B could not find the deposit record of Alice after checking account book B.

In this example, money deposit and withdrawal must be strictly ordered. Requests from the same user must be handled by the same clerk to ensure that the status of user operations is consistent.

![Ordering](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6348344851/p32427.png)

To ensure correct ordering, users can queue up to submit requests. A shard can be created and only clerk A is assigned to handle user requests based on the FIFO principle. However, this method leads to low efficiency, even if 10 clerks are assigned to handle requests from 1,000 users. To improve efficiency in this scenario, you can use the following solution:

Create 10 shards for 10 clerks. Assign a clerk to work in each shard. Ensure that operations for the same account are ordered: Use consistent hashing to map users. For example, map users to specific shards by their bank accounts or names. In this case, by using the formula hash\(Alice\) = A, requests from Alice are always mapped to the specific shard whose range contains A. A clerk, for example, Clerk A, is assigned to handle requests in this shard.

If many users are named Alice, the solution can be adjusted. For example, use the hash function to map users to shards by their account IDs or zip codes. Then, user requests can be evenly distributed to each shard.

![Ordering 2](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6348344851/p32428.png)

## Issue 2: at-least-once processing

Alice deposited money at counter A. When handling this deposit request, clerk A received a call. After the call ended, clerk A mistakenly considered that the deposit request of Alice has been handled and started to handle the request from the next user. The deposit request of Alice was lost.

Computers do not make mistakes like clerks and can work more reliably for a longer time. However, computers may fail to process data due to failure or overload. Deposit loss for such reasons is unacceptable. To avoid data loss in this scenario, you can use the following solution:

Clerk A can record the progress of the current request in a notebook \(not account book A\). Then, clerk A calls the next user only after the deposit request of Alice is handled.

![At-least-once processing](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6348344851/p32429.png)

However, this solution may lead to repetition. For example, after handling the deposit request of Alice and updating data in account book A, clerk A was called away but did not record the progress of the current request into the notebook. When clerk A came back and did not find the progress of the request from Alice in the notebook, clerk A may handle the request again.

## Issue 3: exactly-once processing

A repetition does not necessarily result in problems.

If you perform an idempotent operation more than once, you may waste your time. However, such a repetition does not affect the result. For example, balance inquiry is a read-only operation performed by a user. Repeating this operation does not affect the inquiry result. Some non-read-only operations, such as user logoff, can also be performed twice consecutively.

In actual scenarios, most operations are not idempotent, such as money deposit and withdrawal. The repetition of these operations has great impact on the results. What is the solution to repetitions? After handling a user request, clerk A must update data in account book A, record the progress of the current request into the notebook, and then combine two records into a checkpoint.

If clerk A leaves temporarily or permanently, other clerks can continue as follows: If a checkpoint exists for the current user request, proceed to the next user request. If no checkpoint exists for the current user request, handle this request. Guarantee the atomicity of operations.

![Exactly-once processing](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6650973161/p32430.png)

A checkpoint is a persistent object in which you can record the position or time of an element in a shard as the key to indicate that the element is processed.

## Business challenges

The principles described in the preceding issues are not complex. However, changes and uncertainties in the real world make the three issues more complex. For example:

-   The number of users soars on the pay day.
-   Unlike computers, clerks need a break and lunch time.
-   To improve service experience, the bank manager needs to request clerks to work faster at the right time. Can the bank manager determine the right time based on the speed of request processing in a shard?
-   Clerks need to easily hand over account books and checkpoints.

## A business day in a modern bank

-   The bank opens for business at 8:00 in the morning.

    All user requests are assigned to the only shard named Shard0. Clerk A is responsible for handling such requests.

    ![Bank opening at 8:00](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6348344851/p32431.png)

-   Peak hours start from 10:00 in the morning.

    The bank manager decides to split Shard0 into Shard1 and Shard2 from 10:00 in the morning. In addition, the bank manager assigns user requests to the two shards based on the following rules: If the name of a user starts with a letter from A to W, the user request is assigned to Shard1. If the name of a user starts with X, Y, or Z, the user request is assigned to Shard2.

    The following figure shows the status of user requests in shards from 10:00 to 12:00.

    ![Status of user requests in shards from 10:00 to 12:00](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7348344851/p32432.png)

    When clerk A has difficulty in handling requests in two shards, the bank manager dispatches clerk B and clerk C. Clerk B takes over one of the shards. Clerk C is idle.

-   The number of users increases after 12:00.

    The bank manager splits Shard1 into Shard3 and Shard4 to reduce the workload of clerk A. Then, clerk A handles requests in Shard3 and clerk C handles requests in Shard4. After 12:00, user requests that were originally assigned to Shard1 are reassigned to Shard3 and Shard4.

    The following figure shows the status of user requests in the shards after 12:00.

    ![Status of user requests in the shards after 12:00](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7348344851/p32433.png)

-   The number of users decreases from 16:00.

    The bank manager asks clerk A and clerk B to have a break, and asks clerk C to handle requests in Shard2, Shard 3, and Shard4. Later, the bank manager combines Shard2 and Shard3 into a shard named Shard5, and then combines Shard5 and Shard4 into a shard named Shard6. After all user requests in Shard6 are handled, the bank is closed.


## Actual log processing

The preceding process can be abstracted into a typical log processing scenario. To meet the business requirements of banks, an auto scaling and flexible log framework can be used to provide the following features:

-   Shards are automatically scaled in or out.
-   Shards are automatically adapted to the consumers of a consumer group when consumers are added to or removed from the consumer group. In this process, data integrity and ensured and logs are processed in order.
-   Logs are processed only once. This requires consumers to support ordering.
-   The consumption process is monitored to ensure computing resources are allocated correctly.
-   Logs from more sources are supported to allow users to send requests from multiple channels, such as online banking, mobile banking, and electronic checks.

You can use LogHub and the LogHub consumer library to process logs in real time in the preceding scenarios. Then, you can focus on the business logic without the need to worry about traffic scaling or failover.

Based on the LogHub consumer library, you can also use Storm and Spark Streaming to consume log data in Log Service. For more information, visit [the homepage of Log Service](https://www.alibabacloud.com/product/log-service).

