# Pull data from one Logstore to enrich log data in another Logstore.

This topic describes how to use resource functions to pull data from another Logstore to enrich log data in a Logstore.

A hotel stores the personal information of guests in a Logstore named user\_logstore and stores the check-in information of guests in a Logstore named check-in\_logstore. The hotel wants to obtain some fields from the check-in\_logstore Logstore and concatenate the fields with the fields in the user\_logstore Logstore. The hotel can use the res\_log\_logstore\_pull function to pull data from the check-in\_logstore Logstore and use the e\_table\_map or e\_search\_dict\_map function to map data. For more information about the res\_log\_logstore\_pull function, see [res\_log\_logstore\_pull](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Resource functions.md). For more information about the e\_table\_map function, see [e\_table\_map](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Data mapping and enrichment functions.md). For more information about the e\_search\_dict\_map function, see [e\_search\_table\_map](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Data mapping and enrichment functions.md).

## Data transformation

-   Raw data
    -   The Logstore named user\_logstore that is used to store personal information

        ```
        topic:xxx
        city:xxx
        cid:12345
        name:maki
        
        
        topic:xxx
        city:xxx
        cid:12346
        name:vicky
        
        topic:xxx
        city:xxx
        cid:12347
        name:mary
        ```

    -   The Logstore named check-in\_logstore that is used to store check-in information

        ```
        time:1567038284
        status:check in
        cid:12345
        name:maki
        room_number:1111
        
        time:1567038284
        status:check in
        cid:12346
        name:vicky
        room_number:2222
        
        time:1567038500
        status:check in
        cid:12347
        name:mary
        room_number:3333
        
        time:1567038500
        status:leave
        cid:12345
        name:maki
        room_number:1111
        ```

-   Transformation rule

    **Note:** The res\_log\_logstore\_pull function allows you to set a time range or a start time for data enrichment.

    -   If you set a time range in the transformation rule, for example, from\_time=1567038284 and to\_time=1567038500, data that is received in the specified time range by Log Service is pulled for data enrichment.
    -   If you set a start time in the transformation rule, for example, from\_time="begin", data that is received from the specified time by Log Service is pulled for data enrichment.
    For more information about the fields of the res\_log\_logstore\_pull function, see [res\_log\_logstore\_pull](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Resource functions.md).

    -   e\_table\_map function

        This function maps two log entries by using the cid field. If the value of the cid field in the two log entries equals each other, data mapping succeeds. The room\_number field and field value are returned and concatenated with the log entry in the check-in\_logstore Logstore.

        ```
        e_table_map(res_log_logstore_pull(endpoint, ak_id, ak_secret, project, logstore, 
                fields=["cid","room_number"],
                from_time="begin",
                ), "cid","room_number")
        ```

    -   e\_search\_table\_map function

        This function searches for the cid field whose value is 12346 in the check-in\_logstore Logstore, returns the room\_number field and its value, and concatenates the field with these fields of the log entries in the user\_logstore Logstore.

        ```
        e_search_table_map(res_log_logstore_pull(endpoint, ak_id, ak_secret, project, logstore, 
                fields=["cid","room_number"],
                from_time="begin",
                ), "cid=12346","room_number")
        ```

-   Result
    -   e\_table\_map function

        ```
        topic:xxx
        city:xxx
        cid:12345
        name:maki
        room_nuber:1111
        
        topic:xxx
        city:xxx
        cid:12346
        name:vicky
        room_number:2222
        
        topic:xxx
        city:xxx
        cid:12347
        name:mary
        room_number:3333
        ```

    -   e\_search\_table\_map function

        ```
        topic:xxx
        city:xxx
        cid:12346
        name:vicky
        room_number:2222
        ```


## Configure a whitelist rule and a blacklist rule to filter data

-   Configure a whitelist rule
    -   Transformation rule

        Use the fetch\_include\_data field to configure a whitelist rule. In this example, the fetch\_include\_data="room\_number:1111" expression is included in the res\_log\_logstore\_pull function. This expression indicates that only the log entries whose room\_number field value is 1111 are pulled.

        ```
        res_log_logstore_pull(endpoint, ak_id, ak_secret, project, logstore, ["cid","name","room_number","status"],from_time=1567038284,to_time=1567038500,fetch_include_data="room_number:1111")
        ```

    -   Retrieved data

        ```
        status: check in
        cid:12345
        name:maki
        room_number:1111
        
        status:leave
        cid:12345
        name:maki
        room_number:1111
        ```

-   Configure a blacklist rule
    -   Transformation rule

        Use the fetch\_exclude\_data field to configure a blacklist rule. In this example, the fetch\_exclude\_data="room\_number:1111" expression is included in the res\_log\_logstore\_pull function. This expression indicates that only the log entries whose room\_number field value is 1111 are dropped.

        ```
        res_log_logstore_pull(endpoint, ak_id, ak_secret, project, logstore, ["cid","name","room_number","status"],from_time=1567038284,to_time=1567038500,fetch_exclude_data="room_number:1111")
        ```

    -   Retrieved data

        ```
        status:check in
        cid:12346
        name:vicky
        room_number:2222
        
        
        status:check in
        cid:12347
        name:mary
        room_number:3333
        ```

-   Configure a blacklist rule and a whitelist rule
    -   Transformation rule

        If you configure a blacklist rule and a whitelist rule, the blacklist rule is applied first and then the whitelist rule. In this example, the fetch\_exclude\_data="time:1567038285",fetch\_include\_data="status:check in" expression is included in the res\_log\_logstore\_pull function. This expression indicates that the log entries whose time field value is 1567038285 dropped first and then the log entries whose status field value is check in are pulled.

        ```
        res_log_logstore_pull(endpoint, ak_id, ak_secret, project, logstore, ["cid","name","room_number","status"],from_time=1567038284,to_time=1567038500,fetch_exclude_data="time:1567038285",fetch_include_data="status:check in")
        ```

    -   Retrieved data

        ```
        status:check in
        cid:12345
        name:maki
        room_number:1111
        
        
        status:check in
        cid:12346
        name:vicky
        room_number:2222
        
        
        status:check in
        cid:12347
        name:mary
        room_number:3333
        ```


## Enable primary key maintenance to pull data from destination Logstores

You can delete pulled data before you transform other data. To do this, you can enable the primary key maintenance feature. For example, you want to pull the check-in data of customers who have checked in but not checked out from the Logstore named check-in\_logstore. If a pulled log entry of a customer includes the status:leave field, the customer has checked out. You can enable the primary key maintenance feature and the log entry is not transformed.

**Note:**

-   You can set a single field as the value of the primary\_keys parameter. The field must exist in the fields field.
-   Before you enable the primary key maintenance feature, make sure that the Logstore from which you want to pull data has only one shard.
-   If you enable the primary key maintenance feature, you cannot set the delete\_data field to None.

-   Transformation rule

    ```
    res_log_logstore_pull(endpoint, ak_id, ak_secret, project, logstore, ["cid","name","room_number","status","time"],from_time=1567038284,to_time=None,primary_keys="cid",delete_data="status:leave")
    ```

-   Retrieve data

    The status:leave field in the preceding log entry indicates that the customer whose name is maki has checked out. Therefore, this log entry is not transformed.

    ```
    time:1567038284
    status:check in
    cid:12346
    name:vicky
    room_number:2222
    
    time:1567038500
    status:check in
    cid:12347
    name:mary
    room_number:3333
    ```


