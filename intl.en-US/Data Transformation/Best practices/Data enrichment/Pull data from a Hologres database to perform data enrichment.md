# Pull data from a Hologres database to perform data enrichment

This topic describes how to pull data from a Hologres database to perform data enrichment by using resource functions.

-   The endpoint, username, password, database name, and table name of the Hologres database are obtained.
-   The Hologres database instance resides in the same region as the Log Service project that stores the data pulled from the Hologres database.

An e-commerce platform stores sales data and customer identity information in a Hologres database. When a new user logs on to the platform, the platform recommends products to the user based on the biological sex of the user. In this scenario, the platform can pull data from the Hologres database by using the data transformation feature of Log Service. Then, the platform can store the data of the recommended products in the Logstore.

The platform can use the [res\_rds\_mysql](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Resource functions.md) function to pull data from the Hologres database and use the [e\_search\_table\_map](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Data mapping and enrichment functions.md) function to enrich data.

## Use the e\_search\_table\_map function to enrich log data

-   Raw data
    -   The following table shows a database table in Hologres.

        |product\_id|product\_name|product\_price|product\_sales\_number|sex|
        |-----------|-------------|--------------|----------------------|---|
        |2|lipstick|288|2219|girl|
        |5|watch|1399|265|boy|
        |6|mac|4200|265|boy|
        |3|mouse|20|2583|boy|
        |1|basketball|35|3658|boy|
        |4|notebook|9|5427|girl|

    -   The following example shows the sample log entries in a Logstore of Log Service:

        ```
        __source__:192.168.2.100
        __tag__:__client_ip__:192.168.1.100
        age:22
        name:xiaoli
        profession:students
        sex:girl
        
        __source__:192.168.2.200
        __tag__:__client_ip__:192.168.1.200
        age:21
        name:xiaoming
        profession:students
        sex:boy
        ```

-   Transformation rule

    Compare the sex field in the Logstore and the sex field in the Hologres database. Both fields match only when the sex values are the same. If both fields match, the value of the product\_name field is pulled from the Hologres database and concatenated with the log data in the Logstore into new data.

    ```
    e_search_table_map(res_rds_mysql(address="rds-host", 
                                    username="mysql-username",password="yourpassword",database="yourdatabasename",table="yourtablename",
                                    refresh_interval=60,connector='pgsql'),
                                    inpt="sex",output_fields="product_name", multi_match=True, multi_join=",")
    ```

    **Note:** To ensure data security and network stability, we recommend that you access the Hologres database over a virtual private cloud \(VPC\). You can obtain the endpoint from the network configurations of the Hologres database instance and use the endpoint to access a Hologres database over a VPC. After you obtain the endpoint, replace the value of the address field with the endpoint.

-   Result

    ```
    __source__:192.168.2.100
    __tag__:__client_ip__:192.168.1.100
    __tag__:__receive_time__:1615518043
    __topic__:
    age:22
    name:xiaoli
    product_name:lipstick,notebook
    profession:students
    sex:girl
    
    __source__:192.168.2.200
    __tag__:__client_ip__:192.168.1.200
    __tag__:__receive_time__:1615518026
    __topic__:
    age:21
    name:xiaoming
    product_name:basketball,watch,mac,mouse
    profession:students
    sex:boy
    ```


