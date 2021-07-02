# Shards

Shards are used to control the read and write capacity of Logstores or Metricstores. In Log Service, data is stored in a shard.

## MD5 value range

Each shard has an MD5 value range. and each range is a left-closed and right-open interval in the format of \[BeginKey, EndKey\). Each range does not overlap the ranges of other shards. The entire MD5 value range is within the following range: \[00000000000000000000000000000000, ffffffffffffffffffffffffffffffff\). You can specify the number of shards when you create a Logstore or Metricstore. Log Service evenly divides the entire MD5 value range for each shard.

-   BeginKey: the start of a shard. The value is included in the MD5 value range of the shard.
-   EndKey: the end of a shard. The value is excluded from the MD5 value range of the shard.

In this example, Logstore A has four shards. The following table shows the MD5 value range of each shard.

|Shard ID|Value range|
|:-------|:----------|
|Shard0|\[00000000000000000000000000000000,40000000000000000000000000000000\)|
|Shard1|\[40000000000000000000000000000000,80000000000000000000000000000000\)|
|Shard2|\[80000000000000000000000000000000,c0000000000000000000000000000000\)|
|Shard3|\[c0000000000000000000000000000000,ffffffffffffffffffffffffffffffff\)|

To read data from a shard, you must specify the ID of the shard. To write data to a shard, you can use the load balancing method or specify a hash key.

-   If you use the load balancing method, each data packet is randomly written to an available shard.
-   If you specify a hash key, data is written to the shard whose MD5 value range includes the value of the specified hash key.

    For example, the shard range is shown in [Table 1](#table_nff_jx8_wjo). If you specify 5F as a hash key to write data to a Logstore, the data is written to Shard1 because the MD5 value range of Shard1 contains the hash key 5F. If you specify 8C as a hash key, the data is written to Shard2 because the MD5 value range of Shard2 contains the hash key 8C.


## Shard capacity

Each shard provides the following read capacity and write capacity:

-   Write capacity: 5 MB/s or 500 times/s
-   Read capacity: 10 MB/s or 100 times/s

We recommend that you adjust the number of shards based on the actual data traffic. If the data traffic exceeds the read or write capacity of a shard, you can split the shard into multiple shards to increase the capacity. If the data traffic is much lower than the read or write capacity of a shard, you can merge the shard with another shard to reduce the capacity and save costs.

For example, you have two shards that are in the readwrite state and the shards can provide a maximum write capacity of 10 MB/s. If you need to write data at a speed of 14 MB/s in real time, we recommend that you split one of your shards into two shards. This way, you can have three shards that are in the readwrite state. If you need to write data at a speed of 3 MB/s in real time, we recommend that you merge your shards.

**Note:**

-   If the error code 403 or 500 is frequently returned when you write data by calling the Log Service API, you can go to the CloudMonitor console to check the traffic and status codes. Then, you can determine whether to increase the number of shards.
-   If the data traffic exceeds the capacity of your shards, Log Service attempts to provide the best possible services. However, Log Service cannot ensure the quality of the service.

## Shard status

A shard can be in the readwrite \(read and write\) state or readonly \(read-only\) state.

When you create a shard, the shard is in the readwrite state. If you split or merge the shard, its status changes to readonly. The newly generated shard is in the readwrite state. The status of a shard does not affect the read capacity of the shard. Data can be written to the shards that are in the readwrite state, but cannot be written to the shards that are in the readonly state.

## Splitting and merging

Log Service allows you to split and merge shards.

-   After you split a shard, two more shards are added. The new shards are in the readwrite state and are listed under the original shard. The MD5 value range of the new shards incudes the MD5 value range of the original shard.

    Before you can split a shard, the shard must be in the readwrite state. After you split a shard, the status of the shard changes from readwrite to readonly. This specifies that data can still be read from the shard, but cannot be written to the shard.

-   You can merge two shards into one shard. The new shard is in the readwrite state and is listed under the original shard. The MD5 value range of the new shard incudes the MD5 value range of the two original shards.

    When you merge shards, you must specify a shard that is in the readwrite state. This shard cannot be the last shard in the shard list. Log Service locates the shard whose MD5 value range is next to the specified shard and then merges the two shards. After you merge the shards, the status of the shards changes from readwrite to readonly. This specifies that data can still be read from the shards, but cannot be written to the shards.


