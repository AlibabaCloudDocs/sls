# Associate Log Service with OSS

This topic describes how to create an external store and associate Log Service with Object Storage Service \(OSS\).

-   A project and a Logstore are created. For more information, see [Step 1: Create a project and a Logstore](/intl.en-US/.md).
-   Log data is collected. For more information, see [Data collection](/intl.en-US/Data Collection/Log collection methods.md).
-   The indexing feature is enabled for the Logstore and indexes are configured. For more information, see [Configure indexes](/intl.en-US/Index and query/Configure indexes.md).
-   An OSS bucket is created. For more information, see [Create buckets](/intl.en-US/Quick Start/OSS console/Create buckets.md).
-   CSV objects are uploaded to the OSS bucket. For more information, see [Upload objects](/intl.en-US/Quick Start/OSS console/Upload objects.md).

## Benefits

-   Lower storage cost

    You can store infrequently updated data in OSS buckets. This reduces your storage cost. If you store the data in an ApsaraDB RDS for MySQL instance, you must pay for the instance.

-   Lower traffic cost

    You can store data in OSS buckets and read the data over the internal network.


## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  Click the project in the Projects section.

3.  Choose **Log Storage** \> **Logstores**. On the Logstores tab, click the management icon of the Logstore.

4.  Enter the following query statement in the search box and click **Search & Analytics**.

    Create an external store named user\_meta1 and map the data in the store to objects of the specified OSS bucket. If the execution **result** is **true**, an external store is created and data is mapped.

    ```
    * | create table user_meta1 ( userid bigint, nick varchar, gender varchar, province varchar, gender varchar,age bigint) with ( endpoint='oss-cn-hangzhou-internal.aliyuncs.com',accessid='**************************',accesskey ='****************************',bucket='testoss*********',objects=ARRAY['user.csv'],type='oss')
    ```

    ![External store](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5131201061/p8538.png)

    Configure the name of the external store and schema of the store in the CREATE clause and specify the information about the OSS bucket and objects in the WITH clause.

    |Parameter|Description|
    |:--------|:----------|
    |External store name|The name of the external store, for example user\_meta1.|
    |Schema|The column names, data type of the columns, and other properties of the external store, for example, \(userid bigint, nick varchar, gender varchar, province varchar, gender varchar,age bigint\).|
    |endpoint|The endpoint of the OSS bucket. For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).|
    |accessid|The AccessKey ID of your account.|
    |accesskey|The AccessKey secret of your account.|
    |bucket|The OSS bucket that stores the CSV objects.|
    |objects|The path of the CSV objects. **Note:** The value of the objects parameter is an array. The array can contain multiple elements. Each element represents an OSS object. |
    |type|The type of the external store. Set the value to oss.|

5.  Run the following statement to check whether the external store is created.

    If the returned result is the content that you have defined, the external store is created.

    ```
    * | select * from user_meta1
    ```

    ![Verify the settings](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6051201061/p8539.png)

6.  Use the JOIN syntax to join data in Log Service and OSS.

    For example, you can run the following statement to join data in Log Service and OSS by using the id log field and the userid field in OSS objects. test\_accesslog is the name of the Logstore. user\_meta1 is the name of the external store. Replace the parameter values based on your business requirements.

    ```
    * | select * from test_accesslog l join user_meta1 u on l.userid = u.userid
    ```

    ![Join data](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5131201061/p8540.png)


For more information about how to associate Log Service with OSS, see [Perform an association query and analysis on Log Service logs and OSS tables](/intl.en-US/Index and query/Best practices/Perform an association query and analysis on Log Service logs and OSS tables.md).

