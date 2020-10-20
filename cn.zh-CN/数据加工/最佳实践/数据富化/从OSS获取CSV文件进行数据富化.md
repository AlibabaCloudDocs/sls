# 从OSS获取CSV文件进行数据富化

本文档介绍如何通过资源函数和映射富化函数从OSS中获取数据对日志数据进行富化。

-   已创建访问密钥（AccessKey），用于访问OSS Bucket，详情请参见[为RAM用户创建访问密钥](/cn.zh-CN/安全设置/访问密钥/为RAM用户创建访问密钥.md)。

    推荐创建一个只读权限的AccessKey，用于从OSS获取文件；一个只写权限的AccessKey，用于上传文件到OSS。授权策略详情请参见[基于RAM Policy的权限控制](/cn.zh-CN/开发指南/数据安全/访问控制/RAM Policy/基于RAM Policy的权限控制.md)。

-   已上传CSV文件到OSS，详情请参见[上传文件](/cn.zh-CN/快速入门/上传文件.md)。

    推荐使用只写权限的AccessKey进行上传。


OSS是阿里云提供的海量、安全、低成本、高可靠的云存储服务。针对更新不频繁的数据，建议您存储在OSS上，只需要支付少量的存储费用即可。当您分散存储数据，面临日志数据不完善时，您可以从OSS中获取数据。日志服务数据加工支持使用[res\_oss\_file](/cn.zh-CN/数据加工/数据加工语法/表达式函数/资源函数.md)函数从OSS中获取数据，再使用[tab\_parse\_csv](/cn.zh-CN/数据加工/数据加工语法/表达式函数/表格函数.md)函数构建表格，最后使用[e\_table\_map](/cn.zh-CN/数据加工/数据加工语法/全局操作函数/映射富化函数.md)函数进行字段匹配，返回指定字段和字段值，生成新的日志数据。

## 实践案例

-   原始日志

    ```
    account :  Sf24asc4ladDS
    ```

-   OSS CSV文件数据

    |id|account|nickname|
    |--|-------|--------|
    |1|Sf24asc4ladDS|多弗朗明哥|
    |2|Sf24asc4ladSA|凯多|
    |3|Sf24asc4ladCD|罗杰|

-   加工规则

    通过日志服务Logstore中的account字段和OSS CSV文件中的account字段进行匹配，只有account字段的值完全相同，才能匹配成功。匹配成功后，返回OSS CSV文件中的nickname字段和字段值，与Logstore中的数据拼接，生成新的数据。

    ```
    e_table_map(tab_parse_csv(res_oss_file(endpoint='http://oss-cn-hangzhou.aliyuncs.com',
                                            ak_id=res_local("AK_ID"),
                                            ak_key=res_local("AK_KEY"), 
                                            bucket='test',
                                            file='account.csv',change_detect_interval=30)),
                "account","nickname")
    ```

    res\_oss\_file函数重要字段说明如下表所示。

    |字段|说明|
    |--|--|
    |endpoint|OSS访问域名，详情请参见[访问域名和数据中心](/cn.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md)。|
    |ak\_id|具备读OSS权限的AccessKey ID。出于安全考虑，建议配置为res\_local\("AK\_ID"\)，表示从高级参数配置中获取。高级参数配置操作步骤请参见[创建数据加工任务](/cn.zh-CN/数据加工/创建数据加工任务.md)。

![AccessKey](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/6452813061/p136966.png) |
    |ak\_key|具备读OSS权限的AccessKey Secret。出于安全考虑，建议配置为res\_local\("AK\_KEY"\)，表示从高级参数配置中获取。 |
    |bucket|用于存储CSV文件的OSS Bucket。|
    |file|您已上传的CSV文件的名称。|

-   加工结果

    ```
    account :  Sf24asc4ladDS
    nickname: 多弗朗明哥
    ```


