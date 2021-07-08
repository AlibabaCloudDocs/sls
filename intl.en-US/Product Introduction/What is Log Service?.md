---
keyword: [Log Service, log, metric, trace, observation, analysis]
---

# What is Log Service?

Log Service is a cloud-native observation and analysis platform that provides large-scale, low-cost, and real-time services to process multiple types of data such as logs, metrics, and traces. Log Service allows you to collect, transform, query, analyze, visualize, ship, and consume data. You can also configure alerts in the console. Log Service helps enterprises improve their digital capabilities in terms of R&D, O&M, and data security.

## Learning path

Log Service learning path provides common guides to use Log Service. For more information, see [Log Service Learning Path](https://www.alibabacloud.com/getting-started/learningpath/log). Log Service learning path also provides videos that match the user guides to help you understand Log Service.

## Terms

Before you use Log Service, familiarize yourself with the following terms.

|Term|Description|
|----|-----------|
|Project|A project in Log Service is used to separate different resources of multiple users and control access to specific resources. For more information, see [Project](/intl.en-US/Product Introduction/Basic concepts/Project.md).|
|Logstore|A Logstore in Log Service is used to collect, store, and query log data. For more information, see [Logstore](/intl.en-US/Product Introduction/Basic concepts/Logstore.md).|
|Metricstore|A Metricstore in Log Service is used to collect, store, and query time series data. For more information, see [Metricstore](/intl.en-US/Product Introduction/Basic concepts/Metricstore.md).|
|Log|Logs are records of changes that occur in a system during the running of the system. The records contain information about the operations that are performed on specific objects and the results of the operations. The records are ordered by time. For more information, see [Log](/intl.en-US/Product Introduction/Basic concepts/Log.md).|
|Log group|A log group is a collection of logs and is the basic unit that is used to write and read logs. The logs in a log group contain the same metadata, such as the IP address and log source. For more information, see [Log group](/intl.en-US/Product Introduction/Basic concepts/Log group.md).|
|Time series data|Time series data is a series of data points that are ordered by time. For more information, see [Time series data](/intl.en-US/Product Introduction/Basic concepts/Time series.md).|
|Trace data|Trace data indicates the execution process of an event or a procedure in a distributed system. For more information, see [Trace](/intl.en-US/Product Introduction/Basic concepts/Trace.md).|
|Shard|A shard is used to control the read and write capacity of a Logstore. In Log Service, data is stored in a shard. Each shard has an MD5 hash range and each range is a left-closed and right-open interval. Each range does not overlap with the ranges of other shards. The entire MD5 hash range is within the following range: \[00000000000000000000000000000000, ffffffffffffffffffffffffffffffff\). For more information, see [Shard](/intl.en-US/Product Introduction/Basic concepts/Shard.md).|
|Topic|A topic is a basic management unit in Log Service. You can specify log topics when you collect logs. This way, Log Service classifies logs by log topic. For more information, see [Topic](/intl.en-US/Product Introduction/Basic concepts/Topic.md).|
|Endpoint|An endpoint of Log Service is a URL that is used to access a project. To access projects in different regions, you must use different endpoints. To access the projects in the same region over the Alibaba Cloud internal network or the Internet, you must also use different endpoints. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).|
|AccessKey pair|An AccessKey pair is an identity credential that consists of an AccessKey ID and an AccessKey secret. The AccessKey ID and the AccessKey secret are used for symmetric encryption and identity verification. The AccessKey ID is used to identify a user. The AccessKey secret is used to encrypt and verify a signature string. The AccessKey secret is confidential. For more information, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).|
|Region|The region is the physical location where a data center of Log Service is located. You can specify a region when you create a project. After the project is created, you cannot change the region.|

## Features

Log Service provides the following features to meet the requirements of cloud-native observation and analysis in multiple business scenarios.

## Usage

You can use Log Service by using the following methods.

|Method|Description|
|------|-----------|
|Console|Log Service provides a web console to manage your Log Service resources. For more information, see [Log Service console](https://sls.console.aliyun.com).|
|SDK|Log Service provides SDKs for various programming languages to facilitate secondary development. For more information, see [SDK overview](/intl.en-US/Developer Guide/SDK Reference/Overview.md).|
|API|Log Service provides the API to manage your Log Service resources. This method requires you to sign API requests. For more information, see [API overview](/intl.en-US/Developer Guide/API Reference/Overview.md). **Note:** We recommend that you use SDKs to avoid signature verification. |
|CLI|Log Service provides a command-line interface \(CLI\) to manage your Log Service resources. For more information, see [Command-line interface]().|

## Billing

Log Service supports the pay-as-you-go billing method. You are charged based on your actual usage. Compared with self-managed ELK, Log Service allows you to reduce the total cost by 50%. For more information about metering items and billing items, see [Billable items and billing method](/intl.en-US/Pricing/Billable items and billing method.md).

## Customer feedback

The following figures show the comments of some Log Service customers.

## Activate Log Service

Click Activate Log Service to go to the buy page of Log Service.

