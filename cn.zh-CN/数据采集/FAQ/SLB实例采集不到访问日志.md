# SLB访问日志采集不到

如果发现无法正常采集SLB访问日志，请按照以下步骤排查。

## 1. 确认是否为SLB实例开通了访问日志

每个SLB实例需要单独设置，开通后的产生的访问日志将实时写入您的日志服务Logstore。

请登录SLB控制台， 在左侧导航栏中单击**日志管理** \> **访问日志**，查看**访问日志（7层）**列表中。

-   确认列表中是否存在指定SLB实例。
-   确认SLB实例对应的**SLS日志存储**一列中记录的日志保存位置。

    此处显示的是日志保存的日志服务Project和Logstore，请在正确的位置查看是否存在SLB日志。


## 2. 确认RAM授权是否正确

开通访问日志功能时，系统会指引您完成RAM角色授权，成功授权后才能开通访问日志功能。如果RAM角色没有正常创建、或创建后被删除，都会导致日志采集后无法投递到日志服务Logstore。

**排查方式**

请登录[RAM控制台](https://ram.console.aliyun.com/#/role/list)，在**角色管理**页面查找是否存在AliyunLogArchiveRole。

-   **如果AliyunLogArchiveRole不存在**，请使用主账号登录后并单击[快速授权链接](https://ram.console.aliyun.com/#/role/authorize?request=%7B%22Requests%22%3A%20%7B%22request1%22%3A%20%7B%22RoleName%22%3A%20%22AliyunLogArchiveRole%22%2C%20%22TemplateId%22%3A%20%22Archive%22%7D%7D%2C%20%22ReturnUrl%22%3A%20%22https%3A//sls.console.aliyun.com/%22%2C%20%22Service%22%3A%20%22Log%22%7D)，完成授权所需要的RAM角色创建。

-   **如果AliyunLogArchiveRole存在**，请单击角色名称，查看角色授权策略是否正确。

    以下是默认的授权策略，如果您的策略与默认策略不相同，可能之前修改过授权策略，请将授权策略改为默认的授权策略。

    ```
    {
      "Version": "1",
      "Statement": [
        {
          "Action": [
            "log:PostLogStoreLogs"
          ],
          "Resource": "*",
          "Effect": "Allow"
        }
      ]
    }
    ```


## 3. 确认是否产生日志事件

如果在日志服务控制台没有查看到SLB访问日志，可能是由于没有日志产生。例如：

-   当前实例未配置七层监听。

    **目前只支持SLB七层监听的实例访问日志**，暂不支持四层实例日志采集。常见的七层监听协议有HTTP/HTTPS等，详细说明请查看[监听概述](/cn.zh-CN/传统型负载均衡CLB/CLB用户指南/监听/监听概述.md)。

-   不采集开通功能之前的历史日志。

    开通SLB访问日志功能之后，从当前时间开始采集日志。

-   指定实例没有访问请求。

    必须对实例上的监听进行访问请求，才会产生访问日志。


