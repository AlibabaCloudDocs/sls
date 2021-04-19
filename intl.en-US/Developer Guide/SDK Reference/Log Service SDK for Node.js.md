# Log Service SDK for Node.js

This topic describes how to install and use Log Service SDK for Node.js.

-   Log Service is activated. For more information, see [Activate Log Service](https://www.alibabacloud.com/product/log-service?spm=a2c5t.10695662.1996646101.searchclickresult.536d31bdPTqffd).
-   An AccessKey pair is created and obtained. For more information, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).
-   A Node.js development environment is installed.

## Step 1: Install Log Service SDK for Node.js

1.  [Download Log Service SDK for Node.js](https://github.com/aliyun-UED/aliyun-sdk-js).

2.  Run the following command to install Log Service SDK for Node.js:

    ```
    npm install aliyun-sdk
    ```


## Step 2: Create a project

The following example shows how to create a project. Express is used in the example.

1.  Install Express. For more information, see [Installing](http://expressjs.com/en/starter/installing.html).

2.  Install [morgan](https://www.npmjs.com/package/morgan).

3.  Create an app.js file and add the following code:

    ```
    var express = require('express')
    var morgan = require('morgan')
    var app = express()
    const logger = morgan(function (tokens, req, res) {
      return [
        tokens.method(req, res),
        tokens.url(req, res),
        tokens.status(req, res),
        tokens.res(req, res, 'content-length'), '-',
        tokens['response-time'](req, res), 'ms'
      ].join(' ')
    })
    app.use(logger)
    app.get('/', (req, res) => res.send('Hello World!'))
    app.listen(3000, () => console.log('Example app listening on port 3000!'))
    ```

4.  Run the following command to start the project:

    ```
    node app.js
    ```


## Step 3: Initialize Log Service SDK for Node.js

Add the following code to the app.js file to initialize Log Service SDK for Node.js:

```
   var sls = new ALY.SLS({
    "accessKeyId": "11****ut", // The AccessKey ID of your Alibaba Cloud account. For more information, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md). High security risks may arise if you use the AccessKey pair of your Alibaba Cloud account because the account has permissions to call all API operations. We recommend that you create and use a RAM user to call API operations or perform routine O&M. 
    "secretAccessKey": "TS****7Y", // The AccessKey secret of your Alibaba Cloud account. 
    endpoint: 'http://cn-hangzhou.log.aliyuncs.com', // The endpoint of Log Service. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md). The endpoint of the China (Hangzhou) region is used as an example. Replace it with the actual endpoint.
    apiVersion: '2015-06-01' // The version of the SDK. Set the value to 2015-06-01.
  })
```

## Sample code

Log Service SDK for Node.js provides multiple sample programs. For more information, see [aliyun-sdk-js](https://github.com/aliyun-UED/aliyun-sdk-js/tree/master/samples/sls). The following sample code shows how to create a Logstore:

```
sls.createLogstore({
    projectName: projectName, // The name of the project to which the Logstore belongs.
    logstoreDetail:{
        logstoreName:"test_logstore", // The Logstore name.
        ttl:3, // The data retention period. If you set ttl to 3650, data is permanently stored.
        shardCount:2 // The number of shards.
    }
})
```

