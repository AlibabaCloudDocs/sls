# Collect Apache logs

This topic describes how to configure the Apache mode in the Log Service console to collect logs.

## Log formats

Apache logs can be written in the combined or common format. You can also customize the log format.

-   Syntax of the combined log format:

    ```
    LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
    ```

-   Syntax of the common log format:

    ```
    LogFormat "%h %l %u %t \"%r\" %>s %b" 
    ```

-   Syntax of a custom log format:

    ```
    LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %D %f %k %p %q %R %T %I %O" customized
    ```


|Format field|Key name|Description|
|:-----------|:-------|:----------|
|%a|client\_addr|The IP address of the client.|
|%A|local\_addr|The local IP address.|
|%b|response\_size\_bytes|The size of the response. Unit: bytes. The response format is CLF. If no response is returned, the value of this parameter is -.|
|%B|response\_bytes|The size of the response. Unit: bytes. If no response is returned, the value of this parameter is 0.|
|%D|request\_time\_msec|The period of time for which the request is processed. Unit: millisecond.|
|%h|remote\_addr|The name of the remote host.|
|%H|request\_protocol\_supple|The protocol of the request.|
|%l|remote\_ident|The identity information that is provided by a remote host.|
|%m|request\_method\_supple|The method of the request.|
|%p|remote\_port|The port number of the server.|
|%P|child\_process|The ID of the child process.|
|%q|request\_query|The string that is used to query logs. If no query string exists, the value is an empty string.|
|“%r”|request|The content of the request. The content includes the method name, address, and HTTP protocol fields of the request.|
|%s|status|The HTTP status code of the response.|
|%\>s|status|The HTTP status code of the final response.|
|%f|filename|The name of the requested file.|
|%k|keep\_alive|The number of keep-alive requests.|
|%R|response\_handler|The type of the handler that generates the response on the server.|
|%t|time\_local|The local time when the server receives the request.|
|%T|request\_time\_sec|The time period for which the request is processed. Unit: seconds.|
|%u|remote\_user|The username that you used to log on to the client.|
|%U|request\_uri\_supple|The URI of the request. The URI does not include the string that is used to query logs.|
|%v|server\_name|The name of the server.|
|%V|server\_name\_canonical|The name of the server. The name is specified by using the UseCanonicalName directive.|
|%I|bytes\_received|The number of bytes that are received by the server. To use this key, you must enable the mod\_logio module.|
|%O|bytes\_sent|The number of bytes that are sent by the server. To use this key, you must enable the mod\_logio module.|
|“%\{User-Agent\}i”|http\_user\_agent|The information of the client.|
|“%\{Rererer\}i”|http\_referer|The URL of the web page. The URL is linked to the resource that is being requested.|

During Logtail configuration for Apache logs, you must specify the log format, log file path, and log file name. For example, the following command indicates that the logs are written in the combined format. The path of the log file is /var/log/apache2/access\_log.

```
CustomLog "/var/log/apache2/access_log" combined
```

An Apache log entry is as follows:

```
192.168.1.2 - - [02/Feb/2016:17:44:13 +0800] "GET /favicon.ico HTTP/1.1" 404 209 "http://localhost/x1.html" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/48.0.2564.97 Safari/537.36" 
```

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Import Data section, click **Apache - Text Log**.

3.  In the Specify Logstore step, select the target project and Logstore, and click **Next**.

    You can also click **Create Now** to create a project and a Logstore. For more information, see [Step 1: Create a project and a Logstore](/intl.en-US/Quick Start/Quick start.md).

4.  In the Create Machine Group step, create a machine group.

    -   If a machine group is available, click **Using Existing Machine Groups**.
    -   This section uses ECS instances as an example to describe how to create a machine group. To create a machine group, perform the following steps:
        1.  Install Logtail on ECS instances. For more information, see [Install Logtail on ECS instances](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail on ECS instances.md).

            If Logtail is installed on the ECS instances, click **Complete Installation**.

            **Note:** If you need to collect logs from user-created clusters or servers of third-party cloud service providers, you must install Logtail on these servers. For more information, see [Install Logtail in Linux](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md) or [Install Logtail in Windows](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Windows.md).

        2.  After the installation is complete, click **Complete Installation**.
        3.  On the page that appears, specify the parameters for the machine group. For more information, see [Create an IP address-based machine group](/intl.en-US/Data Collection/Logtail collection/Machine Group/Create an IP address-based machine group.md) or [Create a custom ID-based machine group](/intl.en-US/Data Collection/Logtail collection/Machine Group/Create a custom ID-based machine group.md).
5.  In the Machine Group Settings step, apply the configurations to the machine group.

    Select the created machine group and move the group from **Source Server Groups** to **Applied Server Groups**.

6.  In the Logtail Config step, create a Logtail configuration.

    |Parameter|Description|
    |---------|-----------|
    |Config Name|The name of the Logtail configuration. The name cannot be modified after the Logtail configuration is created. You can also click **Import Other Configuration** to import Logtail configurations from other projects. |
    |Log Path|The directory and name of the log file. The file names can be complete names or names that contain wildcards. For more information, visit [Wildcard matching](http://man7.org/linux/man-pages/man7/glob.7.html). The log files in all levels of subdirectories under a specified directory are monitored if the log files match the specified pattern. Examples:

    -   /apsara/nuwa/ … /\*.log indicates that the files whose extension is .log in the /apsara/nuwa directory and its subdirectories are monitored.
    -   /var/logs/app\_\* … /\*.log\* indicates that each file that meets the following conditions is monitored: The file name contains .log. The file is stored in a subdirectory \(at all levels\) of the /var/logs directory. The name of the subdirectory matches the app\_\* pattern.
**Note:**

    -   Each log file can be collected by using only one Logtail configuration file.
    -   You can include only asterisks \(\*\) and question marks \(?\) as wildcard characters in the log path. |
    |Blacklist|If this switch is turned on, you can configure a blacklist in the **Add Blacklist** section. You can configure a blacklist to skip the specified directories or files during log collection. The names of the specified directories and files support exact match and wildcard match. Examples:     -   If you select **Filter by Directory** from the Filter Type drop-down list and enter /tmp/mydir in the Content column, all files in the directory are skipped.
    -   If you select **Filter by File** from the Filter Type drop-down list and enter /tmp/mydir/file in the Content column, only the specified file is skipped. |
    |Docker File|If the file in the Docker container is a log file, you can directly specify the log path and container tags. Logtail automatically monitors the creation and destruction of containers, and collects log entries of the specified containers based on the specified tags. For more information about container text logs, see [Use the console to collect Kubernetes text logs in the DaemonSet mode](/intl.en-US/Data Collection/Logtail collection/Container log collection/Use the console to collect Kubernetes text logs in the DaemonSet mode.md).|
    |Mode|The default value is **Apache Configuration Mode**. You can select other modes. For more information about how to configure other modes, see [Overview](/intl.en-US/Data Collection/Logtail collection/Text logs/Overview.md).|
    |Log format|Select a log format based on the format that are configured in your Apache log configuration file. To facilitate the query and analysis of log data, we recommend that you select **Custom**.|
    |APACHE Logformat Configuration|Enter the log configuration section that is specified in the Apache configuration file. The section starts with the LogFormat field. If you set **Log Format** to **common** or **combined**, the syntax of the corresponding log format is automatically generated. You must check whether the format of the generated logs is the same as the log format that is specified in the Apache configuration file. |
    |APACHE Key Name|Log Service reads the Apache key names from the content in the **APACHE Logformat Configuration** section.|
    |Drop Failed to Parse Logs|    -   Specifies whether to drop failed-to-parse logs. If you enable the **Drop Failed to Parse Logs** feature, logs that fail to be parsed are not uploaded to Log Service.
    -   If you disable the **Drop Failed to Parse Logs** feature, raw logs are uploaded to Log Service when the raw logs fail to be parsed. |
    |Maximum Directory Monitoring Depth|The maximum number of directory layers that can be recursively monitored during log collection. Valid values: 0 to 1000. The value 0 indicates that only the directory specified in the log path is monitored.|

    You can configure advanced options based on your business requirements. We recommend that you do not modify the settings. The following table describes the parameters in the advanced options.

    |Parameter|Description|
    |:--------|:----------|
    |Enable Plug-in Processing|Specifies whether to enable the plug-in processing feature. If you enable this feature, plug-ins are used to process logs. For more information, see [Process data](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to collect data/Process data.md).|
    |Upload Raw Log|Specifies whether to upload raw logs. If you enable this feature, raw logs are written to the \_\_raw\_\_ field and uploaded together with the parsed logs.|
    |Topic Generation Mode|    -   **Null - Do not generate topic**: This mode is selected by default. In this mode, the topic field is set to an empty string. You can query logs without the need to enter a topic.
    -   **Machine Group Topic Attributes**: This mode is used to differentiate logs that are generated by different servers.
    -   **File Path Regex**: In this mode, you must configure a regular expression in the **Custom RegEx** field. The part of a log path that matches the regular expression is used as the topic name. This mode is used to differentiate logs that are generated by different users or instances. |
    |Log File Encoding|    -   utf8: indicates that UTF-8 encoding is used.
    -   gbk: indicates that GBK encoding is used. |
    |Timezone|The time zone where logs are collected. Valid values:     -   System Timezone: This option is selected by default. It indicates that the time zone where logs are collected is the same as the time zone to which the server belongs.
    -   Custom: Select a time zone. |
    |Timeout|The timeout period of log files. If a log file is not updated within the specified period, Logtail considers the file to be timed out. Valid values:     -   Never: All log files are continuously monitored and never time out.
    -   30 Minute Timeout: If a log file is not updated within 30 minutes, Logtail considers the file to be timed out and no longer monitors the file.

If you select **30 Minute Timeout**, you must specify the **Maximum Timeout Directory Depth** parameter. Valid values: 1 to 3. |
    |Filter Configuration|The filter conditions that are used to collect logs. Only logs that match the specified filter conditions are collected. Examples:     -   Collect logs that meet a condition: Specify the filter condition to Key:level Regex:WARNING\|ERROR if you need to collect only logs of only the WARNING or ERROR severity level.
    -   [Filter out logs that do not meet a condition](http://www.regular-expressions.info/lookaround.html):
        -   Specify the filter condition to Key:level Regex:^\(?!. \*\(INFO\|DEBUG\)\). \* if you need to filter out logs of the INFO or DEBUG severity level.
        -   Specify the filter condition to Key:url Regex:. \*^\(?!.\*\(healthcheck\)\). \* if you need to filter out logs whose URL contains the keyword healthcheck. For example, logs in which the value of the url key is /inner/healthcheck/jiankong.html are not collected.
For more examples, visit [regex-exclude-word](https://stackoverflow.com/questions/2404010/match-everything-except-for-specified-strings) and [regex-exclude-pattern](https://stackoverflow.com/questions/2078915/a-regular-expression-to-exclude-a-word-string). |

7.  In the **Configure Query and Analysis** step, configure the indexes.

    Indexes are configured by default. You can re-configure the indexes based on your business requirements. For more information, see [Enable and configure the index feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).

    **Note:**

    -   You must configure Full Text Index or Field Search. If you configure both of them, the settings of Field Search are applied.
    -   If the data type of index is long or double, the Case Sensitive and Delimiter settings are unavailable.

After you complete the settings, Log Service starts to collect logs.

