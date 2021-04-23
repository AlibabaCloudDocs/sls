# Ship data to Splunk by using the Splunk add-on for Log Service

This topic describes how to use the Splunk add-on for Log Service. The add-on can be used to send log data from Log Service to Splunk.

## Implementation

The following list describes how the Splunk add-on ships log data:

-   Create consumer groups by using Splunk data inputs and use the consumer groups to consume log data from Log Service in real time.
-   Splunk forwarders use the Splunk private protocol or HTTP Event Collector \(HEC\) to forward the log data to Splunk indexers.

**Note:** The Splunk add-on is used to collect only log data. You must install the add-on on Splunk heavy forwarders. However, you do not need to install the add-on on Splunk indexers or search heads.

![Splunk-001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2031649951/p95069.png)

## How it works

![Splunk-002](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3031649951/p95070.png)

-   A data input is a consumer that consumes log data.
-   A consumer group contains multiple consumers. Each consumer in a consumer group consumes different log entries that are stored in a Logstore.
-   Each Logstore contains multiple shards.
    -   Each shard can be allocated to only one consumer.
    -   Each consumer can consume data from multiple shards.
-   The name of a consumer contains important information. This includes the name of the consumer group to which the consumer belongs, the hostname, the process name, and the type of protocol that is used to send Splunk events. This naming convention ensures that each consumer name in a consumer group is unique.

For more information, see [Use consumer groups to consume log data](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Consumption by consumer groups/Use consumer groups to consume log data.md).

## Preparations

-   Obtain an AccessKey pair that is used to access Log Service.

    You can use the AccessKey pair of a Resource Access Management \(RAM\) user to access a Log Service project. For more information, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md) and [Configure an AccessKey pair for a RAM user to access a source Logstore and a destination Logstore](/intl.en-US/Data Transformation/Authorize Log Service/Configure an AccessKey pair for a RAM user to access a source Logstore and a destination
         Logstore.md).

    You can use the permission assistant feature to grant permissions to a RAM user. For more information, see [Use the permission assistant to grant permissions](/intl.en-US/Developer Guide/Access control RAM/Use the permission assistant to grant permissions.md). The following example shows a common permission policy that is configured for a RAM user.

    **Note:** <Project name\> specifies the name of the destination project in Log Service. <Logstore name\> specifies the name of the destination Logstore. Replace the values with the actual values. You can use the wildcard character \(\*\) to specify multiple projects and Logstores.

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

-   Check the version of Splunk and the operating system on which Splunk is run.
    -   Make sure the latest version of the add-on is used.
    -   Make sure that the operating system is Linux, macOS, or Windows.
    -   Make sure that the version of Splunk heavy forwarders is 8.0 or later and the version of Splunk indexers is 7.0 or later.
-   Configure HEC on Splunk. For more information, see [Configure HTTP Event Collector on Splunk Enterprise](https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/UsetheHTTPEventCollector#Configure_HTTP_Event_Collector_on_Splunk_Enterprise).

    If you use HEC to send events to Splunk indexers, make sure that HEC is configured on Splunk. If you use the Splunk private protocol to send events to Splunk indexers, skip this step.

    **Note:** Before you can use HEC, create one or more Event Collector tokens. The indexer acknowledgment feature cannot be enabled when you create an Event Collector token.


## Install the add-on

You can log on to the Splunk web interface and install the add-on. We recommend that you use one of the following methods.

**Note:** The Splunk add-on is used to collect only log data. You must install the add-on on Splunk heavy forwarders. However, you do not need to install the add-on on Splunk indexers or search heads.

-   Method 1
    1.  Click the **![Splunk-004](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3031649951/p95176.png)** icon.
    2.  On the Apps page, click **Find More Apps**.
    3.  On the Browse More Apps page, search for **Alibaba Cloud Log Service Add-on for Splunk**, and click **Install**.
    4.  After the add-on is installed, restart Splunk as prompted.
-   Method 2
    1.  Click the **![Splunk-004](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3031649951/p95176.png)** icon.
    2.  On the Apps page, click **Install app from file**.
    3.  On the Upload app page, select the destination TGZ file from your local host, and click **Upload**.

        You can click [App Search Results](https://splunkbase.splunk.com/app/4934/) and download the destination TGZ file on the Alibaba Cloud Log Service Add-on for Splunk page.

    4.  Click **Install**.
    5.  After the add-on is installed, restart Splunk as prompted.

## Configure the Splunk add-on

If Splunk is not running on an Elastic Compute Service \(ECS\) instance, you can use an AccessKey pair of your Alibaba Cloud to access Log Service. To configure the Splunk add-on, perform the following steps:

1.  On the Splunk web interface, click Alibaba Cloud Log Service Add-on for Splunk.

2.  Configure an account.

    On the page that appears, choose **Configuration** \> **Account**. On the Configuration page, click the Account tab. On this tab, click Add. In the Add Account dialog box, set an AccessKey pair that can be used to access Log Service.

    **Note:** You must enter an AccessKey ID in the Username field and the related AccessKey secret in the Password field.

3.  Set the severity level of Splunk add-on logs.

    Choose **Configuration** \> **Logging**. On the Configuration page, click the Logging tab. On this tab, select a severity level from the Log level drop-down list.

4.  Create a data input.

    1.  Click **inputs** to open the Inputs page.

    2.  Click **Create New Input**. In the Add sls\_datainput dialog box, set the parameters of the data input.

        |Parameter|Required|Description|Example value|
        |---------|--------|-----------|-------------|
        |Name|Yes|The unique name of the data input. Data type: string.|N/A|
        |Interval|Yes|The period of time that is required to restart the Splunk data input process after the process stops. Data type: integer.|Default value: 10.|
        |Indexing|Yes|The Splunk index. Data type: string.|N/A|
        |SLS AccessKey|Yes|The Alibaba Cloud AccessKey pair that consists of an AccessKey ID and an AccessKey secret. Data type: string. **Note:** You must enter an AccessKey ID in the Username field and the related AccessKey secret in the Password field.

|The AccessKey pair that you enter when you configure an account for the data input.|
        |SLS endpoint|Yes|The endpoint of Log Service. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).|        -   cn-huhehaote.log.aliyuncs.com
        -   https://cn-huhehaote.log.aliyuncs.com |
        |SLS project|Yes|The name of a Log Service project. For more information, see [Manage a project](/intl.en-US/Data Collection/Preparation/Manage a project.md).|N/A|
        |SLS logstore|Yes|The name of a Log Service Logstore. For more information, see [Manage a Logstore](/intl.en-US/Data Collection/Preparation/Manage a Logstore.md).|N/A|
        |SLS consumer group|Yes|The name of a consumer group. Data type: string. If you want to use multiple data inputs to consume data that is stored in the same Logstore, you must specify the same consumer group name for the data inputs. For more information, see [Use consumer groups to consume log data](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Consumption by consumer groups/Use consumer groups to consume log data.md).|N/A|
        |SLS cursor start time|Yes|The start time of log data consumption. Data type: string. This parameter is valid only when the first consumer group is created. For the next time, data is consumed from the last checkpoint. **Note:** The start time is the log receiving time.

|Valid values: begin, end, and a time in the ISO 8601 format, for example, 2018-12-26 0:0:0+8:00.|
        |SLS heartbeat interval|Yes|The interval at which a heartbeat is sent between the consumer and the server. Data type: integer. Unit: seconds.|Default value: 60.|
        |SLS data fetch interval|Yes|The interval at which logs are pulled from Log Service. Data type: integer. If the speed at which logs are received is low, we recommend that you do not set this parameter to a small value. Unit: seconds.|Default value: 1.|
        |Topic filter|No|Filters log data by topic. Separate multiple topics with semicolons \(;\). Data type: string. If a log entry is matched, it is not sent to Splunk.|TopicA;TopicB. This value indicates that log entries whose topic is TopicA or TopicB are dropped.|
        |Unfolded fields|No|Maps the fields in a log entry to the topic of the log entry in the format of \{"topicA": \["field\_nameA1", "field\_nameA2", ...\], "topicB": \["field\_nameB1", "field\_nameB2", ...\], ...\}|\{"actiontrail\_audit\_event": \["event"\] \}. This value indicates that the event field is mapped to the log topic actiontrail\_audit\_event in the JSON format.|
        |Event source|No|The source of Splunk events. Data type: string.|N/A|
        |Event source type|No|The type of the Splunk event data source. Data type: string.|N/A|
        |Event retry times|No|The number of retries to consume data. Data type: integer.|Default value: 0. This value indicates unlimited retries.|
        |Event protocol|Yes|The protocol that is used to send Splunk events to a Splunk indexer. If you use the Splunk private protocol to send Splunk events, you do not need to set the following parameters in the table.|        -   HTTP for HEC
        -   HTTPS for HEC
        -   Private protocol |
        |HEC host|Yes|The HEC host. This parameter is valid only if you use HEC to send Splunk events. Data type: string. For more information, see [Set up and use HTTP Event Collector in Splunk Web](https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/UsetheHTTPEventCollector).|N/A|
        |HEC port|Yes|The HEC port. This parameter is valid only if you use HEC to send Splunk events. Data type: integer.|N/A|
        |HEC token|Yes|The HEC token. This parameter is valid only if you use HEC to send Splunk events. Data type: string. For more information, see [HEC token](https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/UsetheHTTPEventCollector#About_Event_Collector_tokens).|N/A|
        |HEC timeout|Yes|The HEC timeout period. This parameter is valid only if you use HEC to send Splunk events. Data type: integer. Unit: seconds.|Default value: 120.|


If you run Splunk on an ECS instance, you can assign a RAM role to the ECS instance. Then, the ECS instance can assume the RAM role to access Log Service. To configure the Splunk add-on, perform the following steps:

**Note:** You must make sure that Splunk is run on the ECS instance to which a RAM role is assigned.

1.  Assign a RAM role to an ECS instance.

    In this step, you must first create a RAM role. Then, you must grant permissions to the RAM role, and assign the RAM role to an ECS instance. For more information, see [Bind an instance RAM role](/intl.en-US/Security/Instance RAM roles/Bind an instance RAM role.md).

    For more information about the permission policy of the RAM role, see the permission policy in [Preparations](#section_l84_pj5_oiy).

2.  On the Splunk web interface, click Alibaba Cloud Log Service Add-on for Splunk.

3.  Configure an account.

    On the page that appears, choose **Configuration** \> **Account**. On the Configuration page, click the Account tab. On this tab, click Add. In the Add Account dialog box, configure the RAM role of the ECS instance. To add an account, enter ECS\_RAM\_ROLE in the Username field, and enter the name of the RAM role that is created in [Step 1](#step_zxt_z2e_vgv) in the Password field.

4.  Create a data input.

    1.  Click **inputs** to open the Inputs page.

    2.  Click **Create New Input**. In the Add sls\_datainput dialog box, set the parameters of the data input.

        The SLS AccessKey parameter must be set to the account created in [Step 3](#step_9e4_i89_n8h). For more information about other parameters, see[Table 1](#table_3m7_ks5_n9x).


## Operations

-   Query data

    Make sure that the data input is in the Enabled state. On the Splunk web interface, click Search & Reporting. On the App: Search & Reporting page, query audit logs that are sent to Splunk.

    ![Splunk-003](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3031649951/p95102.png)

-   Query Log Service operational logs
    -   Enter `index="_internal" | search "SLS info"` in the search bar to query Log Service INFO logs.
    -   Enter `index="_internal" | search "error"` in the search bar to query Log Service ERROR logs.

## Performance and security

-   Performance

    The performance of the add-on and data transmission bandwidth changes based on the following factors:

    -   Endpoint: You can access Log Service by using an endpoint of the Internet, classic network, virtual private clouds \(VPCs\), or the global acceleration-based Internet. In most cases, we recommend that you use a classic network endpoint or a VPC endpoint. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).
    -   Bandwidth: the bandwidth of data transmission between Log Service and Splunk heavy forwarders and between Splunk heavy forwarders and indexers.
    -   Processing capability of Splunk indexers: the capabilities of indexers to receive data from Splunk heavy forwarders.
    -   Number of shards: A higher number of shards in a Logstore indicates a higher data transmission capability. You must check the number of shards based on the speed at which raw logs are generated. For more information, see [Manage shards](/intl.en-US/Data Collection/Preparation/Manage shards.md).
    -   Number of Splunk data inputs: A higher number of data inputs in a consumer group that is configured for a Logstore indicates a higher throughput.

        **Note:** The number of shards in a Logstore affects the concurrent consumption of the Logstore.

    -   Number of CPU cores and memory resources occupied by Splunk heavy forwarders: In most cases, one Splunk data input consumes 1 GB to 2 GB of memory resources and 1 CPU core.
    If sufficient memory and CPU resources are allocated, one Splunk data input can consume log data at a speed of 1 MB to 2 MB per second. You must check the number of shards based on the speed at which raw logs are generated.

    For example, if logs are received in a Logstore at a speed of 10 MB per second, you must create at least 10 shards in the Logstore and configure 10 data inputs in the Splunk add-on. If you deploy the Splunk add-on on a single server, the server must have 10 idle CPU cores and 12 GB of available memory resources.

-   High availability

    A consumer group stores checkpoints on the server. When a consumer stops consuming data, another consumer continues to consume data from the last checkpoint. You can create Splunk data inputs on multiple servers. If a server stops running or is damaged, a Splunk data input on another server continues to consume data from the last checkpoint. You can also launch more Splunk data inputs than the number of shards on multiple servers. This allows data to be consumed from the last checkpoint if an exception occurs.

-   HTTPS-based data transmission
    -   Log Service

        To use HTTPS to encrypt the data that is transmitted between your program and Log Service, make sure that the endpoint is prefixed by https://, for example, https://cn-beijing.log.aliyuncs.com.

        The server certificate \*.aliyuncs.com is issued by GlobalSign. By default, most Linux and Windows servers are preconfigured to trust this certificate. If the server does not trust this certificate, see [Install a trusted root CA or self-signed certificate](https://success.outsystems.com/Support/Enterprise_Customers/Installation/Install_a_trusted_root_CA__or_self-signed_certificate).

    -   Splunk

        To use HTTPS-based HEC, enable the SSL feature when you enable HEC in the Global Settings dialog box. For more information, see [Configure HTTP Event Collector on Splunk Enterprise](https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/UsetheHTTPEventCollector#Configure_HTTP_Event_Collector_on_Splunk_Enterprise).

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

        -   Check whether the number of consumer groups that are configured for a Logstore exceeds the quota.

            You can configure a maximum of 20 consumer groups for a Logstore. We recommend that you delete the consumer groups that are no longer required. If more than 20 consumer groups are configured for a Logstore, the ConsumerGroupQuotaExceed error is returned.

-   What can I do if a permission error occurs?
    -   Check whether you are authorized to access Log Service.
        -   Command: `index="_internal" | search "error"`
        -   Exception logs:

            ```
            aliyun.log.consumer.exceptions.ClientWorkerException: 
            error occour when create consumer group, 
            errorCode: SignatureNotMatch, 
            errorMessage: signature J70VwxYH0+W/AciA4BdkuWxK6W8= not match
            ```

    -   Check whether the RAM role of your ECS instance is authorized.
        -   Command: `index="_internal" | search "error"`
        -   Exception logs:

            ```
            ECS RAM Role detected in user config, but failed to get ECS RAM credentials. Please check if ECS instance and RAM role 'ECS-Role' are configured appropriately.
            ```

            ECS-Role is the RAM role that you create. The ECS-Role variable is displayed as the actual value.

        -   Possible causes:
            -   Check whether the SLS AccessKey parameter of Data Input is configured as the account that has a RAM role.
            -   Check whether the RAM role is properly configured for the account. The Username parameter must be set to ECS\_RAM\_ROLE and the Password parameter must be set to the name of the RAM role.
            -   Check whether the RAM role is assigned to the ECS instance.
            -   Check whether the trusted entity type of the RAM role is set to Alibaba Cloud Service. Check whether the selected trusted service is ECS.
            -   Check whether the ECS instance to which the RAM role is assigned is the ECS instance on which you run Splunk.
    -   Check whether you are authorized to access HEC.
        -   Command: `index="_internal" | search "error"`
        -   Exception logs:

            ```
            ERROR HttpInputDataHandler - Failed processing http input, token name=n/a, channel=n/a, source_IP=127.0.0.1, reply=4, events_processed=0, http_input_body_size=369
            
            WARNING pid=48412 tid=ThreadPoolExecutor-0_1 file=base_modinput.py:log_warning:302 | 
            SLS info: Failed to write [{"event": "{\"__topic__\": \"topic_test0\", \"__source__\": \"127.0.0.1\", \"__tag__:__client_ip__\": \"10.10.10.10\", \"__tag__:__receive_time__\": \"1584945639\", \"content\": \"goroutine id [0, 1584945637]\", \"content2\": \"num[9], time[2020-03-23 14:40:37|1584945637]\"}", "index": "main", "source": "sls log", "sourcetype": "http of hec", "time": "1584945637"}] remote Splunk server (http://127.0.0.1:8088/services/collector) using hec. 
            Exception: 403 Client Error: Forbidden for url: http://127.0.0.1:8088/services/collector, times: 3
            ```

        -   Possible causes:
            -   HEC is not configured or started.
            -   The HEC-related parameters of data inputs are invalid. For example, if you use HTTPS-based HEC, you must enable the SSL feature.
            -   The indexer acknowledgment feature is disabled.
-   What can I do if a consumption delay occurs?

    You can view the status of a consumer group in the Log Service console. For more information, see [View consumer group status](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Consumption by consumer groups/Use consumer groups to consume log data.md).

    Increase the number of shards in the Logstore or create more data inputs in the same consumer group. For more information, see [Performance and security](#section_0p1_mj7_a9l).

-   What can I do if network jitter occurs?

    -   Command: `index="_internal" | search "SLS info: Failed to write"`
    -   Exception logs:

        ```
        WARNING pid=58837 tid=ThreadPoolExecutor-0_0 file=base_modinput.py:log_warning:302 |
        SLS info: Failed to write [{"event": "{\"__topic__\": \"topic_test0\", \"__source__\": \"127.0.0.1\", \"__tag__:__client_ip__\": \"10.10.10.10\", \"__tag__:__receive_time__\": \"1584951417\", \"content2\": \"num[999], time[2020-03-23 16:16:57|1584951417]\", \"content\": \"goroutine id [0, 1584951315]\"}", "index": "main", "source": "sls log", "sourcetype": "http of hec", "time": "1584951417"}] remote Splunk server (http://127.0.0.1:8088/services/collector) using hec. 
        Exception: ('Connection aborted.', ConnectionResetError(54, 'Connection reset by peer')), times: 3
        ```

    Splunk events are automatically retransmitted if network jitter occurs. If the issue persists, contact your network administrator.

-   How do I modify the start time of consumption?

    **Note:** The SLS cursor start time parameter is valid only when a consumer group is created for the first time. For the next time, data is consumed from the last checkpoint.

    1.  On the Input page of the Splunk web interface, disable the related data input.
    2.  Log on to the [Log Service console](https://sls.console.aliyun.com). Find the Logstore from which data is consumed, and delete the related consumer group in the **Data Consumption** section.
    3.  On the Input page of the Splunk web interface, find the data input, and choose **Actions** \> **Edit**. In the dialog box that appears, modify the SLS cursor start time parameter. Then, restart the data input.

