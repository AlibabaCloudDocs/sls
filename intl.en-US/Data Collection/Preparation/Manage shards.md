# Manage shards

Log data is stored on a shard of a Logstore where read and write operations are performed. This topic describes how to split, merge, and delete shards in the Log Service console.

When you create a Logstore, you must set the number of shards for the Logstore. After the Logstore is created, you can split or merge shards to increase or decrease the number of shards in the Logstore.

-   Each shard supports a write speed of 5 MB/s and a read speed of 10 MB/s. If the read or write speed does not meet your requirements, we recommend that you split the shard.

    You can split a shard of a Logstore on the **Logstore Attributes** page of the Logstore.

-   If the data traffic is less than the maximum read or write speed, we recommend that you merge shards.

## Split a shard

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click the project in which you want to query and analyze logs.

3.  On the **Log Storage** \> **Logstores** tab, find the Logstore that you want to modify, and then choose **![Modify a Logstore](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5023359951/p52318.png)** \> **Modify**.

4.  On the **Logstore Attributes** page, click **Modify**.

5.  Find the shard that you want to split, and click **Split** in the Actions column.

    **Note:** The shard that you split must be in the readwrite state.

    ![Split a shard](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2119824951/p2594.png)

6.  Select the number of new shards into which you want to split the original shard, and click **OK**.

7.  Click **Save**.

    After you split the shard, the readwrite state is changed to the readonly state. You can consume data from the shard that is in the readonly state. However, you cannot write data to the shard. New shards are in the readwrite state, and are listed below the original shard. The MD5 value ranges of the new shards cover the MD5 value range of the original shard.


## Enable the automatic sharding feature

Log Service provides the automatic sharding feature. If you enable the automatic sharding feature, shards are automatically split if the following two conditions are met:

-   The data write speed exceeds the maximum [write speed](/intl.en-US/Product Introduction/Limits/Data read and write.md) of the current shard for more than 5 minutes.
-   The number of shards that are in the readwrite state does not exceed the specified maximum number of shards in the Logstore.

**Note:** Automatic sharding does not apply to the shards that are split from a shard in the last 15 minutes.

You can enable the automatic sharding feature when you create or modify a Logstore. If you turn on the Automatic Sharding switch, you must set the Maximum Shards parameter.

-   Automatic Sharding

    For example, the Automatic Sharding switch is turned on, and the data write speed exceeds the maximum [write speed](/intl.en-US/Product Introduction/Limits/Data read and write.md) of the current shard for more than 5 minutes. In this case, Log Service automatically increases the number of shards.

-   Maximum Shards

    If you turn on the Automatic Sharding switch, you must set the Maximum Shards parameter. The maximum number of shards is 64.


## Merge shards

You can merge shards of a Logstore. If you click Merge in the Actions column of a shard, Log Service locates the shard next to the specified shard, and merges the two shards.

**Note:** When you merge shards, you must specify a shard that is in the readwrite state. However, you cannot specify the last shard that is in the readwrite state.

1.  On the **Log Storage** \> **Logstores** tab, find the Logstore that you want to modify, and then choose **![Modify a Logstore](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5023359951/p52318.png)** \> **Modify**.

2.  On the **Logstore Attributes** page, click **Modify**.

3.  Specify the shard that you want to merge, and click **Merge** in the Actions column.

    ![Merge shards](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2119824951/p2596.png)

4.  Click **Save**.

    After you merge shards, the specified shard and the shard next to the specified shard are both in the readonly state. A shard is added and is in the readwrite state. The MD5 value range of the new shard covers the MD5 value ranges of the two original shards.


## Delete a shard

-   Automatic deletion

    You can specify a data retention period when you create a Logstore. If you specify the retention period, the shards in the Logstore and the data stored in the shards are automatically deleted when the retention period expires.

-   Manual deletion

    If you turn on the Permanent Storage switch when you create a Logstore, we recommend that you delete the shards and data of a Logstore by deleting the Logstore. For more information, see [Delete a Logstore](/intl.en-US/Data Collection/Preparation/Manage a Logstore.md).


## Shard-related API operations

|Action|Operation|
|------|---------|
|Split a shard|[SplitShard](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/SplitShard.md)|
|Merge shards|[MergeShards](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/MergeShards.md)|
|Query shards|[ListShards](/intl.en-US/Developer Guide/API Reference/Logstore related APIs/ListShards.md)|

