# Aggregate data from multiple source Logstores

Log Service allows you to aggregate data from multiple source Logstores to a single destination Logstore. To do this, you can configure a data transformation task for each source Logstore. This topic describes how to aggregate data from multiple source Logstores to a destination Logstore in a typical scenario.

For example, assume that a multinational information company stores the access logs of its website in different Logstores that belong to different Alibaba Cloud accounts. The company needs to aggregate the access logs from the Logstores in the UK \(London\) region for data query and analysis. To do this, the company can use the e\_output function in Log Service to aggregate the transformation results to a destination Logstore.

## Aggregate data from multiple source Logstores

-   Raw log entries
    -   The raw log entries are stored in a Logstore named Logstore\_1 of a project named Project\_1. The project belongs to Account 1 and resides in the UK \(London\) region.

        ```
        Log entry 1
        request_id: 1
        http_host:  m1.abcd.com
        http_status:  200
        request_method:  GET
        request_uri:  /pic/icon.jpg
        
        Log entry 2
        request_id: 2
        http_host:  m2.abcd.com
        http_status:  301
        request_method:  POST
        request_uri:  /data/data.php
        ```

    -   The raw log entries are stored in a Logstore named Logstore\_2 of a project named Project\_2. The project belongs to Account 2 and resides in the UK \(London\) region.

        ```
        Log entry 1
        request_id: 3
        host:  m3.abcd.com
        status:  404
        request_method:  GET
        request_uri:  /category/abc/product_id
        
        Log entry 2
        request_id: 4
        host:  m4.abcd.com
        status:  200
        request_method:  GET
        request_uri:  /data/index.html
        ```

-   Transformation requirements
    -   Aggregate the log entries whose `http_status` is 200 in Logstore\_1 of Account 1 and Logstore\_2 of Account 2 to Logstore\_3 of Account 3.
    -   Set the log field names in Logstore\_1 of Account 1 and Logstore\_2 of Account 2. Set the field name `host` to `http_host` and the field name `status` to `http_status`.
-   DSL orchestration
    -   Configure the following transformation rules for Logstore\_1 of Account 1. On the Create Data Transformation Rule page, set Target Name to target\_logstore, set Target Project to Project\_3, set Target Logstore to Logstore\_3, and specify the authorization method. For more information, see [Create a data transformation rule](/intl.en-US/Data Transformation/Create a data transformation rule.md).

        ```
        e_if(e_match("http_status", "200"), e_output("target_logstore"))
        ```

        ![Transformation rule](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1373836061/p58876.png)

    -   Configure the following transformation rules for Logstore\_2 of Account 2. On the Create Data Transformation Rule page, set Target Name to target\_logstore, set Target Project to Project\_3, set Target Logstore to Logstore\_3, and specify the authorization method.

        ```
        e_if(e_match("status", "200"), e_compose(e_rename("status", "http_status", "host", "http_host"), e_output("target_logstore")))
        ```

-   Result

    The following log entries are aggregated in the Logstore\_3 of Project\_3 that belongs to Account 3. The project resides in the UK \(London\) region.

    ```
    Log entry 1
    request_id: 1
    http_host:  m1.abcd.com
    http_status:  200
    request_method:  GET
    request_uri:  /pic/icon.jpg
    
    Log entry 2
    request_id: 4
    http_host:  m4.abcd.com
    http_status:  200
    request_method:  GET
    request_uri:  /data/index.html
    ```


