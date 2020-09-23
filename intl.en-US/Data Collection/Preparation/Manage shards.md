# Manage shards

This topic describes how to split, merge, and delete shards in the Log Service console.

When you create a Logstore, you must set the number of shards for the Logstore. After the Logstore is created, you can split or merge shards to increase or decrease the number of shards in the Logstore.

-   You can write data to a shard at a maximum rate of 5 MB/s and read data from a shard at a maximum rate of 10 MB/s. If your data traffic exceeds the service capacity of a shard, we recommend that you split the shard.

    You can split a shard of a Logstore on the **Logstore Attributes** page of the Logstore.

-   If the data traffic is much less than the maximum read/write capability of shards, we recommend that you merge shards.

## Split a shard

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the **Projects** section, click the target project.

3.  Choose **Log Management** \> **Logstores**. Click the management icon of the target Logstore, and then select **![Modify a Logstore](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5023359951/p52318.png)** \> **Modify**.

4.  On the **Logstore Attributes** page, click **Modify** in the upper-right corner.

5.  Find the shard that you want to split, and click **Split** in the Actions column.

    **Note:** The shard that you split must be in the readwrite state.

    ![Split a shard](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2119824951/p2594.png)

6.  Select the number of new shards into which you want to split the original shard, and click **OK**.

7.  Click **Save**.

    After the shard is split, its status changes from readwrite to readonly. You can consume data from the shard, but you cannot write data to the shard. Newly generated shards are in the readwrite state, and they are listed below the original shard. Their MD5 value ranges cover the MD5 value range of the original shard.


## Enable automatic sharding

Log Service provides the automatic sharding feature. If you enable the automatic sharding feature, shards are automatically split if the following two conditions are met:

-   The data traffic exceeds the [write capacity](/intl.en-US/Product Introduction/Limits/Data read and write.md) of the existing shards for more than 5 minutes.
-   The number of shards in the readwrite state does not exceed the specified maximum number of shards in the Logstore.

**Note:** Automatic sharding does not apply to the shards that are split from a shard in the last 15 minutes.

You can enable the automatic sharding feature when you create or modify a Logstore. If you enable the feature for the Logstore, you must specify the maximum number that the shards can be split into in the Logstore.

-   Automatic sharding

    If the amount of data written to the Logstore exceeds the [write capability](/intl.en-US/Product Introduction/Limits/Data read and write.md) of the existing shards for more than 5 minutes, the number of shards is increased to accommodate the data traffic.

-   Maximum shards

    The maximum number that shards in the Logstore can be automatically split into is 64.


## Merge shards

You can merge shards of a Logstore. When you specify a shard for merging, Log Service locates the shard next to the specified shard, and merges the two shards.

**Note:**

-   The specified shard that you specify for merging must be in the readwrite state. In addition, the shard must not be the last shard that is in the readwrite state.
-   After the shards are merged, their status changes to the readonly state. The new shard is in the readwrite state. The MD5 value ranges of the two shards cover the MD5 value range of the new shard.

1.  Choose **Log Management** \> **Logstores**. Click the management icon of the target Logstore, and then select **![Modify a Logstore](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5023359951/p52318.png)** \> **Modify**.

2.  On the **Logstore Attributes** page, click **Modify** in the upper-right corner.

3.  Specify the shard that you want to merge, and click **Merge** in the Actions column.

    ![Merge shards](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2119824951/p2596.png)

4.  Click **Save**.


## Delete a shard

-   Automatic deletion

    You can specify a data retention period when you create a Logstore. If you specify the retention period, shards in the Logstore and data stored in the shards are automatically deleted when the retention period expires.

-   Manual deletion

    You can turn on the permanent retention switch when you create a Logstore. This allows you to delete the shards and data from the Logstore by deleting the Logstore. For more information, see [Delete a Logstore](/intl.en-US/Data Collection/Preparation/Manage a Logstore.md).


