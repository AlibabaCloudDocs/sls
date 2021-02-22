# 从OSS获取数据出错

如果加工规则中涉及OSS资源的加载，则有可能会产生资源的加载或刷新错误。本文档主要介绍从OSS获取数据出错的原因以及排查处理方法。

在成功读取源Logstore数据后，加工引擎开始对源Logstore的日志事件进行加工。如果加工规则中涉及OSS、RDS、Logstore等外联资源的加载，则也有可能会产生资源的加载或刷新错误。

## 错误影响

请参见[错误影响](/cn.zh-CN/数据加工/FAQ/加工规则错误.md)。

## 错误排查方法

请参见[错误排查方法](/cn.zh-CN/数据加工/FAQ/加工规则错误.md)。

## OSS获取数据常见出错样例

**说明：** 下文中AK\_ID和AK\_KEY为AccessKeyId和AccessKeySecret的简称。

-   文件路径错误或者文件格式错误。

    OSS中不存在data这个目录或者文件格式写错，从而引发的404错误日志。

    -   加工规则样例

        ```
        # 假设正确目录是test
        e_set("oss_file",res_oss_file(endpoint, ak_id=res_local("AK_ID"),ak_key=res_local("AK_KEY"), bucket, 'data/test.txt', format='text', change_detect_interval=20))
        # 此格式在test中不存在
        e_set("oss_file",res_oss_file(endpoint, ak_id=res_local("AK_ID"),ak_key=res_local("AK_KEY"), bucket, 'test/test.dat', format='text', change_detect_interval=20))
        ```

    -   错误日志

        ```
        message:  Exception: {'status': 404, 'x-oss-request-id': '5D49****878', 'details': {'Code': 'NoSuchKey', 'Message': 'The specified key does not exist.', 'RequestId': '5D4***8878', 'HostId': 'lo***g.oss-cn-hangzhou.aliyuncs.com', 'Key': 'oss/test.txt'}}
        ```

    -   排查方法

        检查报错的日志事件，如果报错信息中`status`是404，并且`Messages`是关于文件的，首先要检查自己的文件目录是否写错。

    -   解决方案

        将`res_oss_file`函数加工编排中的`file`参数改成正确的文件目录即可。

        ```
        # 假如正确的目录是test
        e_set("oss_file",res_oss_file(endpoint, ak_id=res_local("AK_ID"),ak_key=res_local("AK_KEY"), bucket, 'test/test.txt', format='text', change_detect_interval=20))
        ```

-   文件解码错误。

    对从OSS上拉取的文件，解码的时候执行错误或者使用了不存在的解码方式。

    -   加工规则样例

        ```
        e_set("oss_file",res_oss_file(endpoint, ak_id=res_local("AK_ID"),ak_key=res_local("AK_KEY"), bucket, 'test/test.txt', format='text', change_detect_interval=20, encoding='unicode'))
        ```

    -   错误日志

        ```
        {
          "reason": "LookupError: unknown encoding: unicode"
        }
        ```

    -   排查方法

        检查报错的日志事件，如果报错信息是`unknown encoding: unicode`，检查文件解码参数`encoding`是否写错。

    -   解决方案

        将`res_oss_file`函数加工编排中的`encoding`参数改成正确的解码格式即可。

        ```
        # 假如正确的解码格式是utf8
        e_set("oss_file",res_oss_file(endpoint, ak_id=res_local("AK_ID"),ak_key=res_local("AK_KEY"), bucket, 'test/test.txt', format='text', change_detect_interval=20,encoding='utf8'))
        ```

-   Endpoint出错。

    使用`res_oss_file`函数的时候`Endpoint`填写错误。

    -   Endpoint不存在。
        -   加工规则样例

            ```
            e_set("oss_file",res_oss_file("https://oss-cn-asd.aliyuncs.com", ak_id=res_local("AK_ID"),ak_key=res_local("AK_KEY"), 'your bucket', 'test/test.txt', format='text', change_detect_interval=20))
            ```

        -   错误日志

            ```
            message:  get_oss_bytes get errors:{'status': -2, 'x-oss-request-id': '', 'details': "RequestError: HTTPSConnectionPool(host='log-etl-staging.oss-cn-asd.aliyuncs.com', port=443)
            ```

        -   排查方法

            检查报错的日志事件，如果报错信息是`-2`，并且`Message`信息是关于`RequestError`的，检查endpoint是否存在。

        -   解决方案

            将`res_oss_file`函数加工编排中的`endpoint`参数值修改正确即可。

    -   OSS Endpoint与bucket名字不匹配。

        授权的AccessKey拥有在杭州region的操作权，而Endpoint写成了上海。

        -   加工规则样例

            ```
            e_set("oss_file",res_oss_file("https://oss-cn-shanghai.aliyuncs.com", ak_id=res_local("AK_ID"),ak_key=res_local("AK_KEY"), 'your bucket', 'test/test.txt', format='text', change_detect_interval=20))
            ```

        -   错误日志

            ```
            message:  get_oss_bytes get errors:{'status': 403, 'x-oss-request-id': '5D7219353A90A2852B234D14', 'details': {'Code': 'AccessDenied', 'Message': 'The bucket you are attempting to access must be addressed using the specified endpoint. Please send all future requests to this endpoint.', 'RequestId': '5D7**14', 'HostId': 'log-**.oss-cn-shanghai.aliyuncs.com', 'Bucket': 'log-**', 'Endpoint': 'oss-cn-hangzhou.aliyuncs.com'}}
            ```

        -   排查方法

            检查报错的日志事件，如果报错信息是403，并且`Message`信息是关于bucket和endpoint的，检查`endpoint`是否与AccessKey不属于同一region。

        -   解决方案

            将`res_oss_file`函数加工编排中的`encoding`参数改成正确的解码格式即可。

-   权限错误
    -   AccessKey出错。

        使用`res_oss_file`函数的时候AccessKey填写错误。

        -   加工规则样例

            ```
            e_set("oss_file",res_oss_file(endpoint, ak_id=res_local("AK_ID"),ak_key=res_local("AK_KEY"), 'your bucket', 'test/test.txt', format='text', change_detect_interval=20))
            ```

        -   错误日志

            ```
            message: Exception: {'status': 403, 'x-oss-request-id': '5D***BEB6', 'details': {'Code': 'InvalidAccessKeyId', 'Message': 'The OSS Access Key Id you provided does not exist in our records.', 'RequestId': '5D4***BEB6', 'HostId': 'lo***g.oss-cn-hangzhou.aliyuncs.com', 'OSSAccessKeyId': 'LT***Ai'}}
            ```

        -   排查方法

            检查报错的日志事件，如果报错信息是403，并且`Message`信息是关于AccessKey的，检查AccessKey是否写错。

        -   解决方案

            将`res_oss_file`函数加工编排中的AccessKey信息修改正确即可。

    -   Bucket权限错误。

        使用`res_oss_file`函数的时候bucket出错。

        -   加工规则样例

            ```
            e_set("oss_file",res_oss_file(endpoint='http://oss-cn-hangzhou.aliyuncs.com',ak_id=os.getenv("SLS_ACCESS_KEY_ID"),
                                 ak_key=os.getenv("SLS_ACCESS_KEY_SECRET"),
                                 bucket='log', file='test.txt',
                                 change_detect_interval=30, encoding='utf8'))
            ```

        -   错误日志

            ```
            message: Exception: {'status': 403, 'x-oss-request-id': '5D674CE8BE0EBC45166026C5', 'details': {'Code': 'AccessDenied', 'Message': 'You have no right to access this object because of bucket acl.', 'RequestId': '5D4***BEB6', 'HostId': 'log.oss-cn-hangzhou.aliyuncs.com'}}
            ```

        -   排查方法

            检查报错的日志事件，如果报错信息是403，并且`Message`信息是关于bucket的，检查bucket是否有权限操作。

        -   解决方案

            将`res_oss_file`函数加工编排中的bucket信息修改正确即可。

    -   Bucket不存在。

        使用`res_oss_file`函数的时候bucket出错。示例中bucket在OSS上是不存在的。

        -   加工规则样例

            ```
            e_set("oss_file",res_oss_file(endpoint='http://oss-cn-hangzhou.aliyuncs.com',ak_id=os.getenv("SLS_ACCESS_KEY_ID"),
                                 ak_key=os.getenv("SLS_ACCESS_KEY_SECRET"),
                                 bucket='twiss', file='test.txt',
                                 change_detect_interval=30, encoding='utf8'))
            ```

        -   错误日志

            ```
            message: Exception: {'status': 404, 'x-oss-request-id': '5D75F6E9BB4097C678A381EF', 'details': {'Code': 'NoSuchBucket', 'Message': 'The specified bucket does not exist.', 'RequestId': '5D75F6E9BB4097C678A381EF', 'HostId': 'twiss.oss-cn-hangzhou.aliyuncs.com', 'BucketName': 'twiss'}}
            ```

        -   排查方法

            检查报错的日志事件，如果报错信息是403，并且`Message`信息是关于bucket的，检查bucket是否存在。

        -   解决方案

            将`res_oss_file`函数加工编排中的bucket信息修改正确即可。

-   定时更新文件出错日志说明。

    此类日志只会在后台日志记录中出现，以下介绍三种定时更新中出错说明。

    -   如下日志表示第一次可能因为网络问题获取资源失败，准备重试。

        ```
        {
          "reason":"Failed to pull data from oss for the first time and it is preparing to re-pull ..."
        }
        ```

    -   如下日志表示后台每隔一段时间进行更新，新资源函数获取数据失败，准备重试。

        ```
        {
          "reason":"get_oss_byte get errors,begin retry ..."
        }
        ```

    -   如下日志表示重试失败，准备再次重试，最多重试三次。

        ```
        {
          "reason":"get_oss_byte get errors,refresh_interval ..."
        }
        ```


