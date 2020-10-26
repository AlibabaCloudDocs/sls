# Transfer data across regions

Log Service allows you to transfer data across regions by using the data transformation feature. This topic describes the scenarios, procedure, and billing of data transmission across regions.

## Scenarios

If your cloud services are deployed in different regions, the service-related logs are distributed across these regions. If you want to collect these logs, you can use the data transformation feature of Log Service.

![Data transmission across regions](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1659763061/p174611.png)

## Procedure

1.  Enter a transformation rule on the data transformation page and click **Preview Data** to view the result. For more information, see [Create a data transformation rule](/intl.en-US/Data Transformation/Create a data transformation rule.md).

    **Note:** You do not need to enter any transformation rule if you want to transfer raw logs.

    ![Transformation rule](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2370863061/p174756.png)

2.  Click **Save as Transformation Rule** if no error occurs.

3.  In the Create Data Transformation Rule dialog box, set the **Target Region** parameter.

    For information about other parameters, see [Create a data transformation rule](/intl.en-US/Data Transformation/Create a data transformation rule.mdtable_q5z_2r5_23k).

    ![Select the destination region](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2370863061/p174706.png)


## FAQ

-   How am I charged for data transmission across regions?

    You are charged based on the size of compressed data during data transmission. For example, assume that the compression ratio is 10:1 and you need to transfer 10 MB of raw data. The size of the data after compression is 1 MB. This way, only 1 MB of data incurs fees. For more information about the billing methods, see [Billing items and methods](/intl.en-US/Pricing/Billing overview.md).

-   How do I increase the data compression ratio?

    For example, to increase the data compression ratio in the web tracking scenario, you can set fixed values for the \_\_topic\_\_, \_\_tag\_\_, and \_\_source\_\_ fields in different log entries to transfer them as a log group. This is because log data is compressed by log group. If you do not set fixed values for the \_\_source\_\_, \_\_topic\_\_, and \_\_tag\_\_fields, multiple log groups may be generated and the data compression ratio will be reduced.

    ![Increase the data compression ratio](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1659763061/p174640.png)

-   What can I do if the network is unstable?
    -   If the network is unstable when you transfer data across borders, you do not need to perform operations. The data transmission task automatically retries if the network is unstable.
    -   You can activate Dynamic Route for CDN \(DCDN\) to improve network stability.

        ![DCDN acceleration](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1659763061/p174643.png)

        1.  For more information about how to activate DCDN, see [Enable the global acceleration feature](/intl.en-US/Data Collection/Collection acceleration/Enable the global acceleration feature.md).
        2.  On the Create Data Transformation Rule page, select **DCDN Acceleration** and click **DCDN Acceleration Connectivity Test**.

            ![DCDN acceleration](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2370863061/p174750.png)


