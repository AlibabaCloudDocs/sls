# Import trace data from SkyWalking

This topic describes how to forward trace data from SkyWalking to Log Service by using Logtail.

A trace instance is created. For more information, see [Create a trace instance]().

## Limits

Only the GRPC protocol of SkyWalking V3 is supported. The release version of SkyWalking is 8.0.0 and later.

## Step 1: Configure data import

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Import Data section, select **SkyWalking**.

3.  Select the Project and the $\{instance\}-Traces Logstore where your trace instance resides.

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

    **Note:** If you want to apply a machine group immediately after it is created, the heartbeat status of the machine group may be **FAIL**. This issue occurs because the machine group has not been connected to Log Service. In this case, you can click **Automatic Retry**. If the issue persists, see [What can I do if the Logtail client has no heartbeat?](/intl.en-US/Data Collection/FAQ/Troubleshooting for Logtail machine group without heartbeat in the log Service Console.md)

6.  In the Specify Data Source step, add the following parameters and click **Next**.

    $\{instance\} is the ID of your trace instance. We recommend that you replace this parameter based on your actual scenario.

    **Note:** If your Logtail local port 11800 is occupied, you can replace it with another available port.

    ```
    {
        "inputs" : [
            {
                "detail" : {
                    "Address" : "0.0.0.0:11800"
                },
                "type" : "service_skywalking_agent_v3"
            }
        ],
        "aggregators" : [
            {
                "detail" : {
                    "MetricsLogstore" : "$\{instance\}-metrics",
                    "TraceLogstore" : "$\{instance\}-traces"
                },
                "type" : "aggregator_skywalking"
            }
        ],
        "global" : {
            "AlwaysOnline" : true,
            "DelayStopSec" : 300
        }
    }
    ```

7.  Preview the data, configure indexes, and then click **Next**.

    By default, Log Service enables the Full Text Index to query and analyze logs. You can also manually or automatically configure Field Search. For more information, see [Configure indexes](/intl.en-US/Index and query/Configure indexes.md).

    **Note:**

    -   To query and analyze logs, you must enable the Full Text Index or Field Search. If you configure both of them, the settings of Field Search prevail.
    -   If the data type of the index is long or double, the case sensitive and delimiter settings are unavailable.

## Step 2: Configure the SkyWalking client

Configure the SkyWalking client to send data to the address of the Logtail listener. See the following details:

-   If you are using Java Agent, replace the value of the collector.backend\_service parameters. For more information, see [Agent configuration](https://github.com/apache/skywalking/blob/master/docs/en/setup/service-agent/java-agent/README.md).
-   If you are using .net core Agent, use the `dotnet skyapm config $\{service\}$\{endpoint\}` command to generate the configuration file. In this case, you must replace the $\{service\} variable with the actual service name. You must replace the $\{endpoint\} variable with the machine group IP address and the port number that is configured in Step [5](#step_fn9_9yq_89g). For more information, see [SkyAPM-donet](https://github.com/SkyAPM/SkyAPM-dotnet).
-   If you are using another agent or SDK to send data, replace the backend address with the machine group IP address and the port number that is configured in Step [5](#step_fn9_9yq_89g).

-   [View the details of a trace instance]()
-   [Query and analyze trace data]()

