# Create an IP address-based machine group

Log Service allows you to create an IP address-based machine group. This topic describes how to create an IP address-based machine group in the Log Service console.

-   A project and a Logstore are created. For more information, see [Create a project and a Logstore](/intl.en-US/Quick Start/Quick start.md).
-   At least one server is available.
    -   If you use an ECS instance, you must ensure that the ECS instance and Log Service project share the same Alibaba Cloud account and belong to the same region.
    -   If the ECS instance is purchased by another Alibaba Cloud account, you must configure a user identifier before you create an IP address-based machine group. If the server is provided by another cloud service vendor or is deployed in a user-created data center, you must also perform the preceding operation. For more information, see [Configure an account ID for a server](/intl.en-US/Data Collection/Logtail collection/Machine Group/Configure an account ID for a server.md).
-   Logtail is installed on the server. For more information, see [Install Logtail on ECS instances](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail on ECS instances.md).

1.  Obtain the IP addresses of the servers.

    The IP addresses obtained by Logtail are recorded in the ip field of the app\_info.json file.

    On the servers where Logtail is installed, you can go to the following path to check the app\_info.json file:

    -   Linux: /usr/local/ilogtail/app\_info.json
    -   64-bit Windows: C:\\Program Files \(x86\)\\Alibaba\\Logtail\\app\_info.json
    -   32-bit Windows: C:\\Program Files\\Alibaba\\Logtail\\app\_info.json
    The following figure shows the IP address of the server in Linux.

    ![IP addresses](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8696549951/p10497.png)

2.  Log on to the [Log Service console](https://sls.console.aliyun.com).

3.  In the Projects section, click the destination project.

4.  In the left-side navigation pane, click **Machine Groups**.

5.  On the right of the **Machine Groups**, choose **![Machine groups](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2123359951/p52484.png)** \> **Create Machine Group**.

6.  In the Create Machine Group dialog box, set the required parameters, and then click **OK**. The following table describes the parameters.

    |Parameter|Description|
    |---------|-----------|
    |Name|The name of the machine group. The name must be 3 to 128 characters in length, and can contain only lowercase letters, digits, hyphens \(-\), and underscores \(\_\). The name must start and end with a lowercase letter or digit. **Note:** After the machine group is created, you cannot change its name. Proceed with caution. |
    |Identifier|Select **IP Addresses**.|
    |Topic|The topic of the machine group. This topic is used to differentiate log data that is generated in different servers. For more information, see [Configure a log topic](/intl.en-US/Data Collection/Logtail collection/Text logs/Configure a log topic.md).|
    |IP addresses|Enter the IP addresses that are obtained in [Step 1](#ip). **Note:**

    -   If a machine group contains multiple servers, you must separate the IP addresses with line feeds.
    -   Windows and Linux servers cannot be added to the same machine group. |

7.  View the status of the machine group.

    1.  In the list of machine groups, click the destination machine group.

    2.  On the Machine Group Settings page, view the machine group status.

        If the **Heartbeat** status is **OK**, the server is connected to Log Service. If the status is **FAIL**, see [What can I do if the Logtail client has no heartbeat?]()

        ![View the machine group status](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8696549951/p97821.png)


