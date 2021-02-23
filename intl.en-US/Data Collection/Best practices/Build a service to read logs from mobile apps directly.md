# Build a service to read logs from mobile apps directly

In the era of mobile Internet, it is increasingly common to upload data through mobile apps. We expect that logs in mobile apps can be directly uploaded to Log Service, instead of being transferred by an app server, so that users can focus more on their business logic development.

In normal mode, the AccessKey of the Alibaba Cloud account is required when logs are written to Log Service for authentication and anti-tampering. If a mobile app accesses Log Service in this mode, you need to save your AccessKey information on the mobile client, which poses a data security risk of AccessKey disclosure. Once the AccessKey is disclosed, you must upgrade the mobile app and replace the AccessKey, which is too costly. Another approach for uploading logs from mobile clients to Log Service is to use users' app servers. However, in this mode, if the number of mobile apps is large, the app servers must carry all the data on mobile clients. This mode has a high requirement on the server specification.

To avoid the preceding problems, Log Service provides a more secure and convenient scheme to collect mobile app logs. It uses RAM to build a direct data transfer service for mobile apps based on mobile services. In contrast to the scheme of directly using the AccessKey to access Log Service, you do not need to store the AccessKey on the app side in this scheme. This eliminates the risk of AccessKey disclosure. A temporary token with a lifecycle gives more safety. You can also configure more complex access control policies for the token, for example, limiting the access permission of the IP address segment. The cost of this scheme is low. You do not need many app servers because the mobile apps are directly connected to the cloud platform and only the control flow is sent to the app server.

You can create a RAM role for operating Log Service and configure an app as a RAM user to assume this role, so that you can build a Log Service-based direct log transfer service for mobile apps within 30 minutes. Direct data transfer is a service that allows mobile apps to directly access Log Service, with only the control flow sent to the app server.

## Advantages

Using RAM to build a Log Service-based direct data transfer service for mobile apps has the following advantages:

-   This access mode is more secure, providing flexible and temporary permission assignment and authentication.
-   Fewer app servers are required, reducing the cost. The mobile apps are directly connected to the cloud platform and only the control flow is sent to the app server.
-   Log Service supports high concurrency. A large number of users can use the service at the same time. Large upload and download bandwidths are provided.
-   Log Service supports auto scaling, providing unlimited storage space.

The following figure shows the architecture.

![架构图](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1607549951/p32395.png)

Description

|Module|Description|
|:-----|:----------|
|Android or iOS mobile app|The app on the mobile phone of the end user and the log source.|
|LOG|Alibaba Cloud Log Service, responsible for storing log data uploaded by the app.|
|RAM or STS|Alibaba Cloud RAM, responsible for providing the user identity management and resource access control services, and generating temporary access credentials.|
|App server|The app background service developed for the Android or iOS apps. It manages tokens used by the app for data upload and download, and metadata information of data uploaded by users in the app.|

## Configuration process

1.  The app applies for a temporary access credential from the app server.

    To avoid the risk of information leakage, the Android or iOS app does not store the AccessKey ID and AccessKey secret. Therefore, the app must request a temporary upload credential \(a token\) from the app server. The token is only valid for a certain period. For example, if a token is set to be valid for 30 minutes \(which can be specified by the app server\), the Android or iOS app can use this token to access Log Service within the next 30 minutes. However, 30 minutes later, the app must request a new token.

2.  The app server checks the validity of the request and then returns a token to the app.
3.  After obtaining the token, the mobile app can access Log Service.

This topic describes how the app server applies for the token from the RAM service and how the Android or iOS app obtains the token.

## Procedure

## 1. Authorize a RAM role to operate Log Service.

Create a RAM role for operating Log Service and configure an app as a RAM user to assume this role. For more information, see [Assign a RAM role to an Alibaba Cloud account](/intl.en-US/Developer Guide/Access control RAM/Assign a RAM role to an Alibaba Cloud account.md).

After the configuration is complete, you can obtain the following information:

-   AccessKey ID and AccessKey secret of the RAM user
-   Resource path of the role

## 2. Build an app server.

This topic provides sample programs in multiple languages. The download URLs are listed at the end of this topic.

Each downloaded language package contains the configuration file config.json, which contains the following information:

```
{
    "AccessKeyID" : "",
    "AccessKeySecret" : "",
    "RoleArn" : "",
    "TokenExpireTime" : "900",
    "PolicyFile": "policy/write_policy.txt"
}
                
```

**Note:**

1.  AccessKeyID: the ID of your AccessKey.
2.  AccessKeySecret: the secret of your AccessKey.
3.  RoleArn: the resource path of the role.
4.  TokenExpireTime: the expiration time of the token obtained by the Android or iOS app. The minimum value is 900, in seconds. The default value can be left unchanged.
5.  PolicyFile: The file that lists the permissions that the token can grant. The default value does not need to be changed.

This topic describes two token files that define the most common permissions in the policy directory.

-   write\_policy.txt: specifies a token that grants the write permission for the project to this user.
-   readonly\_policy.txt: specifies a token that grants the read permission for the project to this user.

You can also design your policy file as required.

Response format

```
//Correct response
{
    "StatusCode":200,
    "AccessKeyId":"STS.3p***dgagdasdg",
    "AccessKeySecret":"rpnwO9***tGdrddgsR2YrTtI",
   "SecurityToken":"CAES+wMIARKAAZhjH0EUOIhJMQBMjRywXq7MQ/cjLYg80Aho1ek0Jm63XMhr9Oc5s˙∂˙∂3qaPer8p1YaX1NTDiCFZWFkvlHf1pQhuxfKBc+mRR9KAbHUefqH+rdjZqjTF7p2m1wJXP8S6k+G2MpHrUe6TYBkJ43GhhTVFMuM3BZajY3VjZWOXBIODRIR1FKZjIiEjMzMzE0MjY0NzM5MTE4NjkxMSoLY2xpZGSSDgSDGAGESGTETqOio6c2RrLWRlbW8vKgoUYWNzOm9zczoqOio6c2RrLWRlbW9KEDExNDg5MzAxMDcyNDY4MThSBTI2ODQyWg9Bc3N1bWVkUm9sZVVzZXJgAGoSMzMzMTQyNjQ3MzkxMTg2OTExcglzZGstZGVtbzI=",
   "Expiration":"2017-11-12T07:49:09Z",
}

//Error response
{
    "StatusCode":500,
    "ErrorCode":"InvalidAccessKeyId.NotFound",
    "ErrorMessage":"Specified access key is not found."
}

                
```

Correct response description: The following table describes the five variables that constitute a token.

|Variable|Description|
|:-------|:----------|
|StatusCode|The result returned when the app retrieves the token. The app returns 200 if the token is successfully retrieved.|
|AccessKeyId|The AccessKey ID that the Android or iOS app obtains when initializing LogClient.|
|AccessKeySecret|The AccessKey secret that the Android or iOS app obtains when initializing LogClient.|
|SecurityToken|The token that the Android or iOS app uses to access Log Service.|
|Expiration|The time period before the token expires. The Android SDK automatically determines the validity of the token and then retrieves a new one as needed.|

Error response description:

|Variable|Description|
|:-------|:----------|
|StatusCode|The result returned when the app retrieves the token. The app returns 500 if the token fails to be retrieved.|
|ErrorCode|The error cause.|
|ErrorMessage|The error description.|

Running method of the sample code:

For Java 1.7 or later, after downloading and decompressing the package, create a Java project, copy the dependency, code, and configuration to the project, and run the main function. By default, the program listens on port 7080 and waits for the HTTP request. Perform the operations in other languages in a similar way.

## 3. The mobile client constructs an HTTP request to obtain a token from the app server.

The formats of HTTP request and response are as follows:

```
Request URL: GET https://localhost:7080/

Response:
{
"StatusCode":"200",
"AccessKeyId":"STS.XXXXXXXXXXXXXXX",
"AccessKeySecret":"",
"SecurityToken":"",
"Expiration":"2017-11-20T08:23:15Z"
}
                
```

**Note:** All examples in this topic are used to demonstrate how to set up a server. You can implement custom development based on these examples.

## Source code download

Sample code of the app server: [PHP, Java, Ruby, Node.js](https://github.com/xiongcw/aliyun_log_sts_server_example).

