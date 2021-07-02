# Metricstore

A Metricstore in Log Service is a unit that is used to collect, store, and query time series data.

Each Metricstore belongs to a project. You can create multiple Metricstore in a project. You can create multiple Metricstores in a project based on your business requirements. In most cases, you can create a Metricstore for each type of time series data. For example, if you want to collect the monitoring metrics of hosts, cloud services, and business applications, you can create a project named demo-monitor, and then create three Metricstores named host-metrics, cloud-service-metrics, and app-metrics in the project to store the metrics.

You must specify a Metricstore when you write, query, analyze, or consume time series data.

-   Log Service uses Metricstore as a collection unit to collect time series data.
-   Log Service uses Metricstore as a storage unit to store and consume time series data.
-   Log Service allows you to query and analyze time series data by using the SQL-92 syntax and the PromQL syntax.

