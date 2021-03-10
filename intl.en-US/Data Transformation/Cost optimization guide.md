# Cost optimization guide

The data transformation feature helps reduce your time and labor costs to tidy data and boost your business. This topic describes how to configure rules to transform data at an optimal cost.

## Typical configurations

We recommend that you import log data into one or more Logstores and use the data transformation feature to dispatch transformed data to destination Logstores. In addition, we recommend that you configure retention periods and indexes for data in different destination Logstores. For more information, see [Data transformation basics](/intl.en-US/Data Transformation/Data transformation basics.md) and [Performance guide](/intl.en-US/Data Transformation/Performance guide.md).

![Typical configurations](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0453749951/p59422.png)

## Cost factors

When you use Log Service, your costs depend on the following factors. For more information, see [Billing methods](/intl.en-US/Pricing/Pay-as-you-go.md).

-   The amount of data imported per day
-   The data retention period
-   The number of indexes that you create

The following examples describe how to optimize costs.

## Optimize imported logs

Assume that you collect logs from an application and import 100 GB of log data into a source Logstore per day. You also create full-text indexes for the log data and set a data retention period of 30 days. In this case, you are billed about USD 320 per month.

However, you want to filter the logs from pods of a certain type, such as operations logs and error logs. These logs account for 20% of the total amount of the raw logs. You also want to retain these logs for 30 days and retain other logs for seven days. In this case, we recommend that you use the following method to optimize costs:

-   Create a source Logstore that retains logs for three days. Do not create indexes for the log data.
-   Create a destination Logstore to store operations logs and error logs for 30 days and create indexes for the log data.
-   Create another destination Logstore to store other logs for seven days and create indexes for the log data.

In this case, you are billed about USD 240 per month. This method can reduce your costs by 25%.

You can use the data transformation feature to retain important logs for 60 days and the other logs for seven days. If you want to retain 20% of your logs, your costs are reduced by 12% and the retention period of those logs is doubled.

## Optimize log entries in a log

Assume that you collect logs from an application and import 100 GB of log data into a source Logstore per day. You also create full-text indexes for the log data and set a data retention period of 30 days. In this case, you are billed about USD 320 per month.

The following is a raw log entry with a size of 1021 bytes.

```
__source__:  1.2.3.4
__topic__:  ddos_access_log
body_bytes_sent:  3866
cc_action:  none
cc_blocks:  
cc_phase:  
content_type:  text/x-flv
host:  www.dbb.mock-domain.com
http_cookie:  i1=w1;x2=q2
http_referer:  http://www.cbc.mock-domain.com
http_user_agent:  Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/40.0.2214.115 Safari/537.36
http_x_forwarded_for:  105.120.151.10
https:  true
isp_line:  BGP
matched_host:  www.cbd.mock-host.com
method:  GET
real_client_ip:  105.120.160.17
remote_addr:  105.120.160.0
remote_port:  48196
request_length:  2946
request_method:  GET
request_time_msec:  78920
request_uri:  /request/nvwlvvkhw
server_name:  www.bd.mock-host.com
status:  502
time:  2019-07-22T17:40:26+08:00
ua_browser:  mozilla
ua_browser_family:  
ua_browser_type:  
ua_browser_version:  9.0
ua_device_type:  
ua_os:  windows_7
ua_os_family:  
upstream_addr:  106.120.157.15:80
upstream_ip:  109.120.152.11
upstream_response_time:  0.858
upstream_status:  200
user_id:  st0s2b5
```

If you require only some fields in the log entry, you can retain the destination fields for 30 days and create indexes for these fields. You can retain other fields for only three days. In this case, we recommend that you use the following method:

-   Create a source Logstore that retains logs for three days. Do not create indexes for the log data.
-   Create a destination Logstore to store operations logs and error logs for 30 days and create indexes for the log data.

If the size of a transformed log entry is 60% the size of the raw log entry, you are billed about USD 224 per month. This method can reduce your costs by 30%.

The following is the 618-byte log entry transformed from the 1021-byte raw log entry.

```
__source__:  1.2.3.4
__topic__:  ddos_access_log
body_bytes_sent:  3866
content_type:  text/x-flv
host:  www.dbb.mock-domain.com
http_referer:  http://www.cbc.mock-domain.com
ua_browser:  mozilla
ua_browser_family:  
ua_browser_type:  
ua_browser_version:  9.0
ua_device_type:  
ua_os:  windows_7
http_x_forwarded_for:  105.120.151.10
matched_host:  www.cbd.mock-host.com
method:  GET
real_client_ip:  105.120.160.17
request_length:  2946
request_uri:  /request/nvwlvvkhw
status:  502
upstream_addr:  106.120.157.15:80
upstream_ip:  109.120.152.11
upstream_response_time:  0.858
upstream_status:  200
user_id:  st0s2b5
```

