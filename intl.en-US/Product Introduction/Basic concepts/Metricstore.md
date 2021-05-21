# Metricstore

A Metricstore is the unit for time series data collection, storage, and query in Log Service.

Each Metricstore belongs to only one project. However, you can create multiple Metricstores in the same project. Log Service provides various Metricstores for different types of time series data. For example, if you need to collect host metrics, cloud service metrics, and application metrics, you can create a project named demo-monitor and create three Metricstores named host-metrics, cloud-service-metrics, and app-metrics in the project.

When you read or write time series data, you must specify a destination Metricstore. The Metricstore provides the following features:

-   Collects time series data and allows you to write real-time data to the Metricstore.
-   Stores time series data and supports real-time data consumption.
-   Supports queries on time series data. Two query languages are supported: SQL-92 and PromQL.

