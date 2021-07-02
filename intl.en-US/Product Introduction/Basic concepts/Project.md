# Project

A project in Log Service is used to separate different resources of multiple users and control access to specific resources.

A project contains resources such as Logstores, Metricstores, and machine groups, and provides an endpoint that you can use to access the resources of Log Service. We recommend that you use different projects to manage the data in different applications, services, or projects.

-   A project organizes and manages Logstores or Metricstores. You can use Log Service to collect and store the logs of different projects, services, or environments. You can specify different projects to facilitate log data consumption, export, and analysis.
-   A project facilitates access control. You can grant a Resource Access Management \(RAM\) user the permissions to manage a specified project.
-   A project provides an endpoint that you can use to access the resources in the project. Log Service allocates an exclusive endpoint to each project. You can use the endpoint of a project to read, write, and manage log data. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).

