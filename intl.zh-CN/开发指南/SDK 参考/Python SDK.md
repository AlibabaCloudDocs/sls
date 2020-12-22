# Python SDK

本文介绍安装日志服务Python SDK及使用Python SDK完成常见操作的相关步骤。

-   已开通日志服务。更多信息，请参见[开通日志服务](https://www.alibabacloud.com/product/log-service?spm=a2c5t.10695662.1996646101.searchclickresult.536d31bdPTqffd)。
-   已创建并获取AccessKey。更多信息，请参见[访问密钥](/intl.zh-CN/开发指南/API 参考/访问密钥.md)。
-   已安装Python开发环境。

    日志服务Python SDK支持Pypy2、Pypy3、Python2.6、Python2.7、Python3.3、Python3.4、Python3.5、Python3.6和Python3.7版本。您可以执行python -V命令检查您已安装的Python版本。如果未安装，请从[Python官网](https://www.python.org/downloads/)下载安装包并完成安装。

-   已安装Python的包管理工具Pip。

    您可以执行pip -V命令检查您已安装的Pip版本。如果未安装，请从[Pip官网](https://pip.pypa.io/en/latest/installing/)下载安装包并完成安装。

    **说明：** 在Windows环境中，如果提示**不是内部或外部命令**，请在环境变量中编辑Path，增加Python和pip的安装路径。pip的安装路径一般为Python所在目录的Scripts文件夹。配置完成后，您可能需要重启电脑使环境变量生效。


## 步骤1：安装Python SDK

Python SDK支持所有运行Python的操作系统，例如Linux、macOS X和Windows。

1.  在命令行工具中，以管理员身份执行如下命令安装Python SDK。

    ```
    pip install -U aliyun-log-python-sdk
    ```

2.  验证安装结果。

    如果提示如下类似信息，表示安装Python SDK成功。

    ![python sdk](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1482564061/p179978.png)


## 步骤2：创建日志服务Client

LogClient是日志服务的Python客户端，用于管理Project、Logstore等日志服务资源。使用Python SDK发起日志服务请求，您需要初始化一个Client实例。

**说明：** 如果您要使用https连接，则需在endpoint中加入https://前缀，例如https://cn-hangzhou.log.aliyuncs.com。

```
from aliyun.log import LogClient
endpoint = 'cn-hangzhou.log.aliyuncs.com'   #日志服务的域名。更多信息，请参见[服务入口](/intl.zh-CN/开发指南/API 参考/服务入口.md)。此处以杭州为例，其它地域请根据实际情况填写。
accessKeyId = 'your_access_id'    #阿里云访问密钥AccessKey ID。更多信息，请参见[访问密钥](/intl.zh-CN/开发指南/API 参考/访问密钥.md)。阿里云主账号AccessKey拥有所有API的访问权限，风险很高。强烈建议您创建并使用RAM账号进行API访问或日常运维。
accessKey = 'your_access_key'     #阿里云访问密钥AccessKey Secret。
client = LogClient(endpoint, accessKeyId, accessKey)     #创建LogClient。
```

## Python SDK示例

日志服务Python SDK提供丰富的示例程序，方便参考或直接使用，更多信息，请参见[aliyun-log-python-sdk](https://github.com/aliyun/aliyun-log-python-sdk)。此处以创建Project和Logstore为例进行说明，示例代码如下所示：

```
res = client.create_project("my-project", "description") #输入Project名称和描述。
res = client.create_logstore('my-project', 'my-logstore', ttl=30, shard_count=3)   #输入Project名称、Logstore名称、数据保存时长和Shard数据。如果ttl配置为3650，表示永久保存。
res.log_print() 
```

