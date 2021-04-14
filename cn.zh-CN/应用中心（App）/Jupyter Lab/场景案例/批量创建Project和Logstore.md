# 批量创建Project和Logstore

本文介绍批量创建Project和Logstore的操作方法。

## 步骤一：初始化日志服务Client

LogClient是日志服务的Python客户端，用于管理Project、Logstore等日志服务资源。使用Python SDK发起日志服务请求，您需要初始化一个Client实例。示例代码如下所示：

```
# Setup basic client
from aliyun.log.logclient import LogClient

# 日志服务的服务入口。更多信息，请参见[服务入口](/cn.zh-CN/开发指南/API 参考/服务入口.md)。
endpoint = "cn-huhehaote.log.aliyuncs.com"

# 阿里云访问密钥AccessKey ID和AccessKey Secret。更多信息，请参见[访问密钥](/cn.zh-CN/开发指南/API 参考/访问密钥.md)。
accessId = "YOUR_ACCESS_ID"
accessKey= "YOUR_ACCESS_KEY"

client = LogClient(endpoint, accessId, accessKey)
```

## 步骤二：创建Project和Logstore

示例代码如下：

1.  批量配置Project和Logstore信息。

    ```
    import time
    from aliyun.log.listlogstoresrequest import ListLogstoresRequest 
    
    # config project and logstore to create
    project_logstores = {
        "your-project-name" : {
            "project_description" : "project description",
            "logstores" : ["logstore1","logstore2"]
        }
    }
    ```

2.  批量创建Project和Logstore。

    ```
    # Get Project List
    def get_project_names():
        res = client.list_project()
        projects = res.get_projects()
    
        return [i['projectName'] for i in projects]
    
    # Get Logstore under project
    def get_project_logstores(project):
        res = client.list_logstores(ListLogstoresRequest(project=project))
        return res.get_logstores()
    
    project_names = get_project_names()
    for i in project_logstores:
        project = i
        project_description = project_logstores[i]['project_description']
        logstores = project_logstores[i]['logstores']
        
        if project not in project_names:
            client.create_project(project, project_description)
            time.sleep(0.1)
            print("create project %s" % project)
        else:
            print("project %s is already exists" % project)
        logstores_now = get_project_logstores(project)
        
        for logstore in logstores:
            if logstore not in logstores_now:
                print("create logstore %s" % logstore)
                client.create_logstore(project, logstore)
            else:
                print("logstore %s is already exists" % logstore)
    ```


