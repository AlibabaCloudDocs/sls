# Query logs by using the JDBC API

You can use the JDBC API to connect a database such as a MySQL database to Log Service and use the ANSI SQL-92 syntax to query logs.

-   An AccessKey pair is created for an Alibaba Cloud account or a RAM user.

    If you use an AccessKey pair of a RAM user, the RAM user must belong to the Alibaba Cloud account to which the project you want to connect belongs. In addition, the RAM user must be granted read permissions on the project.

-   A project and a Logstore are created. For more information, see [Step 1: Create a project and a Logstore](/intl.en-US/Quick Start/Quick start.md).

## Parameters

This section describes the parameters that are used to connect a MySQL database to Log Service.

```
mysql -hcn-shanghai-intranet.log.aliyuncs.com -ubq2sjzesjmo86kq -p4fdO1fTDDuZP -P10005
use sample-project; 
```

**Note:** MySQL does not support pagination for queries connected through JDBC.

The following table describes the parameters in the command.

|Parameter|Description|
|:--------|:----------|
|host|The endpoint of Log Service. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md). You can access Log Service over the classic network or a virtual private cloud \(VPC\). An example of a VPC-type endpoint is cn-hangzhou-intranet.log.aliyuncs.com.|
|port|The port used to access Log Service. Default value: 10005.|
|user|The AccessKey ID of your Alibaba Cloud account.|
|password|The AccessKey secret of your Alibaba Cloud account.|
|database|The Log Service project.|
|table|The Logstore.|

## Query syntax

-   Filtering syntax

    **Note:** You must include the \_\_date\_\_ or \_\_time\_\_ field in a WHERE clause to limit the time range of a query. The data type of the \_\_date\_\_ field is timestamp, and the data type of the \_\_time\_\_ field is bigint. Examples:

    -   \_\_date\_\_ \> '2017-08-07 00:00:00' and \_\_date\_\_ < '2017-08-08 00:00:00'
    -   \_\_time\_\_ \> 1502691923 and \_\_time\_\_ < 1502692923
    The following table describes the filtering syntax of a WHERE clause.

    |Semantics|Examples|Description|
    |:--------|:-------|:----------|
    |String search|key = "value"|Queries data after word-delimiting.|
    |String fuzzy search|    -   key has 'valu\*'
    -   key like 'value\_%'
|Queries data in fuzzy match mode after word-delimiting.|
    |Value comparison|num\_field \> 1|The comparison operators include greater than \(\>\), greater than or equal to \(\>=\), equal to \(=\), less than \(<\), and less than or equal to \(<=\).|
    |Logical operations|and or not|Examples: a = "x" and b ="y" and a = "x" and not b ="y".|
    |Full-text search|\_\_line\_\_ ="abc"|Requires the special key \(\_\_line\_\_\).|

-   Computation syntax

    For more information about supported computation operators, see [Real-time analysis](/intl.en-US/Index and query/Real-time log analysis.md).

-   SQL-92 syntax

    The SQL-92 syntax includes the filtering syntax and calculation syntax. Example:

    ```
    status>200 |select avg(latency),max(latency) ,count(1) as c GROUP BY  method  ORDER BY c DESC  LIMIT 20
    ```

    You can combine the filtering condition expression in the preceding statement with a time condition expression and include the expressions in a WHERE clause. This clause follows the SQL-92 syntax standard, as shown in the following statement:

    ```
    select avg(latency),max(latency) ,count(1) as c from sample-logstore where status>200 and __time__>=1500975424 and __time__ < 1501035044 GROUP BY  method  ORDER BY c DESC  LIMIT 20
    ```


## Access Log Service by using the JDBC API

-   Use an application to query data in Log Service

    You can use an application that supports the MySQL connector to access Log Service and use the MySQL syntax to query data in Log Service. You can use the JDBC API or MySQLdb API to access Log Service. The following script shows how to use the JDBC API to access Log Service.

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
                System.out.println("Connected database successfully...") ;
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
                if (stmt ! = null) {
                    try {
                        stmt.close();
                    } catch (SQLException e) {
                        e.printStackTrace();
                    }
                }
                if (conn ! = null) {
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

-   Use a tool to access Log Service

    Use a MySQL client to access Log Service over the classic network or a VPC.

    ![Example](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9159713061/p5647.png)

    -   Enter your project name at 1.
    -   Enter your Logstore name at 2.

