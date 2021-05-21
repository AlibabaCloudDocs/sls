# Connect to a data warehouse

The LogShipper feature of Log Service ships logs to storage services such as Object Storage Service \(OSS\), Table Store, and MaxCompute. It cooperates with E-MapReduce \(Spark and Hive\) and MaxCompute for offline computing.

## Data warehousing \(offline computing\)

Data warehousing \(offline computing\) is the supplement to real-time computing, but they are used for different purposes.

|Mode|Advantage|Disadvantage|Application scope|
|:---|:--------|:-----------|:----------------|
|Real-time computing|Fast|Simple computing|Mainly used for incremental computation in monitoring and real-time analysis|
|Offline computing \(data warehousing\)|Accurate and powerful|Relatively slow|Mainly used for full computation in Business Intelligence \(BI\), and data statistics and comparison|

To satisfy the current data analysis requirements, you need to perform both real-time computing and data warehousing \(offline computing\) on the same set of data. For example, you need to perform the following operations when processing access logs:

-   Use Realtime Compute to display the data, including the current PV, UV, and operator information, on the dashboard in real time.
-   Conduct detailed analysis of the full data every night to obtain the growth, year-on-year or month-on-month growth, and top ranking data.

In the world of Internet, there are two classic data processing architectures:

-   [Lamdba architecture](http://lambda-architecture.net/): When data comes in, the architecture can stream and at the same time save the data to the data warehouse. However, when you initiate a query, the results are returned from real-time computing and offline computing based on query conditions and complexities.
-   [Kappa architecture](http://milinda.pathirage.org/kappa-architecture.com/): Kafka-based architecture. The offline computing feature is weakened. All data is stored in Kafka, and all queries are fulfilled with real-time computing.

Log Service provides an architecture that is more similar to the Lamdba architecture.

## LogHub and LogShipper for both real-time and offline computing

Create a Logstore first, and configure LogShipper in the console to enable data warehouse connection. Currently, the following data warehouses are supported:

-   [OSS](/intl.en-US/Product Introduction/What is OSS?.md): large-scale object storage service
-   [TableStore](/intl.en-US/Product Introduction/What is Tablestore?.md): NoSQL data storage service
-   [MaxCompute](/intl.en-US/Product Introduction/What is MaxCompute?.md): data computing service

![Offline scenarios](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4357549951/p32467.png)

LogShipper provides the following features:

-   Quasi real-time: connects to a data warehouse in minutes.
-   Enormous data volume: does not need to worry about concurrency.
-   Retry on error: performs automatic retries or API-based manual retries in case of faults.
-   Task API: uses APIs to acquire the log shipping status for different time periods.
-   Automatic compression: supports data compression to reduce the storage bandwidth.

## Typical scenario 1: Log auditing

A is responsible for maintaining a forum and part of his job is to conduct audits and offline analysis of all access logs on the forum.

-   Department G wants A to capture user visits over the past 180 days, and to provide the access logs within a given period of time on demand.
-   The operations team must prepare an access log report on a quarterly basis.

A uses Log Service to collect log data from the servers and enables the LogShipper feature, allowing Log Service to automatically collect, ship, and compress logs. When an audit is required, the logs within the time period can be authorized to a third party. To conduct offline analysis, use E-MapReduce to run a 30-minute offline task. In this way, two jobs are done at minimal cost. In addition, Data Lake Analytics \(DLA\) can be used to analyze the log data shipped to OSS.

## Typical scenario 2: Real-time and offline analysis of log data

As an open-source software enthusiast, B prefers to use Spark for data analysis. His requirements are as follows:

-   Collect logs from the mobile client by using APIs.
-   Use Spark Streaming to conduct real-time log analysis and collect statistics on online user visits.
-   Use Hive to conduct offline analysis in T+1 mode.
-   Grant downstream agencies access to the log data for analysis in other dimensions.

With a combination of Log Service, OSS, E-MapReduce or DLA, and Resource Access Management \(RAM\), you can fulfill such requirements.

