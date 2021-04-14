# 查询Project和Logstore列表

本文介绍批量查询Project和Logstore的操作方法。

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

## 步骤二：查询Project和Logstore列表

示例代码如下：

```
import time
import pandas as pd
from aliyun.log.listlogstoresrequest import ListLogstoresRequest 
    
def get_projects(endpoint):
    client = LogClient(endpoint, accessId, accessKey)
    # Get Project List
    res = client.list_project()
    projects = res.get_projects()


    # 查询每个Project下的Logstore列表。
    datas = []
    for i in projects:
        res = client.list_logstores(ListLogstoresRequest(project=i['projectName']))
        for logstore in res.get_logstores():
            datas.append({
                'region' : i['region'],
                'project' : i['projectName'],
                'logstore' : logstore,
            })
            time.sleep(0.01)
    return datas

datas = []
for endpoint in ALL_ENDPOINTS:
    print("get projects from %s" % endpoint)
    datas.extend(get_projects(endpoint))

df_project_logstores = pd.DataFrame(datas)

# 可视化展示所有Project和Logstore列表。
df_project_logstores
```

