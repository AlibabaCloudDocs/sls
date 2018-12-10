# Data encoding method {#reference_uvb_r5q_12b .reference}

Protocol Buffer is a structured data interchange format developed by Google. It is widely used in many internal and external services of Google.  Currently, Log Service uses Protocol Buffer format as  the standard log writing format.  You must serialize the original log data into Protocol Buffer data streams before writing logs to Log Service by using APIs.

```
syntax = "proto2";
package sls;

import "gogo.proto";

option (gogoproto.sizer_all) = true;
option (gogoproto.marshaler_all) = true;
option (gogoproto.unmarshaler_all) = true;

message LogContent
{
    required string Key = 1;
    required string Value = 2;
}  

message Log
{
    required uint32 Time = 1;// UNIX Time Format
    
    repeated LogContent Contents= 2;

}

message LogTag
{
    required string Key = 1;
    required string Value = 2;
}

message LogGroup
{
    repeated Log Logs= 1;
    optional string Category = 2;
    optional string Topic = 3;
    optional string Source = 4;
    optional string MachineUUID = 5;
    repeated LogTag LogTags = 6;
}

message SlsLogPackage
{
    required bytes data = 1;  // the serialized data of LogGroup , may be compressed
    optional int32 uncompress_size = 2;  
}

message SlsLogPackageList
{
    repeated SlsLogPackage packages = 1;
}

message LogGroupList
{
    repeated LogGroup LogGroups = 1;
}
```

**Note:** 

-   Protocol Buffer does not require the key-value pair to be unique. You must avoid such situation. Otherwise, the behavior is undefined.
-   For more information about Protocol Buffer format, see [Github](https://github.com/google/protobuf).
-   For more information about the API for writing logs to Log Service, see [PostLogStoreLogs](reseller.en-US/API Reference/Logstore related APIs/PostLogstoreLogs.md).

