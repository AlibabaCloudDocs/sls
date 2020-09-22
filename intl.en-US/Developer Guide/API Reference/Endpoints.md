# Endpoints

This topic describes the Log Service endpoints for different network types.

## Overview

Log Service provides endpoints for different network types, for example, Alibaba Cloud internal network and the Internet. You can go to the Overview page of a project in the Log Service console to view the endpoint of the region where the project resides.

![Endpoints](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7062470061/p132286.png)

## Endpoints for the Internet

A Log Service endpoint is a URL that is used to access a project and the logs in the project. The endpoint is associated with the project name and the region where the project resides. Log Service is available in multiple regions. The following table lists the endpoints for the Internet in each region.

|Region|Endpoint|
|:-----|:-------|
|China \(Hangzhou\)|cn-hangzhou.log.aliyuncs.com|
|China \(Shanghai\)|cn-shanghai.log.aliyuncs.com|
|China \(Qingdao\)|cn-qingdao.log.aliyuncs.com|
|China \(Beijing\)|cn-beijing.log.aliyuncs.com|
|China \(Zhangjiakou-Beijing Winter Olympics\)|cn-zhangjiakou.log.aliyuncs.com|
|China \(Hohhot\)|cn-huhehaote.log.aliyuncs.com|
|China \(Ulanqab\)|cn-wulanchabu.log.aliyuncs.com|
|China \(Shenzhen\)|cn-shenzhen.log.aliyuncs.com|
|China \(Heyuan\)|cn-heyuan.log.aliyuncs.com|
|China \(Guangzhou\)|cn-guangzhou.log.aliyuncs.com|
|China \(Chengdu\)|cn-chengdu.log.aliyuncs.com|
|China \(Hong Kong\)|cn-hongkong.log.aliyuncs.com|
|Japan \(Tokyo\)|ap-northeast-1.log.aliyuncs.com|
|Singapore \(Singapore\)|ap-southeast-1.log.aliyuncs.com|
|Australia \(Sydney\)|ap-southeast-2.log.aliyuncs.com|
|Malaysia \(Kuala Lumpur\)|ap-southeast-3.log.aliyuncs.com|
|Indonesia \(Jakarta\)|ap-southeast-5.log.aliyuncs.com|
|UAE \(Dubai\)|me-east-1.log.aliyuncs.com|
|US \(Silicon Valley\)|us-west-1.log.aliyuncs.com|
|Germany \(Frankfurt\)|eu-central-1.log.aliyuncs.com|
|US \(Virginia\)|us-east-1.log.aliyuncs.com|
|India \(Mumbai\)|ap-south-1.log.aliyuncs.com|
|UK \(London\)|eu-west-1.log.aliyuncs.com|
|Russia \(Moscow\)|rus-west-1.log.aliyuncs.com|

When you access a project, you must configure an endpoint based on the project name and the region where the project resides. Format:

```
<project_name>.<region_endpoint>
```

The following example shows an endpoint. In this example, the project name is big-game and the region is China \(Hangzhou\).

```
big-game.cn-hangzhou.log.aliyuncs.com
```

**Note:**

-   You must specify a region when you create a Log Service project.
-   After the project is created, you cannot change the region or migrate data across regions.
-   You must select an endpoint that matches the region to configure an endpoint for the project. The endpoint is used to call API operations.

## Endpoints for the classic network and VPC

You can also use endpoints for the classic network and Virtual Private Cloud \(VPC\) to call Log Service API operations on Elastic Compute Service \(ECS\) instances. The following table lists the endpoints in each region.

**Note:**

-   If you are using the endpoints for the classic network and VPC to call Log Service API operations, you can transmit data only over HTTP and HTTPS.
-   Access to Log Service by using the endpoints for the classic network and VPC does not generate Internet traffic of ECS instances. Therefore, you can save the Internet bandwidth of ECS instances.

|Region|Root endpoint|
|:-----|:------------|
|China \(Hangzhou\)|cn-hangzhou-intranet.log.aliyuncs.com|
|China \(Shanghai\)|cn-shanghai-intranet.log.aliyuncs.com|
|China \(Qingdao\)|cn-qingdao-intranet.log.aliyuncs.com|
|China \(Beijing\)|cn-beijing-intranet.log.aliyuncs.com|
|China \(Shenzhen\)|cn-shenzhen-intranet.log.aliyuncs.com|
|China \(Heyuan\)|cn-heyuan-intranet.log.aliyuncs.com|
|China \(Guangzhou\)|cn-guangzhou-intranet.log.aliyuncs.com|
|China \(Zhangjiakou-Beijing Winter Olympics\)|cn-zhangjiakou-intranet.log.aliyuncs.com|
|China \(Hohhot\)|cn-huhehaote-intranet.log.aliyuncs.com|
|China \(Ulanqab\)|cn-wulanchabu-intranet.log.aliyuncs.com|
|China \(Chengdu\)|cn-chengdu-intranet.log.aliyuncs.com|
|China \(Hong Kong\)|cn-hongkong-intranet.log.aliyuncs.com|
|US \(Silicon Valley\)|us-west-1-intranet.log.aliyuncs.com|
|Japan \(Tokyo\)|ap-northeast-1-intranet.log.aliyuncs.com|
|Singapore \(Singapore\)|ap-southeast-1-intranet.log.aliyuncs.com|
|Australia \(Sydney\)|ap-southeast-2-intranet.log.aliyuncs.com|
|Malaysia \(Kuala Lumpur\)|ap-southeast-3-intranet.log.aliyuncs.com|
|Indonesia \(Jakarta\)|ap-southeast-5-intranet.log.aliyuncs.com|
|UAE \(Dubai\)|me-east-1-intranet.log.aliyuncs.com|
|Germany \(Frankfurt\)|eu-central-1-intranet.log.aliyuncs.com|
|US \(Virginia\)|us-east-1-intranet.log.aliyuncs.com|
|India \(Mumbai\)|ap-south-1-intranet.log.aliyuncs.com|
|UK \(London\)|eu-west-1-intranet.log.aliyuncs.com|
|Russia \(Moscow\)|rus-west-1-intranet.log.aliyuncs.com|

The following example shows an endpoint. In this example, the project name is big-game and the region is China \(Hangzhou\).

```
big-game.cn-hangzhou-intranet.log.aliyuncs.com
```

## Endpoint for the global acceleration feature

In addition to Alibaba Cloud internal network and the Internet, you can also use the [global acceleration feature](/intl.en-US/Data Collection/Collection acceleration/Overview.md) to collect data to Log Service. Compared with access to Log Service over the Internet, access to Log Service by using the global acceleration feature provides lower latency and higher stability. This feature is more suitable for data collection and consumption scenarios that require reliable and fast data transmission.

The endpoint for the global acceleration feature in all regions is log-global.aliyuncs.com.

**Note:** By default, the global acceleration feature is disabled. You must enable it before use. For more information, see [Enable the global acceleration feature](/intl.en-US/Data Collection/Collection acceleration/Enable the global acceleration feature.md).

