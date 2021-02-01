# 查询和分析JSON日志

本文以查询和分析JSON类型的网站日志为例，帮助您快速上手JSON日志的查询和分析操作。

已采集到JSON日志。更多信息，请参见[使用极简模式采集日志](/intl.zh-CN/数据采集/Logtail采集/采集文本日志/使用极简模式采集日志.md)。

## 注意事项

在查询和分析JSON日志中的字段时，需注意以下事项：

-   查询和分析语句格式为`查询语句|分析语句`。在分析语句中，您必须使用双引号（""）包裹字段名称，使用单引号（''）包裹字符串。
-   您需为目标字段加上所有的父路径，格式为KEY1.KEY2.KEY3。例如concent.request.request\_length。
-   日志服务支持查询和分析JSON对象中的叶子节点，但不支持查询和分析包含叶子节点的子节点。
-   日志服务不支持查询和分析值为JSON数组的字段，也不支持查询和分析JSON数组中的字段。

## 步骤1：配置索引

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在Project列表区域，单击目标Project。

3.  在**日志存储** \> **日志库**页签中，单击目标Logstore。

4.  在Logstore的查询和分析页面，单击页面右上角的**查询分析属性** \> **属性**。

    如果您还未开启索引，请单击页面右上角的**开启索引**。

5.  配置字段索引。

    您可以手动逐条配置索引，也可以单击**自动生成索引**，日志服务会根据预览数据中的第一条日志自动配置索引。

    **说明：**

    -   如果您要使用分析功能，必须在配置索引时打开对应字段的统计功能。更多信息，请参见[配置索引](/intl.zh-CN/查询与分析/配置索引.md)。
    -   日志服务默认已为部分保留字段开启索引。更多信息，请参见[保留字段](/intl.zh-CN/产品简介/限制说明/保留字段.md)。
    -   日志服务支持JSON对象中的叶子节点建立索引，但不支持包含叶子节点的子节点建立索引。例如您可以为request\_time字段建立索引，但不能为time字段建立索引。
    -   日志服务不支持值为JSON数组的字段建立索引，也不支持JSON数组中的字段建立索引。例如body\_bytes\_sent字段的值为JSON数组，不能建立索引。
    -   为JSON对象中的字段配置索引时，需加父路径，格式为KEY1.KEY2。例如time.request\_time。
    ![配置索引](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1957881161/p211906.png)

6.  单击**确定**。

    **说明：** 配置索引后，只对新采集的数据生效。如果您要查询历史数据，请使用重建索引功能。具体操作，请参见[重建索引](/intl.zh-CN/查询与分析/查询语法与功能/重建索引.md)。


## 步骤2：查询日志

您可以在Logstore的查询和分析页面，输入查询语句，选择时间范围，单击**查找/分析**，进行日志查询操作。

-   查询请求状态为200的日志。

    ```
    content.status:200
    ```

-   查询请求长度大于70的日志。

    ```
    content.request.request_length > 70
    ```

-   查询GET请求的日志。

    ```
    content.request.request_method:GET
    ```


## 步骤3：分析日志

您可以在Logstore的查询和分析页面，输入查询和分析语句，选择时间范围，单击**查找/分析**，进行日志分析操作。

-   统计不同请求状态对应的日志数量。

    ```
    * | SELECT "content.status", COUNT(*) AS PV GROUP BY "content.status"
    ```

    ![PV](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5038271161/p232778.png)

-   计算不同请求时长对应的请求数量，并按照请求时长进行升序排序。

    ```
    * | SELECT "content.time.request_time", COUNT(*) AS count GROUP BY "content.time.request_time" ORDER BY "content.time.request_time"
    ```

    ![请求时长](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5038271161/p232779.png)

-   计算不同请求方法对应的平均请求时长。

    ```
    * | SELECT avg("content.time.request_time") AS avg_time,"content.request.request_method"  GROUP BY "content.request.request_method"
    ```

    ![平均请求时长](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5038271161/p232780.png)


## 参考信息：日志样例

JSON日志样例如下所示：

![日志样例](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1957881161/p211913.png)

