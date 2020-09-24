# Ship data to Splunk by using the Splunk add-on for Log Service

This topic describes how to use the Splunk add-on for Log Service to send log data from Log Service to Splunk.

## Implementation

The following list describes how the Splunk add-on ships log data:

-   Create consumer groups by using Splunk data inputs and use the consumer groups to consume log data from Log Service in real time.
-   Splunk forwarders forward the log data to Splunk indexers by using the Splunk private protocol or HTTP Event Collector \(HEC\).

**Note:** The Splunk add-on is used only to collect log data. You must install the add-on on Splunk heavy forwarders. However, you do not need to install the add-on on Splunk indexers or search heads.

![Splunk-001](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2031649951/p95069.png)

## Mechanism

![Splunk-002](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3031649951/p95070.png)

-   A data input is a consumer that consumes log data.
-   A consumer group consists of multiple consumers. Each consumer in a consumer group consumes different data from a Logstore.
-   Each Logstore has multiple shards.
    -   Each shard can be allocated to only one consumer.
    -   Each consumer can consume data from multiple shards.
-   The name of a consumer consists of the name of the consumer group to which the consumer belongs, the hostname, the process name, and the type of the protocol used to send Splunk events. This naming convention ensures that each consumer name in a consumer group is unique.

For more information, see [Use consumer groups to consume log data](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Consumption by consumer groups/Use consumer groups to consume log data.md).

## Before you begin

-   Obtain an AccessKey pair that is used to access Log Service.

    You can use the AccessKey pair of a RAM user to access a Log Service project. For more information, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md) or [Configure an AccessKey pair for a RAM user to access a source Logstore and a destination Logstore](/intl.en-US/Data Transformation/Authorize Log Service/Configure an AccessKey pair for a RAM user to access a source Logstore and a destination
         Logstore.md).

    You can use the permission assistant feature to grant permissions to a RAM user. For more information, see [Use the permission assistant to grant permissions](/intl.en-US/Developer Guide/Access control RAM/Use the permission assistant to grant permissions.md). The following example shows the common permission policy configured for a RAM user.

    **Note:** <Project name\> specifies the name of the target project in Log Service. <Logstore name\> specifies the name of the target Logstore. Replace the values based on your business scenarios. You can use the wildcard character \(\*\) to specify multiple projects and Logstores.

    ```
    {
      "Version": "1",
      "Statement": [
        {
          "Action": [
            "log:ListShards",
            "log:GetCursorOrData",
            "log:GetConsumerGroupCheckPoint",
            "log:UpdateConsumerGroup",
            "log:ConsumerGroupHeartBeat",
            "log:ConsumerGroupUpdateCheckPoint",
            "log:ListConsumerGroup",
            "log:CreateConsumerGroup"
          ],
          "Resource": [
            "acs:log:*:*:project/<Project name>/logstore/<Logstore name>",
            "acs:log:*:*:project/<Project name>/logstore/<Logstore name>/*"
          ],
          "Effect": "Allow"
        }
      ]
    }
    ```

-   Check the version of Splunk and the operating system on which Splunk runs.
    -   Make sure the latest version of the add-on is used.
    -   Make sure that the operating system is Linux, macOS, or Windows.
    -   Make sure that the version of Splunk heavy forwarders is 8.0 or later and the version of Splunk indexers is 7.0 or later.
-   Configure HEC on Splunk. For more information, see [Configure HTTP Event Collector on Splunk Enterprise](https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/UsetheHTTPEventCollector#Configure_HTTP_Event_Collector_on_Splunk_Enterprise).

    If you use HEC to send events to Splunk indexers, make sure that HEC is configured on Splunk. If you use the Splunk private protocol to send events to Splunk indexers, skip this step.

    **Note:** You must create one or more Event Collector tokens before you can use HEC. The indexer acknowledgment feature cannot be enabled when you create an Event Collector token.


## Install the Splunk add-on

You can log on to the Splunk web interface and use one of the following methods to install the add-on:

**Note:** The Splunk add-on is used only to collect log data. You must install the add-on on Splunk heavy forwarders. However, you do not need to install the add-on on Splunk indexers or search heads.

-   Method 1
    1.  Click the ![Splunk-004](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3031649951/p95176.png) icon.
    2.  On the Apps page, click **Find More Apps**.
    3.  On the Browse More Apps page, search for **Alibaba Cloud Log Service Add-on for Splunk**, and click **Install**.
    4.  After the installation is complete, restart Splunk as prompted.
-   Method 2
    1.  Click the ![Splunk-004](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3031649951/p95176.png) icon.
    2.  On the Apps page, click **Install app from file**.
    3.  On the Upload app page, select the target .tgz file from your local host, and click **Upload**.

        You can click [App Search Results](https://splunkbase.splunk.com/app/4934/) and download the target .tgz file on the Alibaba Cloud Log Service Add-on for Splunk page.

    4.  Click **Install**.
    5.  After the installation is complete, restart Splunk as prompted.

## Configure the Splunk add-on

1.  On the Splunk web interface, click Alibaba Cloud Log Service Add-on for Splunk.

2.  Configure an account.

    On the page that appears, click **Configuration**. On the Configuration page, click the Account tab. On this tab, click Add. In the Add Account dialog box, configure an AccessKey pair that you use to access Log Service.

    **Note:** You must enter an AccessKey ID in the Username field and the corresponding AccessKey secret in the Password field.

3.  Configure the severity level of Splunk add-on logs.

    Click **Configuration**. On the Configuration page, click the Logging tab. On this tab, select a severity level from the Log level drop-down list.

4.  Create a data input.

    1.  Click **inputs**. On the Inputs page,

    2.  click **Create New Input**. In the Add sls\_datainput dialog box, configure the parameters of the data input.

        |Parameter|Required|Description|Example values|
        |---------|--------|-----------|--------------|
        |Name|Yes|The unique name of the data input. Data type: string.|N/A|
        |Interval|Yes|The interval that the data input restarts after exit. Unit: seconds. Data type: integer.|Default value: 10.|
        |Index|Yes|The Splunk index. Data type: string.|N/A|
        |SLS AccessKey|Yes|The Alibaba Cloud AccessKey pair that consists of an AccessKey ID and an AccessKey secret. Data type: string. **Note:** You must enter an AccessKey ID in the Username field and the corresponding AccessKey secret in the Password field.

|The AccessKey pair that you enter when you configure an account for the data input.|
        |SLS endpoint|Yes|The endpoint of Log Service. Data type: string. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).|        -   cn-huhehaote.log.aliyuncs.com
        -   https://cn-huhehaote.log.aliyuncs.com |
        |SLS project|Yes|The name of a Log Service project. Data type: string. For more information, see [Manage a project](/intl.en-US/Data Collection/Preparation/Manage a project.md).|N/A|
        |SLS logstore|Yes|The name of a Log Service Logstore. Data type: string. For more information, see [Manage a Logstore](/intl.en-US/Data Collection/Preparation/Manage a Logstore.md).|N/A|
        |SLS consumer group|Yes|The name of a consumer group. Data type: string. If you want to use multiple data inputs to consume data from the same Logstore, you must configure the same consumer group name for the data inputs. For more information, see [Use consumer groups to consume log data](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Consumption by consumer groups/Use consumer groups to consume log data.md).|N/A|
        |SLS cursor start time|Yes|The start time of log data consumption. Data type: string. This parameter is valid only when the first consumer group is created. Then, data is consumed from the last checkpoint. **Note:** The start time is the log receiving time.

|Valid values: begin, end, and a time in the ISO 8601 format \(for example, 2018-12-26 0:0:0+8:00\).|
        |SLS heartbeat interval|Yes|The heartbeat interval between the consumer and the server. Data type: integer. Unit: seconds.|Default value: 60.|
        |SLS data fetch interval|Yes|The interval at which logs are pulled from Log Service. Data type: integer. If the log receiving rate is low, we recommend that you do not set this parameter to a small value. Unit: seconds.|Default value: 1.|
        |Topic filter|No|Filters out log data by topic. The semicolon \(;\) is used to separate multiple topics. Data type: string. If a log entry is matched, it is not sent to Splunk.|TopicA;TopicB. This value indicates that log entries with the topic TopicA or TopicB are dropped.|
        |Unfolded fields|No|Maps the fields in a log entry to the topic of the log entry in the format of \{" topicA": \["field\_nameA1", "field\_nameA2", ...\], "topicB": \["field\_nameB1", "field\_nameB2", ...\], ...\}|\{"actiontrail\_audit\_event": \["event"\] \}. This value indicates that the event field is mapped to the log topic actiontrail\_audit\_event in the JSON format.|
        |Event source|No|The source of Splunk events. Data type: string.|N/A|
        |Event source type|No|The type of the Splunk event data source. Data type: string.|N/A|
        |Event retry times|No|The number of retries to consume data. Data type: integer.|Default value: 0. This value indicates unlimited retries.|
        |Event protocol|Yes|The protocol used to send Splunk events to a Splunk indexer. If you use the Splunk private protocol to send Splunk events, you do not need to specify the following parameters in the table.|        -   HTTP for HEC
        -   HTTPS for HEC
        -   Private protocol |
        |HEC host|Yes|The HEC host. This parameter is valid only when you use HEC to send Splunk events. Data type: string. For more information, see [Set up and use HTTP Event Collector in Splunk Web](https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/UsetheHTTPEventCollector).|N/A|
        |HEC port|Yes|The HEC port. This parameter is valid only when you use HEC to send Splunk events. Data type: integer.|N/A|
        |HEC token|Yes|The HEC token. This parameter is valid only when you use HEC to send Splunk events. Data type: string. For more information, see [HEC token](https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/UsetheHTTPEventCollector#About_Event_Collector_tokens).|N/A|
        |HEC timeout|Yes|The HEC timeout period. This parameter is valid only when you use HEC to send Splunk events. Data type: integer. Unit: seconds.|Default value: 120.|


## Operations

-   Query data

    Make sure that the data input is in the Enabled state. On the Splunk web interface, click Search & Reporting. On the App: Search & Reporting page, query audit logs that are sent to Splunk.

    ![Splunk-003](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3031649951/p95102.png)

-   Query Log Service operational logs
    -   Enter `index="_internal" | search "SLS info"` in the search bar to query Log Service INFO logs.
    -   Enter `index="_internal" | search "error` in the search bar to query Log Service ERROR logs.

## Performance and security

-   Performance

    The performance of the add-on and data transmission bandwidth depend on the following factors:

    -   Endpoint: You can access Log Service by using an endpoint of the public network, classic network, virtual private clouds \(VPC\), or the global acceleration-based public network. In most cases, we recommend that you use a classic network endpoint or a VPC endpoint. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).
    -   Bandwidth: the bandwidth of data transmission between Log Service and Splunk heavy forwarders and between Splunk heavy forwarders and indexers.
    -   Processing capability of Splunk indexers: the capabilities of indexers to receive data from Splunk heavy forwarders.
    -   Number of shards: A higher number of shards in a Logstore indicates a higher data transmission capability. You must decide the number of shards in a Logstore based on the receiving rate of raw logs. For more information, see [Manage shards](/intl.en-US/Data Collection/Preparation/Manage shards.md).
    -   Number of Splunk data inputs: A higher number of data inputs in a consumer group that is configured for a Logstore indicates a higher throughput.

        **Note:** The number of shards in a Logstore affects the concurrent consumption of the Logstore.

    -   Number of CPU cores and memory resources occupied by Splunk heavy forwarders: In most cases, one Splunk data input consumes 1 GB to 2 GB of memory resources and 1 CPU core.
    If sufficient memory and CPU resources are allocated, one Splunk data input can consume log data at a rate of 1 MB to 2 MB per second.

    For example, if logs are received in a Logstore at a rate of 10 MB per second, you must create at least 10 shards in the Logstore and configure 10 data inputs in the Splunk add-on. If you deploy the Splunk add-on on a single server, the server must have 10 idle CPU cores and 12 GB of available memory resources.

-   High availability

    A consumer group stores checkpoints on the server. When a consumer stops consuming data, another consumer continues to consume data from the last checkpoint. You can create Splunk data inputs on multiple servers. If a server stops running or is damaged, a Splunk data input on another server continues to consume data from the last checkpoint. You can also launch more Splunk data inputs than the number of shards on multiple servers. This allows data to be consumed from the last checkpoint if an exception occurs.

-   HTTPS-based data transmission
    -   Log Service

        To use HTTPS to encrypt the data transmitted between your program and Log Service, you must set the prefix of the endpoint to https://, for example, https://cn-beijing.log.aliyuncs.com.

        The server certificate \*.aliyuncs.com is issued by GlobalSign. By default, most Linux and Windows servers are preconfigured to trust this certificate. If the server does not trust this certificate, see [Install a trusted root CA or self-signed certificate](https://success.outsystems.com/Support/Enterprise_Customers/Installation/Install_a_trusted_root_CA__or_self-signed_certificate).

    -   Splunk

        To use HTTPS-based HEC, you must enable the SSL feature when you enable HEC in the Global Settings dialog box. For more information, see [Configure HTTP Event Collector on Splunk Enterprise](https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/UsetheHTTPEventCollector#Configure_HTTP_Event_Collector_on_Splunk_Enterprise).

-   AccessKey pair protection

    The AccessKey pair that you use to access Log Service and HEC tokens are encrypted and stored in Splunk.


## FAQ

-   What can I do if a configuration error occurs?
    -   Check the configurations of the data inputs. For information about configuration parameters, see [Table 1](#table_3m7_ks5_n9x).
    -   Check the configurations of Log Service. Example error: failed to create a consumer group.
        -   Command: `index="_internal" | search "error"`
        -   Exception logs:

            ```
            aliyun.log.consumer.exceptions.ClientWorkerException: 
            error occour when create consumer group, 
            errorCode: LogStoreNotExist, 
            errorMessage: logstore xxxx does not exist
            ```

        -   Check whether the number of consumer groups configured for a Logstore exceeds the quota.

            You can configure a maximum of 20 consumer groups for a Logstore. We recommend that you delete unnecessary consumer groups. If more than 20 consumer groups are configured for a Logstore, the ConsumerGroupQuotaExceed error is returned.

-   What do I do if a permission error occurs?
    -   Check whether you are authorized to access Log Service.
        -   Command: `index="_internal" | search "error"`
        -   Exception logs:

            ```
            aliyun.log.consumer.exceptions.ClientWorkerException: 
            error occour when create consumer group, 
            errorCode: SignatureNotMatch, 
            errorMessage: signature J70VwxYH0+W/AciA4BdkuWxK6W8= not match
            ```

    -   Check whether you are authorized to access HEC.
        -   Command: `index="_internal" | search "error"`
        -   Exception logs:

            ```
            ERROR HttpInputDataHandler - Failed processing http input, token name=n/a, channel=n/a, source_IP=127.0.0.1, reply=4, events_processed=0, http_input_body_size=369
            
            WARNING pid=48412 tid=ThreadPoolExecutor-0_1 file=base_modinput.py:log_warning:302 | 
            SLS info: Failed to write [{"event": "{\"__topic__\": \"topic_test0\", \"__source__\": \"127.0.0.1\", \"__tag__:__client_ip__\": \"10.10.10.10\", \"__tag__:__receive_time__\": \"1584945639\", \"content\": \"goroutine id [0, 1584945637]\", \"content2\": \"num[9], time[2020-03-23 14:40:37|1584945637]\"}", "index": "main", "source": "sls log", "sourcetype": "http of hec", "time": "1584945637"}] remote Splunk server (http://127.0.0.1:8088/services/collector) using hec. 
            Exception: 403 Client Error: Forbidden for url: http://127.0.0.1:8088/services/collector, times: 3
            ```

        -   Possible causes
            -   HEC is not configured or started.
            -   The HEC-relevant parameters of data inputs are invalid. For example, if you use HTTPS-based HEC, you must enable the SSL feature.
            -   The indexer acknowledgment feature is disabled.
-   What do I do if a consumption delay occurs?

    You can view the status of consumer groups in the Log Service console. For more information, see [View consumer group status](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Consumption by consumer groups/Use consumer groups to consume log data.md).

    Increase the number of shards in the Logstore or create more data inputs in the same consumer group. For more information, see [Performance and security](#section_0p1_mj7_a9l).

-   What do I do if network jitters occur?

    -   Command: `index="_internal" | search "SLS info: Failed to write"`
    -   Exception logs:

        ```
        WARNING pid=58837 tid=ThreadPoolExecutor-0_0 file=base_modinput.py:log_warning:302 |
        SLS info: Failed to write [{"event": "{\"__topic__\": \"topic_test0\", \"__source__\": \"127.0.0.1\", \"__tag__:__client_ip__\": \"10.10.10.10\", \"__tag__:__receive_time__\": \"1584951417\", \"content2\": \"num[999], time[2020-03-23 16:16:57|1584951417]\", \"content\": \"goroutine id [0, 1584951315]\"}", "index": "main", "source": "sls log", "sourcetype": "http of hec", "time": "1584951417"}] remote Splunk server (http://127.0.0.1:8088/services/collector) using hec. 
        Exception: ('Connection aborted.', ConnectionResetError(54, 'Connection reset by peer')), times: 3
        ```

    Splunk events are automatically retransmitted if network jitters occur. If the problem persists, contact your network administrator for troubleshooting.

-   Modify the start time of data consumption

    **Note:** The SLS cursor start time parameter is valid only when you create a consumer group for the first time. From the next time, data is consumed from the last checkpoint.

    1.  On the Input page of the Splunk Web UI, disable the target data input.
    2.  Log on to the [Log Service console](https://sls.console.aliyun.com). Find the Logstore from which data is consumed, and delete the consumer group under **Data Consumption**.
    3.  On the Input page of the Splunk Web UI, find the target data input, and choose **Actions** \> **Edit**. In the dialog box that appears, modify the SLS cursor start time parameter. Restart the data input.

