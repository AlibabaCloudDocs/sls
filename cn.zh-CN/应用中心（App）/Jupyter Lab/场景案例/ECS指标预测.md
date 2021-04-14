# ECS指标预测

通过Jupyter Lab中内置的模板，模拟ECS指标数据，并结合机器学习预测函数，预测指标趋势。本文介绍如何通过日志服务和机器学习来预测ECS指标的使用方法。

-   已创建保存时序库结果的日志库。更多信息，请参见[管理Logstore](/cn.zh-CN/数据采集/准备工作/管理Logstore.md)。
-   已创建接入模拟数据的时序库。更多信息，请参见[管理MetricStore](/cn.zh-CN/时序存储/管理MetricStore.md)。

## 步骤一：初始化日志服务Client

LogClient是日志服务的Python客户端，用于管理Project、Logstore等日志服务资源。使用Python SDK发起日志服务请求，您需要初始化一个Client实例。示例代码如下所示：

```
# Setup basic client
# !pip install -U matplotlib
import time
import json
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

import aliyun.log as sls

# 日志服务的服务入口。更多信息，请参见[服务入口](/cn.zh-CN/开发指南/API 参考/服务入口.md)。
endpoint = "cn-beijing.log.aliyuncs.com"

# 阿里云访问密钥AccessKey ID和AccessKey Secret。更多信息，请参见[访问密钥](/cn.zh-CN/开发指南/API 参考/访问密钥.md)。
accessId = "YOUR_ACCESS_ID"
accessKey= "YOUR_ACCESS_KEY"
# Project名称。
project  = "YOUR_SLS_PROJECT"
# MetricStore名称。
metricstore = "YOUR_SLS_METRICSTORE"
# 保存巡检结果的Logstore。
sink_logstore = 'YOUR_SLS_LOGSTORE_FOR_RESULTS_WRITE' 
# 设置任务名称。
task_name = "YOUR_TASK_NAME" 
# 创建LogClient。
client = sls.LogClient(endpoint, accessId, accessKey)
```

## 步骤二：写入ECS指标数据到日志服务

1.  通过代码生成ECS指标模拟数据。

    示例代码如下所示：

    ```
    #定义生成数据方法。
    class MockParam:
        def __init__(self, a, b, omega, phi, is_show=False):
            self.a = a
            self.b = b
            self.omega = omega
            self.phi = phi
            self.is_show = is_show
    
        def run(self, x_data):
            y_data = self.a * np.sin(self.omega / (2.0 * np.pi) * x_data + self.phi) + self.b
            if self.is_show:
                plt.figure(figsize=(20, 3))
                plt.plot(x_data, y_data)
                plt.show()
            return y_data
    
        
    def gen_mock_cpu_data(st_time, ed_time, step=150, mock_param=None):
        st_time = st_time // step * step
        ed_time = st_time + (ed_time - st_time) // step * step
        n = (ed_time - st_time) // step + 1
        x_data = np.linspace(st_time, ed_time, n)
        if mock_param is not None:
            y_data = mock_param.run(x_data)
            return x_data, y_data
        return None, None
    
    #调用并显示生成数据。
    end_time = int(time.mktime(time.localtime()))
    start_time = end_time - 24 * 60 * 60
    
    mock_param = MockParam(10, 1, 10, 1, is_show=True)
    x_data, y_data = gen_mock_cpu_data(start_time, end_time, mock_param=mock_param)
    
    print(len(x_data))
    print(len(y_data))
    ```

    生成的ECS指标模拟数据，如下图所示。

    ![data](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8358828161/p262925.png)

2.  获取已创建的Project和Logstroe。

    示例代码如下所示：

    ```
    import time
    from aliyun.log.logitem import LogItem
    from aliyun.log.putlogsrequest import PutLogsRequest
    from aliyun.log import IndexConfig
    
    
    # hostname: iZhp3fqc8ciryj43wtn76xZ
    # metricname: cpu_sys_util
    # value: 0.83
    
    hostname = "iZhp3fqc8ciryj43wtn76xZ"
    metric_name = "cpu_sys_util"
    
    # 获取Project和Logstore。
    print("create project & logstore")
    try:
        res = client.get_project(project)
    except:
        res = client.create_project(project, "a simple demo project")
    try:
        res = client.get_logstore(project, logstore)
    except:
        res = client.create_logstore(project, logstore, ttl=30, shard_count=2)
    
    # 开启索引。
    print("create index")
    request_json = {
         "keys": {
           "hostname": {
             "caseSensitive": False,
             "token": [
               ",", " ", "\"", "\"", ";", "=",  "(", ")", "[", "]",
               "{", "}", "?", "@", "&", "<", ">", "/", ":", "\n", "\t"
             ],
             "type": "text",
             "doc_value": True
           },
           "metricname": {
             "caseSensitive": False,
             "token": [
               ",", " ", "\"", "\"", ";", "=",  "(", ")", "[", "]",
               "{", "}", "?", "@", "&", "<", ">", "/", ":", "\n", "\t"
             ],
             "type": "text",
             "doc_value": True
           },
           "value": {
             "doc_value": True,
             "type": "long"
           }
         },
         "storage": "pg",
         "ttl": 2,
         "index_mode": "v2",
         "line": {
           "caseSensitive": False,
           "token": [
             ",", " ", "\"", "\"", ";", "=", "(", ")", "[", "]", "{",
             "}", "?", "@", "&", "<", ">", "/", ":", "\n", "\t"
           ]
         }
       }
    request = IndexConfig()
    request.from_json(request_json)
    res = client.create_index(project, logstore, request)
    res.log_print()
    print("wait for 1 minute")
    time.sleep(60)
    ```

3.  将ECS指标数据写入日志库。

    示例代码如下所示：

    ```
    # 写入日志数据。
    print("upload data")
    log_items = []
    for x, y in zip(x_data, y_data):
        log_time = int(x)
        log_content = list()
        log_content.append(("hostname", "{}".format(hostname)))
        log_content.append(("metricname", metric_name))
        log_content.append(("value", "{}".format(y)))
        log_item = LogItem(timestamp=log_time, contents=log_content)
        log_items.append(log_item)
    
    putReq = PutLogsRequest(project=project, logstore=logstore, topic=topic, logitems=log_items)
    res = client.put_logs(putReq)
    res.log_print()
    ```


## 步骤三：调测并输出预测结果

完成数据准备后，您可以编写代码调用机器学习函数，对数据进行预测分析，并输出预测结果。

1.  查询日志。

    示例代码如下：

    ```
    # Query SLS Logstore
    query = '''__topic__: PREDICT_DEMO and metricname: cpu_sys_util | select __time__ as t, value, hostname from log order by t limit 10000'''
    datas = []
    for i in client.get_log_all(project, logstore, start_time, end_time, query=query):
        for log in i.logs:
            datas.append(log.get_contents())
    
    df_ret = pd.DataFrame(datas)
    print(df_ret)
    ```

2.  使用ts\_predicate\_ar或者ts\_predicate\_arma函数进行预测，并输出结果。
    -   使用机器学习函数ts\_predicate\_ar进行预测，示例代码如下：

        ```
        query = '''__topic__: PREDICT_DEMO and metricname: cpu_sys_util | select ts_predicate_ar(t, value, 40, 100) from ( select __time__ as t, value from log order by t ) limit 10000'''
        datas = []
        for i in client.get_log_all(project, logstore, start_time, end_time, query=query):
            for log in i.logs:
                datas.append(log.get_contents())
        
        df_ret = pd.DataFrame(datas)
        
        # Visualize predicted values
        df_ret.set_index("unixtime", inplace=True)
        df_ret = df_ret.astype("double")
        print(df_ret)
        df_ret[["predict", "src"]].plot(figsize=(20, 3))
        ```

        ECS指标预测趋势，如下图所示。

        ![predict1](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8358828161/p262932.png)

    -   使用机器学习函数ts\_predicate\_arma进行预测，示例代码如下。

        ```
        query = '''__topic__: PREDICT_DEMO and metricname: cpu_sys_util | select ts_predicate_arma(t, value, 50, 1, 100) from ( select __time__ as t, value from log order by t ) limit 10000'''
        datas = []
        for i in client.get_log_all(project, logstore, start_time, end_time, query=query):
            for log in i.logs:
                datas.append(log.get_contents())
        
        df_ret = pd.DataFrame(datas)
        
        # Visualize predicted values
        df_ret.set_index("unixtime", inplace=True)
        df_ret = df_ret.astype("double")
        print(df_ret)
        df_ret[["predict", "src"]].plot(figsize=(20, 3))
        ```

        ECS指标预测趋势，如下图所示。

        ![data2](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8358828161/p262936.png)


