# Log Service SDK for IOS

Alibaba Cloud Log Service SDKs are implemented based on [Overview](/intl.en-US/Developer Guide/API Reference/Overview.md) and currently provide the log writing function.

GitHub: [https://github.com/aliyun/aliyun-log-ios-sdk](https://github.com/aliyun/aliyun-log-ios-sdk?spm=5176.doc43145.2.3.AKDn3Z)

## Swift

```
 /*
    Use the endpoint, AccessKey ID, and AccessKey Secret to build the Log Service client.
    @endPoint: see https://www.alibabacloud.com/help/doc-detail/29008.htm
 */
let myClient = try! LOGClient(endPoint: "",
                              accessKeyID: "",
                              accessKeySecret: "",
                              projectName:"")
/* Create a log group. */
Let loggroup = try! Loggroup (topic: "mtopic", source: "msource ")
      /* Store a log. */
    let log1 = Log()
          try! log1. PutContent("K11", value: "V11")
         try! log1. PutContent("K12", value: "V12")
         try! log1. PutContent("K13", value: "V13")
    logGroup.PutLog(log1)
   /* Store a log. */
    Let log2 = Log ()
          try! log2. Putcontent ("k21", value: "V21 ")
         try! log2. PutContent("K22", value: "V22")
         try! log2. PutContent("K22", value: "V22")
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

See GitHub: [https://github.com/lujiajing1126/AliyunLogObjc](https://github.com/lujiajing1126/AliyunLogObjc)

