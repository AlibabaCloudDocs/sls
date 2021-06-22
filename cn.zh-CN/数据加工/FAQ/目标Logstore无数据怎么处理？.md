# 目标Logstore无数据怎么处理？

您将数据加工结果分发至目标Logstore后，如果目标Logstore中无数据，可参见本文解决。

## 场景一：加工语句中的存储目标与创建数据加工规则页面中的存储目标不一致

名为website\_log的Logstore中有5000条数据，其中SourceIP为192.0.2.54的数据有1000条，SourceIP为192.0.2.28的数据有1000条，SourceIP为192.0.2.136的数据有1000条，SourceIP为其他值的日志有2000条。现对这些数据进行加工，分发到不同的目标Logstore（54\_log\_target、28\_log\_target、136\_log\_target）中。

-   加工需求
    -   将SourceIP为192.0.2.54的数据分发至名为54\_log的Logstore中。
    -   将SourceIP为192.0.2.28的数据分发至名为28\_log的Logstore中。
    -   将SourceIP为192.0.2.136的数据分发至名为136\_log的Logstore中。
    -   将其他不符合加工条件的数据全部丢弃。
-   加工语句

    三个目标Logstore分别为54\_log、28\_log、136\_log。

    ```
    e_if(e_search("SourceIP==192.0.2.54"),    
      e_output(name="54-target",
                 project="sls-test",
                 logstore="54_log"))
    e_if(e_search("SourceIP==192.0.2.28"),
        e_output(name="28-target",
                 project="sls-test",
                 logstore="28_log"))
    e_if(e_search("SourceIP==192.0.2.136"),
        e_output(name="136-target",
                 project="sls-test",
                 logstore="136_log"))
    e_drop()
    ```

-   存储目标

    三个目标Logstore分别为54\_log\_target、28\_log\_target、136\_log\_target。

    ![存储目标](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1830193261/p284156.png)

-   加工结果

    名为54\_log\_target、28\_log\_target、136\_log\_target的Logstore中无数据。

-   原因分析

    您在设置加工语句时，在e\_output函数中设置了project（目标Project名称）和logstore（目标Logstore名称），又在创建数据加工规则面板中，设置了不一样的**目标Project**和**目标库**。当加工语句（DSL）与创建数据加工规则面板中设置的目标Logstore不同时，DSL参数中的目标Logstore将覆盖创建数据加工规则页面中设置的目标Logstore（加工仅使用目标配置的访问授权）。即在本案例中，加工结果被分发至名为54\_log、28\_log、136\_log的Logstore中。

    **说明：** 在使用e\_output函数和e\_coutput函数时，创建数据加工规则面板中设置的存储目标需与函数中设置的存储目标一致。更多信息，请参见[e\_output、e\_coutput](/cn.zh-CN/数据加工/数据加工语法/全局操作函数/事件操作函数.md)。

-   解决方案

    修改加工语句，确保加工语句（DSL）与创建数据加工规则面板中设置的目标Logstore一致。

    ```
    e_if(e_search("SourceIP==192.0.2.54"),    
      e_output(name="54-target",
                 project="sls-test",
                 logstore="54_log_target"))
    e_if(e_search("SourceIP==192.0.2.28"),
        e_output(name="28-target",
                 project="sls-test",
                 logstore="28_log_target"))
    e_if(e_search("SourceIP==192.0.2.136"),
        e_output(name="136-target",
                 project="sls-test",
                 logstore="136_log_target"))
    e_drop()
    ```


## 场景二：未设置加工语句

-   加工语句

    无

-   存储目标

    ![未设置加工语句](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1830193261/p284204.png)

-   加工结果

    只有名为54\_log\_target的Logstore中有数据。

-   原因分析

    您未设置加工语句但在创建数据加工规则面板中设置了多个存储目标时，所有的数据都被分发到存储目标1的Logstore中。存储目标1为系统默认的存储目标。


**说明：**

-   设置了加工语句且要进行多目标分发时，如果未使用e\_drop\(\)语句，则所有通过加工处理且未被丢弃的数据都被分发至存储目标1的Logstore中。
-   未设置加工语句但要进行多目标分发时，所有数据都被分发至存储目标1的Logstore中。其他存储目标无意义。
-   仅设置了e\_drop\(\)语句且要进行单目标分发时，所有的数据被丢弃，目标Logstore中无数据。
-   未设置加工语句且要进行单目标分发，此时的加工功能相当于纯复制功能。

## 场景三：加工延迟

如果您确保您的加工规则设置正确，而目标Logstore中仍无数据，可能是因为加工延迟。更多信息，请参见[加工延迟](https://developer.aliyun.com/article/746592)。

当出现加工延迟时，建议您：

-   修改源Logstore和目标Logstore的Shard数目。更多信息，请参见[性能指南](/cn.zh-CN/数据加工/性能指南.md)。
-   优化加工语句的逻辑，例如优化正则表达式。

