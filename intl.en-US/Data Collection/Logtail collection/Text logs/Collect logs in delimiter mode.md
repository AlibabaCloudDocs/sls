# Collect logs in delimiter mode

Log Service allows you to collect logs in delimiter mode. After logs are collected, you can transform and ship the logs, and perform multidimensional log analysis. This topic describes how to create a Logtail configuration file to collect logs in delimiter mode by using the Log Service console.

-   A project and a Logstore are created. For more information, see [Create a project](/intl.en-US/Data Collection/Preparation/Manage a project.md) and [Create a Logstore](/intl.en-US/Data Collection/Preparation/Manage a Logstore.md).
-   Ports 80 and 443 of the server from which you want to collect logs are enabled.

## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Import Data section, select **Delimiter Mode - Text Log**.

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
    |Config Name|The name of the Logtail configuration file. The name must be unique in a project and cannot be modified after the file is created. You can also click **Import Other Configuration** to import a Logtail configuration file from another project. |
    |Log Path|The directory and name of the log file. The file names can be complete names or names that contain wildcards. For more information, see [Wildcard matching](http://man7.org/linux/man-pages/man7/glob.7.html). The log files in all levels of subdirectories under a specified directory are monitored if the log files match the specified pattern. Examples:

    -   /apsara/nuwa/… /\*.log indicates that the files whose extension is .log in the /apsara/nuwa directory and its subdirectories are monitored.
    -   /var/logs/app\_\*/\*.log indicates that each file that meets the following conditions is monitored: The file name contains .log. The file is stored in a subdirectory \(at all levels\) of the /var/logs directory. The name of the subdirectory matches the app\_\* pattern.
**Note:**

    -   By default, each log file can be collected by using only one Logtail configuration file.
    -   To use multiple Logtail configuration files to collect one log file, we recommend that you create a symbolic link that points to the directory where the file is located. For example, assume that you want to collect two copies of the log.log file from the /home/log/nginx/log/log.log directory. You can run the following command to create a symbolic link that points to the directory. When you configure the Logtail configuration files, use the original path in one Logtail configuration file and use the symbolic link in the other Logtail configuration file.

        ```
ln -s /home/log/nginx/log /home/log/nginx/link_log
        ```

    -   You can include only asterisks \(\*\) and question marks \(?\) as wildcard characters in the log path. |
    |Docker File|If you collect logs from Docker containers, you can turn on the **Docker File** switch and configure the paths and tags of the containers. Logtail monitors the containers that are being created and destroyed, filters the logs of the containers by tag, and collects the filtered logs. For more information about container text logs, see [Use the console to collect Kubernetes text logs in DaemonSet mode](/intl.en-US/Data Collection/Logtail collection/Container log collection/Use the console to collect Kubernetes text logs in DaemonSet mode.md).|
    |Blacklist|If you turn on the **Blacklist** switch, you can configure a blacklist to skip the specified directories or files when you collect logs. You can use exact match or wildcard match to specify directories and files. Examples:    -   If you select **Filter by Directory** from the Filter Type drop-down list and enter /tmp/mydir in the Content column, all files in the directory are skipped.
    -   If you select **Filter by File** from the Filter Type drop-down list and enter /tmp/mydir/file in the Content column, only the specified file is skipped. |
    |Mode|The default mode is **Delimiter Mode**. You can change the mode.|
    |Log Sample|Enter a sample log entry that is retrieved from a log source in an actual scenario. Example:    ```
127.0.0.1|#|-|#|13/Apr/2020:09:44:41 +0800|#|GET /1 HTTP/1.1|#|0.000|#|74|#|404|#|3650|#|-|#|curl/7.29.0
    ```

**Note:** The delimiter mode applies only to single-line logs. If you want to collect multi-line logs, we recommend that you select [Simple Mode - Multi-line](/intl.en-US/Data Collection/Logtail collection/Text logs/Collect logs in simple mode.md) or [Full Regex Mode](/intl.en-US/Data Collection/Logtail collection/Text logs/Collect logs in full regex mode.md). |
    |Delimiter|Select a delimiter based on the log format. For example, you can select a vertical bar \(\|\). For more information, see [Appendix: delimiters and sample log entries](/intl.en-US/Data Collection/Logtail collection/Text logs/Collect logs in delimiter mode.md). **Note:** If you select **Hidden Characters** as the delimiter, you must enter the character in the following format: `0xthe hexadecimal ASCII code of the non-printable character`. For example, to use the non-printable character whose hexadecimal ASCII code is 01, you must enter **0x01**. |
    |Quote|If a log field contains delimiters, you must specify a pair of quotes to enclose the field. Log Service parses the content that is enclosed in a pair of quotes into a new complete field. Select a quote based on the log format. **Note:** If you select **Hidden Characters** as the quote, you must enter the character in the following format: `0xthe hexadecimal ASCII code of the non-printable character`. For example, to use the non-printable character whose hexadecimal ASCII code is 01, you must enter **0x01**. |
    |Extracted Content|Log Service extracts the log content based on the specified sample log and delimiter. The extracted log content is delimited into values. You must specify a key for each value.|
    |Incomplete Entry Upload|This feature specifies whether to upload a log entry whose number of parsed fields is less than the number of the specified keys. If you enable this feature, the log entry is uploaded. Otherwise, the log entry is dropped. For example, if you set the delimiter to the vertical bar \(\|\), the log entry 11\|22\|33\|44\|55 is parsed into the following fields: 11, 22, 33, 44, and 55. You can set the keys to A, B, C, D, and E, respectively.

    -   If you turn on the **Incomplete Entry Upload** switch, 55 is uploaded as the value of the D key when Log Service collects the log entry 11\|22\|33\|55.
    -   If you turn off the **Incomplete Entry Upload** switch, the log entry 11\|22\|33\|55 is dropped because the field value does not match the specified key. |
    |Use System Time|Specifies whether to use the system time.    -   If you turn on the **Use System Time** switch, the timestamp of a log entry indicates the system time of the server when the log entry is collected.
    -   If you turn off the **Use System Time** switch, you must set the **Specified Time Key** and **Time Format** based on the time fields of **Extracted Content**. For more information about the time formats, see [Time formats](/intl.en-US/Data Collection/Logtail collection/Text logs/Time formats.md).

For example, you can set the **Specify Time Key** parameter to **time\_local** and set the **Time Format** parameter to **%d/%b/%Y:%H:%M:%S**. The timestamp of a log entry is the value of the **time\_local** field. |
    |Drop Failed to Parse Logs|Specifies whether to drop logs that fail to be parsed.    -   If you turn on the **Drop Failed to Parse Logs** switch, logs that fail to be parsed are not uploaded to Log Service.
    -   If you turn off the **Drop Failed to Parse Logs** switch, raw logs are uploaded to Log Service if the raw logs fail to be parsed. |
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

    By default, Log Service enables the Full Text Index to query and analyze logs. You can also manually or automatically configure Field Search. For more information, see [Configure indexes](/intl.en-US/Index and query/Configure indexes.md).

    **Note:**

    -   To query and analyze logs, you must enable the Full Text Index or Field Search. If you configure both of them, the settings of Field Search prevail.
    -   If the data type of the index is long or double, the case sensitive and delimiter settings are unavailable.

## Appendix: delimiters and sample log entries

DSV formatted logs use line feeds as boundaries. Each line indicates a log entry. The fields of each log entry are delimited by using single-character delimiters or multi-character delimiters. If a field contains delimiters, you can enclose the field with a pair of quotes.

-   Single-character delimiter

    The following example shows sample log entries with single-character delimiters:

    ```
    05/May/2016:13:30:28,10.10.*.*,"POST /PutData?Category=YunOsAccountOpLog&AccessKeyId=****************&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=******************************** HTTP/1.1",200,18204,aliyun-sdk-java
    05/May/2016:13:31:23,10.10.*.*,"POST /PutData?Category=YunOsAccountOpLog&AccessKeyId=****************&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=******************************** HTTP/1.1",401,23472,aliyun-sdk-java
    ```

    For the log entries with single-character delimiters, you must specify each delimiter. You can also use a pair of quotes in the log entry.

    -   Delimiter: Available single-character delimiters include the tab \(\\t\), vertical bar \(\|\), space, comma \(,\), and semicolon \(;\). You can also specify a non-printable character as the delimiter. A double quotation mark \("\) cannot be used as the delimiter.

        However, a double quotation mark \("\) can be used as a quote. You can place the double quotation mark at the border of a field, or in the field. If a double quotation mark \("\) is included in a log field but is not used as a quote, it must be escaped as double quotation marks \(""\). When Log Service parses log fields, the double quotation marks \(""\) are automatically converted to a double quotation mark \("\). For example, you can specify a comma \(,\) as the delimiter and double quotation mark \("\) as the quote in a log field. You must enclose the field that contains commas by using a pair of quotes. In addition, you must escape the double quotation mark \("\) in the field to double quotation marks \(""\). The log format after processing is 1999,Chevy,"Venture ""Extended Edition, Very Large""","",5000.00. The log entry can be parsed into five fields: 1999, Chevy, Venture "Extended Edition, Very Large", an empty field, and 5000.00.

    -   Quote: If a log field contains delimiters, you must specify a pair of quotes to enclose the field. Log Service parses the content that is enclosed in a pair of quotes into a new complete field.

        Available quotes include the tab \(\\t\), vertical bar \(\|\), space, comma \(,\), and semicolon \(;\). You can also specify a non-printable character as the quote.

        For example, you can specify a comma \(,\) as the delimiter and double quotation mark \("\) as the quote in log fields. The log entry is 1997,Ford,E350,"ac, abs, moon",3000.00. The log entry can be parsed into five fields: 1997, Ford, E350, ac, abs, moon, and 3000.00.

-   Multi-character delimiter

    The following example shows sample log entries with multi-character delimiters:

    ```
    05/May/2016:13:30:28&&10.200.**.**&&POST /PutData?Category=YunOsAccountOpLog&AccessKeyId=****************&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=pD12XYLmGxKQ%2Bmkd6x7hAgQ7b1c%3D HTTP/1.1&&200&&18204&&aliyun-sdk-java
    05/May/2016:13:31:23&&10.200.**.**&&POST /PutData?Category=YunOsAccountOpLog&AccessKeyId=****************&Date=Fri%2C%2028%20Jun%202013%2006%3A53%3A30%20GMT&Topic=raw&Signature=******************************** HTTP/1.1&&401&&23472&&aliyun-sdk-java
    ```

    A multi-character delimiter can contain two or three characters, such as \|\|, &&&, and ^\_^\). Log Service parses log field based on delimiters. You do not need to use quotes to enclose the log field to be parsed.

    **Note:** You must make sure that the delimiters in a field cannot be parsed into a new field. Otherwise, Log Service does not parse the field as expected.

    For example, you can specify && as the delimiter. The log entry 1997&&Ford&&E350&&ac&abs&moon&&3000.00 is parsed into the following five fields: 1997, Ford, E350, ac&abs&moon, and 3000.00.


