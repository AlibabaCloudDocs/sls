# Create an IP address-based machine group

This topic describes how to create an IP address-based machine group in the Log Service console.

-   A project and a Logstore are created. For more information, see [Create a Logtail project and a Logstore](/intl.en-US/Quick Start/Quick start.md).
-   At least one server is available.
    -   If Logtail is installed on an ECS instance, you must ensure that the ECS instance and Log Service project share the same Alibaba Cloud account and belong to the same region.
    -   If Logtail is installed on an ECS instance purchased by another Alibaba Cloud account, you must configure a user identifier before you create an IP address-based machine group. If Logtail is installed on the server that is provided by another cloud service vendor, or is a local IDC, you must also perform the preceding operation. For more information, see [Configure an account ID for a server](/intl.en-US/Data Collection/Logtail collection/Machine Group/Configure an account ID for a server.md).
-   Logtail is installed on the server. For more information, see [Install Logtail in Linux](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md) and [Install Logtail in Windows](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Windows.md).

1.  Obtain the IP addresses of the servers.

    The IP addresses obtained by Logtail are recorded in the ip field of the app\_info.json file.

    On the servers where Logtail is installed, you can go to each of the following path to check the app\_info.json file:

    -   Linux: /usr/local/ilogtail/app\_info.json
    -   64-bit Windows: C:\\Program Files \(x86\)\\Alibaba\\Logtail\\app\_info.json
    -   32-bit Windows: C:\\Program Files\\Alibaba\\Logtail\\app\_info.json
    The following figure shows the IP address of the server in Linux.

    ![IP addresses](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8696549951/p10497.png)

2.  Log on to the [Log Service console](https://sls.console.aliyun.com).

3.  In the Projects section, click the name of the target project.

4.  In the left-side navigation pane, click **Machine Groups**.

5.  Click the ![Machine Groups](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2123359951/p52484.png) icon next to **Machine Groups**, and select **Create Machine Group** from the shortcut menu.

6.  In the Create Machine Group dialog box, set the following required parameters, and then click **OK**.

    |Parameter|Description|
    |---------|-----------|
    |Name|The name of the machine group. It must be 3 to 128 characters in length, and can contain only lowercase letters, digits, hyphens \(-\), and underscores \(\_\). The name must start and end with a lowercase letter or digit. **Note:** After the machine group is created, you cannot change its name. Proceed with caution. |
    |Identifier|Select **IP Addresses**.|
    |Topic|The topic of the machine group. The topic is used to differentiate log data that is generated in different servers. For more information, see [Configure a log topic](/intl.en-US/Data Collection/Logtail collection/Text logs/Configure a log topic.md).|
    |IP addresses|Enter the IP addresses that are obtained in [step 1](#ip). **Note:**

    -   If a machine group contains multiple servers, you must separate the IP addresses with line breaks.
    -   Windows and Linux servers are not allowed in the same machine group. |

7.  View the status of the machine group.

    1.  In the Machine Groups list, click the name of the target machine group.

    2.  In Machine Group Settings page, you can view the machine group status.

        If the **heartbeat** status is **OK**, the server is connected to Logtail. If the status is **FAIL**, see [What can I do if the Logtail client has no heartbeat?]().

        ![View the machine group status](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8696549951/p97821.png)


