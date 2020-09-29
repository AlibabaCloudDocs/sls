# Collect and analyze NGINX monitoring logs

NGINX provides a built-in status page that allows you to monitor the status of NGINX. This topic describes how to use the Logtail feature of Log Service to collect NGINX status information. The topic also describes how to query and analyze the collected status information, and customize alerts to monitor your NGINX cluster.

Logtail is installed on the server that you use to collect MySQL binary logs. For more information, see [Install Logtail in Linux](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md) or [Install Logtail in Windows](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Windows.md).

**Note:** Servers that run Linux support Logtail 0.16.0 or later. Servers that run Windows support Logtail 1.0.0.8 or later.

## Step 1: Prepare the environment

Perform the following steps to enable the NGINX status page:

1.  Run the following command to check whether NGINX supports the status feature. For more information, see [Module ngx\_http\_stub\_status\_module](http://nginx.org/en/docs/http/ngx_http_stub_status_module.html).

    ```
    nginx -V 2>&1 | grep -o with-http_stub_status_module
    with-http_stub_status_module 
    ```

    If the message `with-http_stub_status_module` is returned, NGINX supports the status feature.

2.  Configure the NGINX status feature.

    Enable the status feature in the NGINX configuration file. This file is stored in the /etc/nginx/nginx.conf directory. Use the following example to configure the status feature. For more information, see [Enable Nginx Status Page](https://easyengine.io/tutorials/nginx/status-page/).

    **Note:** In the preceding example, allow 10.10.XX.XX indicates that only the server whose IP address is 10.10.XX.XX is allowed to access the NGINX status page.

    ```
         location /private/nginx_status {
           stub_status on;
           access_log   off;
           allow 10.10.XX.XX;
           deny all;
         }                       
    ```

3.  Run the following command to verify whether the server on which Logtail is installed can access the NGINX status page:

    ```
    $curl http://10.10.XX.XX/private/nginx_status
    ```

    If the following message is returned, the NGINX status page is enabled:

    ```
    Active connections: 1
    server accepts handled requests
    2507455 2507455 2512972
    Reading: 0 Writing: 1 Waiting: 0                       
    ```


## Step 2: Collect NGINX monitoring logs

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Import Data section, select **Custom Data Plug-in**.

3.  In the Specify Logstore step, select the target project and Logstore, and click **Next**.

    You can also click **Create Now** to create a project and a Logstore. For more information, see [Step 1: Create a project and a Logstore](/intl.en-US/Quick Start/Quick start.md).

4.  In the Create Machine Group step, create a machine group.

    -   If a machine group is available, click **Using Existing Machine Groups**.
    -   This section uses ECS instances as an example to describe how to create a machine group. To create a machine group, perform the following steps:
        1.  Install Logtail on ECS instances. For more information, see [Install Logtail on ECS instances](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail on ECS instances.md).

            If Logtail is installed on the ECS instances, click **Complete Installation**.

            **Note:** If you need to collect logs from user-created clusters or servers of third-party cloud service providers, you must install Logtail on these servers. For more information, see [Install Logtail in Linux](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md#) or [Install Logtail in Windows](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Windows.md).

        2.  After the installation is complete, click **Complete Installation**.
        3.  On the page that appears, specify the parameters for the machine group. For more information, see [Create an IP address-based machine group](/intl.en-US/Data Collection/Logtail collection/Machine Group/Create an IP address-based machine group.md) or [Create a custom ID-based machine group](/intl.en-US/Data Collection/Logtail collection/Machine Group/Create a custom ID-based machine group.md).
5.  In the Machine Group Settings step, apply the configurations to the machine group.

    Select the created machine group and move the group from **Source Server Groups** to **Applied Server Groups**.

6.  In the Specify Data Source step, set the **Config Name** and **Plug-in Config** parameters.

    -   inputs: Required. The Logtail configurations for log collection.

        **Note:** You can configure only one type of data source in the inputs field.

    -   processors: Optional. The Logtail configurations for data processing. You can configure one or more processing methods in the processors field. For more information, see [Process data](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to collect data/Process data.md).
    ```
    {
    "inputs": [
     {
          "type": "metric_http",
          "detail": {
              "IntervalMs": 60000,
              "Addresses": [
                  "http://10.10.XX.XX/private/nginx_status",
                  "http://10.10.XX.XX/private/nginx_status",
                  "http://10.10.XX.XX/private/nginx_status"
              ],
              "IncludeBody": true
          }
     }
    ],
    "processors": [
     {
          "type": "processor_regex",
          "detail": {
              "SourceKey": "content",
              "Regex": "Active connections: (\\d+)\\s+server accepts handled requests\\s+(\\d+)\\s+(\\d+)\\s+(\\d+)\\s+Reading: (\\d+) Writing: (\\d+) Waiting: (\\d+)[\\s\\S]*",
              "Keys": [
                  "connection",
                  "accepts",
                  "handled",
                  "requests",
                  "reading",
                  "writing",
                  "waiting"
              ],
              "FullMatch": true,
              "NoKeyError": true,
              "NoMatchError": true,
              "KeepSource": false
          }
     }
    ]
    }                                
    ```

    The following table describes the required parameters.

    |Parameter|Type|Required|Description|
    |:--------|:---|:-------|:----------|
    |type|string|Yes|The type of the data source. Set the value to metric\_http.|
    |IntervalMs|int|Yes|The interval between two successive requests. Unit: milliseconds.|
    |Addresses|String|Yes|The list of URLs that you want to monitor.|
    |IncludeBody|bool|No|Specifies whether to collect the request body. Default value: false. If you set the value to true, the content of the request body is stored in the field named content.|

7.  In the **Configure Query and Analysis** step, configure the indexes.

    Indexes are configured by default. You can re-configure the indexes based on your business requirements. For more information, see [Enable and configure the index feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).

    **Note:**

    -   You must configure Full Text Index or Field Search. If you configure both of them, the settings of Field Search are applied.
    -   If the data type of index is long or double, the Case Sensitive and Delimiter settings are unavailable.

One minute after the collection configurations are complete, you can view the collected status data, as shown in the following example. By default, Log Service provides dashboards to display NGINX monitoring data.

```
_address_:http://10.10.XX.XX/private/nginx_status  
_http_response_code_:200  
_method_:GET  
_response_time_ms_:1.83716261897  
_result_:success  
accepts:33591200  
connection:450  
handled:33599550  
reading:626  
requests:39149290  
waiting:68  
writing:145                  
```

## Step 3: Query and analyze NGINX monitoring logs

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Projects section, click a project.

3.  On the **Log Management** \> **Logstores** tab, choose **![Management icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9484688951/p52166.png)** \> **Search & Analysis** to the right of a Logstore.

4.  In the upper-right corner of the page, click **15 Minutes \(Relative\)** to set a time range for the query.

    You can select a relative time or time frame, or customize a time range.

    **Note:** The query results may contain logs that are generated one minute earlier or later than the specified time range.

5.  Enter a query statement in the search box, and then click **Search & Analyze**.

    For more information, see [Real-time log analysis](/intl.en-US/Index and query/Real-time log analysis.md) and [Create an alert rule](/intl.en-US/Query and visualization/Alarm/Create an alert rule.md).

    -   Query logs
        -   To query the status of the server whose IP address is 10.10.0.0, execute the following statement:

            ```
            _address_ : 10.10.0.0
            ```

        -   To query the requests whose response time is greater than 100 ms, execute the following statement:

            ```
            _response_time_ms_ > 100
            ```

        -   To query the requests whose HTTP status code is not 200, execute the following statement:

            ```
            not _http_response_code_ : 200
            ```

    -   Analyze logs
        -   To query the average number of waiting connections, reading connections, writing connections, and connections every 5 minutes, execute the following statement:

            ```
            *| select  avg(waiting) as waiting, avg(reading)  as reading,  avg(writing)  as writing,  avg(connection)  as connection,  from_unixtime( __time__ - __time__ % 300) as time group by __time__ - __time__ % 300 order by time limit 1440                       
            ```

        -   To query the top 10 servers with the largest number of waiting connections, execute the following statement:

            ```
            *| select  max(waiting) as max_waiting, address, from_unixtime(max(__time__)) as time group by address order by max_waiting desc limit 10                        
            ```

        -   To query the total number of requests and failed requests, execute the following statement:

            ```
            * | select  count(distinct(address)) as total                       
            ```

            ```
            not _result_ : success | select  count(distinct(address))                        
            ```

        -   To query the 10 most recent failed requests, execute the following statement:

            ```
            not _result_ : success | select _address_ as address, from_unixtime(__time__) as time  order by __time__ desc limit 10                       
            ```

        -   To query the total number of requests handled every 5 minutes, execute the following statement:

            ```
            *| select  avg(handled) * count(distinct(address)) as total_handled, avg(requests) * count(distinct(address)) as total_requests,  from_unixtime( __time__ - __time__ % 300) as time group by __time__ - __time__ % 300 order by time limit 1440                       
            ```

        -   To query the average latency of requests every 5 minutes, execute the following statement:

            ```
            *| select  avg(_response_time_ms_) as avg_delay,  from_unixtime( __time__ - __time__ % 300) as time group by __time__ - __time__ % 300 order by time limit 1440                      
            ```

        -   To query the number of valid and invalid requests, execute the following statement:

            ```
            not _http_response_code_ : 200  | select  count(1)                     
            ```

            ```
            _http_response_code_ : 200  | select  count(1)                       
            ```


