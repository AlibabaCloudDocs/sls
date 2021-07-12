# Collect logs in simple mode

When you collect logs in simple mode, logs are not parsed. Each log entry is collected and sent to Log Service as a whole. This simplifies the process of log collection. This topic describes how to create a Logtail configuration in simple mode in the Log Service console to collect logs.

-   A project and a Logstore are created. For more information, see [Create a project](/intl.en-US/Preparation/Manage a project.md) and [Create a Logstore](/intl.en-US/Preparation/Manage a Logstore.md).
-   Ports 80 and 443 of the server from which you want to collect logs are enabled.

The simple mode supports the following types of text logs:

-   Single-line text log

    Each log line is collected as a log entry. Two log entries in a log file are separated by a line feed. In single-line mode, you must specify the directories and names of log files. Then, Logtail collects logs by line from the specified files.

-   Multi-line text log

    Multiple log lines are collected as a log entry by default. In multi-line mode, you must specify the directories and names of log files. In addition, you must enter a sample log entry and configure a regular expression to match the start part in the first line of a log entry. Logtail uses the regular expression to match the start part in the first line of a log entry and reckons unmatched lines as part of the log entry.


**Note:** If you collect logs in simple mode, the timestamp of a log entry indicates the system time of the server when the log entry is collected.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Import Data section, select **Multi-line - Text Log**.

    This example shows how to collect multi-line text logs. To collect single-line text logs, select **Single Line - Text Log**.

3.  Select a destination project and Logstore, and then click **Next**.

    You can also click **Create Now** to create a project and a Logstore.

4.  Create a machine group.

    -   If a machine group is available, click **Using Existing Machine Groups**.
    -   If no machine groups are available, perform the following steps to create a machine group. In this example, an ECS instance is used.
        1.  On the ECS Instances tab, select the destination ECS instance and click **Execute Now**.

            For more information, see [Install Logtail on ECS instances](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail on ECS instances.md).

            **Note:** If you need to collect logs from self-managed clusters or servers of third-party cloud service providers, you must install Logtail. For more information, see [Install Logtail in Linux](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md) or [Install Logtail in Windows](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Windows.md).

        2.  After the installation, click **Complete Installation**.
        3.  On the Create machine group page, enter a **Name** and click **Next**.

            Log Service allows you to create IP address-based machine groups and custom ID-based machine groups. For more information, see [Create an IP address-based machine group](/intl.en-US/Data Collection/Logtail collection/Machine Group/Create an IP address-based machine group.md) and [Create a custom ID-based machine group](/intl.en-US/Data Collection/Logtail collection/Machine Group/Create a custom ID-based machine group.md).

5.  Select and move the destination machine group from **Source Machine Groups** to **Applied Server Groups**, and then click **Next**.

    **Note:** If you want to apply a machine group immediately after it is created, the heartbeat status of the machine group may be **FAIL**. This issue occurs because no servers in the machine group have been connected to Log Service. In this case, you can click **Automatic Retry**. If the issue persists, see [What can I do if the Logtail client has no heartbeat?](/intl.en-US/Data Collection/FAQ/Troubleshooting for Logtail machine group without heartbeat in the log Service Console.md)

6.  Create a Logtail configuration and click **Next**.

    ![Simple mode](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1780131161/p186218.png)

    |Parameter|Description|
    |---------|-----------|
    |Config Name|The name of the Logtail configuration. The name must be unique in a project and cannot be modified after the file is created. You can also click **Import Other Configuration** to import a Logtail configuration from another project. |
    |Log Path|The directories and files from which logs are collected. The file names can be complete names or names that contain wildcards. For more information, see [Wildcard matching](http://man7.org/linux/man-pages/man7/glob.7.html). If these files match the specified pattern, the files in all levels of subdirectories under a specified directory are monitored. Examples:

    -   /apsara/nuwa/\*\*/\*.log indicates that the files whose name is suffixed by .log in the /apsara/nuwa directory and its recursive sub-directories are monitored.
    -   /var/logs/app\_\*/\*.log indicates that each file that meets the following conditions is monitored: The file name contains .log. The file is stored in a subdirectory \(at all levels\) of the /var/logs directory. The name of the subdirectory matches the app\_\* pattern.
**Note:**

    -   By default, each log file can be collected by using only one Logtail configuration.
    -   To use multiple Logtail configurations to collect one log file, we recommend that you create a symbolic link that points to the directory where the file is located. For example, you want to collect two copies of the log.log file from the /home/log/nginx/log/log.log directory. You can run the following command to create a symbolic link that points to the directory. When you configure the Logtail configurations, use the original path in one Logtail configuration and use the symbolic link in the other Logtail configuration.

        ```
ln -s /home/log/nginx/log /home/log/nginx/link_log
        ```

    -   You can use only asterisks \(\*\) and question marks \(?\) as wildcard characters in the log path. |
    |Blacklist|If you turn on the **Blacklist** switch, you can configure a blacklist to skip the specified directories or files when you collect logs. You can use exact match or wildcard match to specify directories and files. Examples:    -   If you select **Filter by Directory** from the Filter Type drop-down list and enter /home/admin/dir1 in the Content column, all files in the /home/admin/dir1 directory are skipped.
    -   If you select **Filter by Directory** from the Filter Type drop-down list and enter /home/admin/dir\* in the Content column, the files in all subdirectories whose names are prefixed by dir in the /home/admin/ directory are skipped.
    -   If you select **Filter by Directory** from the Filter Type drop-down list and enter /home/admin/\*/dir in the Content column, all files in the dir subdirectory \(level-2 directory\) of in the /home/admin/ directory are skipped.

For example, the files in the /home/admin/a/dir directory are skipped, but the files in the /home/admin/a/b/dir directory are collected.

    -   If you select **Filter by File** from the Filter Type drop-down list and enter /home/admin/private\*.log in the Content column, all files whose names are prefixed by private and suffixed by .log in the /home/admin/ directory are skipped.
    -   If you select **Filter by File** from the Filter Type drop-down list and enter /home/admin/private\*/\*\_inner.log in the Content column, all files whose names are suffixed by \_inner.log and prefixed by private in the /home/admin/ directory are skipped.

For example, the /home/admin/private/app\_inner.log file is skipped, but the /home/admin/private/app.log file is collected.

**Note:**

    -   You can use only asterisks \(\*\) and question marks \(?\) as wildcard characters in the log path.
    -   The blacklist matching process has computational overhead. We recommend that you specify less than 10 blacklist entries. |
    |Docker File|If you collect logs from Docker containers, you can turn on the **Docker File** switch and configure the paths and tags of the containers. Logtail monitors the containers when they are created and destroyed, filters the logs of the containers by tag, and collects the filtered logs. For more information about container text logs, see [Use the console to collect Kubernetes text logs in DaemonSet mode](/intl.en-US/Data Collection/Logtail collection/Container log collection/Use the console to collect Kubernetes text logs in DaemonSet mode.md).|
    |Mode|The default mode is **Simple Mode - Multi-line**. You can select another mode.|
    |Log Sample|Enter a sample log entry that is retrieved from a log source in an actual scenario. Then, Log Service extracts a regular expression to match the start part in the first line of the log entry. Example:    ```
[2020-10-01T10:30:01,000] [INFO] java.lang.Exception: exception happened
    at TestPrintStackTrace.f(TestPrintStackTrace.java:3)
    at TestPrintStackTrace.g(TestPrintStackTrace.java:7)
    at TestPrintStackTrace.main(TestPrintStackTrace.java:16)
    ```

If you collect single-line text logs in simple mode, you do not need to set this parameter. |
    |Regex to Match First Line|The regular expression that Logtail uses to match the start part in the first line of a log entry. The unmatched lines are collected as part of a log entry. You can specify a regular expression to match the start part in the first line of a log entry. You can also use the regular expression that is automatically generated by Log Service.     -   Automatically generate a regular expression to match the start part in the first line of a log entry.

After you enter a sample log entry, click **Auto Generate**. A regular expression is generated to match the start part in the first line of the log entry.

    -   Specify a regular expression to match the start part in the first line of a log entry.

After you enter a sample log entry, click **Manual** to specify a regular expression to match the start part in the first line of the log entry. Then, click **Validate** to check whether the regular expression is valid. For more information, see [How do I modify a regular expression?](/intl.en-US/Data Collection/FAQ/How do I test a regular expression?.md).

If you collect single-line text logs in simple mode, you do not need to set this parameter. |
    |Drop Failed to Parse Logs|Specifies whether to drop logs that fail to be parsed.    -   If you turn on the **Drop Failed to Parse Logs** switch, logs that fail to be parsed are not uploaded to Log Service.
    -   If you turn off the **Drop Failed to Parse Logs** switch, raw logs are uploaded to Log Service if the raw logs fail to be parsed. |
    |Maximum Directory Monitoring Depth|The maximum depth at which the specified log directory is monitored. Valid values: 0 to 1000. The value 0 indicates that only the directory specified in the log path is monitored.|

    You can configure advanced settings based on your business requirements. We recommend that you do not modify the settings. The following table describes the parameters in the advanced settings.

    |Parameter|Description|
    |:--------|:----------|
    |Enable Plug-in Processing|If you turn on the **Enable Plug-in Processing** switch, you can configure the Logtail plug-in to process logs. For more information, see [Overview](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to process data/Overview.md). **Note:** If you turn on the **Enable Plug-in Processing** switch, the Upload Raw Log, Timezone, Drop Failed to Parse Logs, Filter Configuration, and Incomplete Entry Upload \(Delimiter mode\) fields become unavailable. |
    |Upload Raw Log|If you turn on the **Upload Raw Log** switch, each raw log is uploaded to Log Service as a value of the \_\_raw\_\_ field together with the parsed log.|
    |Topic Generation Mode|The topic generation mode. For more information, see [Log topics](/intl.en-US/Data Collection/Logtail collection/Text logs/Log topics.md).     -   **Null - Do not generate topic**: This mode is selected by default. In this mode, the topic field is set to an empty string. You do not need to enter a topic to query logs.
    -   **Machine Group Topic Attributes**: This mode is used to differentiate logs that are generated by different servers.
    -   **File Path RegEx**: In this mode, you must configure a regular expression in the **Custom RegEx** field. The part of a log path that matches the regular expression is used as the topic name. This mode is used to differentiate logs that are generated by different users or instances. |
    |Log File Encoding|The encoding format of the log file. Valid values: utf8 and gbk.|
    |Timezone|The time zone where logs are collected. Valid values:     -   System Timezone: This option is selected by default. It indicates that the time zone where logs are collected is the same as the time zone to which the server belongs.
    -   Custom: Select a time zone. |
    |Timeout|The timeout period of log files. If a log file is not updated within the specified period, Logtail reckons the file to be timed out. Valid values:     -   Never: All log files are continuously monitored and never time out.
    -   30 Minute Timeout: If a log file is not updated within 30 minutes, Logtail reckons the file to be timed out and no longer monitors the file.

If you select **30 Minute Timeout**, you must specify the **Maximum Timeout Directory Depth** parameter. Valid values: 1 to 3. |
    |Filter Configuration|The filter conditions that are used to collect logs. Only logs that match the specified filter conditions are collected. Examples:     -   Collect logs that meet a condition: If you set **Key** to **level** and **Regex** to **WARNING\|ERROR**, only WARNING-level and ERROR-level logs are collected.
    -   Filter out logs that do not meet a condition. For more information, see [Regular-Expressions.info](http://www.regular-expressions.info/lookaround.html).
        -   If you set **Key** to **level** and **Regex** to **^\(?!.\*\(INFO\|DEBUG\)\).\***, INFO-level or DEBUG-level logs are not collected.
        -   If you set **Key** to **url** and **Regex** to **.\*^\(?!.\*\(healthcheck\)\).\***, logs whose URL contain healthcheck are not collected. For example, the log entry is not collected if the value of the Key field is url and the value of the Value field is /inner/healthcheck/jiankong.html.
For more examples, see [regex-exclude-word](https://stackoverflow.com/questions/2404010/match-everything-except-for-specified-strings) and [regex-exclude-pattern](https://stackoverflow.com/questions/2078915/a-regular-expression-to-exclude-a-word-string). |

    Click **Next** to complete the configuration and use Logtail to collect logs.

    **Note:**

    -   The Logtail configurations require at most 3 minutes to take effect.
    -   If an error occurs when you use Logtail to collect logs, see [Diagnose collection errors](/intl.en-US/Data Collection/FAQ/How do I view log collection error messages in the log service console?.md).
7.  Preview the data, configure indexes, and then click **Next**.

    By default, Log Service enables the Full Text Index to query and analyze logs. You can also manually or automatically configure Field Search. For more information, see [Configure indexes](/intl.en-US/Index and query/Configure indexes.md).

    **Note:**

    -   To query and analyze logs, you must enable the Full Text Index or Field Search. If you configure both of them, the settings of Field Search prevail.
    -   If the data type of the index is long or double, the Case-Sensitive switch and the Delimiter field are unavailable.

