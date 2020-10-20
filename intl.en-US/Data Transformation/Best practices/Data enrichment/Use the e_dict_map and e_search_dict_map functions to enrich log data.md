# Use the e\_dict\_map and e\_search\_dict\_map functions to enrich log data

This topic describes how to enrich log data by using mapping functions such as e\_dict\_map and e\_search\_dict\_map.

The mapping functions in Log Service include common mapping functions and search mapping functions. This section describes the differences between these two types of functions.

-   Common mapping functions map data by using the full-text matching method. Common mapping functions include the e\_dict\_map and e\_table\_map functions. The input data of the e\_dict\_map function is in the dictionary format. The input data of the e\_table\_map function is in the format of a table obtained by using resource functions. For more information about the e\_dict\_map function, see [e\_dict\_map](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Data mapping and enrichment functions.md). For more information about the e\_table\_map function, see [e\_table\_map](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Data mapping and enrichment functions.md). For more information about resource functions, see [Resource functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Resource functions.md).

    For example, you can use the e\_dict\_map function to transform HTTP status codes in NGNIX logs into data of the Text type.

    |HTTP status code|Text|
    |----------------|----|
    |200|Success|
    |300|Redirect|
    |400|Request error|
    |500|Server error|

-   Search mapping functions use [query strings](/intl.en-US/Data Transformation/Data processing syntax/General reference/Query string syntax.md) to map fields. You can specify regular expressions or wildcard characters in query strings and use the exact match or fuzzy match method to map data. Search mapping functions include the e\_search\_dict\_map and e\_search\_table\_map functions. The input data of the e\_search\_dict\_map function is in the dictionary format. The input data of the e\_search\_table\_map function is in the format of a table obtained by using resource functions. For more information about the e\_dict\_map function, see [e\_search\_dict\_map](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Data mapping and enrichment functions.md). For more information about the e\_table\_map function, see [e\_search\_table\_map](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Data mapping and enrichment functions.md). For more information about resource functions, see [Resource functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Resource functions.md).

    For example, you can use the e\_search\_dict\_map function to transform HTTP status codes that match the specified patterns in NGNIX logs into data of the Text type.

    |HTTP status code|Text|
    |----------------|----|
    |2XX|Success|
    |3XX|Redirect|
    |4XX|Request error|
    |5XX|Server error|


## Use the e\_dict\_map function to enrich log data

This section describes how to use the e\_dict\_map function to enrich log data.

-   Raw log entry

    ```
    http_host:  m1.abcd.com
    http_status:  300
    request_method:  GET
    
    http_host:  m2.abcd.com
    http_status:  200
    request_method:  POST
    
    http_host:  m3.abcd.com
    http_status:  400
    request_method:  GET
    
    http_host:  m4.abcd.com
    http_status:  500
    request_method:  GET
    ```

-   Transformation requirements

    Transform the status codes in the http\_status field into data of the Text type and add the transformed data to the status\_desc field.

-   Transformation rule

    ```
    e_dict_map({"400": "Request error", "500": "Server error", "300": "Redirect", "200": "Success"}, "status", "status_desc")
    ```

    **Note:** The preceding transformation rule includes only four HTTP status codes. For more information, see [HTTP status codes](https://www.restapitutorial.com/httpstatuscodes.html). If the value of the http\_status field is 401 or 404, the corresponding value must be included in the source dictionary. Otherwise, the data mapping will fail.

-   Result

    ```
    http_host:  m1.abcd.com
    http_status:  300
    request_method:  GET
    status_desc: Redirect
    
    http_host:  m2.abcd.com
    http_status:  200
    request_method:  POST
    status_desc: Success
    
    http_host:  m3.abcd.com
    http_status:  400
    request_method:  GET
    status_desc: Request error
    
    http_host:  m4.abcd.com
    http_status:  500
    request_method:  GET
    status_desc: Server error
    ```


## Use the e\_search\_dict\_map function to enrich log data

This section describes how to use the e\_search\_dict\_map function to enrich log data.

-   Raw log entry

    ```
    http_host:  m1.abcd.com
    http_status:  200
    request_method:  GET
    body_bytes_sent: 740
    
    http_host:  m2.abcd.com
    http_status:  200
    request_method:  POST
    body_bytes_sent: 1123
    
    http_host:  m3.abcd.com
    http_status:  404
    request_method:  GET
    body_bytes_sent: 711
    
    http_host:  m4.abcd.com
    http_status:  504
    request_method:  GET
    body_bytes_sent: 1822
    ```

-   Transformation requirements

    Add a field named type to each log entry. The value of this field is decided based on the values of the http\_status and body\_bytes\_sent fields in each log entry.

    -   If the value of the http\_status field matches the 2XX pattern and the value of the body\_bytes\_sent field is less than 1000 in a log entry, set the value of the type added to the log entry to Normal.
    -   If the value of the http\_status field matches the 2XX pattern and the value of the body\_bytes\_sent field is equal to or greater than 1000 in a log entry, set the value of the type field added to the log entry to Too long.
    -   If the value of the http\_status field in a log entry matches the 3XX pattern, set the value of the type field added to the log entry to Redirect.
    -   If the value of the http\_status field in a log entry matches the 4XX pattern, set the value of the type field added to the log entry to Error.
    -   If the value of the http\_status field in a log entry does not match either of the preceding patterns, set the value of the type field added to the log entry to Others.
-   Transformation rule

    ```
    e_search_dict_map({'http_status~="2\d+" and body_bytes_sent < 1000': "Normal", 'http_status~="2\d+" and body_bytes_sent >= 1000': "Too long", 'http_status~="3\d+"': "Redirect", 'http_status~="4\d+"': "Error",  "*": "Others"}, "http_status", "type")
    ```

    If you want to use a dictionary to enrich your log data, you can create a dictionary by using braces \(\{\}\) or based on resources allocated to the task, Object Storage Service \(OSS\) resources, and tables. For more information, see [Build dictionaries](/intl.en-US/Data Transformation/Best practices/Data enrichment/Build dictionaries and tables for data enrichment.md).

-   Result

    ```
    type: Normal
    http_host:  m1.abcd.com
    http_status:  200
    request_method:  GET
    body_bytes_sent: 740
    
    type: Too long
    http_host:  m2.abcd.com
    http_status:  200
    request_method:  POST
    body_bytes_sent: 1123
    
    type: Error
    http_host:  m3.abcd.com
    http_status:  404
    request_method:  GET
    body_bytes_sent: 711
    
    type: Others
    http_host:  m4.abcd.com
    http_status:  504
    request_method:  GET
    body_bytes_sent: 1822
    ```


