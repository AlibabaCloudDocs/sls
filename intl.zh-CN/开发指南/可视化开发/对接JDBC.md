# 对接JDBC

本文以实际案例演示如何使用JDBC连接日志服务、读取日志数据，及使用MySQL协议和SQL语法来计算日志。

已为目标字段设置字段索引并开启统计功能。更多信息，请参见[配置索引](/intl.zh-CN/查询与分析/配置索引.md)。

MySQL是当前流行的关系型数据库，很多软件支持通过MySQL传输协议和SQL语法获取MySQL数据。您只需要对SQL语法熟悉，即可完成对接。日志服务提供了MySQL协议查询和分析日志数据。您可以使用标准MySQL客户端连接到日志服务，使用标准的SQL语法计算和分析日志。支持MySQL传输协议的客户端包括MySQL client，JDBC和Python MySQLdb。

JDBC的使用场景：

-   使用可视化类工具，例如DataV、Tableau或Kibana通过MySQL协议连接日志服务。具体操作，请参见[通过JDBC协议分析日志](/intl.zh-CN/查询与分析/通过JDBC协议分析日志.md)。
-   使用Java的JDBC、Python的MySQLdb等库在程序中访问日志服务，在程序中处理查询结果。

## 数据示例

以共享单车日志为例，共享单车日志内容包括用户年龄、性别、电量使用量、车辆ID、操作延时、纬度、锁类型、经度、操作类型、操作结果和开锁方式。数据保存在名为project:trip\_demo的Project下，名为Logstore:ebike的Logstore中。Project所在地域是cn-hangzhou。

日志示例如下：

```
Time :10-12 14:26:44
__source__: 11.164.232.105 
__topic__: v1 
age: 55 
battery: 118497.673842 
bikeid: 36 
gender: male 
latency: 17 
latitude: 30.2931185245 
lock_type: smart_lock 
longitude: 120.052840484 
op: unlock 
op_result: ok 
open_lock: bluetooth 
userid: 292
```

## JDBC统计

1.  创建一个Maven项目，在POM依赖中添加JDBC依赖，示例代码如下所示。

    ```
    <dependency>
     <groupId>MySQL</groupId>
     <artifactId>mysql-connector-java</artifactId>
     <version>5.1.6</version>
    </dependency>
    ```

2.  使用Java程序通过JDBC查询日志数据，示例代码如下所示。

    ```
    /**
    * Created by mayunlei on 2017/6/19.
    */
    import com.mysql.jdbc.*;
    import java.sql.*;
    import java.sql.Connection;
    import java.sql.Statement;
    /**
    * Created by mayunlei on 2017/6/15.
    */
    public class jdbc {
     public static void main(String args[]){
          //配置您创建的Project名称，Logstore名称等相关信息。
         final String endpoint = "cn-hangzhou-intranet.sls.aliyuncs.com";//日志服务内网或VPC域名。
         final String port = "10005"; //通过JDBC访问日志默认使用10005作为端口号。
         final String project = "trip-demo";
         final String logstore = "ebike";
         final String accessKeyId = "";
         final String accessKey = "";
         Connection conn = null;
         Statement stmt = null;
         try {
             //步骤1 ： 加载JDBC驱动。
             Class.forName("com.mysql.jdbc.Driver");
             //步骤2 ： 创建一个链接。
             conn = DriverManager.getConnection("jdbc:mysql://"+endpoint+":"+port+"/"+project,accessKeyId,accessKey);
             //步骤3 ： 创建statement。
             stmt = conn.createStatement();
             //步骤4 ： 定义查询语句，查询2017年10月11日全天日志中满足条件op = "unlock"的日志条数。
             String sql = "select count(1) as pv,avg(latency) as avg_latency from "+logstore+"  " +
                     "where     __date__  >=  '2017-10-11 00:00:00'   " +
                     "     and  __date__  <   '2017-10-12 00:00:00'" +
                     " and     op ='unlock'";
             //步骤5 ： 执行查询条件。
             ResultSet rs = stmt.executeQuery(sql);
             //步骤6 ： 提取查询结果。
             while(rs.next()){
                 //Retrieve by column name
                 System.out.print("pv:");
                 //获取结果中的pv
                 System.out.print(rs.getLong("pv"));
                 System.out.print(" ; avg_latency:");
                 //获取结果中的avg_latency
                 System.out.println(rs.getDouble("avg_latency"));
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


