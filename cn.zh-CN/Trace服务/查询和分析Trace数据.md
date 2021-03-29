# 查询和分析Trace数据

接入Trace数据后，您可以执行查询Trace数据、设置分组统计等操作。

## 操作步骤

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  打开Trace分析页面。

    1.  在日志应用区域，单击**Trace服务**。

    2.  在Trace实例列表中，单击目标实例。

    3.  在左侧导航栏中，单击Trace分析。

3.  设置查询条件，选择时间范围，并单击**查询/分析**。

    日志服务已预设Service、Operation、Duration、Status、Attribute、Resource等字段的查询条件，您只需根据需求选择即可。关于字段的详细说明，请参见[Trace数据格式](/cn.zh-CN/Trace服务/Trace数据格式.md)。

    **说明：**

    -   查询条件中**Duration**配置项的单位为毫秒，而实际延迟时间Duration的单位为微秒。例如您配置**Duration**为10 ms，则在过滤条件表达式中显示为**duration \>= 10000**。
    -   **Duration**中的时间区间默认为左闭右开形式。
    -   **Attribute**、**Resource**字段为JSON Object类型，日志服务支持按照该字段中的Key、Value进行过滤。
    例如，您要查询最近一小时内延迟时间大于10 ms的user服务的Trace数据，可设置过滤条件如下图所示。

    ![Trace数据过滤条件](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0924946161/p254120.png)

4.  在Trace分析页签中，查看查询结果。

    ![查询结果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0924946161/p254134.png)

5.  设置分组统计。

    1.  在Trace分析页签中，单击**分组统计**。

    2.  选择分组统计的条件，例如选择**service**。

    3.  查看分组统计结果。

        日志服务将根据**service**维度，列出各个服务的相关信息（例如Span个数、OPS、平均延迟等）。

        ![分组统计](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0924946161/p254149.png)


