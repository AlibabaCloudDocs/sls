# 操作Shard {#concept_yks_jj5_vdb .concept}

Logstore读写日志必定保存在某一个分区（Shard）上。每个日志库（Logstore）分若干个分区，您在创建Logstore时需要为该Logstore设置Shard的数量，创建完成后还可以分裂或合并Shard，以达到增加或减少Shard的目的。

对于已存在的Shard，您可以进行以下操作：

-   [分裂Shard](#)
-   [自动分裂Shard](#)
-   [合并Shard](#)
-   [删除Shard](#)

## 分裂Shard {#section_awx_35f_vdb .section}

每个分区（Shard）能够处理5M/s的数据写入和10M/s的数据读取，当数据流量超过分区服务能力时，建议您及时增加分区。扩容分区通过分裂（split）操作完成。

**使用指南**

在分裂分区时，需要指定一个处于readwrite状态的ShardId和一个MD5。MD5要求必须大于分区的BeginKey并且小于EndKey。

分裂操作可以从一个分区中分裂出另外两个分区，即分裂后分区数量增加2。在分裂完成后，被指定分裂的原分区状态由readwrite变为readonly，数据仍然可以被消费，但不可写入新数据。两个新生成的分区状态为readwrite，排列在原有分区之后，且两个分区的MD5范围覆盖了原来分区的范围。

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。
2.  选择所需的项目，单击项目名称。
3.  在Logstore列表页面，选择所需的日志库并单击操作列下的**修改**。
4.  选择要分裂的分区，单击右侧的**分裂**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13025/15608462742594_zh-CN.png)

5.  确认分区的分裂位置。

    默认均分Shard，分裂位置为当前Shard MD5范围的中间。您也可以选择当前分区要分裂的数量。

6.  单击**确认** 并关闭对话框。

    分裂操作完成后，原分区变为readonly状态，两个新生成的分区的MD5范围覆盖了原来分区的范围。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13025/15608462752595_zh-CN.png)


## 自动分裂Shard {#section_zw3_x4v_22b .section}

**使用指南**

除手动分裂Shard之外，日志服务还支持自动分裂Shard功能。您可以在创建或修改Logstore时开启该功能，并设定Shard自动分裂后的最大数目。

开启自动分裂Shard功能后，满足以下条件时，Shard会自动分裂：

1.  数据量超出当前已有Shard的服务能力，且持续5分钟以上。
2.  Logstore中readwrite状态的Shard数目未超过设定的最大shard总数。

**说明：** 最近15分钟内分裂出来的新Shard不会自动分裂。

![](images/6426_zh-CN.png "自动分裂Shard")

|配置项|说明|
|:--|:-|
|自动分裂shard|Shard自动分裂功能开关。开启该功能后，满足条件的Shard会在数据量超出Shard服务能力时自动分裂。|
|最大分裂数|Shard自动分裂后的最大数目。开启自动分裂Shard功能后，最大可支持自动分裂至64个分区。|

## 合并Shard {#section_tqj_l5f_vdb .section}

您可以通过合并（merge）操作缩容分区。合并操作可以将指定分区与其右侧相邻分区的范围合并，并将此范围赋予新生成的readwrite分区，两个被合并的原分区变为readonly状态。

**使用指南**

在合并操作时，必须指定一个处于readwrite状态的分区，指定的分区不能是最后一个readwrite分区。服务端会自动找到所指定分区的右侧相邻分区，并将两个分区范围合并。在合并完成后，所指定的分区和其右侧相邻分区变成只读（readonly）状态，数据仍然可以被消费，但不能写入新数据。同时新生成一个 readwrite 状态的分区，新分区的MD5范围覆盖了原来两个分区的范围。

**操作步骤**

1.  登录日志服务管理控制台。
2.  选择所需的项目，单击项目名称。
3.  在Logstore列表页面，选择所需的日志库并单击操作列下的**修改**。
4.  选择要合并的分区，单击右侧的**合并** 并关闭对话框即可。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13025/15608462752596_zh-CN.png)

    在合并完成后，所指定的分区和其右侧相邻分区变成只读（readonly）状态，新生成的readwrite分区的MD5范围覆盖了原来两个分区的范围。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13025/15608462752597_zh-CN.png)


## 删除Shard {#section_m3g_45f_vdb .section}

Logstore的生命周期即数据保存时间支持设置为1~3000天，分区及分区中的日志数据在超出该时间后会自动删除。readonly 分区不参与计费。您也可以将某个Logstore中的日志数据设置为永久保存，这部分Shard和日志数据将永远不会自动删除。修改日志数据自动保存时间请参考[操作Logstore](intl.zh-CN/用户指南/准备工作/操作Logstore.md)。

您也可以通过删除Logstore的方式删除整个日志库中的所有分区。

