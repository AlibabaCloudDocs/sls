# Overview

If you have complex logs that cannot be parsed in basic modes such as regular expression, NGINX, and JSON, you can use Logtail plug-ins to parse logs. You can configure Logtail plug-ins for one or more processing methods. Then, Logtail executes the processing methods in sequence.

## Limits

-   Performance limits

    When a plug-in is used to process data, Logtail consumes more resources. Most of these resources are CPU resources. You can modify the Logtail parameter settings based on your needs. For more information, see [t13060.dita\#concept\_sdg\_czb\_wdb](/intl.en-US/Data Collection/Logtail collection/Install/Configure the startup parameters of Logtail.md). If raw data is generated at a speed higher than 5 MB/s, we recommend that you do not use multiple plug-ins to process the data. You can use a Logtail plug-ins to simplify data processing, and then use the [data transformation](/intl.en-US/Data Transformation/Overview.md) feature to further process the data.

-   Limits on text logs

    Log Service allows you to process text logs in basic modes such as the regular expression, NGINX, or JSON mode. Log Service also allows you to use Logtail plug-ins to process text logs.However, Logtail plug-ins have the following limits on text logs:

    -   If you enable the plug-in processing feature, some advanced features of the specified mode become unavailable. For example, you cannot configure the filter, upload raw logs, configure the system time zone, drop logs that fail to be parsed, or upload incomplete log entries \(in delimiter mode\).
    -   Plug-ins use the line mode to process text logs. In this mode, file-level metadata such as \_\_tag\_\_:\_\_path\_\_ and \_\_topic\_\_ is stored in each log entry. If you use Logtail plug-ins to process data, the following limits apply to tag-related features:
        -   You cannot use the contextual query and LiveTail features because these features depend on fields such as \_\_tag\_\_:\_\_path\_\_.
        -   The name of the \_\_topic\_\_ field is renamed to \_\_log\_topic\_\_.
        -   Fields such as \_\_tag\_\_:\_\_path\_\_ no longer have original field indexes. You must configure indexes for these fields.

## Usage notes

When you configure data processing methods, you must set the key in the configuration file to processors and set the value to an array of JSON objects. Each object of the array contains the details of a processing method.

Each processing method contains the type and detail fields. The type field specifies the type of the processing method and the detail field contains configuration details.

```
"processors" : [
      {
          "type" : "processor_split_char",
          "detail" : {"SourceKey" : "content",
              "SplitSep" : "|",
              "SplitKeys" : ["method", "type", "ip", "time", "req_id", "size", "detail"]
          }
      },
      {
          "type" : "processor_anchor",
          "detail" : "SourceKey" : "detail",
              "Anchors" : [
                  {
                      "Start" : "appKey=",
                      "Stop" : ",env=",
                      "FieldName" : "appKey",
                      "FieldType" : "string"
                  }            
              ]
          }
  ]
```

The following table describes available Logtail plug-ins and the operations that you can perform by using these plug-ins.

|Logtail plug-in|Description|
|---------------|-----------|
|processor\_regex|You can extract fields by using the processor\_regex plug-in and matching the specified fields based on a regular expression. For more information, see [Extract log fields by using a regular expression](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to process data/Extract fields.md).|
|processor\_anchor|You can anchor strings and extract fields by using the processor\_anchor plug-in, start keyword, and stop keyword. For more information, see [Extract log fields by using start and stop keywords](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to process data/Extract fields.md).|
|processor\_split\_char|You can delimit fields by using the processor\_split\_char plug-in and a specified single-character delimiter. For more information, see [Extract log fields by using a single-character delimiter](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to process data/Extract fields.md).|
|processor\_split\_string|You can delimit fields by using the processor\_split\_string plug-in and a specified multi-character delimiter. For more information, see [Extract log fields by using a multi-character delimiter](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to process data/Extract fields.md).|
|processor\_split\_key\_value|Use the processor\_split\_key\_value plug-in in key-value pair mode to extract fields. For more information, see [Extract log fields by splitting key-value pairs](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to process data/Extract fields.md).|
|processor\_add\_fields|You can add fields to a log entry by using the processor\_add\_fields plug-in. For more information, see [Add fields](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to process data/Add fields.md).|
|processor\_drop|You can drop specified fields from a log entry by using the processor\_drop plug-in. For more information, see [Drop fields](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to process data/Drop fields.md).|
|processor\_rename|You can rename specified fields by using the processor\_rename plug-in. For more information, see [Rename columns](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to process data/Rename columns.md).|
|processor\_packjson|You can encapsulate one or more fields into a JSON-formatted field by using the processor\_packjson plug-in. For more information, see [Encapsulate fields](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to process data/Encapsulate fields.md).|
|processor\_json|You can expand specified fields from JSON objects by using the processor\_json plug-in. For more information, see [Expand JSON fields](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to process data/Expand JSON fields.md).|
|processor\_filter\_regex|You can filter logs by using the processor\_filter\_regex plug-in. For more information, see [Filter logs](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to process data/Filter logs.md).|
|processor\_gotime|You can extract time information from a specified field in a time format supported by Golang. Then, you can configure the information as the log time by using the processor\_gotime plug-in. For more information, see [Time format supported by Golang](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to process data/Extract log time.md).|
|processor\_strptime|You can extract time information from a specified field in a time format supported by strptime. Then, you can configure the information as the log time by using the processor\_strptime plug-in. For more information, see [Time format supported by strptime](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to process data/Extract log time.md).|
|processor\_geoip|You can transform IP addresses in logs into geo locations by using the processor\_geoip plug-in. The geo locations include the country, province, city, longitude, and latitude. For more information, see [Transform IP addresses](/intl.en-US/Data Collection/Logtail collection/Customize Logtail plug-ins to process data/Transform IP addresses.md).|

