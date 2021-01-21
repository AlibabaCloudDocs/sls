# Add fields

You can add fields to a log entry by using the processor\_add\_fields plug-in. This topic describes the parameters of the processor\_add\_fields plug-in. This topic also provides examples to show how to configure the plug-in.

## Parameters

The following table describes the parameters that you can specify in the detail parameter when you set the type parameter to processor\_add\_fields.

**Note:** Only Logtail V0.16.28 or later supports the plug-in.

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|Fields|Map|No|The key-value pairs to be added. You can specify multiple key-value pairs in the parameter.|
|IgnoreIfExist|Boolean|No|Specifies whether to retain key-value pairs that have the same key. Default value: false. This value indicates that a key-value pair is not retained if the key is the same as another specified key.|

## Configuration example

The following example shows how to add the aaa2 and aaa3 fields to a log entry.

-   Raw log entry

    ```
    "aaa1":"value1"
    ```

-   Logtail configurations for data processing

    ```
    {
      "processors":[
        {
          "type":"processor_add_fields",
          "detail": {
            "Fields": {
              "aaa2": "value2",
              "aaa3": "value3"
            }
          }
        }
      ]
    }
    ```

-   Result

    ```
    "aaa1":"value1"
    "aaa2":"value2"
    "aaa3":"value3"
    ```


