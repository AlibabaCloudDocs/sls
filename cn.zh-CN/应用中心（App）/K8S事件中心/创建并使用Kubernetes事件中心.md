# 创建并使用Kubernetes事件中心

本文介绍如何创建Kubernetes事件中心及相关操作，包括查看事件总览、查询事件详情、查看Pod生命周期、配置告警和自定义查询等操作。

Kubernetes事件中心记录了集群的状态变更，包括创建Pod、运行Pod、删除Pod、组件异常等。Kubernetes事件中心实时汇聚Kubernetes中的所有事件并提供存储、查询、分析、可视化、告警等能力。

## 免费策略

Kubernetes事件中心关联的Logstore在90天内免费（每天允许免费写入256M数据，相当于25万条事件。默认一个Kubernetes线上集群每天产生的事件在1000条左右）。事件存储时间默认为90天，因此如果您不调整事件保存时间，可一直免费使用Kubernetes事件中心。例如：

-   不调整存储时间（默认90天），集群每天产生1000条事件，则事件中心永久免费。
-   调整存储时间为105天，集群每天产生1000条事件，则超过90天后，事件中心每天收取的费用约0.1元，费用详情请参见[按量付费](/cn.zh-CN/产品定价/按量付费.md)。

## 步骤一：创建事件中心

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在日志应用区域，单击**K8s事件中心**。

3.  在事件中心管理页面，单击**添加**。

4.  在添加事件中心页面，配置相关参数。

    -   选择**已有Project**，可从**Project**下拉框中选择已创建的Project。
    -   选择**从容器服务选择K8s集群**，可从**K8s集群**下拉框中选择已创建的K8s集群。通过此方式创建事件中心，默认创建一个名为k8s-log-\{cluster-id\}的Project。
5.  单击**下一步**，完成创建。

    **说明：** 创建事件中心后，默认在您选择的日志服务Project中创建一个名为k8s-event的Logstore，并创建相关联的报表和告警等。


## 步骤二：部署Eventer和NodeProblemDetector

您需要在Kubernetes集群中配置事件采集和node-problem-detector后才能正常使用K8s事件中心。

-   阿里云Kubernetes配置方式

    阿里云Kubernetes应用市场中的ack-node-problem-detector已集成node-problem-detector和事件采集功能，您只需要部署该组件即可，该组件详细部署请参见[场景3：使用node-problem-detector与eventer实现节点异常告警](/cn.zh-CN/Kubernetes集群用户指南/可观测性/监控管理/事件监控.md)。

    1.  登录[容器服务控制台](https://cs.console.aliyun.com/)。
    2.  在左侧导航栏中，选择**市场** \> **应用目录**。
    3.  在阿里云应用页签下，单击**ack-node-problem-detector**。
    4.  在**参数**页签下，修改eventer节点中的相关信息。

        -   enabled：将**eventer** \> **sinks** \> **sls**下的enabled设置为true。
        -   topic：可选，设置为您的集群名称，只支持英文字母a-z、下划线（\_）、连接号（-）。
        -   project：设置为您创建事件中心时的Project名称。
        -   logstore：只能设置为k8s-event。
        ```
          sinks:
             sls:
               enabled: true
               # If you want the monitoring results to be notified by sls, set enabled to true.
               topic: "my-cluster"
               project:  "{sls-project-name}"
               # You can view the project information by logging in to the
               # SLS console. Please fill in the name of the project here.
               # eg: your project name is k8s-log-cc18a5f3443dhdss22654da,
               # then you can fill k8s-log-cc18a5f3443dhdss22654da to project label.
               logstore: "k8s-event"
               # You can view the project information by logging in to the
               # SLS console. Please fill the logstore address in here.
        ```

    5.  单击**创建**，完成部署。
-   自建Kubernetes配置方式
    1.  配置事件采集。更多信息，请参见[采集Kubernetes事件](/cn.zh-CN/数据采集/Logtail采集/采集容器日志/采集Kubernetes事件.md)。
    2.  配置node-problem-detector，详情请参见[Github](https://github.com/kubernetes/node-problem-detector)。

## 步骤三：使用事件中心

创建K8s事件中心并部署Eventer和NodeProblemDetector后，即可使用K8s事件中心，包括查看事件总览、查询事件详情、查看Pod生命周期、配置告警和自定义查询等操作。

在K8s事件中心页面，找到目标事件中心实例，单击**![k8s事件中心-002](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7706498951/p77384.png)**图标，可进行如下操作。

|操作|说明|
|--|--|
|查看事件总览|单击**事件总览**，查看核心事件的汇总统计信息。例如：总体错误数以及和昨天/上周的对比、告警项统计、重要事件趋势、Pod OOM详细信息等。 **说明：** 目前Pod OOM信息不能精确到Pod，只能定位到事件发生的节点、进程名、进程号。您可以通过自定义查询查找Pod OOM发生时间点附近的Pod重启事件，以此定位到具体的Pod。 |
|查询事件详情|单击**事件详情查询**，查看按照各种维度（事件等级、事件类型、事件目标、Host、Namespace、Name）过滤后的事件的统计信息以及详情。|
|查看Pod生命周期|单击**Pod生命周期**，以图形化方式展示Pod整个生命周期中的事件信息，还可通过事件等级筛选重要的Pod事件。|
|配置告警|单击**告警配置**，配置事件的告警，具体操作请参见表格下方的[操作步骤](#step_nmt_vqo_f3p)。|
|自定义查询|单击**自定义查询**，自定义查询条件查询相关信息，查询条件请参见[查询与分析语法规则](/cn.zh-CN/查询与分析/查询简介.md)。 事件中心的所有事件都保存在Logstore中，您可以使用Logstore中的所有功能，例如自定义查询、消费事件进行自定义处理、创建自定义报表、创建自定义告警等。

如果您要访问事件中心所在的Project，可通过以下两种方式获取Project名称。

-   通过自定义查询页面的URL定位到Project。URL规则为`[https://sls.console.aliyun.com/lognext/app/k8s-event/project/k8s-log-xxxx/logsearch/k8s-event](https://sls.console.aliyun.com/lognext/app/k8s-event/project/k8s-log-c0ae5df15fbf34b47ba3a9684e6ee2bee/logsearch/k8s-event)`，Project字段的后一个字段即为日志服务Project名称，例如k8s-log-xxxx。
-   在集群管理页签的事件中心列表中，查看目标事件中心对应的Project名称。 |
|配置自定义告警|除了内置的告警外，事件中心还支持配置自定义告警。 在自定义查询页面，输入对应K8s事件的查询语句，单击**另存为告警**完成自定义告警配置。更多信息，请参见[告警简介](/cn.zh-CN/可视化与告警/告警/简介.md)。

例如：创建一个FailedPreStopHook的告警，您可以在查询页面中输入`* and FailedPreStopHook | SELECT "object-namespace", "object-name", "reason", "message"`，单击**另存为告警**，配置参数后保存即可。

**说明：** 如果您自定义配置的告警名称是前缀K8s，则该告警配置会在目标事件的告警配置页签的全部告警事件显示中，否则只显示在告警详情中。 |

配置告警具体操作如下所示。

1.  在K8s事件中心，找到目标事件中心实例，单击**![k8s事件中心-002](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7706498951/p77384.png)**图标。

2.  单击**告警配置**，进入告警配置页面。

3.  添加通知方式。

    1.  单击**添加通知方式**。

    2.  在添加通知方式页面，配置相关参数。

        |参数|说明|
        |--|--|
        |通知方式名称|通知方式的名称。|
        |告警间隔|两次告警通知之间的时间间隔，默认为5分钟。 **说明：** 建议告警间隔最小设置为2分钟，防止收到过多的告警信息。 |
        |通知类型|包括短信、语音、邮件、钉钉机器人、WebHook自定义和通知中心，可选择一种或多种通知类型。更多信息，请参见[通知方式](/cn.zh-CN/可视化与告警/告警/通知方式.md)。|

    3.  单击**确定**。

4.  开启告警通知

    1.  在全部告警事件区域，单击**修改**。

    2.  找到待开启的告警事件，单击开启图标，并选择合适的告警通知。

        **说明：** 建议您先开启所有告警，若发现告警通知太多，可适当关闭告警或调整通知间隔。

    3.  单击**保存**。


## 删除事件中心

在**K8s事件中心** \> **集群管理**页面中，找到目标事件中心实例，单击**![k8s事件中心](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7706498951/p85814.png)**图标，删除事件中心。

## 常见问题

-   K8s事件中心无数据。

    部署好K8s事件中心后，新产生的事件会自动采集到K8s事件中心，您可以在自定义查询页面进行搜索（建议将右上角时间范围调整到1天）。如果无数据，一般有两个原因：

    -   部署K8s事件中心后，K8s集群还未产生事件。

        您可以通过`kubectl get events --all-namespaces`命令检查集群内是否有新事件产生。

    -   部署Eventer和NodeProblemDetectors时，参数填写错误。
        -   如果您使用的是阿里云Kubernetes集群，请在**容器服务控制台** \> **应用** \> **发布**中，找到对应的集群，单击**ack-node-problem-detector**后的**更新**，检查参数配置，详情配置请参见[步骤二：部署Eventer和NodeProblemDetector](#section_7eu_9y2_50c)。
        -   如果您使用的是自建Kubernetes集群，参数配置请参见[采集Kubernetes事件](/cn.zh-CN/数据采集/Logtail采集/采集容器日志/采集Kubernetes事件.md)。
-   如何查看事件对应容器的日志？
    -   如果您使用的是阿里云Kubernetes集群，请在**容器服务控制台** \> **应用** \> **容器组**中，找到目标集群，将**命名空间**选择为**kube-system**，在搜索框中输入eventer关键词找到目标容器，在其详情页面查看日志。
    -   如果您使用的是自建Kubernetes集群，请查看namespace为**kube-system**下文件名前缀为eventer-sls的Pod日志。

