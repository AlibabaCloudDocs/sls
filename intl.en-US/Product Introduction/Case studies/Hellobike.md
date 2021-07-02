# Hellobike

Hellobike saves 30% of costs by using Log Service instead of Kafka, Elasticsearch, and ClickHouse to collect and manage log data. Log Service meets the requirements of Hellobike on system stability, capacity scalability, and log query and analysis.

## Company profile

Hellobike is a leading platform for local transportation and life services in China. Hellobike is committed to providing more convenient transportation methods and better inclusive life services based on digital dividends. For more information, visit the website of [Hellobike](https://www.hello-inc.com/).

## Scenarios

Hellobike provides bicycle and moped rental services on a time-sharing basis. The company launched the bike-sharing service to solve the "last mile" issue. Hellobike is dedicated to building an efficient O&M system and a management system by applying the outcomes of technological innovation to intelligent terminals. To do this, Hellobike installs smart locks on its bicycles, equips the bicycles with positioning chips, establishes an intelligent planning and scheduling system at the backend, and promotes intensive operations based on intelligent O&M ports. Hellobike has expanded its business to more than 400 cities, with more than 400 million registered users whose accumulated riding distance has reached 23.7 billion kilometers. The company has implemented online real-time scheduling based on smart locks and seamlessly integrated bicycle data and app data. Therefore, Hellobike needs to collect, analyze, and store the data in real time.

## Challenges

In the previous architecture of Hellobike, data is collected to Kafka, and then written to the Elasticsearch, Logstash, and Kibana \(ELK\) stack for log queries and written to ClickHouse for log analysis. Terabytes of incremental data is written to Elasticsearch per day, posing a big challenge on the stability of Elasticsearch. The write operations of Elasticsearch are delayed during data queries. The data query feature is unavailable in most cases because the system needs to write large amounts of data first. To address this bottleneck, the system creates only one replica for the data that is created on the current day, and creates more replicas on the next day. However, this solution greatly compromises data reliability. In addition, self-managed Kafka, Elasticsearch, and ClickHouse require high costs.

## Solution

Log Service helps Hellobike collect and query terabytes of logs in real time and provides scalable storage.

-   Real-time data collection

    Log Service provide native support for the Kafka protocol. Each client of Hellobike can seamlessly migrate data to Log Service by only setting the Kafka address to the Kafka protocol address of Log Service.

-   Elastic scaling

    Log Service uses shards to read and write data. If traffic increases, you can manually split shards to expand the write throughput. You can also enable the automatic sharding feature. If the traffic reaches the upper limit, Log Service automatically increases the number of shards.

-   Data query and analysis

    Log Service provides data query and analysis capabilities. You can query specific data by keyword or numeric range. You can also perform a recursive query for JSON fields or perform a union query. Log Service allows you to analyze logs by using the SQL-92 syntax. You can analyze tens of billions of log entries within seconds. The SQL-92 syntax supports more than 200 functions. You can use the SQL JOIN syntax to perform an associated analysis on the data in Log Service and the data from Object Storage Service \(OSS\) or MySQL databases.


![Hellobike_Solution](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8867534261/p271803.png)

