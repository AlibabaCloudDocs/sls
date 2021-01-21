# Filter logs

You can filter logs by using the processor\_filter\_regex plug-in. This topic describes the parameters of the processor\_filter\_regex plug-in. This topic also provides examples to show how to configure the plug-in.

## Parameters

The following table describes the parameters that you can specify in the detail parameter when you set the type parameter to processor\_filter\_regex.

**Note:** A log entry is collected only when it exactly matches the regular expression specified in the Include parameter and does not match the regular expression specified in the Exclude parameter.

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|Include|JSON Object|No|A key is the name of a log field. A value is specified by a regular expression. If a key-value pair matches a field of a log entry, the log entry is collected.|
|Exclude|JSON Object|No|A key is the name of a log field. A value is specified by a regular expression. If a key-value pair matches a field of a log entry, the log entry is dropped.|

## Configuration example

In this example, only logs whose ip is prefixed by 10, method is POST, and browser is not aliyun. \* are collected.

-   Raw log entry
    -   Log entry 1

        ```
        "ip" : "10. **. **.**"
        "method" : "POST"
        "browser" : "aliyun-sdk-java"
        ```

    -   Log entry 2

        ```
        "ip" : "10. **. **.**"
        "method" : "POST"
        "browser" : "chrome"
        ```

    -   Log entry 3

        ```
        "ip" : "192.168. *.*"
        "method" : "POST"
        "browser" : "ali-sls-ilogtail"
        ```

-   Logtail configurations for data processing

    ```
    {
       "type" : "processor_filter_regex",
        "detail" : {
             "Include" : {
                "ip" : "10\\..*",
                "method" : "POST"
             },
             "Exclude" : {
                "browser" : "aliyun. *"
             }
        }
    }
    ```

-   Result

    |Log entry|Collected|Cause|
    |:--------|:--------|:----|
    |Log entry 1|No|The value of the browser parameter matches the regular expression specified in the Exclude parameter.|
    |Log entry 2|Yes|All the filter conditions are met.|
    |Log entry 3|No|The value of the ip parameter does not match the regular expression specified in the Include parameter.|


