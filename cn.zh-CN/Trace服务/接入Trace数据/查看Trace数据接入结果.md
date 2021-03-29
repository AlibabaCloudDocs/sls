# 查看Trace数据接入结果

当您接入Trace数据到日志服务后，您可以通过查询相关的日志确认是否已成功接入Trace数据到日志服务。

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  打开**自定义查询**页面。

    1.  在日志应用区域，单击**Trace服务**。

    2.  在Trace实例列表中，单击目标实例。

    3.  在左侧导航栏中，单击**自定义查询**。

3.  输入查询语句。

    例如您要确认是否已成功接入user服务相关的Trace数据，可以执行如下查询语句。

    ```
    service:user
    ```

4.  查看查询结果。

    如果查询结果中包含user服务的日志，则表示您已成功接入user服务相关的Trace数据。

    ![查询结果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7232956161/p254447.png)


