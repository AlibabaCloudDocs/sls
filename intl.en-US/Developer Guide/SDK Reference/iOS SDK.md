# iOS SDK

Log Service SDK for iOS is implemented based on the [Log Service API](/intl.en-US/Developer Guide/API Reference/Overview.md). You can use the SDK to write logs to Log Service.

Download link: [https://github.com/aliyun/aliyun-log-go-sdk](https://github.com/aliyun/aliyun-log-ios-sdk?spm=5176.doc43145.2.3.AKDn3Z).

## Swift

```
 /*
    Use an endpoint, AccessKey ID, and AccessKey secret to create a Log Service client.
    @endPoint: The endpoint of Log Service. For more information, visit https://www.alibabacloud.com/help/doc-detail/29008.htm
 */
let myClient = try! LOGClient(endPoint: "",
                              accessKeyID: "",
                              accessKeySecret: "",
                              projectName:"")
/* Create a log group. */
let logGroup = try! LogGroup(topic: "mTopic",source: "mSource")
   /* Write a log to the log group. */
    let log1 = Log()
          try! log1.PutContent("K11", value: "V11")
         try! log1.PutContent("K12", value: "V12")
         try! log1.PutContent("K13", value: "V13")
    logGroup.PutLog(log1)
   /* Write a log to the log group. */
    let log2 = Log()
          try! log2.PutContent("K21", value: "V21")
         try! log2.PutContent("K22", value: "V22")
         try! log2.PutContent("K23", value: "V23")
    logGroup.PutLog(log2)
 /* Send the log. */
 myClient.PostLog(logGroup,logStoreName: ""){ response, error in
        // handle response however you want
        if error?.domain == NSURLErrorDomain && error?.code == NSURLErrorTimedOut {
            print("timed out") // note, `response` is likely `nil` if it timed out
        }
    }
```

## Objective-C

For more information, see [https://github.com/lujiajing1126/AliyunLogObjc](https://github.com/lujiajing1126/AliyunLogObjc).

