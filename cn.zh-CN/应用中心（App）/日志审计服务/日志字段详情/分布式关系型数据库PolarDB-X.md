# 分布式关系型数据库PolarDB-X

本文介绍分布式关系型数据库PolarDB-X SQL审计日志的字段详情。

|字段名称|字段说明|
|----|----|
|\_\_topic\_\_|日志主题，固定为drds\_audit\_log。|
|instance\_id|PolarDB-X实例ID|
|instance\_name|PolarDB-X实例名|
|owner\_id|阿里云账户ID|
|region|PolarDB-X实例所在地域|
|db\_name|PolarDB-X数据库名|
|user|执行SQL的用户名|
|client\_ip|访问PolarDB-X实例的客户端IP地址|
|threat\_client\_ip|访问PolarDB-X实例的客户端IP地址的威胁情报。更多信息，请参见[威胁情报字段](/cn.zh-CN/应用中心（App）/日志审计服务/生成威胁情报.md)。|
|client\_port|访问PolarDB-X实例的客户端端口|
|sql|执行的SQL语句|
|trace\_id|SQL执行的TRACE ID。如果是事务，则显示为跟踪ID、短划线（-）和数字，例如drdsabcdxyz-1。|
|sql\_code|模板SQL的HASH值|
|hint|SQL执行的HINT|
|table\_name|查询涉及的表名。多表之间以半角逗号（,）分隔。|
|sql\_type|SQL类型。包括Select、Insert、Update、Delete、Set、Alter、Create、Drop、Truncate、Replace和Other。|
|sql\_type\_detail|SQL解析器名称|
|response\_time|响应时间，单位：微秒。|
|affect\_rows|SQL执行返回的行数。增删改时表示影响的行数，查询语句表示返回的行数。|
|fail|SQL执行是否出错。包括：-   0：成功
-   1：失败 |
|sql\_time|SQL开始执行的时间|

