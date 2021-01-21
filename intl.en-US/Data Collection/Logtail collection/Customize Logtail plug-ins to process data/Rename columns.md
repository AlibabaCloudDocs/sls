# Rename columns

You can rename specified fields by using the processor\_rename plug-in. This topic describes the parameters of the processor\_rename plug-in. This topic also provides examples to show how to configure the plug-in.

## Parameters

The following table describes the parameters that you can specify in the detail parameter when you set the type parameter to processor\_rename.

**Note:** Only Logtail V0.16.28 or later supports the plug-in.

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|NoKeyError|Boolean|Yes|Specifies whether to report an error if a field to be renamed is not matched. Default value: false. This value indicates that no error is reported if a field to be renamed is not matched.|
|SourceKeys|String array|Yes|The source fields to be renamed.|
|DestKeys|String array|Yes|The renamed fields.|

## Configuration example

The following example shows how to rename the aaa1 field to bbb1 and the aaa2 field to bbb2.

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
          "type":"processor_rename",
          "detail": {
            "SourceKeys": ["aaa1","aaa2"],
            "DestKeys": ["bbb1","bbb2"],
            "NoKeyError": true
          }
        }
      ]
    }
    ```

-   Result

    ```
    "bbb1":"value1"
    "bbb2":"value2"
    "aaa3":"value3"
    ```


