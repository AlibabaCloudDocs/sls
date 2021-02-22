# How can I fix errors that occur during data pulls from OSS?

If the transformation rule of a Logstore also pulls data from Object Storage Service \(OSS\), data pull or update errors may occur. This topic introduces such errors and their corresponding troubleshooting methods.

After reading data from a Logstore, the data transformation engine starts to transform log events in the Logstore. If the transformation rule involves data pulls from external resources such as OSS, ApsaraDB for RDS, and other Logstores, data pull or update errors may occur.

## Error impact

For more information, see [Error impact](/intl.en-US/Data Transformation/FAQ/How can I fix errors related to data transformation rules?.md).

## Troubleshooting methods

For more information, see [Solution](/intl.en-US/Data Transformation/FAQ/How can I fix errors related to data transformation rules?.md).

## Example errors

**Note:** In the following examples, AK\_ID refers to AccessKey ID and AK\_KEY refers to AccessKey secret.

-   Incorrect object path or format

    A 404 error occurs if the "data" directory does not exist in OSS or the object format is incorrect.

    -   Transformation rule

        ```
        # In this example, the correct directory is "test".
        e_set("oss_file",res_oss_file(endpoint, ak_id=res_local("AK_ID"),ak_key=res_local("AK_KEY"), bucket, 'data/test.txt', format='text', change_detect_interval=20))
        # The "text" format does not exist in the "test" directory.
        e_set("oss_file",res_oss_file(endpoint, ak_id=res_local("AK_ID"),ak_key=res_local("AK_KEY"), bucket, 'test/test.dat', format='text', change_detect_interval=20))
        ```

    -   Error message

        ```
        message:  Exception: {'status': 404, 'x-oss-request-id': '5D49****878', 'details': {'Code': 'NoSuchKey', 'Message': 'The specified key does not exist.', 'RequestId': '5D4***8878', 'HostId': 'lo***g.oss-cn-hangzhou.aliyuncs.com', 'Key': 'oss/test.txt'}}
        ```

    -   Error cause

        This error occurs because the specified object path is incorrect. If a 404 error occurs and the error message involves the object, the object path is incorrect.

    -   Troubleshooting method

        Correct the `file` settings in the orchestration syntax of the `res_oss_file` function.

        ```
        # In this example, the correct directory is "test".
        e_set("oss_file",res_oss_file(endpoint, ak_id=res_local("AK_ID"),ak_key=res_local("AK_KEY"), bucket, 'test/test.txt', format='text', change_detect_interval=20))
        ```

-   Object decoding error

    A decoding error occurs if an object pulled from OSS fails to be decoded or a nonexistent decoding method is used.

    -   Transformation rule

        ```
        e_set("oss_file",res_oss_file(endpoint, ak_id=res_local("AK_ID"),ak_key=res_local("AK_KEY"), bucket, 'test/test.txt', format='text', change_detect_interval=20, encoding='unicode'))
        ```

    -   Error message

        ```
        {
          "reason": "LookupError: unknown encoding: unicode"
        }
        ```

    -   Error cause

        This error occurs because the specified `encoding` is incorrect. If `unknown encoding: unicode` error message is reported, it means the specified parameter is incorrect.

    -   Troubleshooting method

        Correct the `encoding` settings in the orchestration syntax of the `res_oss_file` function.

        ```
        # In this example, the decoding format is UTF-8.
        e_set("oss_file",res_oss_file(endpoint, ak_id=res_local("AK_ID"),ak_key=res_local("AK_KEY"), bucket, 'test/test.txt', format='text', change_detect_interval=20,encoding='utf8'))
        ```

-   Endpoint errors

    An endpoint error occurs if the specified `endpoint` in the `res_oss_file` function is incorrect.

    -   The specified endpoint does not exist.
        -   Transformation rule

            ```
            e_set("oss_file",res_oss_file("https://oss-cn-asd.aliyuncs.com", ak_id=res_local("AK_ID"),ak_key=res_local("AK_KEY"), 'your bucket', 'test/test.txt', format='text', change_detect_interval=20))
            ```

        -   Error message

            ```
            message:  get_oss_bytes get errors:{'status': -2, 'x-oss-request-id': '', 'details': "RequestError: HTTPSConnectionPool(host='log-etl-staging.oss-cn-asd.aliyuncs.com', port=443)
            ```

        -   Error cause

            This error occurs because the specified endpoint does not exist. If the status value is `-2` and the error message involves `RequestError`, it means the specified endpoint is invalid.

        -   Troubleshooting method

            Correct the `endpoint` settings in the orchestration syntax of the `res_oss_file` function.

    -   The specified OSS endpoint does not match the bucket name.

        In this example, the AccessKey pair owner has permissions to manage resources in the China \(Hangzhou\) region, but the specified endpoint corresponds to the China \(Shanghai\) region.

        -   Transformation rule

            ```
            e_set("oss_file",res_oss_file("https://oss-cn-shanghai.aliyuncs.com", ak_id=res_local("AK_ID"),ak_key=res_local("AK_KEY"), 'your bucket', 'test/test.txt', format='text', change_detect_interval=20))
            ```

        -   Error message

            ```
            message:  get_oss_bytes get errors:{'status': 403, 'x-oss-request-id': '5D7219353A90A2852B234D14', 'details': {'Code': 'AccessDenied', 'Message': 'The bucket you are attempting to access must be addressed using the specified endpoint. Please send all future requests to this endpoint.', 'RequestId': '5D7**14', 'HostId': 'log-**.oss-cn-shanghai.aliyuncs.com', 'Bucket': 'log-**', 'Endpoint': 'oss-cn-hangzhou.aliyuncs.com'}}
            ```

        -   Error cause

            This error occurs because the specified `endpoint` and the AccessKey pair do not belong to the same region. If a 403 error occurs and the error message involves the specified bucket and endpoint, it means the endpoint and AccessKey pair belong to different regions.

        -   Troubleshooting method

            Correct the `encoding` settings in the orchestration syntax of the `res_oss_file` function.

-   Permission errors
    -   The specified AccessKey ID or AccessKey secret is incorrect.

        In this example, the specified AccessKey ID in the `res_oss_file` function is incorrect.

        -   Transformation rule

            ```
            e_set("oss_file",res_oss_file(endpoint, ak_id=res_local("AK_ID"),ak_key=res_local("AK_KEY"), 'your bucket', 'test/test.txt', format='text', change_detect_interval=20))
            ```

        -   Error message

            ```
            message: Exception: {'status': 403, 'x-oss-request-id': '5D***BEB6', 'details': {'Code': 'InvalidAccessKeyId', 'Message': 'The OSS Access Key Id you provided does not exist in our records.', 'RequestId': '5D4***BEB6', 'HostId': 'lo***g.oss-cn-hangzhou.aliyuncs.com', 'OSSAccessKeyId': 'LT***Ai'}}
            ```

        -   Error cause

            This error occurs because the specified AccessKey ID or AccessKey secret is incorrect. If a 403 error occurs and the error message involves the AccessKey ID or AccessKey secret, it means the AccessKey ID or AccessKey secret is incorrect.

        -   Troubleshooting method

            Correct the AK\_ID and AK\_KEY settings in the orchestration syntax of the `res_oss_file` function.

    -   The AccessKey pair owner has no permission to access the specified bucket.

        In this example, a bucket access error occurs when the `res_oss_file` function is used.

        -   Transformation rule

            ```
            e_set("oss_file",res_oss_file(endpoint='http://oss-cn-hangzhou.aliyuncs.com',ak_id=os.getenv("SLS_ACCESS_KEY_ID"),
                                 ak_key=os.getenv("SLS_ACCESS_KEY_SECRET"),
                                 bucket='log', file='test.txt',
                                 change_detect_interval=30, encoding='utf8'))
            ```

        -   Error message

            ```
            message: Exception: {'status': 403, 'x-oss-request-id': '5D674CE8BE0EBC45166026C5', 'details': {'Code': 'AccessDenied', 'Message': 'You have no right to access this object because of bucket acl.', 'RequestId': '5D4***BEB6', 'HostId': 'log.oss-cn-hangzhou.aliyuncs.com'}}
            ```

        -   Error cause

            This error occurs because the AccessKey pair owner has no permission to access the specified bucket. If a 403 error occurs and the error message involves the specified bucket, it means the AccessKey pair owner has no permission to access the bucket.

        -   Troubleshooting method

            Correct the bucket settings in the orchestration syntax of the `res_oss_file` function.

    -   The specified bucket does not exist.

        In this example, a bucket error occurs when the `res_oss_file` function is used. The error occurs because the specified bucket does not exist in OSS.

        -   Transformation rule

            ```
            e_set("oss_file",res_oss_file(endpoint='http://oss-cn-hangzhou.aliyuncs.com',ak_id=os.getenv("SLS_ACCESS_KEY_ID"),
                                 ak_key=os.getenv("SLS_ACCESS_KEY_SECRET"),
                                 bucket='twiss', file='test.txt',
                                 change_detect_interval=30, encoding='utf8'))
            ```

        -   Error message

            ```
            message: Exception: {'status': 404, 'x-oss-request-id': '5D75F6E9BB4097C678A381EF', 'details': {'Code': 'NoSuchBucket', 'Message': 'The specified bucket does not exist.', 'RequestId': '5D75F6E9BB4097C678A381EF', 'HostId': 'twiss.oss-cn-hangzhou.aliyuncs.com', 'BucketName': 'twiss'}}
            ```

        -   Error cause

            This error occurs because the specified bucket does not exist in OSS. If a 403 error occurs and the error message involves the bucket, it means the bucket does not exist.

        -   Troubleshooting method

            Correct the bucket settings in the orchestration syntax of the `res_oss_file` function.

-   Object update errors

    The object update error messages are included in logs. The following examples describe three of the error messages.

    -   The following error message indicates that the first data pull from OSS fails due to network problems and the transformation rule retries the data pull.

        ```
        {
          "reason":"Failed to pull data from oss for the first time and it is preparing to re-pull ..."
        }
        ```

    -   The following error message indicates that data of the specified object is updated at regular intervals, but the resource function fails to pull new data from OSS and retries the data pull.

        ```
        {
          "reason":"get_oss_byte get errors,begin retry ..."
        }
        ```

    -   The following error message indicates that the retry fails and the resource function starts another retry. A maximum of three retries can be performed.

        ```
        {
          "reason":"get_oss_byte get errors,refresh_interval ..."
        }
        ```


