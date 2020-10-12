# 关联Logstore与MySQL数据库进行查询分析

本文以游戏公司数据分析场景为例，介绍日志服务Logstore与MySQL数据库关联分析功能。

-   已采集日志到日志服务，详情请参见[数据采集](/cn.zh-CN/数据采集/采集方式.md)。
-   已为日志字段创建索引，详情请参见[开启并配置索引](/cn.zh-CN/查询与分析/开启并配置索引.md)。
-   已有可用的MySQL数据库。

    RDS MySQL数据库详情请参见[创建数据库和账号](/cn.zh-CN/RDS MySQL 数据库/快速入门/创建数据库和账号.md)。


某游戏公司，主要包括两大类数据：用户游戏日志数据和用户元数据。日志服务可实时采集用户游戏日志数据，包括操作、目标、血、魔法值、网络、支付手段、点击位置、状态码、用户ID等信息。然而用户元数据，包括用户的性别、注册时间、地区等信息，不能打印到日志中，所以一般存储到数据库中。现在该公司希望将用户游戏日志与用户元数据进行联合分析，获得最佳的游戏运营方案。

针对上述需求，日志服务查询分析引擎，提供Logstore和外部数据源（ExternalStore，例如MySQL数据库、OSS等）联合查询分析功能。您可以使用SQL的JOIN语法把用户游戏日志和用户元数据关联起来，分析与用户属性相关的指标。除此之外，您还可以将计算结果直接写入外部数据源中，便于结果的进一步处理。

![背景信息](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8440559951/p41587.png)

1.  在MySQL数据库中，创建用户属性表。

    创建一张名为chiji\_user的数据表，用于保存用户ID、昵称、性别、年龄、注册时间、账户余额和注册省份。

    ```
    CREATE TABLE `chiji_user` ( 
      `uid` int(11) NOT NULL DEFAULT '0', 
      `user_nick` text, 
      `gender` tinyint(1) DEFAULT NULL, 
      `age` int(11) DEFAULT NULL, 
      `register_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP, 
      `balance` float DEFAULT NULL, 
      `province` text, PRIMARY KEY (`uid`) 
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8
    ```

2.  添加白名单。

    -   如果是RDS MySQL数据库，需添加白名单地址100.104.0.0/16、11.194.0.0/16和11.201.0.0/16，详情请参见[设置白名单](/cn.zh-CN/RDS MySQL 数据库/数据安全/加密/设置白名单.md)。
    -   如果是自建的MySQL数据库，需添加白名单地址100.104.0.0/16、11.194.0.0/16和11.201.0.0/16到MySQL数据库的安全组。
3.  创建ExternalStore。

    1.  安装日志服务CLI，详情请参见[命令行工具CLI](/cn.zh-CN/开发指南/命令行工具CLI.md)。

    2.  创建配置文件/root/config.json。

    3.  在/root/config.json文件中添加如下脚本，并根据实际情况替换参数配置。

        ```
        { 
             "externalStoreName": "chiji_user", 
             "storeType": "rds-vpc", 
             "parameter": { 
             "vpc-id": "vpc-m5eq4irc1pucpk85f****", 
             "instance-id": "rm-m5ep2z57814qs****", 
             "host": "example.com", 
             "port": "3306", 
             "username": "testroot", 
             "password": "****", 
             "db": "chiji", 
             "table": "chiji_user", 
             "region": "cn-qingdao" 
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
        |db|数据库|
        |table|数据表|

    4.  创建ExternalStore。

        其中project\_name为日志服务Project名称，请根据实际值替换。

        ```
        aliyunlog log create_external_store --project_name="log-rds-demo" --config="file:///root/config.json" 
        ```

4.  使用JOIN语法进行联合查询分析。

    1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

    2.  在Project列表区域，单击目标Project。

    3.  在**日志存储** \> **日志库**页签中，单击目标Logstore。

    4.  执行查询分析语句。

        指定日志中的userid字段和数据库表中的uid字段关联Logstore和MySQL数据库。

        -   分析活跃用户的性别分布。

            ```
            * | select case gender when 1 then '男性' else '女性' end as gender , count(1) as pv from log l join chiji_user u on l.userid = u.uid group by gender order by pv desc
            ```

            ![关联分析](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8440559951/p41594.png)

        -   分析不同省份活跃度。

            ```
            * | select province , count(1) as pv from log l join chiji_user u on l.userid = u.uid group by province order by pv desc
            ```

            ![活跃度](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8440559951/p41596.png)

        -   分析不同性别的消费情况。

            ```
            * | select case gender when 1 then '男性' else '女性' end as gender , sum(money) as money from log l join chiji_user u on l.userid = u.uid group by gender order by money desc
            ```

5.  保存查询分析结果到MySQL数据库中。

    1.  在MySQL数据库中，创建名为report的数据表，该表存储每分钟的PV值。

        ```
        CREATE TABLE `report` ( 
          `minute` bigint(20) DEFAULT NULL, 
          `pv` bigint(20) DEFAULT NULL 
        ) ENGINE=InnoDB DEFAULT CHARSET=utf8
        ```

    2.  参见[步骤3](#step_oi6_fge_vyf)为report表创建ExternalStore。

    3.  在日志服务Logstore的查询分析页面中，执行如下查询语句将分析结果保存到report表中。

        ```
        * | insert into report select __time__- __time__ % 300 as min, count(1) as pv group by min
        ```

        保存成功后，您可以在MySQL数据库中查看保存结果。

        ![SQL结果](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9440559951/p41597.png)


