# Enable the global acceleration feature

This topic describes how to enable the global acceleration feature for Log Service in the Dynamic Route for CDN \(DCDN\) console.

-   Log Service is activated. A project and a Logstore are created. For more information, see [Create a project and a Logstore](/intl.en-US/Quick Start/Quick start.md).
-   DCDN is activated. For more information, see [t13447.md\#]().

    DCDN provides the HTTP acceleration and HTTPS acceleration features. Before you enable the HTTPS acceleration feature, you must first enable the HTTP acceleration feature.


## Enable the HTTP acceleration feature

1.  Log on to the [DCDN console](https://dcdn.console.aliyun.com/).

2.  In the left-side navigation pane, click **Domain Names**.

3.  On the Domain Names page, click **Add Domain Name**.

4.  On the Add Domain Name page, set the required parameters. The following table describes the parameters.

    ![Domain Name to Accelerate](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5693525061/p8063.png)

    |Parameter|Description|
    |---------|:----------|
    |Domain Name to Accelerate|The domain name that is used to accelerate data transmission. The domain name is in the project\_name.log-global.aliyuncs.com format. Replace project\_name with your actual project name.|
    |Business Type|Select **DCDN**.|
    |Origin Information|Type|Select **Site Domain**.|
    |Domain name|Enter the public domain name of the project. For more information, see [/intl.en-US/Developer Guide/API Reference/Endpoints.md](/intl.en-US/Developer Guide/API Reference/Endpoints.md).|
    |Port|Select **Port 80**.|
    |Acceleration Region|    -   If you select **Mainland China Only** or **Global**, you must apply for an Internet Content Provider \(ICP\) filing from the Ministry of Industry and Information Technology \(MIIT\) of China. For more information, see [Domain filing]().
    -   If you select **Global \(Excluding Mainland China\)**, an ICP filing is not required. |

5.  Click **Next**.

    On the Domain Names page, you can view the **CNAME** of the added domain name.

    ![CNAME on the Domain Names page](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8596549951/p53798.png)

6.  Enable the global acceleration feature in the Log Service console.

    1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

    2.  In the Projects section, click the project for which you want to enable the global acceleration feature.

    3.  On the Overview page, click **Modify** next to **Global Acceleration**.

    4.  In the Global Acceleration dialog box, enter the **CNAME** of the domain name, and click **Enable Acceleration**.

        ![Enable the global acceleration feature](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6693525061/p8065.png)

7.  Verify the configurations of the global acceleration feature.

    After you complete the configurations, you can access the acceleration domain name to check whether the configurations take effect. For example, if the global acceleration feature is enabled for the test-project project, you can run the `curl` command to send a request to the acceleration domain name. The following response indicates that the acceleration configurations take effect:

    ```
    $curl test-project.log-global.aliyuncs.com
    {"Error":{"Code":"OLSInvalidMethod","Message":"The script name is invalid : /","RequestId":"5B55386A2CE41D1F4FBCF7E7"}}
    ```


## Enable the HTTPS acceleration feature

After the HTTP acceleration feature is enabled, you can enable the HTTPS acceleration feature based on your business requirements.

1.  Log on to the [DCDN console](https://dcdn.console.aliyun.com/).

2.  In the left-side navigation pane, click **Domain Names**.

3.  Find the destination domain name, and click **Configure** in the Actions column.

4.  In the left-side navigation pane, click **HTTPS Settings**.

5.  On the page that appears, click Modify in the **SSL Certificate** section.

6.  In the HTTPS Settings dialog box, set the required parameters, and click **OK**. The following table describes the parameters.

    |Parameter|Description|
    |---------|-----------|
    |SSL Acceleration|Turn on the **SSL Acceleration** switch.|
    |Certificate Source|Select **Free Certificate**, and select **Agree to grant Alibaba Cloud permission to apply for a free certificate.** For more information about the HTTPS acceleration feature, see [](). |


## FAQ

What can I do if the `project not exist` error occurs when I access a domain name for acceleration?

The probable cause of the `project not exist` error is that the specified origin site is invalid. In the DCDN console, enter the public domain name of the project. For more information about domain names, see [/intl.en-US/Developer Guide/API Reference/Endpoints.md](/intl.en-US/Developer Guide/API Reference/Endpoints.md).

**Note:** The change of the original domain name takes effect several minutes later.

## What to do next

After you enable the global acceleration feature for the destination project, you can also configure the feature for Logtail or SDK-based log collection.

-   Use Logtail to collect logs
    -   If you enable the global acceleration feature before you install Logtail, select **Global Acceleration** when you install Logtail. For more information, see [/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md](/intl.en-US/Data Collection/Logtail collection/Install/Install Logtail in Linux.md).
    -   If you enable the global acceleration feature after you install Logtail, switch the collection mode to **Global Acceleration**. For more information, see [/intl.en-US/Data Collection/Collection acceleration/Configure log collection acceleration for Logtail.md](/intl.en-US/Data Collection/Collection acceleration/Configure log collection acceleration for Logtail.md).
-   Use an SDK to collect logs

    If you use an SDK to collect logs, you can replace the specified endpoint of the Log Service project with `log-global.aliyuncs.com`.


