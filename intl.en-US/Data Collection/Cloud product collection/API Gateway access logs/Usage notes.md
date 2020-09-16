# Usage notes

Alibaba Cloud API Gateway provides the log management feature that is supported by Log Service. You can use this feature to ship API Gateway access logs to Log Service in real time. Log Service allows you to query, transform, and consume the data in real time. This topic describes the resources, billings, and limits that are related to the log management feature.

API Gateway provides the API hosting service to facilitate microservice aggregation, frontend and backend separation, and system integration. When you send an API request, an access log entry is generated. This log entry contains information such as the IP address of the API caller, request URL, response latency, response status code, and number of bytes of the request and response messages. You can use the log data to monitor the operating status of your web services.

![API Gateway](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6207549951/p5402.png)

## Resources

-   Projects and Logstores

    **Note:** You must not delete the projects or Logstores that are related to the log management feature of API Gateway. Otherwise, API Gateway access logs cannot be sent to Log Service.

-   Dedicated dashboard

    After you enable the feature, a dedicated dashboard is automatically created for API Gateway access logs.

    **Note:** We recommend that you do not make changes to the dedicated dashboards because this may affect the usability of the dashboards. You can customize a dashboard to display query results. For more information, see [Create a dashboard](/intl.en-US/Query and visualization/Dashboard/Create a dashboard.md).

    |Dashboard|Description|
    |---------|-----------|
    |Logstore Name\_apigateway\_access\_logs|Displays the overall log statistics of API Gateway, including the number of requests, request success rate, request error rate, request latency, number of apps that call API operations, request errors, top API groups, top APIs, and top request latencies.|


## Billing

-   You are not charged for using the log management feature of API Gateway.
-   After API Gateway access logs are shipped to Log Service, you are charged for the storage space that the log data occupies and the number of read/write operations. You are also charged for reading, transforming, and shipping the data. For more information, see [Billing methods](https://www.alibabacloud.com/product/log-service/pricing?spm=a3c0i.139163.9288850920.1.7690637avzyiqo).

## Limits

-   The API Gateway instance and the Log Service project that stores API Gateway access logs must reside in the same region.
-   You can specify only one project and Logstore to store API Gateway access logs generated in a region.

