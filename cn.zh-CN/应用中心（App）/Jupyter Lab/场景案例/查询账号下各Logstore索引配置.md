# 查询账号下各Logstore索引配置

查看已开启索引的Logstore列表和索引字段信息，有助于您梳理日志库索引的配置情况，便于您对索引进行增减配置。本文介绍如何批量查询索引配置的操作方法。

## 步骤一：获取服务入口

示例代码如下所示：

```
# Setup basic client
from aliyun.log.logclient import LogClient

# 日志服务的服务入口。更多信息，请参见[服务入口](/cn.zh-CN/开发指南/API 参考/服务入口.md)。
ALL_ENDPOINTS = ["https://ap-northeast-1.log.aliyuncs.com",
                 "https://ap-south-1.log.aliyuncs.com",
                 "https://ap-southeast-1.log.aliyuncs.com",
                 "https://ap-southeast-2.log.aliyuncs.com",
                 "https://ap-southeast-3.log.aliyuncs.com",
                 "https://ap-southeast-5.log.aliyuncs.com",
                 "https://cn-beijing.log.aliyuncs.com",
                 "https://cn-chengdu.log.aliyuncs.com",
                 "https://cn-guangzhou.log.aliyuncs.com",
                 "https://cn-hangzhou.log.aliyuncs.com",
                 "https://cn-heyuan.log.aliyuncs.com",
                 "https://cn-hongkong.log.aliyuncs.com",
                 "https://cn-huhehaote.log.aliyuncs.com",
                 "https://cn-north-2-gov-1.log.aliyuncs.com",
                 "https://cn-qingdao.log.aliyuncs.com",
                 "https://cn-shanghai.log.aliyuncs.com",
                 "https://cn-shenzhen.log.aliyuncs.com",
                 "https://cn-wulanchabu.log.aliyuncs.com",
                 "https://cn-zhangjiakou.log.aliyuncs.com",
                 "https://eu-central-1.log.aliyuncs.com",
                 "https://eu-west-1.log.aliyuncs.com",
                 "https://me-east-1.log.aliyuncs.com",
                 "https://rus-west-1.log.aliyuncs.com",
                 "https://us-east-1.log.aliyuncs.com",
                 "https://us-west-1.log.aliyuncs.com"]

# 阿里云访问密钥AccessKey ID和AccessKey Secret。更多信息，请参见[访问密钥](/cn.zh-CN/开发指南/API 参考/访问密钥.md)。
accessId = "YOUR_ACCESS_ID"
accessKey= "YOUR_ACCESS_KEY"
```

## 步骤二：查询日志库

示例代码如下：

```
import time
import pandas as pd
from aliyun.log.logexception import LogException
from aliyun.log.listlogstoresrequest import ListLogstoresRequest 

index_config = None

def get_logstore_index(endpoint):
    client = LogClient(endpoint, accessId, accessKey)

    # 获取Project清单。
    res = client.list_project()
    projects = res.get_projects()

    # 获取每个Project的日志库Logstore。
    datas = []
    for i in projects:
        project = i['projectName']
        res = client.list_logstores(ListLogstoresRequest(project=project))
        for logstore in res.get_logstores():
            try:
                index_config = client.get_index_config(project , logstore)
                index_config_json = index_config.get_index_config().to_json()
            except LogException as ex:
                if ex.get_error_code() == 'IndexConfigNotExist':
                    continue
            
            if 'keys' in index_config_json:
                for column in index_config_json['keys'].keys():
                    datas.append({
                        'region' : i['region'],
                        'project' : project,
                        'logstore' : logstore,
                        'index_type' : 'column',
                        'index' : column
                    })
            if 'log_reduce' in index_config_json and index_config_json['log_reduce'] == True:
                datas.append({
                        'region' : i['region'],
                        'project' : project,
                        'logstore' : logstore,
                        'index_type' : 'logreduce',
                        'index' : "True"
                })
            if 'line' in index_config_json:
                datas.append({
                        'region' : i['region'],
                        'project' : project,
                        'logstore' : logstore,
                        'index_type' : 'fullindex',
                        'index' : "True"
                })
    return datas

datas = []
for endpoint in ALL_ENDPOINTS:
    datas.extend(get_logstore_index(endpoint))
df_all_logstore_index_detail = pd.DataFrame(datas)

df_all_logstore_with_index = df_all_logstore_index_detail.groupby(['region',
                                    'project']).agg({'logstore':'max'}).reset_index()

# 展示具体Logstore的所有索引配置。
df_all_logstore_index_detail

# 已开启索引的Logstore列表。
df_all_logstore_with_index
```

## 步骤三：将索引配置信息导出到文件

示例代码如下：

```
# 将已开启索引的Logstore列表导出到文件。
df_all_logstore_with_index.to_csv("all_logstore_with_index.csv",sep=';',index=False)

# Logstore中已开启索引的字段详情导出到文件。
df_all_logstore_index_detail.to_csv("all_logstore_index_detail.csv",sep=';',index=False)
```

