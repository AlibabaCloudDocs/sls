# Use the syslog protocol to upload logs

You can use the rsyslog and syslog-ng utilities to collect logs. Then, you can use the syslog protocol to upload the logs to Log Service. This topic describes how to upload logs to Log Service by using the syslog protocol.

## Limits

-   Syslog logs must conform to the [RFC 5424](https://tools.ietf.org/html/rfc5424) protocol.
-   The maximum size of each log entry is 64 KB.
-   Transport Layer Security \(TLS\) 1.2 must be used to ensure the security of data transfer.

## Configurations

If you upload logs by using the syslog protocol, you must specify the address to which the logs are uploaded. The address is in the `project name.Log Service endpoint:syslog protocol port` format, for example, test-project-1.cn-hangzhou-intranet.log.aliyuncs.com:10009. Specify an endpoint based on the region where your Log Service project resides. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md). The syslog port is 10009. You must also specify a Log Service project, a Logstore, and an AccessKey pair in the STRUCTURED-DATA field. The following table describes the parameters.

|Parameter|Description|Example|
|---------|-----------|-------|
|STRUCTURED-DATA|The value is set to Logservice.|Logservice|
|Project|The name of a project. The project must be available in Log Service before you collect logs.|test-project-1|
|Logstore|The name of a Logstore. The Logstore must be available in Log Service before you collect logs.|test-logstore-1|
|access-key-id|The AccessKey ID. We recommend that you use the AccessKey ID of a Resource Access Management \(RAM\) user. For more information, see [Create a RAM user and authorize the RAM user to access Log Service](/intl.en-US/Developer Guide/Access control RAM/Create a RAM user and authorize the RAM user to access Log Service.md).|<yourAccessKeyId\>|
|access-key-secret|The AccessKey secret. We recommend that you use the AccessKey secret of a RAM user. For more information, see [Create a RAM user and authorize the RAM user to access Log Service](/intl.en-US/Developer Guide/Access control RAM/Create a RAM user and authorize the RAM user to access Log Service.md).|<yourAccessKeySecret\>|

## Example 1: Use the rsyslog utility to upload syslog logs to Log Service

By default, the rsyslog utility is installed on Linux servers. You can use the rsyslog utility to collect system logs. Then, you can use the syslog protocol to upload the logs to Log Service. Different versions of rsyslog have different configuration files. You can run the man rsyslogd command to view the rsyslog version.

**Note:** The rsyslog utility must include the gnutls module. If the utility does not include the module, you can run the sudo apt-get install rsyslog-gnutls or sudo yum install rsyslog-gnutls command to install the module.

1.  Open the rsyslog configuration file.

    The default path of the rsyslog configuration file is /etc/rsyslog.conf.

2.  Configure the following settings based on your rsyslog version and add the configurations to the end of your rsyslog configuration file:

    -   Rsyslog v8 or later

        Set `$DefaultNetstreamDriverCAFile` to the path of the root certificate in the system.

        ```
        # Setup disk assisted queues 
        $WorkDirectory /var/spool/rsyslog # where to place spool files 
        $ActionQueueFileName fwdRule1     # unique name prefix for spool files 
        $ActionQueueMaxDiskSpace 1g       # 1gb space limit (use as much as possible) 
        $ActionQueueSaveOnShutdown on     # save messages to disk on shutdown 
        $ActionQueueType LinkedList       # run asynchronously 
        $ActionResumeRetryCount -1        # infinite retries if host is down 
        $ActionSendTCPRebindInterval 100  # close and re-open the connection to the remote host every 100 of messages sent. 
        #RsyslogGnuTLS set to default ca path 
        $DefaultNetstreamDriverCAFile /etc/ssl/certs/ca-bundle.crt 
        template(name="LogServiceFormat" type="string" 
        string="<%pri%>1 %timestamp:::date-rfc3339% %HOSTNAME% %app-name% %procid% %msgid% [logservice project=\"test-project-1\" logstore=\"test-logstore-1\" access-key-id=\"<yourAccessKeyId>\" access-key-secret=\"<yourAccessKeySecret>\"] %msg%\n" 
        ) 
        # Send messages to Loggly over TCP using the template. 
        action(type="omfwd" protocol="tcp" target="test-project-1.cn-hangzhou.log.aliyuncs.com" port="10009" template="LogServiceFormat" StreamDriver="gtls" StreamDriverMode="1" StreamDriverAuthMode="x509/name" StreamDriverPermittedPeers="*.cn-hangzhou.log.aliyuncs.com")
        ```

    -   Rsyslog v7 or earlier

        Set `$DefaultNetstreamDriverCAFile` to the path of the root certificate in the system.

        ```
        # Setup disk assisted queues 
        $WorkDirectory /var/spool/rsyslog       # where to place spool files 
        $ActionQueueFileName fwdRule1           # unique name prefix for spool files $ActionQueueMaxDiskSpace 1g             # 1gb space limit (use as much as possible) $ActionQueueSaveOnShutdown on           # save messages to disk on shutdown 
        $ActionQueueType LinkedList             # run asynchronously 
        $ActionResumeRetryCount -1              # infinite retries if host is down $ActionSendTCPRebindInterval 100        # close and re-open the connection to the remote host every 100 of messages sent. 
        # RsyslogGnuTLS set to default ca path 
        $DefaultNetstreamDriverCAFile /etc/ssl/certs/ca-bundle.crt 
        $ActionSendStreamDriver gtls 
        $ActionSendStreamDriverMode 1 
        $ActionSendStreamDriverAuthMode x509/name 
        $ActionSendStreamDriverPermittedPeer test-project-1.cn-hangzhou.log.aliyuncs.com 
        template(name="LogServiceFormat" type="string" string="<%pri%>1 %timestamp:::date-rfc3339% %HOSTNAME% %app-name% %procid% %msgid% [logservice project=\"test-project-1\" logstore=\"test-logstore-1\" access-key-id=\"<yourAccessKeyId>\" access-key-secret=\"<yourAccessKeySecret>\"] %msg%\n") 
        *.* action(type="omfwd" protocol="tcp" target="test-project-1.cn-hangzhou.log.aliyuncs.com" port="10009" template="LogServiceFormat")
        ```

3.  Restart the rsyslog utility.

    Run the sudo service rsyslog restart, sudo /etc/init.d/syslog-ng restart, or systemctl restart rsyslog command to restart the rsyslog utility.

4.  Run the logger command to generate test logs.

    For example, run the logger hello world! command to generate logs.


## Example 2: Use the syslog-ng utility to upload syslog logs to Log Service

Syslog-ng is an open source utility that runs on UNIX and UNIX-like systems. This utility is based on the syslog protocol. You can run the sudo yum install syslog-ng or sudo apt-get install syslog-ng command to install the syslog-ng utility.

**Note:** By default, the rsyslog utility is installed on Linux servers. This utility is incompatible with the syslog-ng utility. If you want to use the syslog-ng utility, you must first uninstall the rsyslog utility.

1.  Open the syslog-ng configuration file.

    The default path of the syslog-ng configuration file is /etc/syslog-ng/syslog-ng.conf.

2.  Configure the following settings and add the configurations to the end of your syslog-ng configuration file:

    ```
    ### Syslog-ng Logging Config for LogService ### 
    template LogServiceFormat { 
        template("<${PRI}>1 ${ISODATE} ${HOST:--} ${PROGRAM:--} ${PID:--} ${MSGID:--} [logservice project=\"test-project-1\" logstore=\"test-logstore-1\" access-key-id=\"<yourAccessKeyId>\" access-key-secret=\"<yourAccessKeySecret>\"] $MSG\n"); template_escape(no); 
    }; 
    destination d_logservice{ 
         tcp("test-project-1.cn-hangzhou.log.aliyuncs.com" port(10009) 
         tls(peer-verify(required-untrusted)) 
         template(LogServiceFormat)); 
    }; 
    log { 
         source(s_sys); # default use s_sys 
         destination(d_logservice); 
    }; 
    ### END Syslog-ng Logging Config for LogService ###
    ```

3.  Restart the syslog-ng utility.

    Run the sudo /etc/init.d/syslog-ng restart, sudo service syslog-ng restart, or sudo systemctl restart syslog-ng command to restart the syslog-ng utility.

4.  Run the logger command to generate test logs.

    For example, run the logger hello world! command to generate logs.


## Sample log entries

After you upload logs to Log Service, you can view the logs in the Log Service console. For more information about log fields, see [RFC 5424 protocol](https://tools.ietf.org/html/rfc5424).

**Note:** By default, Log Service deletes the STRUCTURED-DATA field to ensure the confidentiality of your AccessKey pair.

![Sample log entries](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5407549951/p42014.png)

|Log field|Description|
|---------|-----------|
|\_\_source\_\_|The hostname in the raw log entry.|
|\_\_topic\_\_|The value is syslog-forwarder.|
|\_\_facility\_\_|The facility information, such as the information of the device and module.|
|\_\_program\_\_|The name of the process.|
|\_\_serverity\_\_|The severity level of the syslog log entry.|
|\_\_priority\_\_|The priority of the syslog log entry.|
|\_\_unixtimestamp\_\_|The UNIX timestamp of the raw log entry. Unit: nanoseconds.|
|content|The msg field in the raw log entry.|

## FAQ

-   How do I simulate log uploading?

    You can use [Netcat](http://netcat.sourceforge.net/) to simulate log uploading. This way, you can check whether the network connection is normal and whether the AccessKey pair is authorized to send syslog logs.

    1.  Log on to the server on which you want to simulate log uploading.
    2.  Run the following command to install Netcat:

        ```
        sudo yum install nmap-ncat
        ```

    3.  Run the following command to connect to Log Service.

        ```
        ncat --ssl <yourProject>.<yourEndpoint> 10009
        ```

        Example:

        ```
        ncat --ssl test-project-1.cn-hangzhou.log.aliyuncs.com 10009
        ```

    4.  Netcat does not check whether network connections are interrupted. After you run a ncat command, you must enter the information that you want to send and press the Enter key within 30 seconds.

        ```
        <34>1 2019-03-28T03:00:15.003Z mymachine.example.com su - ID47 [logservice project="<yourProject>" logstore="<yourLogstore>" access-key-id="<yourAccessKeyID>" access-key-secret="<yourAccessKeySecret>"] this is a test message
        ```

        Example:

        ```
        <34>1 2019-03-28T03:00:15.003Z mymachine.example.com su - ID47 [logservice project="trace-doc-test" logstore="doc-test-001-logs" access-key-id="LTAI4***" access-key-secret="HfJEw***"] this is a test message
        ```

    5.  After you send the syslog log entry, you can preview the log entry in the Log Service console.

        For more information, see [Preview logs](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Consume log data.md).

        ![ncat_output](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3547612261/p272360.png)

-   What can I do if logs fail to be uploaded?

    Troubleshoot the failure based on the error message. For more information, see [Diagnose collection errors]().

-   How do I view rsyslog error logs?

    You can run the vim command to view rsyslog error logs. By default, rsyslog error logs are stored in the /var/log/message directory.

    -   Error message 1

        ```
        dlopen: /usr/lib64/rsyslog/lmnsd_gtls.so: cannot open shared object file: No such file or directory
        ```

        This error message is returned because the rsyslog-gnutls module is not installed. You can run the sudo apt-get install rsyslog-gnutls or sudo yum install rsyslog-gnutls command to install the module. After the module is installed, restart the rsyslog utility.

    -   Error message 2

        ```
        unexpected GnuTLS error -53 - this could be caused by a broken connection. GnuTLS reports:Error in the push function
        ```

        This error message is returned because the TCP connection is closed due to a long period of inactivity. You can ignore this error because rsyslog will re-establish the connection.

-   How do I view syslog-ng error logs?

    You can run the systemctl status syslog-ng.service or journalctl-xe command to view syslog-ng error logs. By default, syslog-ng error logs are stored in journal logs.

    If the following error message is returned, check whether the configuration file format is valid or whether configuration conflicts exist. For example, you cannot configure multiple `internal()` sources.

    ```
    Job for syslog-ng.service failed because the control process exited with error code. See "systemctl status syslog-ng.service" and "journalctl -xe" for details
    ```


