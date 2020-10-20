# 使用Syslog协议上传日志

您可以使用Rsyslog、Syslog-ng采集日志并通过Syslog协议上传到日志服务。本文介绍通过Syslog协议将日志上传到日志服务的操作步骤。

## 相关限制

-   Syslog协议必须为标准的[RFC5424](https://tools.ietf.org/html/rfc5424)协议。
-   每条日志最大支持64KB。
-   为保证数据传输安全性，数据传输必须使用基于TCP的TLS1.2（Transport-level security）。

## 配置方式

使用Syslog协议上传日志时，需配置日志上传地址，格式为`Project名称.日志服务Endpoint:Syslog协议端口`，例如test-project-1.cn-hangzhou-intranet.log.aliyuncs.com:10009，请根据您的日志服务Project所在地域选择Endpoint，详情请参见[服务入口](/cn.zh-CN/开发指南/API 参考/服务入口.md)，Syslog的端口为10009。同时您需要在STRUCTURED-DATA字段中配置日志服务Project、Logstore，阿里云账号AccessKey等信息。

|参数|说明|示例|
|--|--|--|
|STRUCTURED-DATA|固定为Logservice。|Logservice|
|Project|日志服务Project名称，请提前在日志服务中创建Project。|test-project-1|
|Logstore|日志服务Logstore名称，请提前在日志服务中创建Logstore。|test-logstore-1|
|access-key-id|AccessKey ID。建议使用子账号AK，详情请参见[授权](/cn.zh-CN/开发指南/访问控制RAM/创建RAM用户及授权.md)。|<yourAccessKeyId\>|
|access-key-secret|AccessKey Secret。建议使用子账号AK，详情请参见[授权](/cn.zh-CN/开发指南/访问控制RAM/创建RAM用户及授权.md)。|<yourAccessKeySecret\>|

## 示例1：使用Rsyslog采集日志

Linux服务器默认自带Rsyslog。您可以使用Rsyslog采集系统日志，然后通过syslog协议上传到日志服务。不同版本的Rsyslog的配置文件略有不同，您可执行man rsyslogd命令查看Rsyslog版本。

**说明：** 请确保Rsyslog已安装了gnutls模块。如果未安装，请执行sudo apt-get install rsyslog-gnutls或sudo yum install rsyslog-gnutls命令进行安装。

1.  打开Rsyslog配置文件。

    通常Rsyslog配置文件路径为/etc/rsyslog.conf。

2.  根据实际情况，配置如下信息，并添加到Rsyslog配置文件的末尾。

    -   Rsyslog V8及以上

        其中`$DefaultNetstreamDriverCAFile`配置为系统根证书所在路径。

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

    -   Rsyslog V7及以下

        其中`$DefaultNetstreamDriverCAFile`配置为系统根证书所在路径。

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

3.  重启Rsyslog。

    执行sudo service rsyslog restart、sudo /etc/init.d/syslog-ng restart或systemctl restart rsyslog命令重启Rsyslog。

4.  使用logger命令生成测试日志。

    例如执行logger hello world!命令生成日志。


## 示例2：使用Syslog-ng采集日志

syslog-ng是基于syslog协议的Unix和类Unix系统的开源软件。您可以执行sudo yum install syslog-ng或sudo apt-get install syslog-ng命令安装Syslog-ng。

**说明：** Linux服务器上默认安装Rsyslog，但是Rsyslog和Syslog-ng无法同时工作，如果您要使用Syslog-ng请先卸载Rsyslog。

1.  打开Syslog-ng配置文件。

    通常Syslog-ng配置文件地址为/etc/syslog-ng/syslog-ng.conf。

2.  根据实际情况，配置如下信息，并添加到Syslog-ng配置文件的末尾。

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
         source(s_src); # default use s_src 
         destination(d_logservice); 
    }; 
    ### END Syslog-ng Logging Config for LogService ###
    ```

3.  重启Syslog-ng。

    执行sudo /etc/init.d/syslog-ng restart、sudo service syslog-ng restart或sudo systemctl restart syslog-ng命令重启Syslog-ng。

4.  使用logger命令生成测试日志。

    例如执行logger hello world!命令生成日志。


## 日志样例

上传日志到日志服务后，您可以在日志服务控制台查看日志。日志字段详情请参见[RFC5424协议](https://tools.ietf.org/html/rfc5424)。

**说明：** 为避免泄露AccessKey信息，日志服务默认将上报的Logservice字段删除。

![日志样例](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4140559951/p42014.png)

|字段名|说明|
|---|--|
|\_\_source\_\_|原始日志中的hostname字段。|
|\_\_topic\_\_|固定为syslog-forwarder。|
|\_\_facility\_\_|facility（设备、模块）信息。|
|\_\_program\_\_|进程名。|
|\_\_serverity\_\_|日志严重性。|
|\_\_priority\_\_|日志优先级。|
|\_\_unixtimestamp\_\_|原始日志中的时间戳（单位：纳秒）。|
|content|原始日志中的msg字段。|

## 常见问题与排查

-   手动上传日志

    您可使用ncat命令模拟上传日志，以此检查网络连通性以及AccessKey是否具备上报权限。如果您的服务器上没有安装ncat，可执行sudo yum install nmap-ncat命令安装。

    **说明：**

    -   上传时间为0时区时间，例如2019-03-28T03:00:15.003Z的东八区时间为：2019-03-28T11:00:15.003。
    -   ncat命令不会自动判断网络连接中断，请在执行ncat命令30秒内输入待发送的信息并按回车键。
    示例：向日志服务发送一条日志，其中日志服务Project名为test-project-1，Logstore名为test-logstore-1，Project所在地域为cn-hangzhou，具有写入权限的子账号AccessKey ID为<yourAccessKeyId\>、AccessKey Secret为<yourAccessKeySecret\>。

    ```
    [root@iZbp145dd9fccuidd7g**** ~]# ncat --ssl test-project-1.cn-hangzhou.log.aliyuncs.com 10009 
    <34>1 2019-03-28T03:00:15.003Z mymachine.example.com su - ID47 [logservice project="test-project-1" logstore="test-logstore-1" access-key-id="<yourAccessKeyId>" access-key-secret="<yourAccessKeySecret>"] this is a test message
    ```

    上传成功后，可通过日志服务控制台预览日志，详情请参考[日志预览](/cn.zh-CN/消费与投递/实时消费/普通消费.md)。

-   诊断采集错误

    如果手动上传日志失败，您可通过诊断采集错误查看具体报错信息，详情请参见[诊断采集错误]()。

-   查看Rsyslog报错日志

    Rsyslog日志默认保存在/var/log/message中，您可通过vim命令查看。

    -   报错信息（1）

        ```
        dlopen: /usr/lib64/rsyslog/lmnsd_gtls.so: cannot open shared object file: No such file or directory
        ```

        该错误是因为没有安装gnutls模块，请执行sudo apt-get install rsyslog-gnutls或sudo yum install rsyslog-gnutls安装gnutls并重启Rsyslog。

    -   报错信息（2）

        ```
        unexpected GnuTLS error -53 - this could be caused by a broken connection. GnuTLS reports:Error in the push function
        ```

        该错误是因为TCP连接长期闲置被强制关闭，Rsyslog会进行自动重连，您无需关注。

-   查看Syslog-ng报错日志

    Syslog-ng日志默认保存在Journal日志中，您可执行systemctl status syslog-ng.service和journalctl -xe命令查看日志。

    如果出现如下报错，请检查配置文件格式是否合法或配置是否存在冲突，例如不支持配置多个`internal()`。

    ```
    Job for syslog-ng.service failed because the control process exited with error code. See "systemctl status syslog-ng.service" and "journalctl -xe" for details
    ```


