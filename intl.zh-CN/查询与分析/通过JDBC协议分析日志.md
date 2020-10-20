# 通过JDBC协议分析日志

日志服务支持您在数据库（例如MySQL）中使用JDBC协议连接日志服务，并通过标准的SQL 92语法查询分析日志。

-   已创建阿里云主账号的Accesskey或RAM用户的Accesskey。

    如果使用RAM用户的Accesskey，则RAM用户必须是当前Project所属主账号的RAM用户，且具备Project级别的读权限。

-   已创建日志服务Project和Logstore，详情请参见[步骤1：创建Project和Logstore](/intl.zh-CN/快速入门/快速入门.md)。

## 连接参数

此处以MySQL数据库为例，介绍连接参数。

```
mysql -hcn-shanghai-intranet.log.aliyuncs.com -ubq2sjzesjmo86kq -p4fdO1fTDDuZP -P10005
use sample-project; 
```

**说明：** MySQL JDBC不支持分页。

连接参数说明如下表所示。

|参数|说明|
|:-|:-|
|host|日志服务访问域名，详情请参见[服务入口](/intl.zh-CN/开发指南/API 参考/服务入口.md)。目前仅支持经典网络及VPC网络服务入口，例如cn-hangzhou-intranet.log.aliyuncs.com。|
|port|默认使用10005。|
|user|配置为阿里云账号的Accesskey ID。|
|password|配置为阿里云账号的AccessKey secret。|
|database|日志服务Project。|
|table|日志服务Logstore。|

## 查询分析语法

-   过滤语法

    **说明：** 在where条件中必须包含\_\_date\_\_字段或\_\_time\_\_字段来限制查询的时间范围。\_\_date\_\_字段是timestamp类型，\_\_time\_\_字段是bigint类型。例如：

    -   \_\_date\_\_ \> '2017-08-07 00:00:00' and \_\_date\_\_ < '2017-08-08 00:00:00'
    -   \_\_time\_\_ \> 1502691923 and \_\_time\_\_ < 1502692923
    where过滤语法说明如下表所示。

    |语义|示例|说明|
    |:-|:-|:-|
    |字符串搜索|key = "value"|查询的是分词之后的结果。|
    |字符串模糊搜索|    -   key has 'valu\*'
    -   key like 'value\_%'
|查询的是分词之后模糊匹配的结果。|
    |数值比较|num\_field \> 1|比较运算符包括\>、\>= 、= 、< 和<=。|
    |逻辑运算|and or not|例如a = "x" and b ="y"或a = "x" and not b ="y"。|
    |全文搜索|\_\_line\_\_ ="abc"|如果使用全文索引搜索，需使用特殊的key（\_\_line\_\_）。|

-   计算语法

    支持计算操作符，详情请参见[分析语法](/intl.zh-CN/查询与分析/实时分析简介.md)。

-   SQL92语法

    过滤语法和计算语法组合成为SQL 92语法。

    ```
    status>200 |select avg(latency),max(latency) ,count(1) as c GROUP BY  method  ORDER BY c DESC  LIMIT 20
    ```

    您可以将上述查询分析语句中的过滤部分和时间条件组合成为查询条件，变成标准SQL92语法，如下所示：

    ```
    select avg(latency),max(latency) ,count(1) as c from sample-logstore where status>200 and __time__>=1500975424 and __time__ < 1501035044 GROUP BY  method  ORDER BY c DESC  LIMIT 20
    ```


## 通过JDBC协议访问

-   程序调用

    您可以在任何一个支持MySQL connector的程序中使用MySQL语法连接日志服务。例如JDBC、Python MySQLdb，样例如下所示：

    ```
    import com.mysql.jdbc.*;
    import java.sql.*;
    import java.sql.Connection;
    import java.sql.ResultSetMetaData;
    import java.sql.Statement;
    public class testjdbc {
        public static void main(String args[]){
            Connection conn = null;
            Statement stmt = null;
            try {
                //STEP 2: Register JDBC driver
                Class.forName("com.mysql.jdbc.Driver");
                //STEP 3: Open a connection
                System.out.println("Connecting to a selected database...");
                conn = DriverManager.getConnection("jdbc:mysql://cn-shanghai-intranet.log.aliyuncs.com:10005/sample-project","accessid","accesskey");
                System.out.println("Connected database successfully...");
                //STEP 4: Execute a query
                System.out.println("Creating statement...");
                stmt = conn.createStatement();
                String sql = "SELECT method,min(latency,10)  as c,max(latency,10) from sample-logstore where  __time__>=1500975424 and __time__ < 1501035044 and latency > 0  and latency < 6142629 and  not (method='Postlogstorelogs' or method='GetLogtailConfig') group by method " ;
                String sql_example2 = "select count(1) ,max(latency),avg(latency), histogram(method),histogram(source),histogram(status),histogram(clientip),histogram(__source__) from  test10 where __date__  >'2017-07-20 00:00:00'  and  __date__ <'2017-08-02 00:00:00' and __line__='abc#def' and latency < 100000 and (method = 'getlogstorelogS' or method='Get**' and method <> 'GetCursorOrData' )";
                String sql_example3 = "select count(1) from  sample-logstore where     __date__  >       '2017-08-07 00:00:00' and  __date__ <     '2017-08-08 00:00:00' limit 100";
                ResultSet rs = stmt.executeQuery(sql);
                //STEP 5: Extract data from result set
                while(rs.next()){
                    //Retrieve by column name
                    ResultSetMetaData data = rs.getMetaData();
                    System.out.println(data.getColumnCount());
                    for(int i = 0;i < data.getColumnCount();++i) {
                        String name = data.getColumnName(i+1);
                        System.out.print(name+":");
                        System.out.print(rs.getObject(name));
                    }
                    System.out.println();
                }
                rs.close();
            } catch (ClassNotFoundException e) {
                e.printStackTrace();
            } catch (SQLException e) {
                e.printStackTrace();
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                if (stmt != null) {
                    try {
                        stmt.close();
                    } catch (SQLException e) {
                        e.printStackTrace();
                    }
                }
                if (conn != null) {
                    try {
                        conn.close();
                    } catch (SQLException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }
    ```

-   工具类调用

    在经典网络或VPC网络中通过MySQL客户端进行连接。

    ![连接示例](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2440559951/p5647.png)

    -   ①处配置为您的Project。
    -   ②处配置为您的Logstore。

