# Why am I charged for active shards?

This topic describes why you are charged for active shards and the related free quota.

Log Service allows you to read data from and write data to shards. When you create a Logstore, Log Service allocates shard resources to the Logstore. Therefore, you may be charged for active shards.

The free quota of active shards is 31 shard\*day per month. The following examples describe the fees that are incurred by active shards:

-   If you create only one Logstore and use the default settings, it indicates that you rent two shards per day. The total number of shards that you use per month is 62 shard\*day \(2 × 31 = 62\). You are not charged during the first 15 days of a month. However, you are charged from the 16th day. This billing method is applied again in the next month.
-   In this example, assume that you create only one Logstore, set **Shards** to **1**, and turn on the Automatic Sharding switch. On the 30th day of a month, you have only one free shard for the next day. Assume that the amount of written data increases on the 30th day, and Log Service splits the shard to four shards based on your data volume. In this case, you are charged for the active shards from the 30th day. This billing method is applied again in the next month.
-   In this example, assume that you create four Logstores, set **Shards** to **1**, and turn off the Automatic Sharding switch. It indicates that you rent four shards per day. The total number of shards that you use per month is 124 shard\*day \(4 × 31 = 124\). You are not charged during the first seven days of a month. However, you are charged from the eighth day. This billing method is applied again in the next month.

**Note:**

-   Each shard supports a write speed of 5 MB/s and a read speed of 10 MB/s. If the read or write speed cannot meet your requirements, you can split your shards. If the read or write speed exceeds your requirements, you can merge your shards to reduce costs. For more information, see [Shards](/intl.en-US/Product Introduction/Basic concepts/Shards.md).
-   If you create only one Logstore and use only one shard, you are not charged for active shards.

