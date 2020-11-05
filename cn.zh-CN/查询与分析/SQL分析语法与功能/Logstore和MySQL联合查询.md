# Logstore和MySQL联合查询

日志服务支持通过Join语法将Logstore和MySQL数据库进行联合查询，并把查询结果保存到MySQL数据库中。

-   已采集日志到日志服务。更多信息，请参见[数据采集](/cn.zh-CN/数据采集/采集方式.md)。
-   已为日志字段创建索引。更多信息，请参见[开启并配置索引](/cn.zh-CN/查询与分析/开启并配置索引.md)。
-   已有可用的MySQL数据库。更多信息，请参见[创建数据库和账号](/cn.zh-CN/RDS MySQL 数据库/快速入门/创建数据库和账号.md)。

1.  设置MySQL数据库白名单。

    -   如果是RDS MySQL数据库，需添加白名单地址100.104.0.0/16、11.194.0.0/16和11.201.0.0/16。更多信息，请参见[设置白名单](/cn.zh-CN/RDS MySQL 数据库/数据安全/加密/设置白名单.md)。
    -   如果是自建的MySQL数据库，需添加白名单地址100.104.0.0/16、11.194.0.0/16和11.201.0.0/16到MySQL数据库的安全组。
2.  在MySQL数据库中，创建数据表。

3.  创建ExternalStore。

    1.  安装日志服务CLI。更多信息，请参见[命令行工具CLI](/cn.zh-CN/开发指南/命令行工具CLI.md)。

    2.  创建配置文件/root/config.json并添加如下脚本。

        请根据实际值替换如下信息。

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
        |host|数据库地址|
        |port|网络端口|
        |username|数据库用户名|
        |password|数据库密码|
        |db|数据库名称|
        |table|数据库表|

    3.  创建ExternalStore。

        其中project\_name为日志服务Project名称，请根据实际值替换。

        ```
        aliyunlog log create_external_store --project_name="log-rds-demo" --config="file:///root/config.json" 
        ```

4.  使用Join语法进行联合查询。

    1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

    2.  在Project列表区域，单击目标Project。

    3.  在**日志存储** \> **日志库**页签中，单击目标Logstore。

    4.  执行查询分析语句。

        支持的Join语法有INNER JOIN、LEFT JOIN、RIGHT JOIN和FULL JOIN。

        ```
        [ INNER ] JOIN
        LEFT [ OUTER ] JOIN
        RIGHT [ OUTER ] JOIN
        FULL [ OUTER ] JOIN
        ```

        JOIN语法样例如下所示：

        ```
        method:postlogstorelogs | select count(1) , histogram(logstore) from log  l join join_meta m on l.projectid = cast( m.ikey as varchar)
        ```

        **说明：**

        -   仅支持Logstore与MySQL数据库小表（数据量小于20 MB）进行联合查询。
        -   查询分析语句中，Logstore必须写在join关键字前面，ExternalStore写在join关键字后面。
        -   查询分析语句中，必须写ExternalStore名称，系统自动替换成MySQL数据库名+表名。请勿直接填写MySQL表名。
5.  保存查询结果到MySQL数据库中。

    日志服务支持通过Insert语法将查询结果插入到MySQL数据库中。Insert语法样例如下所示：

    ```
    method:postlogstorelogs | insert into method_output  select cast(method as varchar(65535)),count(1) from log group by method
    ```


Python程序样例

```
# encoding: utf-8
from __future__ import print_function
from aliyun.log import *
from aliyun.log.util import base64_encodestring
from random import randint
import time
import os
from datetime import datetime
    endpoint = os.environ.get('ALIYUN_LOG_SAMPLE_ENDPOINT', 'cn-chengdu.log.aliyuncs.com')
    accessKeyId = os.environ.get('ALIYUN_LOG_SAMPLE_ACCESSID', '')
    accessKey = os.environ.get('ALIYUN_LOG_SAMPLE_ACCESSKEY', '')
    logstore = os.environ.get('ALIYUN_LOG_SAMPLE_LOGSTORE', '')
    project = "ali-yunlei-chengdu"
    client = LogClient(endpoint, accessKeyId, accessKey, token)
    #创建ExternalStore
    res = client.create_external_store(project,ExternalStoreConfig("rds_store","region","rds-vpc","vpc id","实例id","实例ip","实例端口","用户名","密码","数据库","数据库表"));
    res.log_print()
    #获取ExternalStore详情
    res = client.get_external_store(project,"rds_store");
    res.log_print()
    res = client.list_external_store(project,"");
    res.log_print();
    # JOIN查询
    req = GetLogsRequest(project,logstore,From,To,"","select count(1) from  "+ logstore +"  s join  meta m on  s.projectid = cast(m.ikey as varchar)");
    res = client.get_logs(req)
    res.log_print();
     # 查询结果写入MySQL数据库
    req = GetLogsRequest(project,logstore,From,To,""," insert into rds_store select count(1) from  "+ logstore );
    res = client.get_logs(req)
    res.log_print();
```

