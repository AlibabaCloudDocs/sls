# Cost optimization

The cost of Log Service is related to two factors:

-   Data volume: Data volume is determined by your business needs.
-   Configurations: You can use configurations that match your data volume and choose the best solution to minimize the cost.

## Configurations optimization

The following two configurations can be optimized:

-   Number of shards

    Each shard can process data at a maximum speed of 5 MB/s. Only shards in the readwrite state incur fees. You can adjust the number of shards so that each shard can process data at a speed of 5 MB/s. You can also merge the shards to reduce the number of shards.

-   Data retention period of a Logstore

    We recommend that you optimize the data retention period of a Logstore based on your requirements for log query and storage.

    -   If you collect logs for stream computing, we recommend that you use only LogHub and do not create indexes.

    -   If you want to store logs for a long time, we recommend that you ship logs to OSS.


## Other optimization recommendations

-   Use Logtail: Logtail allows you to transmit data in batches and resume data transmission by using checkpoints. Logtail can transmit data in real time with an optimal algorithm. Compared with other software products such as Logstash and FluentD, Logtail reduces CPU consumption by 75%.
-   Use large packages \(64 KB - 1 MB\) to write logs by calling API operations. This reduces the number of requests.
-   Configure indexes only for key fields, such as UserID and Action.

