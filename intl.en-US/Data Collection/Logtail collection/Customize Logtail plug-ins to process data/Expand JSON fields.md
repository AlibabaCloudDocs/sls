# Expand JSON fields

You can expand specified fields from JSON objects by using the processor\_json plug-in. This topic describes the parameters of the processor\_json plug-in. This topic also provides examples to show how to configure the plug-in.

## Parameters

The following table describes the parameters that you can specify in the detail parameter when you set the type parameter to processor\_json.

**Note:** Only Logtail V0.16.28 or later supports the plug-in.

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|SourceKey|String|Yes|The name of the source field.|
|NoKeyError|Boolean|No|Specifies whether to report an error if the source field is not matched. Default value: true. This value indicates that an error is reported if the source field is not matched.|
|ExpandDepth|Int|No|The depth of JSON expansion. Default value: 0. This value indicates the depth of JSON expansion is unlimited. If the value is n, the number of depths to expand is n.|
|ExpandConnector|String|No|The character that is used to connect expanded keys. Default value: \_.|
|Prefix|String|No|The prefix that is appended to expanded keys. You can leave this parameter empty.|
|KeepSource|Boolean|No|Specifies whether to retain the source field. Default value: true. This value indicates that the source field is retained.|
|UseSourceKeyAsPrefix|Boolean|No|Specifies whether to append the source field name as a prefix to all expanded keys. Default value: false. This value indicates that the source field name is not appended.|

## Configuration example

The following example shows how to expand the s\_key field from a JSON object and append the j and s\_key fields as a prefix to the expanded keys.

-   Raw log entry

    ```
    "s_key":"{\"k1\":{\"k2\":{\"k3\":{\"k4\":{\"k51\":\"51\",\"k52\":\"52\"},\"k41\":\"41\"}}})"
    ```

-   Logtail configurations for data processing

    ```
    {
      "processors":[
        {
          "type":"processor_json",
          "detail": {
            "SourceKey": "s_key",
            "NoKeyError":true,
            "ExpandDepth":0,
            "ExpandConnector":"-",
            "Prefix":"j",
            "KeepSource": false,
            "UseSourceKeyAsPrefix": true
          }
        }
      ]
    }
    ```

-   Result

    ```
    "s_key":"{\"k1\":{\"k2\":{\"k3\":{\"k4\":{\"k51\":\"51\",\"k52\":\"52\"},\"k41\":\"41\"}}})"
    "js_key-k1-k2-k3-k4-k51":"51"
    "js_key-k1-k2-k3-k4-k52":"52"
    "js_key-k1-k2-k3-k41":"41"
    ```


