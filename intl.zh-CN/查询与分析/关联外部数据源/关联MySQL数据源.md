# 关联MySQL数据源

本文介绍如何创建外部存储，建立日志服务与MySQL数据库的关联。

-   已创建Project和Logstore，详情请参见[步骤1：创建Project和Logstore](/intl.zh-CN/快速入门/快速入门.md)。
-   已采集日志，详情请参见[数据采集](/intl.zh-CN/数据采集/采集方式.md)。

日志服务外部存储功能支持日志服务与普通MySQL数据库或阿里云RDS MySQL数据库关联，您还可以将查询分析结果写入MySQL数据库中，便于进一步处理结果。创建外部MySQL存储的最佳实践，详情请参见[关联Logstore与MySQL数据库进行查询分析](/intl.zh-CN/查询与分析/最佳实践/关联Logstore与MySQL数据库进行查询分析.md)。

## 操作步骤

1.  创建RDS MySQL实例并指定VPC环境，详情请参见[创建RDS MySQL实例](/intl.zh-CN/RDS MySQL 数据库/快速入门/创建RDS MySQL实例.md)。

    此处以阿里云专有网络下的RDS MySQL为例。

2.  设置白名单。

    设置RDS实例的白名单，以允许外部设备访问该RDS实例。设置RDS白名单为100.104.0.0/16、11.194.0.0/16或11.201.0.0/16，详情请参见[设置白名单](/intl.zh-CN/RDS MySQL 数据库/数据安全/加密/设置白名单.md)。

3.  创建RDS MySQL表，详情请参见[CREATE TABLE Statement](https://dev.mysql.com/doc/refman/8.0/en/create-table.html)。

4.  创建ExternalStore。

    1.  安装日志服务CLI，详情请参见[命令行工具CLI](/intl.zh-CN/开发指南/命令行工具CLI.md)。

    2.  创建配置文件/root/config.json。

    3.  在/root/config.json文件中添加如下脚本，并根据实际情况替换参数配置。

        ```
        {
        "externalStoreName":"storeName",
        "storeType":"rds-vpc",
        "parameter":
           {
           "region":"cn-qingdao",
           "vpc-id":"vpc-m5eq4irc1pucp*******",
           "instance-id":"i-m5eeo2whsn*******",
           "host":"localhost",
           "port":"3306",
           "username":"root",
           "password":"****",
           "db":"scmc",
           "table":"join_meta"
           }
        }
        ```

        |参数|说明|
        |:-|:-|
        |region|RDS实例所在地域        -   如果是RDS MySQL，则必须配置region。
        -   如果是自建的MySQL，则region配置为空字符串，即配置为"region": ""。 |
        |vpc-id|VPC ID        -   如果是专有网络下的RDS MySQL，则必须配置vpc-id。
        -   如果是经典网络下的RDS MySQL或者自建的MySQL，则vpc-id配置为空字符串，即配置为"vpc-id": ""。 |
        |instance-id|RDS实例ID        -   如果是RDS MySQL，则必须配置instance-id。
        -   如果是自建的MySQL，则instance-id配置为空字符串，即配置为"instance-id": ""。 |
        |host|RDS域名|
        |port|RDS端口|
        |username|数据库用户名|
        |password|数据库密码|
        |db|数据库|
        |table|数据表|

    4.  创建ExternalStore。

        其中project\_name为日志服务Project名称，请根据实际情况替换。

        ```
        aliyunlog log create_external_store --project_name="log-rds-demo" --config="file:///root/config.json" 
        ```


## 相关操作

-   更新MySQL外部存储。

    ```
    aliyunlog log update_external_store --project_name="log-rds-demo" --config="file:///root/config.json" 
    ```

-   删除MySQL外部存储。

    ```
    aliyunlog log delete_external_store --project_name="log-rds-demo" --store_name=abc
    ```


[Logstore和MySQL联合查询](/intl.zh-CN/查询与分析/SQL分析语法与功能/Logstore和MySQL联合查询.md)

