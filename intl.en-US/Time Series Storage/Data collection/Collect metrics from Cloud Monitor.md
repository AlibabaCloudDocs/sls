# Collect metrics from Cloud Monitor

This topic describes how to create an import task to collect metrics from Cloud Monitor. You can then search, analyze, and transform the metric data in Log Service.

A Metricstore is created. For more information, see [Create a Metricstore](/intl.en-US/Time Series Storage/Manage Metricstores.md).

## Create an import task

1.  Log on to the [Log Service console](https://sls.console.aliyun.com).

2.  In the Import Data section, click the **Cloud Monitor** card.

3.  In the Specify Logstore step, select a project and Metricstore and click **Next**.

    You can also click **Create Now** to create a project or Metricstore. For more information, see [Create a Metricstore](/intl.en-US/Time Series Storage/Manage Metricstores.md).

4.  In the Configure Import Settings step, set the parameters and click **Next**.

    |Parameter|Description|
    |---------|-----------|
    |Configuration Name|Enter the name of the import task.|
    |Import All|Select **Yes** to import all metrics from Cloud Monitor. For more information, see [Predefined metrics](https://metricmeta.oss-cn-hangzhou.aliyuncs.com/listMetricMeta_zh.html).|
    |NameSpace|If you select No for the **Import All** parameter, select the metrics that you want to import from the **NameSpace** drop-down list.|
    |AccessKeyId|Enter an AccessKey ID. We recommend that you use an AccessKey pair of a RAM user. The RAM user must have the AliyunCloudMonitorReadOnlyAccess permission.|
    |AccessKeySecret|Enter the AccessKey secret that corresponds to the AccessKey ID. We recommend that you use an AccessKey pair of a RAM user. The RAM user must have the AliyunCloudMonitorReadOnlyAccess permission.|
    |Import Interval|Specify the interval at which the import task is run. Unit: minutes.|


## View the configurations of an import task

After an import task is created, you can perform the following steps to view the configurations and statistical report of the import task in the Log Service console:

1.  In the Projects section, click the project.

2.  On the **Metricstore Storage** \> **Metricstore** tab, expand the Metricstore node and choose **Data Import** \> **Data Import**.

3.  Click the name of the import task.

4.  On the Import Configuration Overview page, view the configurations and statistical report of the import task.


## What to do next

On the Import Configuration Overview page, you can perform the following operations:

-   Modify the configurations of the import task.

    In the upper-right corner, click **Modify Settings**. For more information, see [Configure data import](#step_72x_uut_l52).

-   Delete the import task.

    In the upper-right corner, click **Delete Configuration**.

    **Note:** After the import task is deleted, it cannot be restored.


