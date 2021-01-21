# Encapsulate fields

You can encapsulate one or more fields into a JSON-formatted field by using the processor\_packjson plug-in. This topic describes the parameters of the processor\_packjson plug-in. This topic also provides examples to show how to configure the plug-in.

## Parameters

The following table describes the parameters that you can specify in the detail parameter when you set the type parameter to processor\_packjson.

**Note:** Only Logtail V0.16.28 or later supports the plug-in.

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|SourceKeys|String array|Yes|The string array-formatted field to be encapsulated.|
|DestKey|String|No|The destination JSON-formatted field.|
|KeepSource|Boolean|No|Specifies whether to retain the source field. Default value: true. This value indicates that the source field is retained.|
|AlarmIfIncomplete|Boolean|No|Specifies whether to trigger an alert if the source field is not found. Default value: true. This value indicates that an alert is triggered if the source field is not found.|

## Configuration example

The following example shows how to encapsulate the a and b fields into the d\_key field.

-   Raw log entry

    ```
    "a":"1"
    "b":"2"
    ```

-   Logtail configurations for data processing

    ```
    {
      "processors":[
        {
          "type":"processor_packjson",
          "detail": {
            "SourceKeys": ["a","b"],
            "DestKey":"d_key",
            "KeepSource":true,
            "AlarmIfEmpty":true
          }
        }
      ]
    }
    ```

-   Result

    ```
    "a":"1"
    "b":"2"
    "d_key":"{\"a\":\"1\",\"b\":\"2\"}"
    ```


