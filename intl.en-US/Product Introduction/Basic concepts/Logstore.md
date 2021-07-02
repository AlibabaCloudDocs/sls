# Logstore

A Logstore in Log Service is a unit that is used to collect, store, and query logs.

Each Logstore belongs to a project. You can create multiple Logstores in a project. You can create multiple Logstores in a specific project based on your business requirements. In most cases, you can create a Logstore for each log type of an application. For example, if you want to collect the operation logs, application logs, and access logs of App A, you can create a project named app-a, and then create three Logstores named operation\_log, application\_log, and access\_log in the project to store the logs.

You must specify a Logstore when you write, query, analyze, transform, consume, or ship logs.

-   Log Service uses the Logstore as a collection unit to collect logs.
-   Log Service uses the Logstore as a storage unit to store, transform, consume, and ship logs.
-   Log Service creates indexes for the Logstore to query and analyze logs.

