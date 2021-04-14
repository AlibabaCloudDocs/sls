# ECS指标流式智能巡检

通过Jupyter Lab中内置的模板，模拟ECS指标数据，并结合机器学习预测函数对指标趋势进行预测。本文介绍日志服务机器学习流式智能巡检的使用方法。

-   已创建保存时序库结果的日志库。更多信息，请参见[管理Logstore](/cn.zh-CN/数据采集/准备工作/管理Logstore.md)。
-   已创建接入模拟数据的时序库。更多信息，请参见[管理MetricStore](/cn.zh-CN/时序存储/管理MetricStore.md)。

## 步骤一：接入模拟数据

接入模拟主机监控数据，用于ECS指标流式智能巡检。

1.  登录[日志服务控制台](https://sls.console.aliyun.com)。

2.  在Project列表区域，单击目标Project。

3.  在左侧导航栏中，单击![metric](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4363418161/p261827.png)图标。

4.  选择已创建时序库对应的**数据接入** \> **模拟接入**，单击![jia](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5363418161/p261831.png)。

5.  在接入数据对话框中，单击**模拟**。

6.  在主机监控模拟接入页面，设置**时间间隔**为**1s**。

7.  单击**开始导入**。


## 步骤二：初始化日志服务Client

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

# SLS Endpoint列表。日志服务的服务入口。更多信息，请参见[服务入口](/cn.zh-CN/开发指南/API 参考/服务入口.md)。
endpoint = "cn-beijing.log.aliyuncs.com"

# 阿里云访问密钥AccessKey ID和AccessKey Secret。更多信息，请参见[访问密钥](/cn.zh-CN/开发指南/API 参考/访问密钥.md)。
accessId = "YOUR_ACCESS_ID"
accessKey= "YOUR_ACCESS_KEY"
#Project名称
project  = "YOUR_SLS_PROJECT"
# MetricStore名称。
metricstore = "YOUR_SLS_METRICSTORE"
#保存巡检结果的Logstore。
sink_logstore = 'YOUR_SLS_LOGSTORE_FOR_RESULTS_WRITE' 
 #设置任务名称。
task_name = "YOUR_TASK_NAME" 
#创建LogClient。
client = sls.LogClient(endpoint, accessId, accessKey)
```

## 步骤三：数据可视化

展示采集到的数据。需指定开始时间、结束时间和展示的指标名称。示例代码如下所示：

```
startTime = "2021-03-19 07:00:00"  # 设置模拟数据开始的时间，请根据当前模拟时间设置。
endTime = "2021-03-19 09:00:00"    # 设置可视化结束时间。
metric_name = 'cpu_util'          # 选择需要可视化的指标。更多信息，请参见[指标说明](/cn.zh-CN/时序存储/数据接入/采集主机监控数据.md)。

request = sls.GetLogsRequest(project, metricstore, fromTime=startTime, toTime=endTime, topic='',
                             query="* | select promql_query_range('{}') from metrics limit 10000".format(metric_name),
                             line=500, offset=0, reverse=False)
res = client.get_logs(request)

df = []
for x in res.get_logs():
    item = {}
    for k, v in x.get_contents().items():
        if k == 'labels':
            tmp = json.loads(v)
            for k_, v_ in tmp.items():
                item[k_] = v_
        else:
            item[k] = v
    df.append(item)
df = pd.DataFrame(df)
df['time'] = pd.to_datetime(df['time'], unit='ms', utc=True).dt.tz_convert('Asia/Shanghai')
df = df.set_index('time')
df['value'] = df['value'].astype(np.float64)

for name, group in df.groupby(['hostname', 'ip']):
    group['value'].plot(title='{} - {} - {}'.format(name[0], name[1], metric_name), figsize=(20, 3))
    plt.show()
```

数据可视化后，如下图所示。

![33](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3739128161/p262444.png)

## 步骤四：流式智能巡检

对采集数据进行流式智能巡检。示例代码如下所示：

```
fromTime = "2021-3-19 00:00:00"
fromStamp = int(time.mktime(time.strptime(fromTime, "%Y-%m-%d %H:%M:%S")))

# 配置数据
data_meta = {
    "query": "",                                     # the query to load time series data / no query to consume data
    "granularity": 60,                               # the granularity of time series. unit: second
    "columns": ["__time_nano__", "hostname", "ip",   # the keywords in time series data
                "cpu_util", "mem_util", "disk_util",
                "net_err_util", "system_load1"],
    "timestamp_name": "__time_nano__",               # a keyword in columns to indicate timestamp
    "entity_names": ["hostname", "ip"],              # a group of keywords in columns to indicate a curve entity
    "parent_names": ["hostname", "ip"],              # a group of keywords in entity_names to indicate a parent entity, for trace analysis (optional)
    "child_names": [],                               # a group of keywords in entity_names to indicate a child entity, for trace analysis (optional)
    "numeric_names": [                               # a category for each metric value, contain: name, upper value, down value
        {
            "numeric_name": "cpu_util",
            "upper_limit": 1e64,
            "down_limit": -1e64
        },
        {
            "numeric_name": "mem_util",
            "upper_limit": 1e64,
            "down_limit": -1e64
        },
        {
            "numeric_name": "disk_util",
            "upper_limit": 1e64,
            "down_limit": -1e64
        },
        {
            "numeric_name": "net_err_util",
            "upper_limit": 1e64,
            "down_limit": -1e64
        },
        {
            "numeric_name": "system_load1",
            "upper_limit": 1e64,
            "down_limit": -1e64
        }
    ],
    "is_sparse": False,                              # a sign to indicate data structure
}

# 配置算法
algo_meta = {
    "algo_items":[                                   # a group of algorithm configuations, one algorithm at least
        {
            "algo_type":7,                           # algorithm ID
            "refer_win_size":1200,                   # a window size of time series for algorithm training
            "delay_interval":60,                     # the interval for algorithm inference
            "parameter": json.dumps({                # algorithm paremeter
                "num_step": 10,                      # the number of value segmentation in time series
                "cycle": 2880                        # the cycle length of time series
            })
        }
    ]
}

# 配置计算资源
schedule_meta = {
    "from_stamp":fromStamp,                          # the start timestamp for time series data
    "to_stamp":-1,                                   # the end timestamp for time series data, default: -1
    "update_period":60,                              # the time duration for algorithm synchronization. unit:minute
    "tick_worker_number":1,                          # the number of tick worker for fetching data
    "model_worker_number":1,                         # the number of model worker for traning algorithm
    "cron_worker_number":1,                          #
    "only_show_anomaly":True                         # the sign to indicate whether only anomalies will be outputted or not
}

# ETL配置
configuration = {
    'accessKeyId': accessId,
    'accessKeySecret': accessKey,
    'fromTime': fromStamp,
    'toTime': 0,
    'logstore': metricstore,
    'parameters': {
        "sls.config.job_mode": json.dumps({"type":"ml"}),
        "config.ml.data_meta": json.dumps(data_meta),
        "config.ml.algo_meta": json.dumps(algo_meta),
        "config.ml.schedule_meta": json.dumps(schedule_meta)
    },
    'roleArn': '',
    'script': '',
    'sinks': [
        {
            'accessKeyId': accessId,
            'accessKeySecret': accessKey,
            'endpoint': '',
            'logstore': sink_logstore,
            'name': 'test',
            'project': project,
            'roleArn': '',
        }
    ],
    'version': 2
}
schedule = {
    'type': 'Resident'
}

# 为sinklogstore设置索引
request_json = {'line':{'token':[',',' ',"'",'"',';','=','(',')','[',']','{','}','?','@','&','<','>','/',':','\n','\t','\r'],'caseSensitive':False,'chn':False},'keys':{'__tag__:__job_name__':{'type':'text','token':[',',' ',"'",'"',';','=','(',')','[',']','{','}','?','@','&','<','>','/',':','\n','\t','\r'],'caseSensitive':False,'alias':'job_name','doc_value':True,'chn':False},'entity':{'type':'json','token':[',',' ',"'",'"',';','=','(',')','[',']','{','}','?','@','&','<','>','/',':','\n','\t','\r'],'caseSensitive':False,'alias':'','doc_value':True,'chn':False,'index_all':True,'max_depth':-1,'json_keys':{}},'meta':{'type':'json','token':[',',' ',"'",'"',';','=','(',')','[',']','{','}','?','@','&','<','>','/',':','\n','\t','\r'],'caseSensitive':False,'alias':'','doc_value':True,'chn':False,'index_all':True,'max_depth':-1,'json_keys':{'logstore_name':{'alias':'','caseSensitive':False,'chn':False,'doc_value':True,'token':[',',' ',"'",'"',';','=','(',')','[',']','{','}','?','@','&','<','>','/',':','\n','\t','\r'],'type':'text'},'project_name':{'alias':'','caseSensitive':False,'chn':False,'doc_value':True,'token':[',',' ',"'",'"',';','=','(',')','[',']','{','}','?','@','&','<','>','/',':','\n','\t','\r'],'type':'text'}}},'result':{'type':'json','token':[',',' ',"'",'"',';','=','(',')','[',']','{','}','?','@','&','<','>','/','\n','\t','\r'],'caseSensitive':False,'alias':'','doc_value':True,'chn':False,'index_all':True,'max_depth':-1,'json_keys':{'dim_name':{'alias':'','caseSensitive':False,'chn':False,'doc_value':True,'token':[',',' ',"'",'"',';','=','(',')','[',']','{','}','?','@','&','<','>','/',':','\n','\t','\r'],'type':'text'},'is_anomaly':{'alias':'','caseSensitive':False,'chn':False,'doc_value':True,'token':[',',' ',"'",'"',';','=','(',')','[',']','{','}','?','@','&','<','>','/',':','\n','\t','\r'],'type':'text'},'score':{'alias':'','doc_value':True,'type':'double'}}}},'log_reduce':False,'max_text_len':2048}
request = sls.IndexConfig()
request.from_json(request_json)
res = client.create_index(project, sink_logstore, request)

job_name = "111-" + task_name
job_display_name = job_name
job_discription = task_name

# 创建ETL任务
client.create_etl(project, job_name, configuration, schedule, job_display_name, job_discription)
```

## 步骤五：获取智能巡检结果

获取流式智能巡检结果。示例代码如下所示：

```
startTime = "2021-3-19 12:00:00"
endTime = "2021-3-19 23:00:00"

query = "* | select date_trunc('second', __time__) as time, json_extract(entity, '$.hostname') as hostname, json_extract(entity, '$.ip') as ip, json_extract(result, '$.dim_name') as metric_name, json_extract(result, '$.score') as anomaly_score, json_extract(result, '$.anomaly_type') as anomaly_type from log order by time desc limit 10000"
request = sls.GetLogsRequest(project, sink_logstore, fromTime=startTime, toTime=endTime, topic='',
                             query=query, line=500, offset=0, reverse=False)
res = client.get_logs(request)

df = []
for x in res.get_logs():
    item = {}
    for k, v in x.get_contents().items():
        item[k] = v
    df.append(item)
df = pd.DataFrame(df)
```

获取巡检结果如下图所示。

![host](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3739128161/p262445.png)

## （可选）步骤六：获取智能巡检异常结果

获取流式智能巡检异常结果。示例代码如下所示：

```
hostname = "iZ2ze931vw5cx6kunqnhgdZ"
metric_name = "mem_util"
anomaly_score = 0.5

query = "* and result.score > "+str(anomaly_score)+" and entity.hostname: "+hostname+" and result.dim_name: "+metric_name+" | select time, value, case when score is null then 0 else score end as score, case when score is null then 0 else 1 end as label from (select A.time, A.value, B.score from (( SELECT __time_nano__/1000000 as time, __value__ as value FROM \"metric-test.prom\" WHERE element_at(__labels__, 'hostname')='"+hostname+"' and __name__='"+metric_name+"') as A left join (select __time__ as time, \"result.score\" as score from log) as B on A.time = B.time)) limit 10000"

request = sls.GetLogsRequest(project, sink_logstore, fromTime=startTime, toTime=endTime, topic='',
                             query=query, line=500, offset=0, reverse=False)
res = client.get_logs(request)

df = []
for x in res.get_logs():
    item = {}
    for k, v in x.get_contents().items():
        item[k] = v
    df.append(item)
df = pd.DataFrame(df)
df['time'] = pd.to_datetime(df['time'], unit='s', utc=True).dt.tz_convert('Asia/Shanghai')
df[['value', 'score', 'label']] = df[['value', 'score', 'label']].astype(np.float64)

figs, ax = plt.subplots(figsize=(20, 3))
df.plot(x='time', y='value', title='{} - {}'.format(hostname, metric_name), ax=ax)
df_ = df[df['label'] > 0]
df_.plot.scatter(x='time', y='value', s=100, c='red', ax=ax)
plt.show()
```

获取巡检异常结果如下图所示。

![yichang](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3739128161/p262447.png)

