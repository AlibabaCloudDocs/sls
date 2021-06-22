# 目标Logstore有多余数据怎么处理？

您将数据加工结果分发至目标Logstore后，如果目标Logstore中有多余数据，可参见本文解决。

名为website\_log的Logstore中有5000条数据，其中SourceIP为192.0.2.54的数据有1000条，SourceIP为192.0.2.28的数据有1000条，SourceIP为192.0.2.136的数据有1000条，SourceIP为其他值的数据有2000条。现对这些数据进行加工，分发到不同的目标Logstore中。

-   加工需求
    -   将SourceIP为192.0.2.54的数据分发至名为54\_log的Logstore中。
    -   将SourceIP为192.0.2.28的数据分发至名为28\_log的Logstore中。
    -   将SourceIP为192.0.2.136的数据分发至名为136\_log的Logstore中。
-   预期结果
    -   名为54\_log的Logstore中有1000条数据，其中SourceIP为192.0.2.54。
    -   名为28\_log的Logstore中有1000条数据，其中SourceIP为192.0.2.28。
    -   名为136\_log的Logstore中有1000条数据，其中SourceIP为192.0.2.136。
-   加工语句

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
    ```

-   存储目标

    ![存储目标](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0522193261/p284172.png)

-   加工结果
    -   （不符合预期）名为54\_log的Logstore中有3000条数据，除SourceIP为192.0.2.54的数据外，还有SourceIP为其他值的数据。
    -   （符合预期）名为28\_log的Logstore中有1000条数据，其中SourceIP为192.0.2.28。
    -   （符合预期）名为136\_log的Logstore中有1000条数据，其中SourceIP为192.0.2.136。
-   原因分析

    日志服务在分发数据加工结果时，符合e\_output函数加工规则的数据被分别分发到对应的目标Logstore中。其他在加工过程中通过DSL（加工语句）处理且未被丢弃的数据将被分发到存储目标1的Logstore中（本案例中为54\_log Logstore）。日志服务以存储目标1为默认存储目标。

-   解决方法

    在加工语句中加上e\_drop\(\)语句，丢弃不符合加工规则的数据。

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


