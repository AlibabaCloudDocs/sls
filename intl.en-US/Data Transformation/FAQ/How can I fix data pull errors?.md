# How can I fix data pull errors?

This topic describes the data pull errors that occur during data transformation and provides methods that you can use to troubleshoot these errors.

## Error handling

For more information about how to troubleshoot data pull errors, see [res\_log\_logstore\_pull](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Resource functions.mdsection_b3c_kth_p0t), [res\_rds\_mysql](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Resource functions.mdsection_49h_ufh_ptu), and [res\_oss\_file](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Resource functions.mdsection_mlb_osw_xzd).

## Troubleshoot common errors

If you use only one resource function, data pull errors occur.

-   Sample transformation rules
    -   ```
res_log_logstore_pull(endpoint="cn-shenzhen.log.aliyuncs.com",ak_id="xxx",
        ak_secret="xxx",project="etl-test-shenzhen",
        fields=["__source__"]),field="processid",output_fields=["xx"]
```

    -   ```
res_rds_mysql(address="xx",username="xx",password="xx",database="xx")
```

    -   ```
res_oss_file(endpoint='xx',ak_id="xx",ak_key="xx",bucket='xx', file='xx',format='xx',change_detect_interval=0)
```

-   Error log

    ```
    aliyun.log.logexception.LogException: {"errorCode": "InvalidEtlConfig", "errorMessage": "ETL config doesn't pass security check, detail: invalid type detected: <class '_ast.Expr'>", "requestId": ""}
    ```

-   Troubleshooting method

    The preceding error log indicates that the syntax of the specified function is invalid. This error log is returned because you use only the res\_log\_logstore\_pull, res\_rds\_mysql, or res\_oss\_file function. You cannot use only one resource function.

-   Solution

    You must use a resource function together with another function, such as e\_table\_map or e\_search\_table\_map. For more information, see [Resource functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Resource functions.md).


## References

-   For information about how to fix errors that occur when you pull data from another Logstore, see [How can I fix errors that occur when I pull Logstore data \(dimension table\)](/intl.en-US/Data Transformation/FAQ/How can I fix errors that occur when I pull Logstore data (dimension table)?.md).
-   For information about how to fix errors that occur when you pull data from Object Storage Service \(OSS\), see [How can I fix errors that occur during data pulls from OSS](/intl.en-US/Data Transformation/FAQ/How can I fix errors that occur during data pulls from OSS?.md).
-   For information about how to fix errors that occur when you pull data from ApsaraDB RDS for MySQL, see [How can I fix errors of the syntax used to pull data from ApsaraDB RDS for MySQL](/intl.en-US/Data Transformation/FAQ/How can I fix errors of the syntax used to pull data from ApsaraDB RDS for MySQL?.md).

