# Node.js SDK

本文介绍安装日志服务Node.js SDK及使用Node.js SDK完成常见操作的相关步骤。

-   已开通日志服务。更多信息，请参见[开通日志服务](https://www.alibabacloud.com/product/log-service?spm=a2c5t.10695662.1996646101.searchclickresult.536d31bdPTqffd)。
-   已创建并获取AccessKey。更多信息，请参见[访问密钥](/intl.zh-CN/开发指南/API 参考/访问密钥.md)。
-   已安装Node.js开发环境。

## 步骤1：安装Node.js SDK

1.  [下载Node.js SDK](https://github.com/aliyun-UED/aliyun-sdk-js)。

2.  执行如下命令安装Node.js SDK。

    ```
    npm install aliyun-sdk
    ```


## 步骤2：搭建项目

本文以使用Express搭建项目为例。

1.  安装Express。更多信息，请参见[installing](http://expressjs.com/en/starter/installing.html)。

2.  安装[morgan](https://www.npmjs.com/package/morgan)。

3.  创建app.js文件并写入如下代码。

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

4.  执行如下命令启动项目。

    ```
    node app.js
    ```


## 步骤3：初始化

在app.js文件中写入如下代码，完成Node.js SDK初始化。

```
   var sls = new ALY.SLS({
    "accessKeyId": "11****ut",         //阿里云访问密钥AccessKey ID。更多信息，请参见[访问密钥](/intl.zh-CN/开发指南/API 参考/访问密钥.md)。阿里云主账号AccessKey拥有所有API的访问权限，风险很高。强烈建议您创建并使用RAM账号进行API访问或日常运维。 
    "secretAccessKey": "TS****7Y",    //阿里云访问密钥AccessKey Secret。 
    endpoint: 'http://cn-hangzhou.log.aliyuncs.com',   //日志服务的域名。更多信息，请参见[服务入口](/intl.zh-CN/开发指南/API 参考/服务入口.md)。此处以杭州为例，其它地域请根据实际情况填写。
    apiVersion: '2015-06-01'                           //SDK版本号，固定值。
  })
```

## 示例

日志服务Node.js SDK提供丰富的示例程序，方便参考或直接使用。更多信息，请参见[aliyun-sdk-js](https://github.com/aliyun-UED/aliyun-sdk-js/tree/master/samples/sls)。此处以创建Logstore为例进行说明，示例代码如下所示：

```
sls.createLogstore({
    projectName: projectName,                 //待创建的Logstore所属的Project名称。
    logstoreDetail:{
        logstoreName:"test_logstore",        //设置Logstore名称。
        ttl:3,                               //设置数据保存时长，如果ttl配置为3650，表示永久保存。
        shardCount:2                         //设置Shard数量。
    }
})
```

