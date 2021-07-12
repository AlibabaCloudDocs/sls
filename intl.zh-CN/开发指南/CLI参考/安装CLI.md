# 安装CLI

日志服务命令行工具CLI支持Project管理、Logstore管理、日志查询和多账户跨域复制等场景。本文介绍安装日志服务命令行工具CLI的操作方法。

## Python版本和运行环境

-   当前CLI版本：0.1.17
-   工具源码：[Aliyun Log CLI](https://github.com/aliyun/aliyun-log-cli)
-   支持Python版本

    Python 2.6、2.7、3.3、3.4、3.5、3.6、3.7、3.8和3.9。

-   运行环境

    Windows、Linux和macOS。


## 在Linux系统安装CLI

1.  登录Linux服务器。

2.  执行如下命令安装CLI。

    ```
    pip3 install aliyun-log-python-sdk aliyun-log-cli -U --no-cache
    ```

3.  验证安装结果。

    运行命令如下：

    ```
    aliyunlog --version
    ```

    如果返回如下类似结果，表示安装完成。

    ```
    log-cli-v-0.1.17, log-python-sdk-v-0.6.53, sys.version_info(major=3, minor=6, micro=8, releaselevel='final', serial=0), linux
    ```


## 在Windows系统安装CLI

1.  登录Windows服务器。

2.  打开Windows命令行。

3.  执行如下命令安装CLI。

    ```
    pip3 install aliyun-log-python-sdk aliyun-log-cli -U --no-cache
    ```

4.  验证安装结果。

    运行命令如下：

    ```
    aliyunlog --version
    ```

    如果返回如下类似结果，表示安装完成。

    ```
    log-cli-v-0.1.17, log-python-sdk-v-0.6.53, sys.version_info(major=3, minor=6, micro=8, releaselevel='final', serial=0), linux
    ```


## 在mac OS系统安装CLI

推荐使用pip3安装日志服务命令工具CLI，参考命令如下：

```
brew install python3
pip3 install -U aliyun-log-cli --no-cache
```

其他操作和Linux系统安装相似，请参见[在Linux系统安装CLI](#section_f83_jhe_v3s)。

[配置CLI](/intl.zh-CN/开发指南/CLI参考/配置CLI.md)

