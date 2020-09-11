# Connect Log Service with JDBC

This topic describes how to use Java Database Connectivity \(JDBC\) to connect to Log Service, read log data, and use the MySQL protocol and SQL syntax to collect log statistics.

The indexing and analytics features are enabled for the destination field. For more information, see [Enable and configure the index feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).

MySQL is a popular relational database. Many software products allow you to obtain MySQL data by using the MySQL protocol and SQL syntax. To connect Log Service with JDBC, you must familiarize yourself with the SQL syntax. Log Service provides MySQL protocol to query and analyze logs. You can use a standard MySQL client to connect to Log Service and use the standard SQL syntax to compute and analyze logs. You can use a standard MySQL client to connect to Log Service and use the standard SQL syntax to collect log statistics and analyze logs. Clients that support the MySQL transport protocol include MySQL client, JDBC, and Python MySQLdb.

JDBC scenarios:

-   Use a visualization tool such as DataV, Tableau, or Kibana to connect to Log Service by using the MySQL protocol.
-   Use libraries such as JDBC in Java or MySQLdb in Python to access Log Service and process query results in the application.

## Example

A bike-sharing log contains the user's age, gender, battery usage, vehicle ID, operation latency, latitude, lock type, longitude, operation type, operation result, and unlocking type. Data is stored in the Logstore:ebike Logstore of the project:trip\_demo project. The project resides in the China \(Hangzhou\) region.

Sample log:

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

## JDBC statistics

1.  Create a Maven project and add the following JDBC dependency to the pom.xml file.

    ```
    <dependency>
     <groupId>MySQL</groupId>
     <artifactId>mysql-connector-java</artifactId>
     <version>5.1.6</version>
    </dependency>
    ```

2.  Use the following code to create a Java application to query logs by using JDBC.

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
          // Set the required parameters, such as the project name and Logstore name.
         final String endpoint = "cn-hangzhou-intranet.sls.aliyuncs.com";// Set the private endpoint of Log Service or a VPC.
         final String port = "10005"; // Set the MySQL port to 10005. JDBC uses this port to access log by default.
         final String project = "trip-demo";
         final String logstore = "ebike";
         final String accessKeyId = "";
         final String accessKey = "";
         Connection conn = null;
         Statement stmt = null;
         try {
             // Step 1: Load the JDBC driver.
             Class.forName("com.mysql.jdbc.Driver");
             // Step 2: Create a connection string.
             conn = DriverManager.getConnection("jdbc:mysql://"+endpoint+":"+port+"/"+project,accessKeyId,accessKey);
             // Step 3: Create a statement.
             stmt = conn.createStatement();
             // Step 4: Create a search statement. Query the number of logs that are generated on October 11, 2017, and meet the condition op = "unlock".
             String sql = "select count(1) as pv,avg(latency) as avg_latency from "+logstore+"  " +
                     "where     __date__  >=  '2017-10-11 00:00:00'   " +
                     "     and  __date__  <   '2017-10-12 00:00:00'" +
                     " and     op ='unlock'";
             // Step 5: Run the statement.
             ResultSet rs = stmt.executeQuery(sql);
             // Step 6: Extract the query results.
             while(rs.next()){
                 //Retrieve by column name
                 System.out.print("pv:");
                 // Obtain pv from the results.
                 System.out.print(rs.getLong("pv"));
                 System.out.print(" ; avg_latency:");
                 // Obtain avg_latency from the results.
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


