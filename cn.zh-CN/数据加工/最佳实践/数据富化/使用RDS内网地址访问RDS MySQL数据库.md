# 使用RDS内网地址访问RDS MySQL数据库

当您的数据分散存储在日志服务Logstore和RDS MySQL数据库中时，您可以通过日志服务数据加工功能从对应数据库获取数据，实现数据富化。本文介绍如何配置数据加工规则及高级参数，实现通过RDS内网地址访问RDS MySQL数据库获取数据。

2019年8月上海地区某共享单车的动态数据（例如订单号、自行车编号、用户ID、地理位置、用户骑车行为等）存储在日志服务的Logstore中，而共享单车的静态数据（例如自行车编号、品牌以及投放批次等）存储在阿里云RDS MySQL数据库中，现在该共享单车公司希望将单车实时动态记录日志与保存在RDS MySQL上的静态数据做富化和数据分析处理，实现共享单车最优调度。

由于通过RDS外网地址访问RDS MySQL数据库会降低RDS MySQL实例的安全性，并且不稳定。日志服务推出VPC反向代理功能，通过RDS内网地址访问RDS MySQL数据库来保证传输速率和安全性。

**说明：**

-   RDS MySQL实例与日志服务Project需处于同一地域，否则无法获取数据。
-   日志服务支持跨账号访问RDS MySQL数据库。
-   使用RDS内网地址访问RDS MySQL数据库时，需设置IP地址段白名单（固定设置为100.104.0.0/16）。更多信息，请参见[设置白名单](/cn.zh-CN/RDS MySQL 数据库/快速入门/设置白名单.md)。
-   日志服务除了支持通过阿里云内网地址访问RDS MySQL数据库外，还支持通过内网地址访问AnalyticDB MySQL和PolarDB MySQL数据库。具体操作，请参见[参考信息：使用内网地址访问AnalyticDB MySQL或PolarDB MySQL数据库](#section_gyj_cgp_l9r)。

## 配置方案

您可以参见如下方案配置数据加工规则和高级参数，实现通过RDS内网地址访问RDS MySQL数据库获取数据。

-   原始数据
    -   RDS MySQL数据库表中的数据样例如下表所示。

        ![RDS数据](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7553749951/p135927.png)

    -   日志服务Logstore中的日志样例如下所示。

        ![日志](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7553749951/p135928.png)

-   加工流程

    1.  在源Logstore中开启数据加工任务。
    2.  使用数据加工中的res\_rds\_mysql等函数从RDS MySQL数据库中拉取数据。
    3.  将加工结果保存到目标Logstore中。
    ![加工流程](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8553749951/p135169.png)

-   加工规则

    ```
    e_table_map(res_rds_mysql(str_format("{}:{}",res_local("config.vpc.instance_id.test1"),res_local("config.vpc.instance_port.test1")), "your rds username", "your rds password", "your database",table="your table",primary_keys="bikeid"), "bikeid",["brand","batch"])
    ```

    **说明：** 通过RDS内网地址访问RDS MySQL数据库，请严格遵循以下语法，请勿使用其他加工语法。

    加工规则格式如下所示：

    ```
    e_table_map(res_rds_mysql(str_format("{}:{}",res_local("config.vpc.instance_id.name"),res_local("config.vpc.instance_port.name")), "数据库账号", "数据库密码", "数据库名称",table="数据库表名"), "field", "output_fields")
    ```

    -   config.vpc.instance\_id.name和config.vpc.instance\_port.name中的name需保持一致，且还需与**高级参数配置**中配置的name保持一致，支持自定义。
    -   field为匹配字段，Logstore中的日志和RDS MySQL数据库表中数据存在共同字段，且字段的值相同，才能匹配成功。
    -   output\_fields为输出字段，字段匹配成功后，返回输出字段，生成一条新的日志。
-   高级参数配置

    通过RDS内网地址访问RDS MySQL数据库，还需在预览数据和保存加工结果时设置**高级参数配置**，其他参数配置请参见[创建数据加工任务](/cn.zh-CN/数据加工/创建数据加工任务.md)。

    ![添加预览配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8310096061/p100061.png)

    config.vpc.vpc\_id.name、config.vpc.instance\_id.name和config.vpc.instance\_port.name中的name需保持一致，且还需与加工规则中配置的name保持一致，支持自定义。

    |Key格式|Key示例值|Value示例值|说明|
    |-----|------|--------|--|
    |config.vpc.vpc\_id.name|config.vpc.vpc\_id.test1|vpc-uf6mskb0b\*\*\*\*n9yj|vpc\_id为RDS MySQL实例所属于的网络类型ID。|
    |config.vpc.instance\_id.name|config.vpc.instance\_id.test1|rm-uf6e61k\*\*\*\*ahd7|instance\_id为RDS MySQL实例ID。|
    |config.vpc.instance\_port.name|config.vpc.instance\_port.test1|3306|instance\_port为RDS MySQL实例内网地址端口。|


## 错误排查

-   RDS白名单未配置

    如果提示`reason: {"errorCode": "InvalidConfig", "errorMessage": "error when calling : res_rds_mysql\nDetail: {\"errorCode\": \"InvalidConfig\", \"errorMessage\": \"Database connection failed, cause: (2003, \\\"Can't connect to MySQL server on '192.168.1.1' (timed out)\\\")\\nDetail: None\", \"requestId\": \"\"}", "requestId": ""}`错误，表示数据加工授权后台授权VPC映射已经成功，但是因为RDS没有配置白名单，导致无法连接到数据库。

-   高级参数配置错误
    -   高级参数配置中name不一致

        如果提示`reason: {"errorCode": "InvalidConfig", "errorMessage": "error when calling : res_rds_mysql\nDetail: {\"errorCode\": \"InvalidConfig\", \"errorMessage\": \"address check failed, please check the configuration of address. address: rm-bp***r5\\nDetail: None\", \"requestId\": \"\"}", "requestId": ""}`错误，表示config.vpc.instance\_port.name中的name与config.vpc.instance\_id.name、config.vpc.vpc\_id.name中的name设置不一致。

    -   参数配置不完整

        如果提示`reason: {"errorCode": "InvalidConfig", "errorMessage": "error when calling : res_rds_mysql\nDetail: {\"errorCode\": \"InvalidConfig\", \"errorMessage\": \"address check failed, please check the configuration of address. address: rm-bp1***9r5\\nDetail: None\", \"requestId\": \"\"}", "requestId": ""}`错误，表示缺少config.vpc.vpc\_id.name配置信息。

-   加工语句错误

    如果提示`reason: {"errorCode": "InvalidConfig", "errorMessage": "error when calling : res_rds_mysql\nDetail: {\"errorCode\": \"InvalidConfig\", \"errorMessage\": \"Database connection failed, cause: (2003, \\\"Can't connect to MySQL server on 'rm-bp***r5.mysql.rds.aliyuncs.com' (timed out)\\\")\\nDetail: None\", \"requestId\": \"\"}", "requestId": ""}`错误，表示使用了错误的加工语法。


## 参考信息：使用内网地址访问AnalyticDB MySQL或PolarDB MySQL数据库

日志服务除了支持通过阿里云内网地址访问RDS MySQL数据库外，还支持通过内网地址访问AnalyticDB MySQL和PolarDB MySQL数据库。相关配置如下所示：

-   PolarDB MySQL数据库

    通过内网地址访问PolarDB MySQL数据库时，需设置IP地址段白名单（固定设置为100.104.0.0/16）。具体操作，请参见[设置白名单](/cn.zh-CN/用户指南/数据安全/加密/设置集群白名单.md)。其他操作请参见[访问RDS MySQL数据库的配置](#section_ke7_52c_4wm)。

-   AnalyticDB MySQL数据库

    通过内网地址访问AnalyticDB MySQL数据库时，需设置IP地址段白名单（固定设置为100.104.0.0/16）。具体操作，请参见[设置白名单]()。其他操作请参见[访问RDS MySQL数据库的配置](#section_ke7_52c_4wm)，其中设置**高级参数配置**时，config.vpc.instance\_id.name的值需配置为PolarDB MySQL实例名加-controller，如下图所示：

    ![ADB](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8310096061/p187384.png)


## 相关操作

获取RDS MySQL数据，完成数据富化后，您可以在日志服务控制台上进行数据分析。例如：分析自行车品牌对共享单车租赁影响、统计每小时用车人数、分析自行车投放市场批次对共享单车租赁影响等。更多信息，请参见[基于日志服务数据加工与RDS MySQL做数据富化以及数据分析](https://yq.aliyun.com/articles/755595?spm=a2c4e.11155435.0.0.33d53312jdskCD)。

