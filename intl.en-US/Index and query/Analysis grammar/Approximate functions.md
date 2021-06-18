# Approximate functions

This topic describes the syntax of approximate functions and provides related examples.

## Sample log entry

The examples of query statements for each function are based on the following sample log entry:

```
client_ip:192.0.2.0
http_user_agent:Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10_5_4; en-us) AppleWebKit/528.4+ (KHTML, like Gecko) Version/4.0dp1 Safari/526.11.2
request_uri:/request/path-1/file-9
scheme:https
server_protocol:HTTP/2.0
request_method:POST
request_length:3682
request_time:100
```

## approx\_distinct function

The approx\_distinct function is used to estimate the number of unique values in the key column. The standard error is 2.3%. The returned value is of the bigint type.

-   Syntax

    ```
    approx_distinct(key)
    ```

    where, the key is the field name.

-   Example

    In this example, the count function is used to calculate the number of page views \(PVs\). The approx\_distinct function is used to estimate the unique values of the client\_ip field as the number of unique visitors \(UVs\).

    ```
    * |select count(*) as pv, approx_distinct(client_ip) as uv
    ```

    ![approx_distinct](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1396893261/p275028.png)


## approx\_percentile function

The approx\_percentile function is used to list the values of the key column in ascending order and returns the value that is approximately at the percentage position.

-   Syntax

    -   To return a single value that is approximately at the percentage position, use the following syntax. The returned value is of the bigint type.

        ```
        approx_percentile(key,percentage)
        ```

    -   To return multiple values that are approximately at the percentage positions, use the following syntax. The returned values are of the array type.

        ```
        approx_percentile(key,array[percentage,percentages...] )
        ```

    Parameters:

    -   key: the field name. Type: bigint.
    -   percentage: the percentage value. Valid values: 0 to 1.
-   Examples
    -   Example 1:

        In this example, the values of the request\_time column are sorted in ascending order. The value that is approximately at the 50% position in the request\_time field is returned.

        ```
        *| select approx_percentile(request_time,0.5)
        ```

        ![approx_percentile](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2396893261/p275047.png)

    -   Example 2:

        In this example, the values of the request\_time column are sorted in ascending order. The values that are approximately at the 10%, 20%, and 70% positions in the request\_time field are returned.

        ```
        *| select approx_percentile(request_time,array[0.1,0.2,0.7])
        ```

        ![approx_percentile](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2396893261/p275048.png)


## numeric\_histogram

The numeric\_histogram function is used to compute the approximate histogram of the key field based on the number of buckets \(the number of histogram columns\). The returned values are the x-axis coordinate points of the approximate histogram and the approximate heights of the corresponding columns. The values are of the JSON format.

-   Syntax

    ```
    numeric_histogram(bucket, key)
    ```

    Parameters:

    -   bucket: the number of columns in the histogram. Type: bigint.
    -   key: the field name. Type: double.
-   Example

    In this example, the approximate histogram of the request duration for the POST method is computed.

    ```
    request_method:POST | select numeric_histogram(10,request_time)
    ```

    ![numeric_histogram](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2396893261/p275060.png)


## numeric\_histogram\_u function

The numeric\_histogram\_u function is used to compute the approximate histogram of the key field based on the number of buckets \(the number of histograms\). The returned values are the x-axis coordinate points of the approximate histogram and the approximate heights of the corresponding columns. The values are displayed in a table with multiple rows and columns.

-   Syntax

    ```
    numeric_histogram_u(bucket, key)
    ```

    Parameters:

    -   bucket: the number of columns in the histogram. Type: bigint.
    -   key: the field name. Type: double.
-   Example

    In this example, the approximate histogram of the request duration for the POST method is computed.

    ```
    request_method:POST | select numeric_histogram_u(10,request_time)
    ```

    ![numeric_histogram_u](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2396893261/p275059.png)


