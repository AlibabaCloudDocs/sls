# 采集Kubernetes事件

本文档主要介绍如何使用eventer将Kubernetes中的事件采集到日志服务。

日志服务支持通过eventer方式和K8s事件中心采集Kubernetes中的事件。

-   eventer方式

    Kubernetes事件采集相关源码请参见[GitHub](https://github.com/AliyunContainerService/heapster/tree/master/events)。

-   K8s事件中心（推荐）

    日志服务还支持通过日志服务控制台的K8s事件中心采集Kubernetes中的事件并自动创建可视化报表和相关告警，详情请参见[创建并使用Kubernetes事件中心](/intl.zh-CN/应用中心（App）/K8S事件中心/创建并使用Kubernetes事件中心.md)。


## 创建采集配置

**说明：**

-   如果您使用的是阿里云Kubernetes，请参见[创建并使用Kubernetes事件中心](/intl.zh-CN/应用中心（App）/K8S事件中心/创建并使用Kubernetes事件中心.md)。
-   如果您使用的是自建Kubernetes，需配置部署示例中的以下参数：endpoint、project、logStore、regionId、internal、accessKeyId和accessKeySecret。

部署示例如下所示。

```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    name: kube-eventer
  name: kube-eventer
  namespace: kube-system
spec:
  replicas: 1
  selector:
    matchLabels:
      app: kube-eventer
  template:
    metadata:
      labels:
        app: kube-eventer
      annotations:
        scheduler.alpha.kubernetes.io/critical-pod: ''
    spec:
      dnsPolicy: ClusterFirstWithHostNet
      serviceAccount: kube-eventer
      containers:
        - image: registry.aliyuncs.com/acs/kube-eventer-amd64:v1.1.0-c93a835-aliyun
          name: kube-eventer
          command:
            - "/kube-eventer"
            - "--source=kubernetes:https://kubernetes.default"
            ## .send to sls
            ## --sink=sls:https://{endpoint}?project={project}&logStore=k8s-event&regionId={region-id}&internal=false&accessKeyId={accessKeyId}&accessKeySecret={accessKeySecret}
            - --sink=sls:https://cn-beijing.log.aliyuncs.com?project=k8s-xxxx&logStore=k8s-event&regionId=cn-beijing&internal=false&accessKeyId=xxx&accessKeySecret=xxx
          env:
          # If TZ is assigned, set the TZ value as the time zone
          - name: TZ
            value: "Asia/Shanghai" 
          volumeMounts:
            - name: localtime
              mountPath: /etc/localtime
              readOnly: true
            - name: zoneinfo
              mountPath: /usr/share/zoneinfo
              readOnly: true
          resources:
            requests:
              cpu: 10m
              memory: 50Mi
            limits:
              cpu: 500m
              memory: 250Mi
      volumes:
        - name: localtime
          hostPath:
            path: /etc/localtime
        - name: zoneinfo
          hostPath:
            path: /usr/share/zoneinfo
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: kube-eventer
rules:
  - apiGroups:
      - ""
    resources:
      - events
    verbs:
      - get
      - list
      - watch
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: kube-eventer
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: kube-eventer
subjects:
  - kind: ServiceAccount
    name: kube-eventer
    namespace: kube-system
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: kube-eventer
  namespace: kube-system
```

|配置项|类型|是否必选|说明|
|:--|:-|:---|:-|
|endpoint|string|必选|日志服务的endpoint，详情请参见[服务入口](/intl.zh-CN/开发指南/API 参考/服务入口.md)。|
|project|string|必选|日志服务的Project。|
|logStore|string|必选|日志服务的Logstore。|
|internal|string|自建Kubernetes：必选。|自建Kubernetes必须设置为false。|
|regionId|string|自建Kubernetes：必选。|日志服务所在地域ID，详情请参见[服务入口](/intl.zh-CN/开发指南/API 参考/服务入口.md)。|
|accessKeyId|string|自建Kubernetes：必选。|AccessKey ID，建议使用RAM用户的AccessKey信息。|
|accessKeySecret|string|自建Kubernetes：必选。|AccessKey Secret，建议使用RAM用户的AccessKey信息。|

## 日志样例

采集到的日志样例如下所示。

```
hostname:  cn-hangzhou.i-***********"
level:  Normal
pod_id:  2a360760-****
pod_name:  logtail-ds-blkkr
event_id:  {  
   "metadata":{  
      "name":"logtail-ds-blkkr.157b7cc90de7e192",
      "namespace":"kube-system",
      "selfLink":"/api/v1/namespaces/kube-system/events/logtail-ds-blkkr.157b7cc90de7e192",
      "uid":"2aaf75ab-****",
      "resourceVersion":"6129169",
      "creationTimestamp":"2019-01-20T07:08:19Z"
   },
   "involvedObject":{  
      "kind":"Pod",
      "namespace":"kube-system",
      "name":"logtail-ds-blkkr",
      "uid":"2a360760-****",
      "apiVersion":"v1",
      "resourceVersion":"6129161",
      "fieldPath":"spec.containers{logtail}"
   },
   "reason":"Started",
   "message":"Started container",
   "source":{  
      "component":"kubelet",
      "host":"cn-hangzhou.i-***********"
   },
   "firstTimestamp":"2019-01-20T07:08:19Z",
   "lastTimestamp":"2019-01-20T07:08:19Z",
   "count":1,
   "type":"Normal",
   "eventTime":null,
   "reportingComponent":"",
   "reportingInstance":""
}
```

|日志字段|类型|说明|
|:---|:-|:-|
|hostname|string|事件发生所在的主机名。|
|level|string|日志等级，包括Normal、Warning。|
|pod\_id|string|Pod的唯一标识，仅在该事件类型和Pod相关时才具有此字段。|
|pod\_name|string|Pod名，仅在该事件类型和Pod相关时才具有此字段。|
|eventId|json|该字段为JSON类型的字符串，为事件的详细内容。|

