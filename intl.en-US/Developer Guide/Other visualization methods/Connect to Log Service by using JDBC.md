# Connect to Log Service by using JDBC

This topic describes how to use Java Database Connectivity \(JDBC\) to connect to Log Service, read log data, and use the MySQL protocol and SQL syntax to collect log statistics.

Indexes are configured and the Enable Analytics switch is turned on for the required fields. For more information, see [Configure indexes](/intl.en-US/Index and query/Configure indexes.md).

MySQL is a popular relational database. Many software products allow you to obtain MySQL data by using the MySQL protocol and SQL syntax. To connect to Log Service by using JDBC, we recommend that you review SQL syntax. Log Service provides the MySQL protocol to query and analyze logs. You can use a standard MySQL client to connect to Log Service and use the standard SQL syntax to collect log statistics and analyze logs. Clients that support the MySQL transport protocol include MySQL client, JDBC, and Python MySQLdb.

JDBC scenarios:

-   Use a visualization tool such as DataV, Tableau, or Kibana to connect to Log Service by using the MySQL protocol. For more information, see [Query logs by using the JDBC API](/intl.en-US/Index and query/Query logs by using the JDBC API.md).
-   Use libraries such as JDBC in Java or MySQLdb in Python to access Log Service and process query results in the application.

## Example

A bike-sharing log contains the information of a user. The information includes the age, gender, battery usage, vehicle ID, operation latency, latitude, lock type, longitude, operation type, operation result, and unlocking type. Data is stored in the Logstore:ebike Logstore of the project:trip\_demo project. The project resides in the China \(Hangzhou\) region.

Sample log:

```
Time :10-12 14:26:44
__source__: 192.168.0.0
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

## JDBC statistics

1.  Create a Maven project and add the following JDBC dependency to the pom.xml file:

    ```
    <dependency>
     <groupId>MySQL</groupId>
     <artifactId>mysql-connector-java</artifactId>
     <version>5.1.6</version>
    </dependency>
    ```

2.  Use the following code to create a Java application to query logs by using JDBC.

    **Note:** You must include the \_\_date\_\_ or \_\_time\_\_ field in a WHERE clause to limit the time range of a query. The data type of the \_\_date\_\_ field is timestamp, and the data type of the \_\_time\_\_ field is bigint. Examples:

    -   \_\_date\_\_ \> '2017-08-07 00:00:00' and \_\_date\_\_ < '2017-08-08 00:00:00'
    -   \_\_time\_\_ \> 1502691923 and \_\_time\_\_ < 1502692923
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
         final String endpoint = "trip-demo.cn-hangzhou-intranet.sls.aliyuncs.com"; // The endpoint of Log Service that is in the classic network or a virtual private cloud (VPC). Set this variable to an endpoint that is specific to your environment. For more information, see [Endpoints for the classic network and VPC](/intl.en-US/Developer Guide/API Reference/Endpoints.md). 
         final String port = "10005"; // If you use JDBC to access logs, set the MySQL port to 10005. 
         final String project = "trip-demo"; // The name of the project. 
         final String logstore = "ebike"; // The name of the Logstore. 
         final String accessKeyId = ""; // The AccessKey ID of your Alibaba Cloud account. For more information, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md). 
         final String accessKey = ""; // The AccessKey secret of your Alibaba Cloud account. 
         Connection conn = null;
         Statement stmt = null;
         try {
             // Step 1: Load the JDBC driver. 
             Class.forName("com.mysql.jdbc.Driver");
             // Step 2: Create a connection string. 
             conn = DriverManager.getConnection("jdbc:mysql://"+endpoint+":"+port+"/"+project,accessKeyId,accessKey);
             // Step 3: Create a statement. 
             stmt = conn.createStatement();
             // Step 4: Enter a query statement to query the number of log entries that were generated on October 11, 2017 and whose op field is set to unlock. 
             String sql = "select count(1) as pv,avg(latency) as avg_latency from "+logstore+"  " +
                     "where     __date__  >=  '2017-10-11 00:00:00'   " +
                     "     and  __date__  <   '2017-10-12 00:00:00'" +
                     " and     op ='unlock'";
             // Step 5: Execute the statement. 
             ResultSet rs = stmt.executeQuery(sql);
             // Step 6: Extract the query result. 
             while(rs.next()){
                 //Retrieve by column name
                 System.out.print("pv:");
                 // Obtain pv from the result. 
                 System.out.print(rs.getLong("pv"));
                 System.out.print(" ; avg_latency:");
                 // Obtain avg_latency from the result. 
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


