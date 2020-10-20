# 从OSS获取IP2Location库进行IP地址数据富化

日志服务数据加工功能支持从OSS获取IP2Location库数据，对日志中的IP地址进行富化，补充IP地址所属的国家、省、市等信息。

-   已创建访问密钥（AccessKey），用于访问OSS Bucket，详情请参见[为RAM用户创建访问密钥](/cn.zh-CN/安全设置/访问密钥/为RAM用户创建访问密钥.md)。

    推荐创建一个只读权限的AccessKey，用于从OSS获取文件；一个只写权限的AccessKey，用于上传文件到OSS。授权策略详情请参见[基于RAM Policy的权限控制](/cn.zh-CN/开发指南/数据安全/访问控制/RAM Policy/基于RAM Policy的权限控制.md)。

-   已从IP2Location官网下载IP地址文件，并上传到OSS，详情请参见[上传文件](/cn.zh-CN/快速入门/上传文件.md)。

    使用只写权限的AccessKey进行上传。


IP2Location提供全球IP地址数据库，可以帮助您精确查找、确定全球范围内IP地址的地理位置。您可以从IP2Location官网下载IP地址文件，上传至OSS。在数据加工过程中，如果您希望根据日志中的IP地址获取国家、省、市等信息，您可以通过[res\_oss\_file](/cn.zh-CN/数据加工/数据加工语法/表达式函数/资源函数.md)函数从OSS上获取IP2Location库数据，然后使用[geo\_parse](/cn.zh-CN/数据加工/数据加工语法/表达式函数/特定结构化数据函数.md)函数解析IP地址，最后使用[e\_set](/cn.zh-CN/数据加工/数据加工语法/全局操作函数/字段赋值函数.md)函数将解析结果中的新字段添加到日志中，实现数据富化。

## 实践案例

-   原始日志

    ```
    ip: 19.0.0.0
    ```

-   加工规则

    ```
    e_set("geo", geo_parse(v("ip"), ip_db=res_oss_file(endpoint='http://oss-cn-hangzhou.aliyuncs.com',
                                                        ak_id=res_local("AK_ID"),
                                                        ak_key=res_local("AK_KEY"),
                                                        bucket='test', 
                                                        file='your ip2location bin file', 
                                                        format='binary', change_detect_interval=20),
                            provider="ip2location"))
    e_json("geo")
    ```

    res\_oss\_file函数重要字段说明如下表所示。

    |字段|说明|
    |--|--|
    |endpoint|OSS访问域名，详情请参见[访问域名和数据中心](/cn.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md)。|
    |ak\_id|具备只读OSS权限的AccessKey ID。出于安全考虑，建议配置为res\_local\("AK\_ID"\)，表示从高级参数配置中获取。高级参数配置操作步骤请参见[创建数据加工任务](/cn.zh-CN/数据加工/创建数据加工任务.md)。

![AccessKey](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/6452813061/p136966.png) |
    |ak\_key|具备只读OSS权限的AccessKey Secret。出于安全考虑，建议配置为res\_local\("AK\_KEY"\)表示从高级参数配置中获取。 |
    |bucket|用于存储IP地址文件的OSS Bucket。|
    |file|您已上传的IP地址文件的名称。|
    |format|使用res\_oss\_file函数从OSS获取IP2Location库数据时，文件输出格式需设置为format='binary'。|

-   加工结果

    ```
    ip: 19.0.0.0
    city: Dearborn
    province: Michigan
    country: United States
    ```


