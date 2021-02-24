# Overview

The log query feature of Function Compute collects the execution logs of functions to Log Service. This feature allows you to debug codes, troubleshoot errors, and analyze data. This topic describes the resources and limits of the log query feature in Function Compute.

## Resources

-   Dedicated project and Logstore

    After you enable the log query feature of Function Compute, Log Service creates a project named in the format of aliyun-fc-region ID-bb47c1e4-cdd4-5318-978e-d748952652c8 and a Logstore named function-log.

-   Dashboard

    After you enable the log query feature of Function Compute, Log Service does not generate a dashboard. You can create a custom dashboard.


## Billing

-   You are not charged for using the log query feature of Function Compute.
-   After function execution logs are collected by Log Service, you are charged for the storage space that the logs occupy, data reads, read/write requests, data transformation, and data shipping. For more information, see [Log Service pricing](https://www.alibabacloud.com/product/log-service/pricing?spm=a3c0i.139163.9288850920.1.7690637avzyiqo).

## Limits

You can write only function execution logs to a dedicated Logstore.

