# Collect logs in IIS mode

Log Service allows you to collect Internet Information Services \(IIS\) logs and perform multidimensional log analysis. This topic describes how to configure Logtail in IIS mode in the Log Service console to collect logs.

-   A project and a Logstore are created. For more information, see [Create a project](/intl.en-US/Data Collection/Preparation/Manage a project.md) and [Create a Logstore](/intl.en-US/Data Collection/Preparation/Manage a Logstore.md).
-   Ports 80 and 443 of the server from which you want to collect logs are enabled.
-   Logs are generated on the server in the IIS, NCSA Common, or W3C Extended format.

    We recommend that you use the W3C Extended log format. If you select the W3C Extended format, you must configure the fields in the W3C Logging Fields dialog box. To do so, you must select **Bytes Sent \(sc-bytes\)** and **Bytes Received \(cs-bytes\)** and use the default settings for other fields.

    ![Logs in the W3C Extended format](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3077050161/p60704.png)


## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Import Data section, select **IIS - Text Log**.

3.  Select a destination project and Logstore, and then click **Next**.

    You can also click **Create Now** to create a project and a Logstore.

4.  Create a machine group.

    -   If a machine group is available, click **Using Existing Machine Groups**.
    -   If no machine groups are available, perform the following steps to create a machine group. An ECS instance is used as an example.
        1.  On the ECS Instances tab, select the destination ECS instance and click **Execute Now**.

            For more information, see [Install Logtail on ECS instances](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail on ECS instances.md).

            **Note:** If you need to collect logs from self-managed clusters or servers of third-party cloud service providers, you must install Logtail. For more information, see [Install Logtail in Linux](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md) or [Install Logtail in Windows](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Windows.md).

        2.  After the installation, click **Complete Installation**.
        3.  Create a machine group and click **Next**.

            Log Service allows you to create IP address-based machine groups and custom ID-based machine groups. For more information, see [Create an IP address-based machine group](/intl.en-US/Data Collection/Logtail collection/Machine Group/Create an IP address-based machine group.md) and [Create a custom ID-based machine group](/intl.en-US/Data Collection/Logtail collection/Machine Group/Create a custom ID-based machine group.md).

5.  Select and move the destination machine group from **Source Server Groups** to **Applied Server Groups**, and then click **Next**.

    **Note:** If you want to apply a machine group immediately after it is created, the heartbeat status of the machine group may be **FAIL**. This is because the machine group has not been connected to Log Service. In this case, you can click **Automatic Retry**. If the problem persists, see [What can I do if the Logtail client has no heartbeat?]()

6.  In the Logtail Config step, create a Logtail configuration file.

    ![Collect logs in IIS mode](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0461131161/p186545.png)

    |Parameter|Description|
    |---------|-----------|
    |Config Name|The name of the Logtail configuration file. The name must be unique in a project and cannot be modified after the file is created.You can also click **Import Other Configuration** to import a Logtail configuration file from another project. |
    |Log Path|The directories and files from which logs are collected. The file names can be complete names or names that contain wildcards. For more information, see [Wildcard matching](http://man7.org/linux/man-pages/man7/glob.7.html). The log files in all levels of subdirectories under a specified directory are monitored if the log files match the specified pattern. Examples:

    -   /apsara/nuwa/… /\*.log indicates that the files whose extension is .log in the /apsara/nuwa directory and its subdirectories are monitored.
    -   /var/logs/app\_\*/\*.log indicates that each file that meets the following conditions is monitored: The file name contains .log. The file is stored in a subdirectory \(at all levels\) of the /var/logs directory. The name of the subdirectory matches the app\_\* pattern.
**Note:**

    -   By default, each log file can be collected by using only one Logtail configuration file.
    -   To use multiple Logtail configuration files to collect one log file, we recommend that you create a symbolic link that points to the directory where the file is located. For example, assume that you want to collect two copies of the log.log file from the /home/log/nginx/log/log.log directory. You can run the following command to create a symbolic link that points to the directory. When you configure the Logtail configuration files, use the original path in one Logtail configuration file and use the symbolic link in the other Logtail configuration file.

        ```
ln -s /home/log/nginx/log /home/log/nginx/link_log
        ```

    -   You can include only asterisks \(\*\) and question marks \(?\) as wildcard characters in the log path. |
    |Blacklist|If you turn on the **Blacklist** switch, you can configure a blacklist to skip the specified directories or files when you collect logs. You can use exact match or wildcard match to specify directories and files. Examples:     -   If you select **Filter by Directory** from the Filter Type drop-down list and enter /tmp/mydir in the Content column, all files in the directory are skipped.
    -   If you select **Filter by File** from the Filter Type drop-down list and enter /tmp/mydir/file in the Content column, only the specified file is skipped. |
    |Docker File|If you collect logs from Docker containers, you can turn on the **Docker File** switch and configure the paths and tags of the containers. Logtail monitors the containers when they are created and destroyed, filters the logs of the containers by tag, and collects the filtered logs. For more information, see [Use the console to collect Kubernetes text logs in DaemonSet mode](/intl.en-US/Data Collection/Logtail collection/Container log collection/Use the console to collect Kubernetes text logs in DaemonSet mode.md).|
    |Mode|The default value is **IIS Configuration Mode**. You can change the mode. For more information about how to configure other modes, see [Overview](/intl.en-US/Data Collection/Logtail collection/Text logs/Overview.md).|
    |Log Format|Select the log format of the logs that are generated on the IIS server.     -   IIS: Microsoft IIS log file format
    -   NCSA: NCSA Common log file format
    -   W3C: W3C Extended log file format |
    |IIS Configuration|Configure the IIS configuration fields.     -   In the Log Format field, if you select IIS or NCSA, the configuration fields are automatically generated.
    -   If you select W3C, enter the content that is specified in the logFile logExtFileFlags field of the IIS configuration file.

        ```
logExtFileFlags="Date, Time, ClientIP, UserName, SiteName, ComputerName, ServerIP, Method, UriStem, UriQuery, HttpStatus, Win32Status, BytesSent, BytesRecv, TimeTaken, ServerPort, UserAgent, Cookie, Referer, ProtocolVersion, Host, HttpSubStatus"
        ```

        -   Default path of the IIS5 configuration file: C:\\WINNT\\system32\\inetsrv\\MetaBase.bin
        -   Default path of the IIS6 configuration file: C:\\WINDOWS\\system32\\inetsrv\\MetaBase.xml
        -   Default path of the IIS7 configuration file: C:\\Windows\\System32\\inetsrv\\config\\applicationHost.config |
    |IIS Key Name|Log Service extracts IIS key names based on the **IIS Configuration** parameter.|
    |Drop Failed to Parse Logs|    -   If you select **Filter by Directory** from the Filter Type drop-down list and enter /tmp/mydir in the Content column, all files in the directory are skipped.
    -   If you select **Filter by File** from the Filter Type drop-down list and enter /tmp/mydir/file in the Content column, only the specified file is skipped. |
    |Maximum Directory Monitoring Depth|The maximum depth at which the specified log directory is monitored. Valid values: 0 to 1000. The value 0 indicates that only the directory specified in the log path is monitored.|

    You can configure advanced options based on your business requirements. We recommend that you do not modify the settings. The following table describes the parameters in the advanced options.

    |Parameter|Description|
    |:--------|:----------|
    |Enable Plug-in Processing|If you turn on the **Enable Plug-in Processing** switch, you can configure the Logtail plug-in to process logs. For more information, see [Overview](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to process data/Overview.md).**Note:** If you turn on the **Enable Plug-in Processing** switch, the Upload Raw Log, Timezone, Drop Failed to Parse Logs, Filter Configuration, and Incomplete Entry Upload \(Delimiter mode\) fields become unavailable. |
    |Upload Raw Log|If you turn on the **Upload Raw Log** switch, each raw log is uploaded to Log Service as a value of the \_\_raw\_\_ field together with the parsed log.|
    |Topic Generation Mode|Sets the topic generation mode. For more information, see [Configure a log topic](/intl.en-US/Data Collection/Logtail collection/Text logs/Configure a log topic.md).    -   **Null - Do not generate topic**: This mode is selected by default. In this mode, the topic field is set to an empty string. You do not need to enter a topic to query logs.
    -   **Machine Group Topic Attributes**: This mode is used to differentiate logs that are generated by different servers.
    -   **File Path RegEx**: In this mode, you must configure a regular expression in the **Custom RegEx** field. The part of a log path that matches the regular expression is used as the topic name. This mode is used to differentiate logs that are generated by different users or instances. |
    |Log File Encoding|The encoding format of the log file. Valid values: utf8 and gbk.|
    |Timezone|The time zone where logs are collected. Valid values:     -   System Timezone: This option is selected by default. It indicates that the time zone where logs are collected is the same as the time zone to which the server belongs.
    -   Custom: Select a time zone. |
    |Timeout|The timeout period of log files. If a log file is not updated within the specified period, Logtail considers the file to be timed out. Valid values:     -   Never: All log files are continuously monitored and never time out.
    -   30 Minute Timeout: If a log file is not updated within 30 minutes, Logtail considers the file to be timed out and no longer monitors the file.

If you select **30 Minute Timeout**, you must specify the **Maximum Timeout Directory Depth** parameter. Valid values: 1 to 3. |
    |Filter Configuration|The filter conditions that are used to collect logs. Only logs that match the specified filter conditions are collected. Examples:     -   Collect logs that meet a condition: If you set **Key** to **level** and **Regex** to **WARNING\|ERROR**, only WARNING-level and ERROR-level logs are collected.
    -   Filter out logs that do not meet a condition. For more information, see [Regular-Expressions.info](http://www.regular-expressions.info/lookaround.html).
        -   If you set **Key** to **level** and **Regex** to **^\(?!. \*\(INFO\|DEBUG\)\). \***, INFO-level or DEBUG-level logs are not collected.
        -   If you set **Key** to **url** and **Regex** to **. \*^\(?!.\*\(healthcheck\)\). \***, logs whose URL contain healthcheck are not collected. For example, the log entry is not collected if the value of the Key field is url and the value of the Vaue field is /inner/healthcheck/jiankong.html.
For more examples, see [regex-exclude-word](https://stackoverflow.com/questions/2404010/match-everything-except-for-specified-strings) and [regex-exclude-pattern](https://stackoverflow.com/questions/2078915/a-regular-expression-to-exclude-a-word-string). |

    Click **Next** to complete the configuration and use Logtail to collect logs.

    **Note:**

    -   The Logtail configurations require at most 3 minutes to take effect.
    -   If an error occurs when you use Logtail to collect logs, see [Diagnose collection errors]().
7.  Preview the data, configure indexes, and then click **Next**.

    By default, Log Service enables the Full Text Index to query and analyze logs. You can also manually or automatically configure Field Search. For more information, see [Enable and configure the indexing feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).

    **Note:**

    -   To query and analyze logs, you must enable the Full Text Index or Field Search. If you configure both of them, the settings of Field Search prevail.
    -   If the data type of the index is long or double, the case sensitive and delimiter settings are unavailable.

## Appendix: Sample logs and field descriptions

The following example shows how to obtain IIS logs in the IIS W3C Extended Log Format:

```
#Software: Microsoft Internet Information Services 7.5
#Version: 1.0
#Date: 2020-09-08 09:30:26
#Fields: date time s-sitename s-ip cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken
2009-11-26 06:14:21 W3SVC692644773 125.67.67. * GET /index.html - 80 - 10.10.10.10 Baiduspider+(+http://www.baidu.com)200 0 64 185173 296 0
```

-   Field prefixes

    |Prefix|Description|
    |:-----|:----------|
    |s-|Indicates a server action.|
    |c-|Indicates a client action.|
    |cs-|Indicates a client-to-server action.|
    |sc-|Indicates a server-to-client action.|

-   Fields

    |Field|Description|
    |:----|:----------|
    |date|The date on which the client sends the request.|
    |time|The time when the client sends the request.|
    |s-sitename|The Internet service name and instance number of the site visited by the client.|
    |s-computername|The name of the server on which the log entry is generated.|
    |s-ip|The IP address of the server on which the log entry is generated.|
    |cs-method|The HTTP request method used by the client, for example, GET or POST.|
    |cs-uri-stem|The URI resource requested by the client.|
    |cs-uri-query|The query string that follows the question mark \(?\) in the HTTP request.|
    |s-port|The port number of the server.|
    |cs-username|The username that the client uses to access the server.    -   Authenticated users are referenced as `domain\username`.
    -   Anonymous users are indicated by a hyphen \(-\). |
    |c-ip|The real IP address of the client that sends the request.|
    |cs-version|The protocol version used by the client, for example, HTTP 1.0 or HTTP 1.1.|
    |cs\(User-Agent\)|The browser used by the client.|
    |Cookie|The content of the sent cookie or received cookie. A hyphen \(-\) is used if no cookie is sent or received.|
    |referer|The previous site visited by the user.|
    |cs-host|The header name of the host.|
    |sc-status|The HTTP status code returned by the server.|
    |sc-substatus|The HTTP sub-status code returned by the server.|
    |sc-win32-status|The Windows status code returned by the server.|
    |sc-bytes|The number of bytes sent by the server.|
    |cs-bytes|The number of bytes received by the server.|
    |time-taken|The processing time of the request. Unit: milliseconds.|


