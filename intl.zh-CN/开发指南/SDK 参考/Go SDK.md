# Go SDK

本文介绍安装日志服务Go SDK及使用Go SDK完成常见操作的相关步骤。

-   已开通日志服务。更多信息，请参见[开通日志服务](https://www.alibabacloud.com/product/log-service?spm=a2c5t.10695662.1996646101.searchclickresult.536d31bdPTqffd)。
-   已创建并获取AccessKey。更多信息，请参见[访问密钥](/intl.zh-CN/开发指南/API 参考/访问密钥.md)。
-   已安装Golang环境。

## 步骤1：安装Go SDK

执行以下命令安装Go SDK：

```
go get -u github.com/aliyun/aliyun-log-go-sdk
```

**说明：** 安装过程中，界面不会打印提示，请耐心等待。如果安装超时，请再次执行以上命令。

## 步骤2：创建日志服务Client

日志服务Client是日志服务的Go客户端，用于管理Project、Logstore等日志服务资源。使用Go SDK发起日志服务请求，您需要初始化一个Client实例。

```
AccessKeyID = "your_accesskey_id"             //阿里云访问密钥AccessKey ID。更多信息，请参见[访问密钥](/intl.zh-CN/开发指南/API 参考/访问密钥.md)。阿里云主账号AccessKey拥有所有API的访问权限，风险很高。强烈建议您创建并使用RAM账号进行API访问或日常运维。
AccessKeySecret = "your_accesskey_secret"     //阿里云访问密钥AccessKey Secret。
Endpoint = "cn-hangzhou.log.aliyuncs.com"     //日志服务的域名。更多信息，请参见[服务入口](/intl.zh-CN/开发指南/API 参考/服务入口.md)。此处以杭州为例，其它地域请根据实际情况填写。 
Client = sls.CreateNormalInterface(Endpoint,AccessKeyID,AccessKeySecret,"")   //创建Client。
```

## Go SDK示例

日志服务Go SDK提供丰富的示例程序，方便参考或直接使用，更多信息，请参见[aliyun-log-go-sdk](https://github.com/aliyun/aliyun-log-go-sdk)。此处以创建Project和Logstore为例进行说明，示例代码如下所示：

```
project, err := client.CreateProject("project_name","description")   //输入Project名称和描述。
    if err != nil {
        fmt.Println(err)
    }
    fmt.Println(project)
    err = client.CreateLogStore("project_name","logstore_name",2,2,true,16)    //输入Project名称、Logstore名称、数据保存时长、Shard数量、开启自动分裂Shard功能和最大分裂数。如果数据保存时长配置为3650，表示永久保存。
    if err != nil {
        fmt.Println(err)
    }
```

