# Collect Kubernetes events

This topic describes how to use the eventer component to collect events from Kubernetes and send the events to Log Service.

Log Service allows you to collect events from Kubernetes by using the kube-eventer or K8s Event Center application.

-   Kube-eventer

    For more information about the source code for Kubernetes event collection, see[GitHub](https://github.com/AliyunContainerService/heapster/tree/master/events).

-   K8s Event Center \(recommended\)

    To collect Kubernetes event data and configure visualized charts and alerts, you can use the K8s Event Center application provided in the Log Service console. For more information, see [Create and use a Kubernetes event center](/intl.en-US/Application/K8s Event Center/Create and use a Kubernetes event center.md).


## Configure event collection

**Note:**

-   If you use Container Service for Kubernetes \(ACK\), see [Create and use a Kubernetes event center](/intl.en-US/Application/K8s Event Center/Create and use a Kubernetes event center.md).
-   If you use self-managed Kubernetes, you must set the endpoint, project, logStore, regionId, internal, accessKeyId, and accessKeySecret parameters.

The following example shows the event collection configuration:

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

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|endpoint|string|Yes|The endpoint of Log Service. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).|
|project|string|Yes|The project in Log Service.|
|logStore|string|Yes|The Logstore in Log Service.|
|internal|string|Required for self-managed Kubernetes|If you use self-managed Kubernetes, set the value to false.|
|regionId|string|Required for self-managed Kubernetes|The region ID of Log Service. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md).|
|accessKeyId|string|Required for self-managed Kubernetes|The AccessKey ID. We recommend that you use the AccessKey ID of a RAM user.|
|accessKeySecret|string|Required for self-managed Kubernetes|The AccessKey secret. We recommend that you use the AccessKey secret of a RAM user.|

## Sample log entry

The following example shows a collected sample log entry:

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

|Log field|Type|Description|
|:--------|:---|:----------|
|hostname|string|The hostname of the server where an event occurs.|
|level|string|The level of a log entry. Valid values: Normal and Warning.|
|pod\_id|string|The unique identifier of a pod. This field is available only if the event type is related to the pod.|
|pod\_name|string|The name of a pod. This field is available only if the event type is related to the pod.|
|eventId|json|The details of an event. The value of this field is a JSON string.|

