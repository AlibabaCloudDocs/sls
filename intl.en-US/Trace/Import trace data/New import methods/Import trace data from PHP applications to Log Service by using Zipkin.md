# Import trace data from PHP applications to Log Service by using Zipkin

This topic describes how to import trace data from PHP applications to Log Service by using Zipkin.

-   A trace instance is created. For more information, see [Create a trace instance](/intl.en-US/Trace/Create a trace instance.md).
-   [PHP](https://www.php.net/downloads.php) is installed.
-   [Composer](https://getcomposer.org/doc/00-intro.md) is installed.

1.  Click [here](https://github.com/openzipkin/zipkin-php-example) to download the official sample code of Zipkin.

2.  Modify the parameters in the functions.php file.

    1.  Modify the $httpReporterURL parameter.

        Replace the $\{endpoint\} variable in the code with the actual value. For more information about the variables, see [Table 1](#table_8wv_xme_wct).

        ```
        $httpReporterURL = 'https://$\{endpoint\}/zipkin/api/v2/spans';
        ```

        |Variable|Description|Example|
        |--------|-----------|-------|
        |$\{endpoint\}|The endpoint. The format is $\{project\}.$\{region-endpoint\}, where:        -   $\{project\}: the name of the Log Service project.
        -   $\{region-endpoint\}: the endpoint of the project. You can access Log Service by using an endpoint of the Internet, the classic network, or a virtual private cloud \(VPC\). For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).
|test-project.cn-hangzhou.log.aliyuncs.com|

    2.  Add the headers parameter when you create the Zipkin\\Reporters\\Http file.

        Replace the variables in the following code with the actual values. For more information about the variables, see [Table 2](#table_9mj_u81_pct).

        ```
            $reporter = new Zipkin\Reporters\Http(
                \Zipkin\Reporters\Http\CurlFactory::create(),
                ['endpoint_url' => $httpReporterURL,
                       'headers' => ['x-sls-otel-project' => '$\{project\}',
                              'x-sls-otel-instance-id' => '$\{instance\}',
                              'x-sls-otel-ak-id' => '$\{access-key-id\}',
                              'x-sls-otel-ak-secret' => '$\{access-key-secret\}']
                ]
            );
        ```

        |Variable|Description|Example|
        |--------|-----------|-------|
        |$\{project\}|The name of the Log Service project.|test-project|
        |$\{instance\}|The name of the trace instance.|test-traces|
        |$\{access-key-id\}|The AccessKey ID of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a Resource Access Management \(RAM\) user who has only the write permissions on the Log Service project. An AccessKey pair consists of an AccessKey ID and an AccessKey secret. For information about how to grant the write permissions on a specific project to a RAM user, see [Use custom policies to grant permissions to a RAM user](/intl.en-US/Developer Guide/Access control RAM/Use custom policies to grant permissions to a RAM user.md). For information about how to obtain an AccessKey pair, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).

|None|
        |$\{access-key-secret\}|The AccessKey secret of your Alibaba Cloud account. We recommend that you use the AccessKey pair of a RAM user who has only the write permissions on the Log Service project.

|None|

3.  Install dependencies.

    ```
    composer install
    ```

4.  Start the service.

    ```
    composer run-frontend
    composer run-backend
    ```

5.  Access the service and then send the trace data to Log Service.

    ```
    curl http://localhost:8081
    ```


-   [View the details of a trace instance](/intl.en-US/Trace/View the details of a trace instance.md)
-   [Query and analyze trace data](/intl.en-US/Trace/Query and analyze trace data.md)

