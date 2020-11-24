# Python SDK

## Download address 

SDK GitHub:

[https://github.com/aliyun/aliyun-log-python-sdk](https://github.com/aliyun/aliyun-log-python-sdk)

## Procedure

Follow these steps to start using the Log Service Python SDK quickly.

## Step 1. Create an Alibaba Cloud account

For more information, see [Sign up with Alibaba Cloud](https://www.alibabacloud.com/help/doc-detail/50482.htm).

## Step 2. Obtain an Alibaba Cloud AccessKey

Before using Log Service Python SDK, you must apply for an [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).

Log on to the [Access Key Management page](https://ak-console.aliyun.com/#/accesskey) . Select an AccessKey for SDK.  If you do not have any, create one and make sure the AccessKey is **enabled**. For more information about how to create an access key, see [Preparations](/intl.en-US/Data Collection/Preparation/Preparations.md).

The AccessKey is used in the following steps and must be kept confidential. For more information about [Configurations](/intl.en-US/Developer Guide/SDK Reference/Configurations.md) how to use the AccessKey in SDK.

## Step 3 Create a Log Service project and a Logstore

Before using Log Service PHP SDK, you must create a Log Service project and a Logstore in the console.

Before using Log Service Python SDK, you must create a Log Service project and a Logstore in the console.

1.  Log on to the Log Service console.
2.  Click Create Project in the upper-right corner.Click **Create Project** in the upper right corner.
3.  Enter the **Project Name** and select the **Region**. Click **Confirm**.
4.  On the Project List page, click the name of the project, and then click **Create**.  [Create a Logstore](/intl.en-US/Data Collection/Preparation/Manage a Logstore.md).

    After you create a project, you can also click **Create** to create a Logstore based on the system prompt.

5.  Complete the configurations, and click **Confirm**.

    Enter the Logstore Name and Data Retention Time.  Select the Number of [Shards](/intl.en-US/Product Introduction/Basic concepts/Shards.md) as needed. In this example, you must configure four shards.


**Note:**

-   Make sure that you use the same Alibaba Cloud account to obtain the Alibaba Cloud AccessKey and create the Log Service project and Logstore.
-   For more information about the concepts of Log Service such as project and Logstore, see [Basic concepts](/intl.en-US/Product Introduction/Basic concepts/Overview.md).
-   A project name must be globally unique in Log Service, and a Logstore name must be unique in the same project.
-   After a project is created, you cannot modify the region  or migrate the project across regions.

## Step 4. Install a Python environment

The Python SDK is a pure Python library and supports all Python operating systems, including Linux, Mac OS  X and Windows. Please install Python as follows:

1.  Download and install the latest Python [installation package](https://www.python.org/downloads/).

    **Note:**

    -   Currently, Python SDK supports the Python 2.6/2.7 and Python  3.3/3.4/3.5/3.6 environments.  You can run the`python  -V`command to query the current version of Python.
    -   Python does not officially support Python 2.6 and Python 3.3. We recommend that you use Python 2.7, Python  3.4, and later versions.
2.  Download and install the Python package management tool  [pip](https://pip.pypa.io/en/latest/installing.html).

    After pip is installed, run`pip  -V` to check whether the installation is successful and query the current pip version. 


## Step 5. Install a Python SDK

Run the following command as an administrator in Shell to install Python SDK.

```
pip install -U aliyun-log-python-sdk
```

## Step 6. Start a Python program

You can start using the Python SDK. To interact with Log Service and obtain the relevant output, run the following sample code in a text editor or Python IDE.

For more information, see [Github/readthedocs](http://aliyun-log-python-sdk.readthedocs.io/).

```
# encoding: utf-8
import time
from aliyun.log.logitem import LogItem
from aliyun.log.logclient import LogClient
from aliyun.log.getlogsrequest import GetLogsRequest
from aliyun.log.putlogsrequest import PutLogsRequest
from aliyun.log.listlogstoresrequest import ListLogstoresRequest
from aliyun.log.gethistogramsrequest import GetHistogramsRequest
def main():
    endpoint = '' # Select the endpoint that matches the region of the project created in the preceding step.
    accessKeyId = '' # Use your Alibaba Cloud AccessKey ID.
    accessKey = '' # Use your Alibaba Cloud AccessKey Secret.
    project = '' # The name of the project created in the preceding step.
    logstore = '' # The name of the Logstore created in the preceding step.
    # Note: Configure four shards for the created Logstore for later testing.
    # Construct a client
    client = LogClient(endpoint, accessKeyId, accessKey)
    # List all LogStores
    req1 = ListLogstoresRequest(project)
    res1 = client.list_logstores(req1)
    res1.log_print()
    topic = ""
    source = ""
    # Send 10 data packets, each of which contains 10 logs.
    for i in range(10):
        logitemList = [] # LogItem list
        for j in range(10):
            contents = [('index', str(i * 10 + j))]
            logItem = LogItem()
            logItem.set_time(int(time.time()))
            logItem.set_contents(contents)
            logitemList.append(logItem)
        req2 = PutLogsRequest(project, logstore, topic, source, logitemList)
        res2 = client.put_logs(req2)
        res2.log_print()
    # List all shards and read the data written in the last minute.
    listShardRes = client.list_shards(project, logstore)
    for shard in listShardRes.get_shards_info():
        shard_id = shard["shardID"]
        start_time = int(time.time() - 60)
        end_time = start_time + 60
        res = client.get_cursor(project, logstore, shard_id, start_time)
        res.log_print()
        start_cursor = res.get_cursor()
        res = client.get_cursor(project, logstore, shard_id, end_time)
        end_cursor = res.get_cursor()
        while True:
            loggroup_count = 100 # Read 100 packets each time.
            res = client.pull_logs(project, logstore, shard_id, start_cursor, loggroup_count, end_cursor)
            res.log_print()
            next_cursor = res.get_next_cursor()
            if next_cursor == start_cursor:
                break
            start_cursor = next_cursor
    # Note: You can use the following interfaces to query data only when the index function is enabled.
    time.sleep(60)
    topic = ""
    query = "index"
    From = int(time.time()) - 600
    To = int(time.time())
    res3 = None
    # Query the number of logs that match the query criteria during the past 10 minutes. Retry if not all execution results are correct.
    while (res3 is None) or (not res3.is_completed()):
        req3 = GetHistogramsRequest(project, logstore, From, To, topic, query)
        res3 = client.get_histograms(req3)
    res3.log_print()
    # Obtain the number of logs that match the query conditions.
    total_log_count = res3.get_total_count()
    log_line = 10
    # Read 10 logs each time until all log data is queried. Retry three times if not all query results are correct during each query.
    for offset in range(0, total_log_count, log_line):
        res4 = None
        for retry_time in range(0, 3):
            req4 = GetLogsRequest(project, logstore, From, To, topic, query, log_line, offset, False)
            res4 = client.get_logs(req4)
            if res4 is not None and res4.is_completed():
                break
            time.sleep(1)
        if res4 is not None:
            res4.log_print()
    listShardRes = client.list_shards(project, logstore)
    shard = listShardRes.get_shards_info()[0]
    # Split a shard
    if shard["status"] == "readwrite":
        shard_id = shard["shardID"]
        inclusiveBeginKey = shard["inclusiveBeginKey"]
        midKey = inclusiveBeginKey[:-1] + str((int(inclusiveBeginKey[-1:])) + 1)
        client.split_shard(project, logstore, shard_id, midKey)
    # Merge shards.
    shard = listShardRes.get_shards_info()[1]
    if shard["status"] == "readwrite":
        shard_id = shard["shardID"]
        client.merge_shard(project, logstore, shard_id)
    # Delete shard
    shard = listShardRes.get_shards_info()[-1]
    if shard["status"] == "readonly":
        shard_id = shard["shardID"]
        client.delete_shard(project, logstore, shard_id)
   # Create an external store.
    res = client.create_external_store(project,ExternalStoreConfig("rds_store","cn-qingdao","rds-vpc","vpc-************","i***********","*. *. *.*","3306","root","sfdsfldsfksflsdfs","meta","join_meta"));
    res.log_print()
    res = client.update_external_store(project,ExternalStoreConfig("rds_store","cn-qingdao","rds-vp","rds-vpc","vpc-************","i************","*. *. *.*","3306","root","sfdsfldsfksflsdfs","meta","join_meta"));
    res.log_print()
    res = client.get_external_store(project,"rds_store");
    res.log_print()
    res = client.list_external_store(project,"");
    res.log_print();
    res = client.delete_external_store(project,"rds_store")
    res.log_print();
    # Use python sdk for query analysis.
    req4 = GetLogsRequest(project, logstore, From, To, topic, "* | select count(1)", 10,0, False)
    res4 = client.get_logs(req4)
    # Use python sdk for join rds query.
    req4 = GetLogsRequest(project, logstore, From, To, topic, "* | select count(1) from "+logstore +" l join rds_store r on l.ikey =r.ekey", 10,0, False)
    res4 = client.get_logs(req4)
    # Use python sdk to insert query results into rds.
    req4 = GetLogsRequest(project, logstore, From, To, topic, "* | insert into rds_store select count(1) ", 10,0, False)
    res4 = client.get_logs(req4)
if __name__ == '__main__':
    main()
```

