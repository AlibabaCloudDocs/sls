# 多目标Logstore数据分发

本文介绍多目标Logstore数据分发的各种场景（动态分发、跨账号分发、跨账号动态分发、复杂场景动态分发等）及操作步骤。

日志服务数据加工功能支持将加工结果分发到多个目标Logstore中。您可以在保存数据加工结果时，设置不同账号的AccessKey信息，将加工结果分发到不同账号的Logstore中；您也可以使用e\_output函数、e\_coutput函数动态获取目标Project和Logstore，将加工结果分发到不同的Logstore中。

**说明：**

-   最多跨20个账号分发加工结果。
-   如果您使用的是e\_output函数，则通过e\_output函数输出日志到指定的Logstore后，对应的日志不再执行后面的加工规则。如果您希望输出日志到指定的Logstore后，对应的日志还能继续执行后面的加工规则，可使用e\_coutput函数，详情请参见[e\_output、e\_coutput](/cn.zh-CN/数据加工/数据加工语法/全局操作函数/事件操作函数.md)。本文以e\_output函数为例。

## 场景1：跨账号分发

例如：某网站所有的访问日志存储在一个Logstore中，现在希望将不同状态的日志分发存储到不同账号下的Logstore中。

针对上述需求，您可以通过日志服务数据加工功能解决。

-   原始日志

    ```
    http_host:  m1.abcd.com
    http_status:  200
    request_method:  GET
    request_uri:  /pic/icon.jpg
    scheme:  https
    
    http_host:  m2.abcd.com
    http_status:  301
    request_method:  POST
    request_uri:  /data/data.php
    scheme:  http
    
    http_host:  m3.abcd.com
    http_status:  404
    request_method:  GET
    request_uri:  /category/abc/product_id
    scheme:  https
    
    http_host:  m4.abcd.com
    http_status:  504
    request_method:  GET
    request_uri:  /data/index.html
    scheme:  https
    ```

-   分发需求

    -   http\_status为2XX的日志存储到存储目标target0中的Logstore0中，并设置日志主题为success\_event。
    -   http\_status为3XX的日志分发到存储目标target1中的Logstore1中，并设置日志主题为redirection\_event。
    -   http\_status为4XX的日志分发到存储目标target2中的Logstore2中，并设置日志主题为unauthorized\_event。
    -   http\_status为5XX的日志分发到存储目标target3中的Logstore3中，并设置日志主题为internal\_server\_error\_event。
    其中target0在账号A下，target1、target2和target3在账号B下。

-   加工规则

    ```
    e_switch(e_match("status", r"2\d+"), e_set("__topic__", "success_event"),
             e_match("status", r"3\d+"), e_compose(e_set("__topic__", "redirection_event"), e_output("target1")),
             e_match("status", r"4\d+"), e_compose(e_set("__topic__", "unauthorized_event"), e_output("target2")),
             e_match("status", r"5\d+"), e_compose(e_set("__topic__", "internal_server_error_event`"), e_output("target3"))
        )
    ```

-   存储目标

    在创建数据加工规则页面中，配置存储目标详情参数说明请参见[创建数据加工任务](/cn.zh-CN/数据加工/创建数据加工任务.md)。

    ![数据分发](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7581341061/p58874.png)

    |标号|存储目标|目标Project和Logstore|AccessKey|
    |--|----|------------------|---------|
    |1|target0|Project0、Logstore0|账号A的AccessKey信息|
    |2|target1|Project1、Logstore1|账号B的AccessKey信息|
    |3|target2|Project2、Logstore2|账号B的AccessKey信息|
    |4|target3|Project3、Logstore3|账号B的AccessKey信息|

-   加工结果

    ```
    ## http\_status为2XX的日志，分发到账号A下logstore0中。
    
    __topic__:  success_event
    http_host:  m1.abcd.com
    http_status:  200
    request_method:  GET
    request_uri:  /pic/icon.jpg
    scheme:  https
    
    ## http\_status为3XX的日志，分发到账号B中的Logstore1中。
    
    __topic__:  redirection_event
    http_host:  m2.abcd.com
    http_status:  301
    request_method:  POST
    request_uri:  /data/data.php
    scheme:  http
    
    ##http\_status为4XX的日志，分发到账号B中的Logstore2中。
    
    __topic__: unauthorized_event
    http_host:  m3.abcd.com
    http_status:  404
    request_method:  GET
    request_uri:  /category/abc/product_id
    scheme:  https
    
    ## http\_status为5XX的日志，分发到账号B中的Logstore3中。
    
    __topic__: internal_server_error_event
    http_host:  m4.abcd.com
    http_status:  504
    request_method:  GET
    request_uri:  /data/index.html
    scheme:  https
    ```


## 场景2：简单场景动态分发

例如：某网站所有的访问日志存储在一个Logstore中，现在希望根据project字段和logstore字段分发存储到不同的Logstore中。

针对上述需求，您可以通过日志服务数据加工功能解决。

-   原始日志

    ```
    __tag__:type: dynamic_dispatch
    host:  a.b.c.com
    project: Project1
    logstore: Logstore1
    http_status:  200
    request_method:  GET
    request_uri:  /pic/icon.jpg
    scheme:  https
    
    __tag__:type: dynamic_dispatch
    host:  m.n.q.com
    project: Project1
    logstore: Logstore2
    http_status:  301
    request_method:  POST
    request_uri:  /data/data.php
    scheme:  http
    
    __tag__:type:  dynamic_dispatch
    host:   e.f.d.com
    project: Project2
    logstore: Logstore1
    http_status:  404
    request_method:  GET
    request_uri:  /category/abc/product_id
    scheme:  https
    
    __tag__:type: dynamic_dispatch
    host:   p.q.t.com
    project: Project2
    logstore: Logstore2
    http_status:  504
    request_method:  GET
    request_uri:  /data/index.html
    scheme:  https
    ```

-   分发需求
    -   根据日志中project字段和logstore字段的值的不同，进行动态分发。
    -   为日志添加\_\_tag:\_\_type字段，值为dynamic\_dispatch。
-   加工规则

    ```
    e_output(project=v("project"), logstore=v("logstore"), tags={"type": "dynamic_dispatch"})
    ```

    e\_output函数动态提取project字段和logstore字段的值进行日志分发。

-   存储目标

    在创建数据加工规则页面中，设置默认存储目标即可，用于存储其他在加工过程中没有被丢弃的日志。

    **说明：** 此场景中，动态分发的目标Project和Logstore由e\_output函数中的配置决定，与创建数据加工规则页面中默认存储目标（标号1）中配置的目标Project和Logstore无关。

    ![动态分发](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1556813061/p135945.png)

-   加工结果

    ```
    ## 分发到Project1下的Logstore1中。
    
    __tag__:type: dynamic_dispatch
    host:  a.b.c.com
    project: Project1
    logstore: Logstore1
    http_status:  200
    request_method:  GET
    request_uri:  /pic/icon.jpg
    scheme:  https
    
    ## 分发到Project1下的Logstore2中。
    
    __tag__:type: dynamic_dispatch
    host:  m.n.q.com
    project: Project1
    logstore: Logstore2
    http_status:  301
    request_method:  POST
    request_uri:  /data/data.php
    scheme:  http
    
    ## 分发到Project2下的Logstore1中。
    
    __tag__:type:  dynamic_dispatch
    host:   e.f.d.com
    project: Project2
    logstore: Logstore1
    http_status:  404
    request_method:  GET
    request_uri:  /category/abc/product_id
    scheme:  https
    
    ## 分发到Project2下的Logstore2中。
    
    __tag__:type: dynamic_dispatch
    host:   p.q.t.com
    project: Project2
    logstore: Logstore2
    http_status:  504
    request_method:  GET
    request_uri:  /data/index.html
    scheme:  https
    ```


## 场景3：跨账号动态分发

例如：某网站所有的访问日志存储在一个Logstore中，现在希望根据project字段和logstore字段动态分发日志到不同账号下的Logstore中。

针对上述需求，您可以通过日志服务数据加工功能解决。

-   原始日志

    ```
    host:  a.b.c.com
    project: Project1
    logstore: Logstore1
    http_status:  200
    request_method:  GET
    request_uri:  /pic/icon.jpg
    scheme:  https
    
    host:  m.n.q.com
    project: Project1
    logstore: Logstore2
    http_status:  301
    request_method:  POST
    request_uri:  /data/data.php
    scheme:  http
    
    host:   e.f.d.com
    project: Project2
    logstore: Logstore1
    http_status:  404
    request_method:  GET
    request_uri:  /category/abc/product_id
    scheme:  https
    
    host:   p.q.t.com
    project: Project2
    logstore: Logstore2
    http_status:  504
    request_method:  GET
    request_uri:  /data/index.html
    scheme:  https
    ```

-   分发需求

    根据日志中project和logstore字段的值的不同，进行动态分发，目标Project属于不同的账号，Project1（包含两个Logstore：Logstore1、Lostore2）属于账号A，Project2（包含两个Logstore：Logstore1、Lostore2）属于账号B。

-   加工规则

    ```
    e_switch(e_match(v("project"), "Project1"), e_output(name="target1", project=v("project"), logstore=v("logstore")),
             e_match(v("project"), "Project2"), e_output(name="target2", project=v("project"), logstore=v("logstore")))
    ```

-   存储目标

    在创建数据加工规则页面中，配置存储目标，详情参数说明请参见[创建数据加工任务](/cn.zh-CN/数据加工/创建数据加工任务.md)。

    **说明：** 此场景中，目标Project和Logstore由e\_output函数中的配置决定，与创建数据加工规则页面中默认存储目标（标号1）中配置的目标Project和Logstore无关。

    ![跨账号动态分发](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7581341061/p135947.png)

    |标号|存储目标|目标Project和Logstore|AccessKey|
    |--|----|------------------|---------|
    |1|target0|Project0、Logstore0|不涉及|
    |2|target1|任意选择，由e\_output函数决定分发目标。|账号A的AccessKey信息|
    |3|target2|任意选择，由e\_output函数决定分发目标。|账号B的AccessKey信息|

-   加工结果

    ```
    ## 分发到账号A的Project1下的Logstore1中。
    
    host:  a.b.c.com
    project: Project1
    logstore: Logstore1
    http_status:  200
    request_method:  GET
    request_uri:  /pic/icon.jpg
    scheme:  https
    
    ## 分发到账号A的Project1下的Logstore2中。
    host:  m.n.q.com
    project: Project1
    logstore: Logstore2
    http_status:  301
    request_method:  POST
    request_uri:  /data/data.php
    scheme:  http
    
    ## 分发到账号B的Project2下的Logstore1中。
    
    host:   e.f.d.com
    project: Project2
    logstore: Logstore1
    http_status:  404
    request_method:  GET
    request_uri:  /category/abc/product_id
    scheme:  https
    
    ## 分发到账号B的Project2下的Logstore2中
    
    host:   p.q.t.com
    project: Project2
    logstore: Logstore2
    http_status:  504
    request_method:  GET
    request_uri:  /data/index.html
    scheme:  https
    ```


## 场景4：复杂场景动态分发

例如：某公司对一款游戏投放了广告，现在关于该游戏的所有API请求信息都存储在一个Logstore中。该公司希望通过解析useragent请求头，将来自不同设备（iOS、Android、Windows）的请求进行分发存储，并对method方法进行数据分析获取广告转化率。

针对上述需求，您可以通过日志服务数据加工和查询分析功能解决。

-   原始日志

    ```
    __source__:127.0.0.0
    __tag__:__receive_time__: 1589541467
    ip:10.0.0.0
    request_method: GET
    user_agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0
    ```

-   分发需求
    -   将Windows用户请求信息分发存储到存储目标target1的Logstore1中。
    -   将iOS用户请求信息分发存储到存储目标target2的Logstore2中。
    -   将Android用户请求信息分发存储到存储目标target3的Logstore3中。
-   加工规则

    本案例中，您可以使用[ua\_parse\_os](/cn.zh-CN/数据加工/数据加工语法/表达式函数/解析函数.md)函数解析user\_agent字段，使用[dct\_get](/cn.zh-CN/数据加工/数据加工语法/表达式函数/字典函数.md)函数获取请求头中的操作系统信息，再使用[e\_set](/cn.zh-CN/数据加工/数据加工语法/全局操作函数/字段赋值函数.md)函数新增一个字段os，值为操作系统信息，然后使用e\_output函数和条件函数进行动态分发。

    ```
    e_set("os", dct_get(ua_parse_os(v("user_agent")),"family"))
    e_if(e_search("os==Windows"),e_output(name="target1"))
    e_if(e_search("os=iOS"),e_output(name="target2"))
    e_if(e_search("os==Android"),e_output(name="target3"))
    ```

-   存储目标

    在创建数据加工规则页面中，配置存储目标，详情参数说明请参见[创建数据加工任务](/cn.zh-CN/数据加工/创建数据加工任务.md)。

    ![默认存储目标](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9081813061/p133427.png)

    |标号|存储目标|目标Project和Logstore|
    |--|----|------------------|
    |1|target0|Project0、Logstore0|
    |2|target1|Project1、Logstore1|
    |3|target2|Project2、Logstore2|
    |4|target3|Project3、Logstore3|

-   查询分析

    在目标Logstore中进行查询分析操作，获取广告转化率，根据下述查询结果可知，Android用户的广告转化率较高。查询分析操作步骤请参见[查询分析日志](/cn.zh-CN/查询与分析/查询分析日志.md)。

    -   在Logstore2的查询分析页面，输入如下查询分析语句，查看iOS用户的GET请求和POST请求占比。

        ```
        * | SELECT Request_method, COUNT(*) as number GROUP BY Request_method
        ```

        ![查询分析](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8453749951/p133463.png)

    -   在Logstore3的查询分析页面，输入如下查询分析语句，查看Android用户的GET请求和POST请求占比。

        ```
        * | SELECT Request_method, COUNT(*) as number GROUP BY Request_method
        ```

        ![查询分析](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8453749951/p133470.png)


