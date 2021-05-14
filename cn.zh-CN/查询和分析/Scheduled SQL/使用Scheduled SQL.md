# 使用Scheduled SQL

日志服务提供Scheduled SQL功能，用于定时分析数据或数据聚合存储。本文以网站日志为例，为您介绍完整的Scheduled SQL操作流程。

A公司将其网站访问日志上传到日志服务的website-log Logstore中完成数据存储与分析。现在A公司希望通过Scheduled SQL功能每10分钟统计一次网站访问成功的次数，并将结果数据保存到dest-log Logstore中。

**说明：** 目前，Scheduled SQL功能在公测阶段，仅支持西南1（成都）和华北2（阿里政务云）。如果您所在地域未支持Scheduled SQL功能，可提交[工单](https://selfservice.console.aliyun.com/ticket/category/sls/today)申请。

## 前提条件

-   已采集网站日志到源Logstore（website-log）。具体操作，请参见[数据采集](/cn.zh-CN/数据采集/采集方式.md)。
-   已创建目标Logstore（dest-log）。具体操作，请参见[创建Logstore](/cn.zh-CN/数据采集/准备工作/管理Logstore.md)。
-   已配置源Logstore的索引。具体操作，请参见[配置索引](/cn.zh-CN/查询和分析/配置索引.md)。

## 步骤一：创建Scheduled SQL作业

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在Project列表区域，单击目标Project。

3.  查询和分析日志。

    1.  在**日志存储** \> **日志库**页签中，单击目标Logstore。

    2.  输入查询和分析语句，选择时间范围，并单击**查询/分析**。

        ```
        * | SELECT COUNT(status) as success_times from website-log where status=200
        ```

        查询分析语句由查询语句和分析语句构成，格式为查询语句\|分析语句，查询分析语句语法请参见[查询语法](/cn.zh-CN/查询和分析/查询语法与功能/查询语法.md)、[SQL分析语法](/cn.zh-CN/查询和分析/SQL分析语法与功能/通用聚合函数.md)。

        **说明：** 本步骤为Scheduled SQL作业的预览操作，用于验证您所使用的查询和分析语句是否正确，执行结果是否有数据。

4.  在统计图表页签中，单击**创建Scheduled SQL**。

5.  创建Scheduled SQL作业。

    1.  完成计算配置，并单击**下一步**。

        |参数|描述|
        |--|--|
        |**作业名**|Scheduled SQL作业的名称。|
        |**描述**|Scheduled SQL作业的描述。|
        |**资源池**|日志服务提供默认型（免费）和增强型（付费）资源池。        -   **默认型**：受到Project级别的SQL并发限制，例如最多允许同时执行15个分析操作。该限制可能影响到在控制台上执行查询和分析操作，从而可能导致Scheduled SQL作业实例计算发生延迟。更多信息，请参见[查询分析与可视化](/cn.zh-CN/产品简介/使用限制/查询分析与可视化.md)。
        -   **增强型**：提供足够的分析并发数，与您在控制台上的查询操作进行资源隔离。根据SQL分析的数据量收取费用，Scheduled SQL作业的调度不收取费用。 |
        |**SQL代码**|显示您在[步骤3](#step_srx_313_nvf)中输入的查询和分析语句。SQL作业运行时，日志服务将执行该查询和分析语句分析数据。 |
        |**目标Region**|目标Project所在地域。|
        |**目标Project**|用于存储SQL分析结果的目标Project名称。|
        |**目标Logstore**|用于存储SQL分析结果的目标Logstore名称。|
        |**写目标授权**|授权日志服务使用默认角色或自定义角色访问目标Logstore，将聚合结果存储到目标Logstore中。具体说明如下：        -   **默认角色**：授权日志服务使用阿里云系统角色AliyunLogETLRole执行对应操作。

**说明：** 仅在首次配置时需要操作，且需要由目标Project所属的阿里云账号完成。

        -   **自定义角色**：授权日志服务使用自定义角色执行对应操作。

在**角色ARN**中输入您自定义角色的ARN，详细说明如下：

            -   如果源Logstore和目标Logstore属于同一阿里云账号，请参见[授予RAM角色访问目标Logstore的权限](/cn.zh-CN/查询和分析/Scheduled SQL/配置自定义角色权限.md)。
            -   如果源Logstore和目标Logstore属于不同的阿里云账号，请参见[授予RAM角色跨账号访问Logstore的权限](/cn.zh-CN/查询和分析/Scheduled SQL/配置自定义角色权限.md)。 |
        |**执行SQL授权**|授权日志服务使用默认角色或自定义角色读取源Logstore数据以及在当前Project下执行SQL分析操作。具体说明如下：        -   **默认角色**：授予日志服务使用阿里云系统角色AliyunLogETLRole执行对应操作。

**说明：** 仅在首次配置时需要操作，且需要由源Project所属的阿里云账号完成。

        -   **自定义角色**：授予日志服务使用自定义角色执行对应操作。

在**角色ARN**中输入您自定义角色的ARN。更多信息，请参见[步骤一：授予RAM角色分析源Logstore日志的权限](/cn.zh-CN/查询和分析/Scheduled SQL/配置自定义角色权限.md)。 |

    2.  完成调度配置，并单击**确定**。

        ![创建SQL作业](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1580969161/p260022.png)

        |参数|描述|
        |--|--|
        |**调度间隔**|调度Scheduled SQL作业的频率，每调度一次Scheduled SQL作业产生一个执行实例。调度间隔决定每个执行实例的调度时间。        -   **每小时**：每小时调度一次Scheduled SQL作业。
        -   **每天**：在每天的某个固定时间点调度一次Scheduled SQL作业。
        -   **每周**：在周几的某个固定时间点调度一次Scheduled SQL作业。
        -   **固定间隔**：按照固定间隔调度Scheduled SQL作业。
        -   **Cron**：通过Cron表达式指定时间间隔，按照指定的时间间隔调度Scheduled SQL作业。

Cron表达式的最小精度为分钟，24小时制，例如0 0/1 \* \* \*表示从00:00开始，每隔1小时运行一次。

当您需要配置时区时，需选择**Cron**模式。常见的时区列表请参见[时区列表](/cn.zh-CN/查询和分析/Scheduled SQL/时区列表.md)。 |
        |**调度时间范围**|调度的时间范围，具体说明如下：        -   **某时间开始**：指定Scheduled SQL作业调用的时间点，即Scheduled SQL作业在该时间点后可被执行，直到被手动停止。
        -   **特定时间范围**：指定Scheduled SQL作业调用的起止时间，即Scheduled SQL作业仅在该时间范围内可被执行。
**说明：** 实例的调度时间必须在该范围内，超出该范围时，Scheduled SQL作业不再产生新实例。 |
        |**SQL时间窗口**|Scheduled SQL作业运行时，日志服务仅分析该时间范围内的日志，该时间范围不能大于**调度间隔**的5倍且不能超过1天。更多信息，请参见[时间表达式语法](/cn.zh-CN/查询和分析/Scheduled SQL/时间表达式语法.md)。例如，**调度间隔**为**固定间隔10分钟**，**开始时间**为**2021-04-01 00:00:00**，**延迟执行**为**30秒**，**SQL时间窗口**为**\[@m-10m,@m\)**，则SQL作业运行时，在00:00:30时刻生成第一个执行实例，分析的是\[23:50:00~00:00:00\)期间的日志。更多信息，请参见[调度与执行场景](/cn.zh-CN/查询和分析/Scheduled SQL/工作原理.md)。 |
        |**SQL超时**|执行SQL分析操作失败时自动重试的阈值。当重试时间超过指定的最大时间或者重试次数超过最大次数时，该执行实例结束，状态为失败。您可以根据失败原因，手动重试该实例。具体操作，请参见[步骤四：重试Scheduled SQL作业实例](#section_h6h_b64_y3y)。|
        |**延迟执行**|调度时间点往后延迟执行的时间。取整范围：0~120，单位：秒。当数据写入Logstore存在延迟等情况时，可通过延迟执行来保证数据的完整性。 |


## 步骤二：查看Scheduled SQL作业详情

1.  在左侧导航栏，选择**作业** \> **SQL执行作业**。

2.  单击目标SQL作业（request-status-count）。

3.  查看Scheduled SQL作业详情。

    包括SQL作业创建时间、最后修改时间、执行实例等信息。

    ![SQL作业](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6523777161/p259819.png)


## 步骤三：查看执行实例

在Scheduled SQL作业运行期间，日志服务会根据调度间隔生成多个执行实例。您可以在执行实例区域查看目标Scheduled SQL作业的所有执行实例。重要信息说明如下：

|参数|说明|
|--|--|
|**实例ID**|Scheduled SQL执行实例的唯一标识。|
|**作业时间**|Scheduled SQL作业的运行时间。|
|**SQL查询区范围**|SQL分析的时间范围，即该实例分析的是该时间范围内的数据。|
|**处理数据量**|SQL分析相关的数据量。具体说明如下：-   **SQL处理行数**：该执行实例在SQL时间窗口内读取到的日志行数。参与计算的数据量。
-   **SQL结果行数**：该执行实例在执行SQL分析后，分析结果中对应的d日志行数。写入目标日志库的数据量。
-   **SQL处理字节**：该执行实例在SQL时间窗口内读取到的日志字节数。参与计算的数据量。
-   **Logstore行数**：该实例将SQL分析结果成功写入目标日志库的行数。实际写入目标日志库的数据量。 |
|**执行状态**|Scheduled SQL执行实例的执行状态，包括运行中（RUNNING）、重试中（STARTING）、成功（SUCCESSED）、失败（FAILED）。|

## 步骤四：重试Scheduled SQL作业实例

当Scheduled SQL作业实例状态为成功或失败时，您可以重新运行Scheduled SQL作业实例。例如某实例状态为失败，且失败原因为授权不正确，则您可以修改授权配置后，单击目标实例对应的**重试**。

**说明：** 一般在补数据场景，可对执行成功的实例进行重试，但建议谨慎操作。一般情况下，只有失败的任务需要重试。

## 相关操作

在SQL作业执行页面，您还可以执行删除、修改Scheduled SQL作业的操作。

**警告：** 删除Scheduled SQL作业后，不可恢复，请谨慎操作。

