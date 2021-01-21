# Drop fields

You can drop specified fields from a log entry by using the processor\_drop plug-in This topic describes the parameters of the processor\_drop plug-in. This topic also provides examples to show how to configure the plug-in.

## Parameters

The following table describes the parameters that you can specify in the detail parameter when you set the type parameter to processor\_drop.

**Note:** Only Logtail V0.16.28 or later supports the plug-in.

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|DropKeys|String array|No|The fields to be dropped. You can drop one or more fields from a log entry.|

## Configuration example

The following example shows how to drop the aaa1 and aaa2 fields from a log entry.

-   Raw log entry

    ```
    "aaa1":"value1"
    "aaa2":"value2"
    "aaa3":"value3"
    ```

-   Logtail configurations for data processing

    ```
    {
      "processors":[
        {
          "type":"processor_drop",
          "detail": {
            "DropKeys": ["aaa1","aaa2"]
          }
        }
      ]
    }
    ```

-   Result

    ```
    "aaa3":"value3"
    ```


