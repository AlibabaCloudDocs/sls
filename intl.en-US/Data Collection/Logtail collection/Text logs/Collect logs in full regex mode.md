# Collect logs in full regex mode

You can use the full regex mode to extract custom fields from log entries. This topic describes how to configure Logtail in full regex mode in the Log Service console to collect logs.

-   A project and a Logstore are created. For more information, see [Create a project](/intl.en-US/Data Collection/Preparation/Manage a project.md) and [Create a Logstore](/intl.en-US/Data Collection/Preparation/Manage a Logstore.md).
-   Ports 80 and 443 of the server from which you want to collect logs are enabled.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Import Data section, click **RegEx - Text Log**.

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

6.  Create a Logtail configuration file and click **Next**.

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
    |Docker File|If you collect logs from Docker containers, you can turn on the **Docker File** switch and configure the paths and tags of the containers. Logtail monitors the containers when they are created and destroyed, filters the logs of the containers by tag, and collects the filtered logs. For more information about container text logs, see [Use the console to collect Kubernetes text logs in DaemonSet mode](/intl.en-US/Data Collection/Logtail collection/Container log collection/Use the console to collect Kubernetes text logs in DaemonSet mode.md).|
    |Mode|The default mode is **Full Regex Mode**. You can change the mode.|
    |Singleline|    -   The single-line mode indicates that each line contains only one log entry. If you want to collect single-line logs, turn on the **Singleline** switch. This way, Log Service collects logs by line.
    -   If you want to collect multi-line logs such as Java program logs, turn off the **Singleline** switch and use the multi-line regex mode to collect logs. |
    |Log Sample|Enter a sample log entry that is retrieved from a log source in an actual scenario. Then, Log Service extracts a regular expression from the log entry. For more information about sample logs, see [Collect single-line logs](#section_wlt_968_p3d) and [Collect multi-line logs](#section_sls_ti6_lyt).|
    |Regex to Match First Line|To collect multi-line logs, you must turn off the **Singleline** switch and configure a regular expression to match the first line of a log entry. You can specify a regular expression to match the first line of a log entry. You can also use the regular expression that is automatically generated by Log Service.    -   Automatically generate a regular expression to match the first line of a log entry

After you enter a sample log entry, click **Auto Generate**. A regular expression is generated to match the first line of the log entry.

    -   Specify a regular expression to match the first line of a log entry

After you enter a sample log entry, click **Manual** to specify a regular expression to match the first line of the log entry. After the configuration, click **Validate** to check whether the regular expression is valid. For more information, see [How do I modify a regular expression?](). |
    |Extract Field|If you turn on the **Extract Field** switch, you can use a regular expression to extract logs in the format of key-value pairs.|
    |RegEx|You must set this parameter if you turn on the **Extract Field** switch.     -   Automatically generate a regular expression

In the **Log Sample** field, select the logs to be extracted, and click **Generate Regular Expression**. A regular expression is automatically generated.

    -   Specify a regular expression

Click **Manual** to specify a regular expression. After you specify the regular expression, click **Validate** to check whether the regular expression can parse or extract logs. For more information, see [How do I modify a regular expression?](). |
    |Extracted Content|You must set this parameter if you turn on the **Extract Field** switch. After you use a regular expression to extract field values, you must specify a key for each value. |
    |Use System Time|You must set this parameter if you turn on the **Extract Field** switch.     -   If you turn on the **Use System Time** switch, the timestamp of a log entry indicates the system time of the server when the log entry is collected.
    -   If you turn off the **Use System Time** switch, you must set the **Specified Time Key** and **Time Format** fields based on the time fields of **Extracted Content**. For more information about the time formats, see [Time formats](/intl.en-US/Data Collection/Logtail collection/Text logs/Time formats.md).

For example, assume that you set the **Specify Time Key** to **time\_local** and **Time Format** to **%d/%b/%Y:%H:%M:%S**. The timestamp of a log entry is the value indicated by the **time\_local** field. |
    |Drop Failed to Parse Logs|Specifies whether to discard logs that fail to be parsed:    -   If you turn on the **Drop Failed to Parse Logs** switch, logs that fail to be parsed are not uploaded to Log Service.
    -   If you turn off the **Drop Failed to Parse Logs** switch, raw logs are uploaded to Log Service if the raw logs fail to be parsed. |
    |Maximum Directory Monitoring Depth|Enter the maximum depth at which the specified log directory is monitored. Valid values: 0 to 1000. The value 0 indicates that only the directory specified in the log path is monitored.|

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

## Collect single-line logs

-   Sample log entry

    ```
    127.0.0.1 - - [10/Sep/2018:12:36:49  0800] "GET /index.html HTTP/1.1" 200 612 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36"
    ```

-   Regular expression

    ```
    (\S )\s-\s(\S )\s\[([^]] )]\s"(\S )\s(\S )\s(\S )"\s(\S )\s(\S )\s"(\S )"\s"([^"] )". *
    ```


## Collect multi-line logs

-   Sample log entry

    ```
    [2018-10-01T10:30:01,000] [INFO] java.lang.Exception: exception happened
        at TestPrintStackTrace.f(TestPrintStackTrace.java:3)
        at TestPrintStackTrace.g(TestPrintStackTrace.java:7)
        at TestPrintStackTrace.main(TestPrintStackTrace.java:16)
    ```

-   The regular expression that is used to match the first line of the log entry

    ```
    \[\d -\d -\w :\d :\d ,\d ]\s\[\w ]\s. *
    ```

-   Regular expression

    ```
    \[(\S )]\s\[(\S )]\s(. *)
    ```


