# Configure the startup parameters of Logtail

To prevent Logtail from consuming excessive server resources and affecting other services, Log Service allows you to limit the collection performance of Logtail. If you need to improve the collection performance of Logtail, you can modify the startup parameters of Logtail.

## Scenarios

You can modify the startup parameters of Logtail in the following scenarios:

-   You need to collect a large number of log files, which occupy a large amount of memory. For example, you need to collect more than 100 files, or monitor more than 5,000 files in a single directory at the same time.
-   The CPU utilization is high due to large log data traffic. For example, Logtail collects log data at a speed of more than 2 MB/s in simple mode or more than 1 MB/s in full regex mode.
-   Logtail sends data to Log Service at a speed of more than 20 MB/s.

## Set the startup parameters

1.  On the server where Logtail is installed, open the file /usr/local/ilogtail/ilogtail\_config.json.

2.  Set the startup parameters as needed.

    The following example shows some startup parameters:

    ```
    {
        ...
        "cpu_usage_limit" : 0.4,
        "mem_usage_limit" : 100,
        "max_bytes_per_sec" : 2097152,
        "process_thread_count" : 1,
        "send_request_concurrency" : 4,
        "buffer_file_num" : 25,
        "buffer_file_size" : 20971520,
        "buffer_file_path" : "",
        ...
    }
    ```

    **Note:**

    -   The following table lists only commonly used startup parameters. For the startup parameters that are not listed in the table, use the default values.
    -   You can add or modify the specified startup parameters as needed.
    The following table describes the startup parameters of Logtail.

    |Parameter|Type|Description|Example|
    |:--------|----|:----------|-------|
    |cpu\_usage\_limit|double|The CPU utilization threshold for Logtail. The threshold is calculated based on a single-core CPU.     -   Valid values: 0.1 to the number of CPU cores of the current server
    -   Default value: 2
For example, if you set the parameter to 0.4, the utilization of a single-core CPU for Logtail is limited to 40%. Logtail restarts when the CPU utilization exceeds 40%.

In most cases, the processing capacity of a single core is about 24 MB/s in simple mode and 12 MB/s in full regex mode.

|"cpu\_usage\_limit" : 0.4|
    |mem\_usage\_limit|int|The memory usage threshold for Logtail.     -   Valid values: 128 MB to the available memory size of the server
    -   Default value: 2048 MB
For example, if you set the parameter to 100, the memory usage is limited to 100 MB. Logtail restarts if the memory usage exceeds 100 MB.

If you need to collect more than 1,000 log files, you can increase the threshold value.

|"mem\_usage\_limit" : 100|
    |max\_bytes\_per\_sec|int|The maximum speed at which Logtail sends raw data per second.     -   Valid values: 1024 bytes/s to 52428800 bytes/s
    -   Default value: 20971520. Unit: bytes/s.
For example, if you set the parameter to 2097152, the speed is limited to 2 MB/s.

**Note:** If you set the parameter to a value greater than 20971520 bytes/s \(20 MB/s\), the speed is not limited.

|"max\_bytes\_per\_sec" : 2097152|
    |process\_thread\_count|int|The number of threads that Logtail uses to process data.     -   Valid values: 1 to 64
    -   Default value: 1
In most cases, each thread provides a write speed of 24 MB/s in simple mode and 12 MB/s in full regex mode. We recommend that you do not modify the default value.

|"process\_thread\_count" : 1|
    |send\_request\_concurrency|int|The number of concurrent requests that Logtail initiates to asynchronously send data.     -   Valid values: 1 to 1000
    -   Default value: 20
If the rate of write transactions per second \(TPS\) is high, you can set this parameter to a large value. Each concurrent request can provide 0.5 MB/s to 1 MB/s network throughput. The number of concurrent requests varies with the network latency.

**Note:** If the value of this parameter is high, concurrent requests may occupy excessive network ports. In this case, you need to adjust TCP parameters.

|"send\_request\_concurrency" : 4|
    |buffer\_file\_num|int|The maximum number of cached files.     -   Valid values: 1 to 100
    -   Default value: 25
If a network error occurs or the written data exceeds the specified threshold, Logtail caches parsed logs to local files in the installation directory. After the network is recovered, Logtail retries to send the cached logs.

|buffer\_file\_num" : 25|
    |buffer\_file\_size|int|The maximum number of a single cached file.     -   Valid values: 1048576 bytes to 104857600 bytes
    -   Default value: 20971520, Unit: bytes.
The maximum disk space that cached files can occupy is calculated by multiplying the value of the buffer\_file\_size parameter by the value of the buffer\_file\_num parameter.

|"buffer\_file\_size" : 20971520|
    |buffer\_file\_path|String|The directory in which cached files are stored. Default value: empty. The default value indicates that cached files are stored in the /usr/local/ilogtail directory where Logtail is installed. If you set the parameter, you must move the cached files named logtail\\\_buffer\\\_file\_ \* to the directory that is specified by the parameter. Then, Logtail can read, send, and delete cached files.

|"buffer\_file\_path" : ""|
    |bind\_interface|String|The name of the Network Interface Card \(NIC\) that is associated with the server. Default value: empty. If you retain the default value, an available NIC is automatically associated with the server. If you set the parameter to eth1, Logtail uses the NIC named eth1 to upload logs.

**Note:** This parameter is valid only for Logtail that runs on Linux.

|"bind\_interface" : ""|
    |check\_point\_filename|String|The path in which the checkpoint file of Logtail is stored. Default value: /tmp/logtail\_check\_point. We recommend that Docker users modify the path and mount the path to the host. Otherwise, when a Docker container is released, Logtail collects duplicate files due to the loss of checkpoint information. For example, you can set the check\_point\_filename parameter to `/data/logtail/check_point.dat` in the Docker container, and add `-v /data/docker1/logtail:/data/logtail` to the startup command. Then, the /data/docker1/logtail directory of the host is mounted to the /data/logtail directory of the Docker container.

|"check\_point\_filename" : /tmp/logtail\_check\_point|
    |user\_config\_file\_path|String|The path of the Logtail configuration file. By default, the Logtail configuration files are stored in the directory where the binary process is stored, and the file name is user\_log\_config.json. We recommend that Docker users modify the path and mount the path to the host. Otherwise, when a Docker container is released, Logtail collects duplicate files due to the loss of checkpoint information. For example, you can set the user\_config\_file\_path parameter to /data/logtail/user\_log\_config.json in the Docker container, and add `-v /data/docker1/logtail:/data/logtail` to the startup command. Then, the /data/docker1/logtail directory of the host is mounted to the /data/logtail directory of the Docker container.

|"user\_config\_file\_path" : user\_log\_config.json|
    |discard\_old\_data|Boolean|Specifies whether to drop historical logs. Default value: true. The default value indicates that the logs generated 12 hours before the current time are dropped.|"discard\_old\_data" : true|
    |working\_ip|String|The server IP address that Logtail reports to Log Service. Default value: empty. The default value indicates that Log Service automatically obtains the server IP address.|"working\_ip" : ""|
    |working\_hostname|String|The server hostname that Logtail reports to Log Service. Default value: empty. The default value indicates that Log Service automatically obtains the hostname of the server.|"working\_hostname" : ""|
    |max\_read\_buffer\_size|long|The maximum size of each log. Default value: 524288, Unit: bytes. If the size of a log exceeds 524288 bytes \(512 KB\), you can modify the parameter value.

|"max\_read\_buffer\_size" : 524288|
    |oas\_connect\_timeout|long|The connection timeout period after Logtail sends a request to obtain the Logtail configuration file or AccessKey pair. Default value: 5. Unit: seconds. If the network is instable, or the connection fails to be established, you can modify the value of this parameter.

|"oas\_connect\_timeout" : 5|
    |oas\_request\_timeout|long|The request timeout period after Logtail sends a request to obtain the Logtail configuration file or AccessKey pair. Default value: 10. Unit: seconds. If the network is instable, or the connection fails to be established, you can modify the value of this parameter.

|"oas\_request\_timeout" : 10|
    |data\_server\_port|long|Set the value to 443. After you set the data\_server\_port parameter to 443, Logtail transfers data to Log Service over the HTTPS protocol. This parameter is available only for Logtail 1.0.10 or later.

|"data\_server\_port": 443|
    |enable\_log\_time\_auto\_adjust|Boolean|After you set the enable\_log\_time\_auto\_adjust parameter to true, the log time changes to the local time of the server. For data security, Log Service checks the time information in requests, including the requests initiated by Logtail. Log Service rejects requests that are initiated 15 minutes earlier or later than the time at the Log Service side. The time when Logtail initiates a request is the local time of the server. In some test scenarios, the local time needs to be adjusted to a future time. If the local time of the server is modified, the request is rejected and Logtail fails to write data to Log Service. You can use this parameter to make the log time to be adaptive to the server local time.

This parameter is available only for Logtail 1.0.19 or later.

**Note:**

    -   If you enable this feature, the offset between the time at the Log Service side and the local time of the server will be added to the log time. The offset is updated only when a request is rejected by Log Service. Therefore, the log time queried by Log Service may be inconsistent with the time when the log is written.
    -   Some logic of Logtail depends on the increment of the system time. We recommend that you restart Logtail after the local time of the server is modified.
|"enable\_log\_time\_auto\_adjust": true|
    |accept\_multi\_config|Boolean|Specifies whether to allow multiple Logtail clients to collect the same file. Default value: false. The default value indicates that the same file cannot be collected by multiple Logtail clients. By default, each file can be collected by only one Logtail client. You can set this parameter to true to remove the limit. Each Logtail client has an independent collection process. If multiple Logtai clients are allowed to collect the same file, multiple times of CPU and memory are consumed.

This parameter is available only for Logtail 0.16.26 or later.

|"accept\_multi\_config": true|

3.  Restart Logtail to apply your settings.

    ```
    /etc/init.d/ilogtaild stop && /etc/init.d/ilogtaild start                        
    ```

    After the restart, you can execute the `/etc/init.d/ilogtaild status` command to check the status of Logtail.


## Appendix: environment variables

The following table describes the relationships between environment variables and Logtail startup parameters. For information about parameter descriptions, see [Table 1](#table_r2i_oh4_os6).

|Parameter|Environment variable|Priority|Supported versions|
|---------|--------------------|--------|------------------|
|cpu\_usage\_limit|cpu\_usage\_limit|If you modify Logtail startup parameters by modifying environment variables and configuration files, the environment variables prevail.|Logtail 0.16.32 or later|
|mem\_usage\_limit|mem\_usage\_limit|
|max\_bytes\_per\_sec|max\_bytes\_per\_sec|
|process\_thread\_count|process\_thread\_count|
|send\_request\_concurrency|send\_request\_concurrency|
|check\_point\_filename|ALIYUN\_LOGTAIL\_CHECK\_POINT\_PATH|If you modify Logtail startup parameters by modifying environment variables and configuration files, the environment variables prevail.|Logtail 0.16.36 or later|
|user\_config\_file\_path|user\_config\_file\_path|If you modify Logtail startup parameters by modifying environment variables and configuration files, the configuration files prevail.|Logtail 0.16.56 or later|
|discard\_old\_data|discard\_old\_data|
|working\_ip|ALIYUN\_LOGTAIL\_WORKING\_IP|
|working\_hostname|ALIYUN\_LOGTAIL\_WORKING\_HOSTNAME|
|max\_read\_buffer\_size|max\_read\_buffer\_size|
|oas\_connect\_timeout|oas\_connect\_timeout|
|oas\_request\_timeout|oas\_request\_timeout|
|data\_server\_port|data\_server\_port|
|accept\_multi\_config|accept\_multi\_config|
|enable\_log\_time\_auto\_adjust|enable\_log\_time\_auto\_adjust|If you modify Logtail startup parameters by modifying environment variables and configuration files, the configuration files prevail.|Logtail 1.0.19 or later|

