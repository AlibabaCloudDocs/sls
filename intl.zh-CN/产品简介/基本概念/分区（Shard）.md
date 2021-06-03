# 分区（Shard）

日志服务使用Shard控制Logstore或MetricStore的读写数据的能力，数据必定保存在某一个Shard中。

## Shard范围

每个Shard均有范围，为MD5左闭右开区间\[BeginKey,EndKey\)。每个Shard范围不会相互覆盖，且属于整个MD5范围内\[00000000000000000000000000000000,ffffffffffffffffffffffffffffffff）。您可以在创建Logstore或MetricStore时指定Shard个数，日志服务将自动平均划分整个MD5范围。

-   BeginKey：指定Shard范围的起始值，Shard范围中包含该值。
-   EndKey：指定Shard范围的结束值，Shard范围中不包含该值。

例如Logstore A中包含4个Shard，各个Shard范围如下：

|Shard ID|范围|
|:-------|:-|
|Shard0|\[00000000000000000000000000000000,40000000000000000000000000000000\)|
|Shard1|\[40000000000000000000000000000000,80000000000000000000000000000000\)|
|Shard2|\[80000000000000000000000000000000,c0000000000000000000000000000000\)|
|Shard3|\[c0000000000000000000000000000000,ffffffffffffffffffffffffffffffff\)|

在Shard读写数据过程中，读数据时必须指定Shard ID，写数据时可通过负载均衡模式或者指定Hash Key的模式。

-   负载均衡模式：每个数据包随机写入当前可用的Shard中。
-   指定Hash Key模式：指定MD5的Key值，数据将被写入包含该Key值的Shard中。

    例如Shard范围如[表 1](#table_nff_jx8_wjo)所示，当您写入数据时指定MD5的Key值为5F时，则数据将被写入包含5F的Shard1上；当您写入数据时指定MD5的Key值为8C时，则数据将被写入包含8C的Shard2上。


## Shard的读写能力

每个Shard提供一定的服务能力，详细说明如下：

-   写入：5 MB/s或500次/s
-   读取：10 MB/s或100次/s

建议您根据实际数据流量规划Shard个数。当数据流量超出读写能力时，及时分裂Shard以增加Shard个数，从而达到更大的读写能力。当数据流量远未达到Shard的最大读写能力时，及时合并Shard以减少Shard个数，从而降低活跃Shard租用费用。

例如您有两个readwrite状态的Shard，最大可提供10 MB/s的数据写入服务。当您实时写入数据流量达到14 MB/s时，建议分裂其中一个Shard，使readwrite状态的Shard数量达到3个。当您实时写入数据流量仅为3 MB/s时，建议您合并两个Shard。

**说明：**

-   当写入数据的API持续报告403或者500错误时，您可以通过Logstore云监控查看流量和状态码判断是否需要增加Shard。
-   超过Shard服务能力的读写，日志服务会尽可能服务，但不保证服务质量。

## Shard状态

Shard状态包括readwrite（读写）和readonly（只读）。

创建Shard时，所有Shard状态均为readwrite状态。执行分裂或合并操作后，Shard状态变更为readonly，并生成新的readwrite状态的Shard。Shard状态不影响其数据读取的性能。readwrite状态的Shard可保证数据写入性能，readonly状态的Shard不提供数据写入服务。

## 分裂与合并

日志服务支持分裂和合并Shard。

-   分裂操作是指将一个Shard分裂为另外两个Shard，即分裂后Shard数量增加2。两个新生成的Shard的状态为readwrite，排列在原Shard之后且两个Shard的MD5范围覆盖原Shard的MD5范围。

    分裂Shard时，需指定一个处于readwrite状态的Shard。分裂完成后，原Shard状态由readwrite变为readonly，该Shard中的数据仍可被消费，但该Shard不支持写入新数据。

-   合并操作是指将两个Shard合并为一个Shard。新生成的Shard的状态为readwrite，排列在原Shard之后且其MD5范围覆盖原来两个Shard的MD5范围。

    合并Shard时，需指定一个处于readwrite状态且未排列在最后一个的Shard，日志服务自动找到所指定Shard右侧相邻的Shard，并进行合并。合并完成后，原来两个Shard的状态由readwrite变为readonly，这两个Shard中的数据仍可被消费，但这两个Shard不支持写入新数据。


