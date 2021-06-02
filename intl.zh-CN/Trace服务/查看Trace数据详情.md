# 查看Trace数据详情

接入Trace数据后，您可以查看Trace数据详情，包括Trace轨迹图、Span数据详情等。

## 操作步骤

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  打开Trace分析页面。

    1.  在日志应用区域，单击**Trace服务**。

    2.  在Trace实例列表中，单击目标实例。

    3.  在左侧导航栏中，单击Trace分析。

3.  在Trace分析页签中，单击目标Trace数据。

4.  查看Trace数据详情。

    ![trace数据详情](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6323556161/p254100.png)

    |序号|说明|
    |--|--|
    |①|Trace轨迹图用于展示整个跟踪链路及Span数据分布情况。详细说明如下：    -   不同的服务通过不同的颜色进行区分。例如本示例中，蓝色代表front-end服务。
    -   轨迹图中的黑线长度代表Span本身的耗时。
    -   时间轴表示整条Trace数据的时间跨度。 |
    |②|该区域中的每一行代表一条Span数据，并展示父级Span和子级Span之间的层级关系。Span数据前面的标号表示该父级Span所拥有的子级Span数量。在该区域，您可以执行如下操作：    -   单击![标号](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6323556161/p254319.png)图标，折叠或展开Span数据。
    -   选中目标Span数据，单击![聚合](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6323556161/p254320.png)图标，系统将只显示该Span数据，实现Span数据聚焦。
    -   单击![取消聚集](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1674556161/p254336.png)图标，取消Span数据聚焦。 |
    |③|单击目标Span数据后，该区域将展示Span数据相关的信息，包括：    -   服务：Span数据所属的服务名。
    -   调用：操作名称。
    -   属性：Span数据的元数据信息。
    -   资源：生成该Span数据所需调用的资源信息。
    -   详细：Span数据的具体内容。详细的字段说明，请参见[Trace数据格式](/intl.zh-CN/Trace服务/Trace数据格式.md)。
    -   日志：Span数据相关的日志内容。 |


