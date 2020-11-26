# Collect Windows event logs

Windows Logtail can use plug-ins to collect Windows event logs. This topic describes how to create a Logtail collection configuration in the log service console to collect Windows event logs.

Logtail is installed on the server. For more information, see [Install Logtail in Windows](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Windows.md).

**Note:** Only Windows Logtail 1.0.0.0 and later versions are supported.

## Procedure

For event logs, Windows provides the [Windows Event Log](https://docs.microsoft.com/en-us/windows/desktop/wes/windows-event-log) and [Event Logging](https://docs.microsoft.com/en-us/windows/desktop/EventLog/event-logging) two sets of Apis, the former is an upgrade of the latter and is only available in Windows Vista and above. The Logtail plug-in automatically selects the API \(Windows Event Log is preferred\) based on the operating system to obtain Windows Event logs.

Windows event logs adopt the publish-subscribe model. An Application or kernel publishes event logs to specified channels, such as Application, Security, and System channels. Logtail calls Windows API operations by using the corresponding Logtail plug-in. You can subscribe to these channels to continuously retrieve event logs and send them to log service.

Logtail can collect events from multiple channels at the same time. For example, Logtail can collect application and system logs at the same time.

![Procedure](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4107549951/p35294.png)

## View channel information

On a Windows Server, **event Viewer** to view channel information.

1.  Click **start**.

2.  Search and open **Event Viewer**.

3.  Expand in the left navigation **Windows logs**.

4.  View the full name of the channel.

    In **Windows logs**, select the target Channel, right-click **property** to view the full channel name. **Windows logs** the full names of common channels are as follows.

    -   Application
    -   Security
    -   Setup
    -   System
5.  View channel information.

    In **Windows logs** to view the level, date and time, source, and event ID of the event, click the target channel.

    In the collection configuration, filters logs based on the log collection information.

    ![Event logs](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4107549951/p35295.png)


## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In data import region, Select **custom Data plug-in**.

3.  In the Specify Logstore step, select the target project and Logstore, and click **Next**.

    You can also click **Create Now** to create a project and a Logstore. For more information, see [Step 1: Create a project and a Logstore](/intl.en-US/Quick Start/Quick start.md).

4.  In the Create Machine Group step, create a machine group.

    -   If a machine group is available, click **Using Existing Machine Groups**.
    -   If no machine group is available, perform the following steps. The following steps take ECS instances as an example to describe how to create a machine group:
        1.  Install Logtail on ECS instances. For more information, see [Install Logtail on ECS instances](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail on ECS instances.md).

            If Logtail is installed on the ECS instances, click **Complete Installation**.

            **Note:** If you need to collect logs from user-created clusters or servers of third-party cloud service providers, you must install Logtail on these servers. For more information, see [Install Logtail in Windows](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Windows.md).

        2.  After the installation is completed, click **Complete Installation**.
        3.  On the page that appears, set relevant parameters for the machine group. For more information, see [Create an IP address-based machine group](/intl.en-US/Data Collection/Logtail collection/Machine Group/Create an IP address-based machine group.md) or [Create a custom ID-based machine group](/intl.en-US/Data Collection/Logtail collection/Machine Group/Create a custom ID-based machine group.md).
5.  In the Machine Group Settings step, apply the configurations to the machine group.

    Select the created machine group and move it from **Source Server Groups** to **Applied Server Groups**.

6.  In data Source settings tab, configure **configuration name** and **plug-ins**.

    -   inputs: Required. The Logtail configurations for log collection.

        **Note:** You can configure only one type of data source in the inputs field.

    -   processors: Optional. The Logtail configurations for data processing. You can configure one or more processing methods in the processors field. For more information, see [Process data](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to collect data/Process data.md).
    In this example, collect **application** and **system** for two channels, set IgnoreOlder to 3 days to avoid collecting too many event logs.

    ```
    {
        "inputs": [
            {
                "type": "service_wineventlog",
                "detail": {
                    "Name": "Application",
                    "IgnoreOlder": 259200
                }
            },
            {
                "type": "service_wineventlog",
                "detail": {
                    "Name": "System",
                    "IgnoreOlder": 259200
                }
            }
        ]
    }
    ```

    |Field|Type|Required|Description|
    |:----|:---|:-------|:----------|
    |type|String|Yse|Data source type, fixed as service\_wineventlog.|
    |Name|String|Yse|The name of the channel to which the event logs to be collected belongs. You can specify only one channel. If this parameter is not configured, the default value is Application, which indicates **application** event logs in the Channel. You can view the full name of a tunnel in windows. For more information, see [step 4](#step_8v9_z3s_6qy).|
    |IgnoreOlder|Unsigned integer|No|Filters logs by event time. This parameter specifies the offset from the start time of collection. Unit: Seconds. Logs generated earlier than the specified time are ignored. Examples:     -   3600: specifies that logs generated one hour before the start time of collection are ignored.
    -   14400: specifies that logs generated four hours before the start time of collection are ignored.
The default value is empty, indicating that event logs are not filtered by time.

**Note:** This parameter takes effect only when Collection is configured for the first time. Logtail records the event collection checkpoints to ensure that event logs are not collected repeatedly. |
    |Level|String|No|Filters logs by event level. The default value is information, warning, error, critical indicates that all the logs except the verbose level are collected. Valid values: information, warning, error, critical, and verbose. You can use commas \(,\) to specify multiple levels. **Note:** This parameter only supports Windows Event Log API, that is, it can only be used on Windows Vista and later. |
    |EventID|String|No|You can filter logs by event ID. You can specify forward filtering \(by single keyword or range\) or reverse filtering \(by range not supported\). The default value is null, indicating that all events will be collected. Examples:     -   1-200 specifies that only event logs with event IDs in the range of 1 to 200 are collected.
    -   20 indicates that only the event log with event ID 20 is collected.
    -   -100 indicates that all event logs except the event ID 100 are collected.
    -   1-200,-100 indicates that the event logs in the range of 1 to 200 except 100 are collected.
You can use commas \(,\) to specify multiple values.

**Note:** This parameter only supports Windows Event Log API, that is, it can only be used on Windows Vista and later. |
    |Provider|String array|No|Filters logs based on the event source. For example, \["App1", "App2"\] indicates that only event logs of App1 and App2 are collected. Other event logs are ignored. The default value is empty, indicating that events from all sources are collected.

**Note:** This parameter only supports Windows Event Log API, that is, it can only be used on Windows Vista and later. |
    |IgnoreZeroValue|boolean|No|Not every event log has all fields. You can use this parameter to filter empty fields. The definition of empty fields depends on the type. For example, 0indicates an empty field. The default value is false, indicating that empty fields are not filtered. |

7.  In the **Configure Query and Analysis** step, configure the indexes.

    Indexes are configured by default. You can re-configure the indexes based on your business requirements. For more information, see [Enable and configure the index feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).

    **Note:**

    -   You must configure Full Text Index or Field Search. If you configure both of them, the settings of Field Search prevail.
    -   If the data type of the index is long or double, the case sensitive and delimiter settings are unavailable.

After collecting Windows logs to log service, you can view these logs in the log service console.

![Raw Logs page](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4107549951/p35298.png)

|Field|Field type|Filtered|Attributes|
|:----|:---------|:-------|:---------|
|activity\_id|String|Yse|The ID of the active global transaction to which the current event belongs. Active events have the same global transaction ID.|
|computer\_name|String|No|The name of the node that generates the current event.|
|event\_data|JSON object|No|The data related to the current event.|
|event\_id|Int|No|The ID of the current event.|
|kernel\_time|int|Yes|The kernel time consumed by the current event, which is generally 0.|
|keywords|JSON array|Yes|The keyword associated with the current event. It is used to classify events.|
|level|String|Yse|The level of the current event.|
|log\_name|String|No|The name of the channel to which the current event is obtained. It is also the name of the channel in the logtail collection configuration. Name parameter.|
|message|String|Yse|The message associated with the current event.|
|message\_error|String|Yse|The error that occurs when the message associated with the current event is parsed.|
|opcode|String|Yse|The operation code associated with the current event.|
|process\_id|int|Yes|The process ID of the current event.|
|processor\_id|int|Yes|The ID of the processor corresponding to the current event, which is typically 0.|
|processor\_time|int|Yes|The processor time consumed by the current event. Typically, it is 0.|
|provider\_guid|String|Yse|The global transaction ID of the current event source.|
|record\_number|Int|No|The record number associated with the current event. The value of event record number increases with the writing of each event. When the number exceeds 2 32\(Event Logging\) or 2. 64\(Windows Event Log\) the template will start from 0 again.|
|related\_activity\_id|String|Yse|The global transaction ID of another activity associated with the activity to which the current event belongs.|
|session\_id|int|Yes|The session ID of the current event, which is typically a 0.|
|source\_name|String|No|The source of the current event, which is in the logtail collection configuration. Provider parameter.|
|Config Restore Destination step|String|Yse|The task associated with the current event.|
|thread\_id|int|Yes|The thread ID of the current event.|
|type|String|No|The API used to obtain the current event.|
|user\_data|JSON object|No|The data associated with the current event.|
|user\_domain|String|Yse|The user domain associated with the current event.|
|user\_identifier|String|Yse|The Security Identifier of the user associated with the event.|
|user\_name|String|Yse|The username associated with the current event.|
|user\_time|int|Yes|The user time consumed by the current event, which is generally 0.|
|user\_type|String|Yse|The user type associated with the current event.|
|version|int|Yes|The version of the current event.|
|xml|String|Yse|The original information of the current event, in XML format.|

