# Markdown图表（下线）

本文介绍Markdown图表的常见语法及如何在日志服务的仪表盘中创建Markdown图表。

-   已采集日志数据。更多信息，请参见[数据采集](/cn.zh-CN/数据采集/采集方式.md)。
-   已创建仪表盘。更多信息，请参见[创建仪表盘](/cn.zh-CN/可视化与告警/仪表盘/创建仪表盘.md)。

**说明：** Markdown图表功能暂时无法使用。

在查询分析日志数据时，将多个统计图表添加到仪表盘中，有助于您快速查看多项分析结果、实时监控多项业务的状态信息。日志服务还支持在仪表盘中增加Markdown图表，该图表使用Markdown语言编辑，您可以在Markdown图表中插入图片、链接、视频等多种元素，使您的仪表盘页面更加友好。

Markdown图表可以根据不同的需求来创建。您可以在Markdown图表中插入背景信息、图表说明、页面注释和扩展信息等文字内容，优化仪表盘的信息表达；插入快速查询或其他Project的仪表盘链接，方便跳转到其他查询页面；插入自定义的图片和视频，让您的仪表盘信息更加丰富、功能更为灵活。

通过Markdown图表可以添加自定义链接到当前Project的其他仪表盘，同时还可以插入图片以便快速区分。当需要对图表参数进行介绍时，也可以插入Markdown图表进行说明。

![展示效果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7622966951/p7247.png)

## 添加Markdown图表

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在Project列表区域，单击目标Project。

3.  在左侧导航栏中，单击![配置监控与告警-001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2453749951/p104975.png)。

4.  单击目标仪表盘。

5.  在仪表盘页面的右上方，单击**编辑**。

6.  将操作栏中的![markdown](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7616317951/p36999.png)拖动到指定位置，即可创建Markdown图表。

7.  双击Markdown图表。

8.  在markdown编辑页面，配置如下参数，并单击**确定**。

    ![创建markdown图表](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7622966951/p32307.png)

    |参数|说明|
    |:-|:-|
    |图表名称|您创建的Markdown图表名称。|
    |显示边框|打开**显示边框**开关，为Markdown图表增加边框。|
    |显示标题|打开**显示标题**开关，为Markdown图表增加标题。|
    |显示背景|打开**显示背景**开关，为Markdown图表添加白色背景。|
    |绑定查询|打开**绑定查询**开关后，可设置查询属性，在Markdown图表中动态显示查询结果。     1.  选择待查询的日志库。
    2.  在查询框中输入查询分析语句并设置查询的时间范围，单击**查询**。

详细说明请参考[查询简介](/cn.zh-CN/查询与分析/查询简介.md)。

**说明：** 查询结果相对于指定的时间范围来说，有1min以内的误差。

显示当前查询结果的第一条数据。

    3.  单击字段旁的⊕，可将查询结果放置在**markdown内容**中光标所在位置。 |
    |**Markdown内容**|在**Markdown内容**中输入您的Markdown语句，右侧的**图表展示**区域会实时展示预览界面。您可以根据预览内容调整Markdown语句。Markdown语句请参见[常用Markdown语法](#section_zhh_y2w_m2b)。|

9.  单击**保存**。


## 修改Markdown图表

1.  在仪表盘页面右上角，单击**编辑**。

2.  修改图表位置和大小。

    您可以拖动Markdown图表到指定位置，拖动图表右下角调整图表大小。

3.  修改Markdown图表属性。

    1.  双击目标Markdown图表。

    2.  在markdown编辑页面，修改相关属性配置，并单击**确定**。

        您可以修改图表名称、显示设置、查询设置、Markdown内容，参数详情说明请参见[添加Markdown图表](#section_wg3_1tv_m2b)。


## 删除图表

1.  在仪表盘页面右上角，单击**编辑**。

2.  单击目标Markdown图表中的**![配置监控与告警](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2453749951/p104976.png)** \> **删除**。

3.  在仪表盘页面右上角，单击**保存**。


## 常用Markdown语法

-   标题
    -   Markdown语句

        ```
        # 一级标题
        ## 二级标题
        ### 三级标题
        ```

    -   效果展示

        ![标题](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7622966951/p7249.png)

-   链接
    -   Markdown语句

        ```
        ### 目录
        
        [图表说明](https://help.aliyun.com/document_detail/69313.html)
        
        [仪表盘](https://help.aliyun.com/document_detail/59324.html)
        ```

    -   效果展示

        ![链接](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7616317951/p7250.png)

-   图片
    -   Markdown语句

        ```
        <div align=center>
        
        ![Alt txt][id]
        
        With a reference later in the document defining the URL location
        
        [id]: https://octodex.github.com/images/dojocat.jpg  "The Dojocat"
        ```

    -   图片预览

        ![图片](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7622966951/p7251.png)

-   特殊标记
    -   Markdown语句

        ```
        ---
        
        __Advertisement :)__
        
        ==some mark==   `some code`
        > Classic markup: :wink: :crush: :cry: :tear: :laughing: :yum:
        >> Shortcuts (emoticons): :-)  8-) ;)
        
        __This is bold text__
        
        *This is italic text*
        
        ---
        ```

    -   效果展示

        ![特殊标记](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7622966951/p7252.png)


