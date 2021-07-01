# How can I fix errors that occur when the data transformation engine reads data from a source Logstore?

This topic describes the causes of the errors that occur when the data transformation engine reads data from a source Logstore. This topic also provides methods that you can use to troubleshoot these errors.

After the data transformation engine is started, it reads data from the source Logstore in streaming mode. The data transformation engine continuously reads data from the source Logstore when the data is being transformed.

![How can I fix errors that occur when the data transformation engine reads data from a source Logstore?](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0553749951/p59308.png)

If the source Logstore cannot be accessed, errors may occur in this step. This issue may occur due to the following causes:

-   The configurations of the source Logstore are invalid.
-   The information of the source Logstore is changed.
-   A network error occurs.

Error impact:

-   If an error occurs when the data transformation engine reads data, Log Service retries the related data transformation task until a retry is successful or until you manually stop retries. If the retry succeeds, the data transformation task runs as expected.
-   If an error occurs after some data is read, Log Service saves breakpoints and retries the data transformation task. After a retry is successful, the data transformation engine continues to read data from the last breakpoint. No data is dropped or duplicated.

## Troubleshoot common errors

-   An invalid AccessKey pair that consist of an AccessKey ID and an AccessKey secret is specified for the source Logstore.
    -   Error logs

        ```
        {
          "errorCode": "Unauthorized", 
          "errorMessage": "AccessKeyId not found: LTAIL3gUus8AEu11"
        }
        { 
          "errorCode": "SignatureNotMatch", 
          "errorMessage": "signature uJfAJbc0ji04gb+cXhh0qWtajpM= not match"
        }
        ```

    -   Troubleshooting method

        Check whether the specified AccessKey ID and AccessKey secret exist and are valid.

-   The information of the source Logstore is changed.

    The configurations of the source Logstore are valid and the related data transformation task is running as expected. However, the information of the source Logstore is changed during the data transformation process. In this case, the source Logstore cannot be accessed.

    -   Error logs

        The information of the source Logstore is changed in the following two conditions:

        -   The source Logstore is deleted. In this case, the following error message is returned:

            ```
            {
              "errorMessage": "Logstore [logstore_name] does not exist."
            }
            ```

        -   The AccessKey ID or the AccessKey secret of the source Logstore is changed. In this case, the following error messages are returned:

            ```
            {
              "errorCode": "Unauthorized", 
              "errorMessage": "AccessKeyId not found: LTAIL3gUus8AEu11"
            }
            { 
              "errorCode": "SignatureNotMatch", 
              "errorMessage": "signature uJfAJbc0ji04gb+cXhh0qWtajpM= not match"
            }
            ```

    -   Troubleshooting method
        -   Check whether the source Logstore is deleted.
        -   Check whether the AccessKey ID or the AccessKey secret of the source Logstore is changed.
-   A network error occurs.
    -   Error log

        ```
        {
          "errorCode": "LogRequestError",
          "errorMessage": "HTTPConnectionPool(host='your_host', port=80): Max retries exceeded with url: your_url (Caused by NewConnectionError: Failed to establish a new connection: [Errno 11001] getaddrinfo failed'"
        }
        ```

    -   Troubleshooting method

        Check whether the network is connected as required.

-   No data is read from the source Logstore.

    No error message is returned. For more information, see [Troubleshoot common errors](/intl.en-US/Data Transformation/FAQ/Troubleshooting overview.md).


