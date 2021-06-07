# How do I collect logs from a server in a corporate intranet?

This topic uses an NGINX server as an example to describe how to collect logs from a server in a corporate intranet to Log Service.

A project and a Logstore are created. For more information, see [Create a project](/intl.en-US/Data Collection/Preparation/Manage a project.md) and [Create a Logstore](/intl.en-US/Data Collection/Preparation/Manage a Logstore.md).

Assume that you have deployed multiple servers in your corporate intranet and these servers do not have access to the Internet. If you need to collect logs from the servers and send them to Log Service for query and analysis, you can authorize one of the servers to access the Internet. Then, you can configure this server as a gateway server, and collect logs from the other servers to Log Service by using the gateway server.

You can configure a reverse proxy server as a gateway server. NGINX is an open source, high-performance HTTP server and reverse proxy server. For more information, visit [the NGINX official website](https://www.nginx.com/resources/wiki/).

## Step 1: Configure a gateway server

To use NGINX to configure a server that has access to the Internet in a corporate intranet as a gateway server, perform the following steps:

1.  Log on to the server that you want to configure as the gateway server.

2.  Install NGINX.

    For more information, see [Install NGINX](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/).

3.  Add the following code to the nginx.conf file:

    ```
    upstream logtail{
      server ${project}.${region-endpoint}:80;
      check interval=1000 rise=2 fall=1 timeout=1000;
    }
    
    upstream logtail_443{
      server ${project}.${region-endpoint}:443;
      check interval=1000 rise=2 fall=1 timeout=1000;
    }
    
    server {
      listen 80;
      server_name ${domain};
      access_log /var/log/nginx/${domain}_access.log main;
      error_log /var/log/nginx/${domain}_error.log debug;
      charset utf-8;
    
    
      location / {
    
        proxy_set_header  Host $host;
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_pass http://logtail;
        break;
      }
    }
    
    server {
      listen 443;
      server_name ${domain};
      access_log /var/log/nginx/${domain}_access.log main;
      error_log /var/log/nginx/${domain}_error.log debug;
      charset utf-8;
    
    
      location / {
    
        proxy_set_header  Host $host;
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_pass https://logtail_443;
        break;
      }
    }
    ```

    |Variable|Description|Example|
    |--------|-----------|-------|
    |$\{project\}|The name of the project in Log Service.|test-project|
    |$\{region-endpoint\}|The endpoint that is used to access the project in Log Service. You can access the project by using the Internet, the classic network, or a virtual private cloud \(VPC\). For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).|cn-hangzhou.log.aliyuncs.com|
    |$\{domain\}|The domain name of the gateway server. You can specify a custom domain name.|logtail.com|


## Step 2: Associate the gateway server with other servers in the corporate intranet

To associate the gateway server with the other servers in the corporate intranet, perform the following steps:

1.  Log on to a server in the corporate intranet.

2.  Install Logtail on the server.

    -   To install Logtail on a Linux server, see [Install Logtail in Linux](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md).
    -   To install Logtail on a Windows server, see [Install Logtail in Windows](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Windows.md).
3.  Add the IP address and domain name of the gateway server to the hosts file.

    ```
    $\{ip\} $\{domain\}
    ```

    |Variable|Description|Example|
    |--------|-----------|-------|
    |$\{ip\}|The IP address of the gateway server.|192.168.XX.XX|
    |$\{domain\}|The domain name of the gateway server. You can specify a custom domain name.|logtail.com|

4.  In the ilogtail\_config.json file, replace the config\_server\_address variable with the domain name of the gateway server.

    ```
    {
        "config_server_address" : "http://$\{domain\}",
        "data_server_list" :
        [
            {
                "cluster" : "cn-hangzhou",
                "endpoint" : "cn-hangzhou.log.aliyuncs.com"
            }
        ],
        "cpu_usage_limit" : 0.4,
        "mem_usage_limit" : 384,
        "max_bytes_per_sec" : 20971520,
        "bytes_per_sec" : 1048576,
        "buffer_file_num" : 25,
        "buffer_file_size" : 20971520,
        "buffer_map_num" : 5,
        "streamlog_open" : false,
        "streamlog_pool_size_in_mb" : 50,
        "streamlog_rcv_size_each_call" : 1024,
        "streamlog_formats":[],
        "streamlog_tcp_port" : 11111
    }
    ```

5.  Repeat [Step 1](#step_1y7_rjr_6cf) to [Step 4](#step_bns_ubd_ldu) for the other servers in the corporate intranet to associate them with the gateway server.


## FAQ

Why am I unable to collect logs when the gateway server and the other servers in my corporate intranet are configured and the heartbeat status of all servers is OK?

To fix this issue, [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm) to contact technical support.

