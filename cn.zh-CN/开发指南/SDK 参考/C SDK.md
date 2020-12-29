# C SDK

阿里云日志服务C SDK主要解决各种平台的日志接入问题，比如兼容MIPS芯片、OpenWrt系统等。

C SDK 使用 cURL 作为网络库，apr/apr-util 库解决内存管理以及跨平台问题。您只需要基于源码进行简单编译便可以使用。

此外我们还有C版本的Producer Library和Producer Lite Library，为您提供跨平台、简洁、高性能、低资源消耗的一站式日志采集方案。

**说明：** C SDK只支持数据写入，不支持其他创建资源、获取数据等接口。

Github项目地址以及更多详细说明参见：

-   [C Producer Library（推荐服务端使用）](https://github.com/aliyun/aliyun-log-c-sdk)
-   [C Producer Lite Library（推荐IOT、智能设备使用）](https://github.com/aliyun/aliyun-log-c-sdk/tree/lite)
-   [C 原生API（推荐二次开发使用）](https://github.com/aliyun/aliyun-log-c-sdk/blob/master/inner_interface.md)

