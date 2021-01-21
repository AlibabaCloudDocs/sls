# Extract log time

You can extract time information from a specified field and parse the time by using the processor\_gotime plug-in or processor\_strptime plug-in. This topic describes the parameters of the processor\_gotime plug-in or processor\_strptime plug-in. This topic also provides examples to show how to configure the plug-ins.

## Time format supported by Golang

The processor\_gotime plug-inparses the time information in a specified field of raw log entries in a time format supported by [Golang](https://golang.org/pkg/time/). The processor\_gotime plug-in also allows you to configure the information as the log time.

-   Parameters

    The following table describes the parameters that you can specify in the detail parameter when you set the type parameter to processor\_gotime.

    **Note:** Only Logtail V0.16.28 or later supports the plug-in.

    |Parameter|Type|Required|Description|
    |:--------|:---|:-------|:----------|
    |SourceKey|String|Yes|The name of the source field.|
    |SourceFormat|String|Yes|The format of the time information in the source field.|
    |SourceLocation|Int|Yes|The source time zone. If the parameter is not specified, the current time zone of the server where Logtail is installed takes effect.|
    |DestKey|String|Yes|The name of the destination field.|
    |DestFormat|String|Yes|The format of the time information in the destination field.|
    |DestLocation|Int|No|The destination time zone. If the parameter is not specified, the current time zone of the server where Logtail is installed takes effect.|
    |SetTime|Boolean|No|Specifies whether to configure the time information as the log time. Default value: true. This value indicates that the time information is configured as the log time.|
    |KeepSource|Boolean|No|Specifies whether to retain the source field. Default value: true. This value indicates that the source field is retained.|
    |NoKeyError|Boolean|No|Specifies whether to report an error if a field is not matched. Default value: true. This value indicates that an error is reported if the source field is not matched.|
    |AlarmIfFail|Boolean|No|Specifies whether to trigger an alert if the time information fails to be extracted. Default value: true. This value indicates that an alert is triggered if the time information fails to be extracted.|

-   Configuration example

    In this example, the time information 2006-01-02 15:04:05 \(UTC+8\) is extracted from the `s_key` field, converted into `2006/01/02 15:04:05 (UTC+9)`, and then included in the d\_key field.

    -   Raw log entry

        ```
        "s_key":"2019-07-05 19:28:01"
        ```

    -   Logtail configurations for data processing

        ```
        {
          "processors":[
            {
              "type":"processor_gotime",
              "detail": {
                "SourceKey": "s_key",
                "SourceFormat":"2006-01-02 15:04:05",
                "SourceLocation":8,
                "DestKey":"d_key",
                "DestFormat":"2006/01/02 15:04:05",
                "DestLocation":9,
                "SetTime": true,
                "KeepSource": true,
                "NoKeyError": true,
                "AlarmIfFail": true
              }
            }
          ]
        }
        ```

    -   Result

        ```
        "s_key":"2019-07-05 19:28:01"
        "d_key":"2019/07/05 20:28:01"
        ```


## Time format supported by strptime

The processor\_strptime plug-in parses the time information in a specified field of raw log entries in a time format supported by [strptime](http://man7.org/linux/man-pages/man3/strptime.3.html). The processor\_strptime plug-in also allows you to configure the information as the log time.

-   Parameters

    The following table describes the parameters that you can specify in the detail parameter when you set the type parameter to processor\_strptime.

    **Note:** Only Logtail V0.16.28 or later supports the plug-in.

    |Parameter|Type|Required|Description|
    |:--------|:---|:-------|:----------|
    |SourceKey|String|Yes|The name of the source field.|
    |Format|String|Yes|The format of the time information in the source field.|
    |AdjustUTCOffset|Boolean|No|Specifies whether to modify the time zone. Default value: false. This value indicates that the time zone is not modified.|
    |UTCOffset|Int|No|The offset that is used to modify the time zone. For example, the value 28800 indicates that the time zone is modified to UTC+8.|
    |AlarmIfFail|Boolean|No|Specifies whether to trigger an alert if the time information fails to be extracted. Default value: true. This value indicates that an alert is triggered if the time information fails to be extracted.|
    |KeepSource|Boolean|No|Specifies whether to retain the source field. Default value: true. This value indicates that the source field is retained.|

-   Configuration examples

    The following examples show how to parse the value of the log\_time field into the `%Y/%m/%d %H:%M:%S` format.

    -   Example 1: The time zone is UTC+8.
        -   Raw log entry

            ```
            "log_time":"2016/01/02 12:59:59"
            ```

        -   Logtail configurations for data processing

            ```
            {
              "processors":[
                {
                  "type":"processor_strptime",
                  "detail": {
                    "SourceKey": "log_time",
                    "Format": "%Y/%m/%d %H:%M:%S"
                  }
                }
              ]
            }
            ```

        -   Result

            ```
            "log_time":"2016/01/02 12:59:59"
            Log.Time = 1451710799
            ```

    -   Example 2: The time zone is UTC+7.
        -   Raw log entry

            ```
            "log_time":"2016/01/02 12:59:59"
            ```

        -   Logtail configurations for data processing

            ```
            {
              "processors":[
                {
                  "type":"processor_strptime",
                  "detail": {
                    "SourceKey": "log_time",
                    "Format": "%Y/%m/%d %H:%M:%S",
                    "AdjustUTCOffset": true,
                    "UTCOffset": 25200
                  }
                }
              ]
            }
            ```

        -   Result

            ```
            "log_time":"2016/01/02 12:59:59"
            Log.Time = 1451714399
            ```


