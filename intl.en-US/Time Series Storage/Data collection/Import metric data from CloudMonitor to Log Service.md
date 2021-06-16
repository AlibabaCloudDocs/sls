# Import metric data from CloudMonitor to Log Service

This topic describes how to import metric data from CloudMonitor to Log Service. Then, you can query, analyze, and transform the metric data.

A Metricstore is created. For more information, see [Create a Metricstore](/intl.en-US/Preparation/Manage Metricstores.md).

## Import data

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Import Data section, select **Cloud Monitor**.

3.  In the Specify Logstore step, select a project and Metricstore and click **Next**.

    You can also click **Create Now** to create a project or Metricstore. For more information, see [Create a project](/intl.en-US/Preparation/Manage a project.md) and [Create a Metricstore](/intl.en-US/Preparation/Manage Metricstores.md).

4.  In the Configure Import Settings step, set the required parameters and click **Next**.

    |Parameter|Description|
    |---------|-----------|
    |Configuration Name|The name of the data import configuration.|
    |Import All|Select **Yes** to import all metric data from CloudMonitor to Log Service. For more information, see [Predefined metrics](https://metricmeta.oss-cn-hangzhou.aliyuncs.com/listMetricMeta_zh.html).|
    |Namespace|If you select No for the **Import All** parameter, select the metrics that you want to import from the **NameSpace** drop-down list.|
    |AccessKeyId|The AccessKey ID of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a Resource Access Management \(RAM\) user who is granted the AliyunCloudMonitorReadOnlyAccess permission.|
    |AccessKeySecret|The AccessKey secret of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a RAM user who is granted the AliyunCloudMonitorReadOnlyAccess permission.|
    |Import Interval|The interval at which data is imported. Unit: minutes.|


## View the data import configuration

After you create a data import configuration, you can view the configuration details and related statistical report in the Log Service console.

1.  In the Projects section, click the name of the project.

2.  Choose **Time Series Storage** \> **Metricstore**. On the Metricstore tab, click the Metricstore and choose **Data Import** \> **Data Import**.

3.  Click the name of the import configuration.

4.  On the Import Configuration Overview page, view the basic information and statistical report of the import configuration.


## What to do next

On the Import Configuration Overview page, you can perform the following operations:

-   Modify the settings of the import configuration.

    In the upper-right corner, click **Modify Settings**. For more information, see [Configure data import](#step_72x_uut_l52).

-   Delete the import configuration.

    Click **Delete Configuration** to delete the data import configuration.

    **Note:** After the data import configuration is deleted, it cannot be recovered.


