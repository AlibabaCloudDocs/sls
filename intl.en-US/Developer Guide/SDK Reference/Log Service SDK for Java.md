# Log Service SDK for Java

This topic describes how to install and use Log Service SDK for Java.

-   Log Service is activated. For more information, see [Activate Log Service](https://www.aliyun.com/product/sls?spm=5176.7933691.J_8058803260.20.3eeb2a665LA0eU).
-   An AccessKey pair is created and obtained. For more information, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).
-   A Java development environment is installed.

    Log Service SDK for Java supports the Java runtime environment of Java SE 6 or later. You can run the java -version command to check the installed Java version. You can download the installation package from the [Java official website](http://developers.sun.com/downloads/) and install the Java development environment.


## Step 1: Install Log Service SDK for Java

You can install Log Service SDK for Java by using one of the following methods:

-   Method 1: Recommended. Add dependencies to your Maven project.

    To use Log Service SDK for Java in Maven, you must add the corresponding dependencies to the pom.xml file. Log Service SDK for Java V0.6.60 is used in this example. Add the following content to <dependencies\>:

    ```
    <dependency>
        <groupId>com.aliyun.openservices</groupId>
        <artifactId>aliyun-log</artifactId>
        <version>0.6.60</version>
        </dependency>
    ```

-   Method 2: Import the JAR package to Eclipse.

    Log Service SDK for Java V0.6.60 is used in this example. To import the JAR package, perform the following steps:

    1.  Download Log Service SDK for Java. For more information, see [Aliyun LOG Java SDK](https://mvnrepository.com/artifact/com.aliyun.openservices/aliyun-log).
    2.  Copy the aliyun-log-0.6.60.jar file to Eclipse.
    3.  Right-click your project name in Eclipse and choose **Properties** \> **Java Build Path** \> **Add JARs**.
    4.  Select the aliyun-log-0.6.60.jar file.

## Step 2: Create a Log Service client

A Log Service client is a Java client that you can use to manage Log Service resources, such as projects and Logstores. To use Log Service SDK for Java to initiate a service request, you must create a client instance.

```
String accessId = "your_access_id"; // The AccessKey ID of your Alibaba Cloud account. For more information, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md). Security risks may arise if you use the AccessKey pair of your Alibaba Cloud account because the account has permissions to call all API operations. We recommend that you create and use a RAM user to call API operations or perform routine O&M.
String accessKey = "your_access_key"; // The AccessKey secret of your Alibaba Cloud account.
String host = "cn-hangzhou-staging-intranet.sls.aliyuncs.com"; // The endpoint of Log Service. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md). The China (Hangzhou) region is used as an example. For other regions, configure the endpoint based on the project name and the region where the project resides.
Client client = new Client(host, accessId, accessKey); // Create the Log Service client.
```

## Sample code

Log Service SDK for Java provides a variety of sample programs. For more information, see [aliyun-log-java-sdk](https://github.com/aliyun/aliyun-log-java-sdk). The following sample code shows how to create a project and a Logstore.

```
// Create a project and a Logstore.
String project = "your-project"; // The name of the project.
String logstore = "your-logstore"; // The name of the Logstore.
int ttl_in_day = 3; // The data retention period. If you set the data retention period to 3650, the data is permanently stored.
int shard_count = 10; // The number of shards.
LogStore store = new LogStore(logstore, ttl_in_day, shard_count);
CreateLogStoreResponse res = client.CreateLogStore(project, store);
```

**Note:**

-   To improve the I/O efficiency of your system, we recommend that you do not use SDKs to write data to Log Service. For more information about how to write data to Log Service, see [Producer Library](/intl.en-US/Data Collection/Other collection methods/SDK collection/Producer Library.md).
-   If you want to consume data in Log Service, we recommend that you do not use the API in Log Service SDK for Java to pull data. We recommend that you use consumer groups to consume data. For more information, see [Use consumer groups to consume log data](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Consumption by consumer groups/Use consumer groups to consume log data.md). This method consumes data in a manner that is transparent to the user, and provides advanced features such as load balancing and sequential consumption.

