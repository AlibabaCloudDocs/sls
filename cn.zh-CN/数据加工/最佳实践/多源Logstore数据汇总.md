# 多源Logstore数据汇总

日志服务支持对每一个源Logstore配置一个数据加工任务，实现多源Logstore数据汇总。本文介绍多源Logstore数据汇总的典型应用场景和对应的操作方法。

某资讯网站业务分布全球，不同资讯频道的用户访问日志被采集存储在阿里云不同账号中的Logstore，现有需求将同一目标区域英国（伦敦）的用户请求访问信息日志进行汇总从而进行后续的查询与分析。对此需求，日志服务提供数据加工功能，可以使用e\_output函数将加工结果汇总到目标Logstore中。

## 跨账号多源Logstore数据汇总

-   原始日志
    -   账号1中的原始日志，其Project地域位于英国（伦敦），Project名称为Project\_1，Logstore名称为Logstore\_1。

        ```
        "日志1"z
        request_id: 1
        http_host:  m1.abcd.com
        http_status:  200
        request_method:  GET
        request_uri:  /pic/icon.jpg
        
        "日志2"
        request_id: 2
        http_host:  m2.abcd.com
        http_status:  301
        request_method:  POST
        request_uri:  /data/data.php
        ```

    -   账号2中的日志，其Project地域为英国（伦敦），Project名称为Project\_2，Logstore名称为Logstore\_2。

        ```
        "日志1"
        request_id: 3
        host:  m3.abcd.com
        status:  404
        request_method:  GET
        request_uri:  /category/abc/product_id
        
        "日志2"
        request_id: 4
        host:  m4.abcd.com
        status:  200
        request_method:  GET
        request_uri:  /data/index.html
        ```

-   加工目标
    -   将账号1的Logstore\_1和账号2的Logstore\_2中所有`http_status`为200的日志事件汇总到账号3的Logstore\_3中。
    -   统一账号1的Logstore\_1和账号2的Logstore\_2中日志事件的字段名称。将`host`统一为`http_host`，`status`统一为`http_status`。
-   SLS DSL规则
    -   在账号1的Logstore\_1中配置如下加工规则，并且在创建数据加工规则面板中，配置目标名称为target\_logstore，目标Project为Project\_3，目标库为Logstore\_3，以及授权方式及相关信息。详细操作请参见[创建数据加工任务](/cn.zh-CN/数据加工/创建数据加工任务.md)。

        ```
        e_if(e_match("http_status", "200"), e_output("target_logstore"))
        ```

        ![加工规则](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8018201061/p58876.png)

    -   在账号2的Logstore\_2中配置如下加工规则，参见账号1配置，并且在创建数据加工规则面板中，配置目标名称为target\_logstore，目标Project为Project\_3，目标库为Logstore\_3，以及授权方式及相关信息。

        ```
        e_if(e_match("status", "200"), e_compose(e_rename("status", "http_status", "host", "http_host"), e_output("target_logstore")))
        ```

-   加工结果

    账号3中汇总的日志，其Project地域位于英国（伦敦），Project名称为Project\_3，Logstore名称为Logstore\_3。

    ```
    "日志1"
    request_id: 1
    http_host:  m1.abcd.com
    http_status:  200
    request_method:  GET
    request_uri:  /pic/icon.jpg
    
    "日志2"
    request_id: 4
    http_host:  m4.abcd.com
    http_status:  200
    request_method:  GET
    request_uri:  /data/index.html
    ```


