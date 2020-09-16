# Enable the log management feature

This topic describes how to enable the log management feature in the API Gateway console. After you enable the feature, you can use Log Service to collect API Gateway access logs.

A project and a Logstore are created. For more information, see [Step 1: Create a project and a Logstore](/intl.en-US/Quick Start/Quick start.md).

## Procedure

1.  Log on to the [API Gateway console](https://apigatewaynext.console.aliyun.com).

2.  In the upper-left corner of the page, select a region.

3.  In the left-side navigation pane, choose **Public APIs** \> **Log Manage**.

4.  On the Log Manage page, click the **Create Log Config** tab.

5.  In the Create Log Config dialog box, select a project and Logstore, and click **OK**.

6.  In the System Prompt dialog box, click **Go to configure**.

7.  In the Log Service console, enable the indexing feature for the Logstore and create indexes. For more information, see [Enable and configure the index feature for a Logstore](/intl.en-US/Index and query/Enable and configure the index feature for a Logstore.md).

    After you create indexes, you can query and ship logs.


After API Gateway access logs are collected by Log Service, you can query, download, ship, and transform the logs. You can also create alerts for the logs. For more information, see [Common operations on logs of Alibaba Cloud services](/intl.en-US/Data Collection/Cloud product collection/Common operations on logs of Alibaba Cloud services.md).

