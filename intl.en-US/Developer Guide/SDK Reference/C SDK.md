# C SDK

The C SDK of Alibaba Cloud Log Service is used to fix the log access problems on various platforms, for example, how Log Service can be compatible on platforms with MIPS chips and the OpenWrt system.

The C SDK uses libcurl as the network library and uses the apr/apr-util library to fix the problems of memory management and cross-platform operations. You only need to compile the source code to use the C SDK.

In addition, the C Producer Library and C Producer Lite Library provide you with a one-stop log collection solution that is simple, highly available, resource-efficient, and applicable to different platforms.

**Note:** The C SDK only allows you to write data, but does not support operations such as creating resources and obtaining data.

For more information, see the following topics on GitHub:

-   [C Producer Library \(recommended for servers\)](https://github.com/aliyun/aliyun-log-c-sdk)
-   [C Producer Lite Library \(recommended for IoT and smart devices\)](https://github.com/aliyun/aliyun-log-c-sdk/tree/lite)
-   [Native API operations of the C SDK \(recommended for secondary development\)](https://github.com/aliyun/aliyun-log-c-sdk/blob/master/inner_interface.md)

