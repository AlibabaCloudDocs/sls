# Log Service SDK for Python

This topic describes how to install and use Log Service SDK for Python.

-   Log Service is activated. For more information, see [Activate Log Service](https://www.aliyun.com/product/sls?spm=5176.7933691.J_8058803260.20.3eeb2a665LA0eU).
-   An AccessKey pair is created and obtained. For more information, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md).
-   A Python development environment is installed.

    Log Service SDK for Python supports PyPy 2, PyPy 3, Python 2.6, Python 2.7, Python 3.3, Python 3.4, Python 3.5, and Python 3.6. You can run the python-V command to check the version of Python that you have installed. You can download the installation package from the [Python official website](https://www.python.org/downloads/) and install the Python development environment.

-   The pip tool is installed to manage Python packages.

    You can run the pip-V command to check the version of pip that you have installed. You can download the installation package from the [pip official website](https://pip.pypa.io/en/latest/installing/) and install pip.

    **Note:** In Windows, if the **'pip' is not recognized as an internal or external command, operable program or batch file** error appears, add the installation paths of Python and pip to the Path environment variable. The installation path of pip is in the Scripts folder in the Python installation directory. After you complete the settings, you may need to restart your computer to ensure that the environment variables take effect.


## Step 1: Install Log Service SDK for Python

Log Service SDK for Python supports all operating systems that run Python, such as Linux, Mac OS X, and Windows.

1.  Run the following command as an administrator in the command-line interface \(CLI\) to install Log Service SDK for Python.

    ```
    pip install -U aliyun-log-python-sdk
    ```

2.  Verify the installation result.

    If the following message appears, Log Service SDK for Python is installed.

    ![python sdk](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5956916061/p179978.png)


## Step 2: Create LogClient

LogClient is the Python client of Log Service. You can use LogClient to manage Log Service resources, such as projects and Logstores. To use the Log Service SDK for Python to initiate a service request, you must create a client instance.

**Note:** If you use HTTPS to access Log Service, you must add the prefix https:// to the endpoint of Log Service, for example, https://cn-hangzhou.log.aliyuncs.com.

```
from aliyun.log import LogClient
endpoint = 'cn-hangzhou.log.aliyuncs.com' // The endpoint of Log Service. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md). The China (Hangzhou) region is used as an example. Enter the endpoint of the region where your Log Service project resides.
accessKeyId = 'your_access_id' // The AccessKey ID of your Alibaba Cloud account. For more information, see [AccessKey pair](/intl.en-US/Developer Guide/API Reference/AccessKey pair.md). High security risks may arise if you use the AccessKey pair of an Alibaba Cloud account because the account has permissions to call all API operations. We recommend that you create and use a RAM user to call API operations or perform routine O&M.
$accessKey = 'your_access_key' // The AccessKey secret of your Alibaba Cloud account.
client = LogClient(endpoint, accessKeyId, accessKey) // Create LogClient.
```

## Sample code

Log Service SDK for Python provides multiple sample programs. For more information, see [aliyun-log-python-sdk](https://github.com/aliyun/aliyun-log-python-sdk). The following sample code shows how to create a project and a Logstore:

```
res = client.create_project("my-project", "description") // Enter the project name and the description.
res = client.create_logstore('my-project', 'my-logstore', ttl=30, shard_count=3) // Enter the project name, Logstore name, data retention period, and the number of shards. If you set ttl to 3650, the data is permanently stored.
res.log_print() 
```

