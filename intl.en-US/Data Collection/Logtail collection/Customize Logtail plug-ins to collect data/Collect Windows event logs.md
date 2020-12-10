# Collect Windows event logs

Logtail allows you to configure plug-ins to collect Windows event logs. This topic describes how to configure Logtail in the Log Service console to collect Windows event logs.

Logtail is installed on the server that you use to collect Windows event logs. For more information, see [Install Logtail in Windows](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Windows.md).

**Note:** Only Windows servers that run Logtail 1.0.0.0 or later are supported.

## Implementation

The [Windows Event Log](https://docs.microsoft.com/en-us/windows/desktop/wes/windows-event-log) API and [Event Logging](https://docs.microsoft.com/en-us/windows/desktop/EventLog/event-logging) API are provided in Windows operating systems to record event logs. The Event Logging API is an upgrade of the Windows Event Log API, and is provided only in the Windows Vista operating system or later. Logtail plug-ins automatically select an API based on the operating system to obtain Windows event logs. Windows Event Log is preferred.

The publish-subscribe model is used to collect Windows event logs, as shown in the following figure. An application or kernel publishes event logs to a specified channel, such as an application, security, or system channel. Logtail uses the corresponding plug-in to call the Windows Event Log API or Event Logging API to subscribe to these channels. This way, Logtail continuously collects event logs and sends the logs to Log Service.

Logtail allows you to collect logs from multiple channels at the same time. For example, you can collect logs from the application channel and system channel.

![Implementation](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4107549951/p35294.png)

## View channel information

You can view the information of channels in the **Event Viewer** of a Windows server.

1.  Click **Start**.

2.  Search for and click **Event Viewer**. The Event Viewer window appears.

3.  In the left-side navigation pane, expand **Windows Logs**.

4.  View the full names of channels.

    Right-click the destination channel under **Windows Logs** and select **Properties**. In the window that appears, you can view the full name of the channel. The following full names of common channels are displayed under **Windows Logs**.

    -   Application
    -   Security
    -   Setup
    -   System
5.  View the information of channels.

    Click the destination channel under **Windows Logs**. In the window that appears, you can view the level, date and time, source, and ID of each event.

    You can set filtering conditions in Logtail configurations based on the preceding fields.

    ![Event logs](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4107549951/p35295.png)


## Procedure

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Import Data section, select **Custom Data Plug-in**.

3.  In the Specify Logstore step, select the target project and Logstore, and click **Next**.

    You can also click **Create Now** to create a project and a Logstore. For more information, see [Step 1: Create a project and a Logstore](/intl.en-US/Quick Start/Quick start.md).

4.  In the Create Machine Group step, create a machine group.

    -   If a machine group is available, click **Using Existing Machine Groups**.
    -   If no machine group is available, perform the following steps. These steps take ECS instances as an example to describe how to create a machine group:
        1.  Install Logtail on ECS instances. For more information, see [Install Logtail on ECS instances](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail on ECS instances.md).

            If Logtail is installed on the ECS instances, click **Complete Installation**.

            **Note:** If you need to collect logs from user-created clusters or servers of third-party cloud service providers, you must install Logtail on these servers. For more information, see [Install Logtail in Windows](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Windows.md).

        2.  After you install Logtail, click **Complete Installation**.
        3.  On the page that appears, set relevant parameters for the machine group. For more information, see [Create an IP address-based machine group](/intl.en-US/Data Collection/Logtail collection/Machine Group/Create an IP address-based machine group.md) or [Create a custom ID-based machine group](/intl.en-US/Data Collection/Logtail collection/Machine Group/Create a custom ID-based machine group.md).
5.  In the Machine Group Settings step, apply the configurations to the machine group.

    Select the created machine group and move it from **Source Server Groups** to **Applied Server Groups**.

6.  In the Specify Data Source step, set the **Config Name** and **Plug-in Config** parameters.

    -   inputs: Required. The Logtail configurations for log collection.

        **Note:** You can configure only one type of data source in the inputs field.

    -   processors: Optional. The Logtail configurations for data processing. You can configure one or more processing methods in the processors field. For more information, see [Process data](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to collect data/Process data.md).
    The following example shows how to collect logs from the **Application** and **System** channels. The value of the IgnoreOlder parameter is set to 259200 \(three days\) to prevent collecting excessive logs.

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

    |Parameter|Type|Required|Description|
    |:--------|:---|:-------|:----------|
    |type|string|Yes|The type of the data source. Set the value to service\_wineventlog.|
    |Name|string|Yes|The name of the channel to which the collected event logs belong. You can specify only one channel. Default value: Application. This value indicates that event logs are collected from the **application** channel. For information about how to obtain the full name of a channel, see [Step 4](#step_ho5_z7a_zjy).|
    |IgnoreOlder|uint|No|Filters logs by event time. This parameter indicates an offset of the time from which event logs are collected. Unit: seconds. Logs generated earlier than the specified time are not collected. Examples:     -   3600: Logs generated 1 hour before the start time of collection are not collected.
    -   14400: Logs generated 4 hours before the start time of collection are not collected.
Default value: null. This value indicates that event logs are not filtered by time during log collection.

**Note:** This parameter takes effect only when logs are collected for the first time. Logtail records checkpoints during log collection. This mechanism prevents repeated log collection. |
    |Level|string|No|Filters logs by severity. Default value: information, warning, error, critical. This value indicates that logs of all severity levels except verbose are collected. Available severity levels include information, warning, error, critical, and verbose. You can specify multiple severity levels for the parameter and separate these severity levels with commas \(,\). **Note:** This parameter is available only when you call the Windows Event Log API. Therefore, this parameter is used only in the Windows Vista operating system or later. |
    |EventID|string|No|Filters logs by event ID. You can specify one or more IDs for positive filtering \(specify an event ID or a range\) or negative filtering \(specify an event ID\). Default value: null. This value indicates that all event logs are collected. Examples:     -   1-200: Only the logs whose event IDs are 1 to 200 are collected.
    -   20: Only the logs whose event IDs are 20 are collected.
    -   -100: All logs are collected, except the logs whose event IDs are 100.
    -   1-200,-100: The logs whose event IDs are 1 to 99 or 101 to 200 are collected.
You can separate multiple IDs or ID ranges with commas \(,\).

**Note:** This parameter is available only when you call the Windows Event Log API. Therefore, this parameter is used only in the Windows Vista operating system or later. |
    |Provider|String array|No|Filters logs by event source. For example, the value \["App1", "App2"\] specifies that only event logs from the sources named App1 and App2 are collected. Default value: null. This value indicates that event logs from all sources are collected.

**Note:** This parameter is available only when you call the Windows Event Log API. Therefore, this parameter is used only in the Windows Vista operating system or later. |
    |IgnoreZeroValue|boolean|No|Filters unavailable fields. Some fields may be unavailable in a log entry. You can set the value of the parameter based on the data type of an unavailable field. For example, set the value to 0 if the data type of an unavailable field is integer. Default value: false. This value indicates that unavailable fields are not filtered. |

7.  In the **Configure Query and Analysis** step, configure the indexes.

    Indexes are configured by default. You can re-configure the indexes based on your business requirements. For more information, see [Enable and configure the indexing feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).

    **Note:**

    -   You must configure Full Text Index or Field Search. If you configure both of them, the settings of Field Search prevail.
    -   If the data type of the index is long or double, the case sensitive and delimiter settings are unavailable.

After Windows event logs are collected to Log Service, you can view the logs in the Log Service console.

![Raw log entry](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4107549951/p35298.png)

|Parameter|Type|Filtered|Description|
|:--------|:---|:-------|:----------|
|activity\_id|string|Yes|The global transaction ID \(GTID\) of the event. Events that belong to the same transaction have the same GTID.|
|computer\_name|string|No|The name of the server where the event occurs.|
|event\_data|JSON object|No|The data related to the event.|
|event\_id|int|No|The ID of the event.|
|kernel\_time|int|Yes|The kernel time that is consumed by the event. In most cases, the value is 0.|
|keywords|JSON array|Yes|The keyword that is associated with the event. Keywords are used to classify events.|
|level|string|Yes|The severity level of the event.|
|log\_name|string|No|The name of the channel from which the event is obtained. The value of this parameter is the same as the value of the Name parameter that is specified in the Logtail plug-in configurations.|
|message|string|Yes|The message that is associated with the event.|
|message\_error|string|Yes|The error that occurs when the message associated with the event is parsed.|
|opcode|string|Yes|The operation code that is associated with the event.|
|process\_id|int|Yes|The process ID of the event.|
|processor\_id|int|Yes|The ID of the processor that is associated with the event. In most cases, the value is 0.|
|processor\_time|int|Yes|The processor time that is consumed by the event. In most cases, the value is 0.|
|provider\_guid|string|Yes|The GTID of the event source.|
|record\_number|int|No|The record number that is associated with the event. The record number increases when an event is written to Log Service. If the number exceeds 2 32 \(Event Logging\) or 2 64 \(Windows Event Log\), the record number starts from 0 again.|
|related\_activity\_id|string|Yes|The GTID of another transaction that is associated with the transaction to which the event belongs.|
|session\_id|int|Yes|The session ID of the event. In most cases, the value is 0.|
|source\_name|string|No|The source of the event. The value of this parameter is the same as the value of the Provider parameter that is specified in the Logtail configurations.|
|task|string|Yes|The task that is associated with the event.|
|thread\_id|int|Yes|The thread ID of the event.|
|type|string|No|The API that is used to obtain the event.|
|user\_data|JSON object|No|The user data that is associated with the event.|
|user\_domain|string|Yes|The user domain that is associated with the event.|
|user\_identifier|string|Yes|The Windows Security Identifier \(SID\) of the user that is associated with the event.|
|user\_name|string|Yes|The username that is associated with the event.|
|user\_time|int|Yes|The user time that is consumed by the event. In most cases, the value is 0.|
|user\_type|string|Yes|The user type that is associated with the event.|
|version|int|Yes|The version of the event.|
|xml|string|Yes|The original information of the event in the XML format.|

