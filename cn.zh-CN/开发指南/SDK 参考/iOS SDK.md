# iOS SDK

本文介绍安装日志服务iOS SDK的示例代码。

## Swift

Swift示例代码如下所示。更多信息，请参见[aliyun-log-ios-sdk](https://github.com/aliyun/aliyun-log-ios-sdk?spm=5176.doc43145.2.3.AKDn3Z)。

```
 /*
    构建日志服务客户端。
    endPoint为服务访问入口。更多信息，请参见[服务入口](/cn.zh-CN/开发指南/API 参考/服务入口.md)。
    accessKeyID和accessKeySecret为阿里云访问密钥。更多信息，请参见[访问密钥](/cn.zh-CN/开发指南/API 参考/访问密钥.md)。
 */
let myClient = try! LOGClient(endPoint: "",
                              accessKeyID: "",
                              accessKeySecret: "",
                              projectName:"")
/* 创建日志组。 */
let logGroup = try! LogGroup(topic: "mTopic",source: "mSource")
   /* 写入一条日志。 */
    let log1 = Log()
          try! log1.PutContent("K11", value: "V11")
         try! log1.PutContent("K12", value: "V12")
         try! log1.PutContent("K13", value: "V13")
    logGroup.PutLog(log1)
   /* 写入一条日志。 */
    let log2 = Log()
          try! log2.PutContent("K21", value: "V21")
         try! log2.PutContent("K22", value: "V22")
         try! log2.PutContent("K23", value: "V23")
    logGroup.PutLog(log2)
 /* 发送日志。 */
 myClient.PostLog(logGroup,logStoreName: ""){ response, error in
        // handle response however you want
        if error?.domain == NSURLErrorDomain && error?.code == NSURLErrorTimedOut {
            print("timed out") // note, `response` is likely `nil` if it timed out
        }
    }
```

## Objective-C

更多信息，请参见[AliyunLogObjc](https://github.com/aliyun/aliyun-log-ios-sdk)。

