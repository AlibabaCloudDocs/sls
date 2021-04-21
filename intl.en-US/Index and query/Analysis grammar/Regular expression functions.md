# Regular expression functions

This topic describes the basic syntax of regular expression functions. This topic also provides some examples.

## Sample log entry

The examples of query statements for each function are based on the following sample log entry:

```
http_user_agent:Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10_5_4; en-us) AppleWebKit/528.4+ (KHTML, like Gecko) Version/4.0dp1 Safari/526.11.2
request_uri:/request/path-1/file-9
scheme:https
server_protocol:HTTP/2.0
region:cn-shanghai
```

## regexp\_extract\_all\(key, regexp\) function

The regexp\_extract\_all\(key, regexp\) function is used to extract the substrings from the value of a specified field. The substrings match a specified regular expression. A collection of substrings is returned. The returned result is an array.

-   Syntax

    ```
    regexp_extract_all(key, regexp)
    ```

    -   key: the name of a specified field. The value of the field must be of the varchar type.
    -   regexp: a regular expression.
-   Example

    Extract all the numbers from the server\_protocol field.

    ```
    *| SELECT regexp_extract_all(server_protocol, '\d+')
    ```

    ![regexp_extract_all](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8229998161/p232875.png)


## regexp\_extract\_all\(key, regexp, N\) function

The regexp\_extract\_all\(key, regexp, N\) function is used to extract the substrings from the value of a specified field. The substrings match a specified regular expression. A collection of substrings that match the Nth capturing group in the regular expression is returned. The returned result is an array.

-   Syntax

    ```
    regexp_extract_all(key, regexp, N)
    ```

    -   key: the name of a specified field. The value of the field must be of the varchar type.
    -   regexp: a regular expression that contains capturing groups. For example, `(\d)(\d)(\d)` indicates three capturing groups.
    -   N: the Nth capturing group. N is an integer that starts from 1.
-   Example

    Extract "Chrome" from the value of the http\_user\_agent field, and calculate the number of requests that are initiated by the Chrome browser.

    ```
    *| SELECT regexp_extract_all(http_user_agent, '(Chrome)',1) AS Chrome, count(*) AS count GROUP BY Chrome
    ```

    ![regexp_extract_all](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8229998161/p235652.png)


## regexp\_extract\(key, regexp\) function

The regexp\_extract\(key, regexp\) function is used to extract and return the first substring in the value of a specified field. The substring matches a specified regular expression. The returned result is a string.

-   Syntax

    ```
    regexp_extract(key, regexp)
    ```

    -   key: the name of a specified field. The value of the field must be of the varchar type.
    -   regexp: a regular expression.
-   Example

    Extract the first number from the server\_protocol field.

    ```
    *|SELECT regexp_extract(server_protocol, '\d+')
    ```

    ![regexp_extract](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8229998161/p232877.png)


## regexp\_extract\(key, regexp, N\) function

The regexp\_extract\(key, regexp, N\) function is used to extract the substrings from the value of a specified field. The substrings match a specified regular expression. The first substring that matches the Nth capturing group in the regular expression is returned. The returned result is a string.

-   Syntax

    ```
    regexp_extract(key, regexp, N)
    ```

    -   key: the name of a specified field. The value of the field must be of the varchar type.
    -   regexp: a regular expression that contains capturing groups. For example, `(\d)(\d)(\d)` indicates three capturing groups.
    -   N: the Nth capturing group. N is an integer that starts from 1.
-   Example

    Extract the file information from the value of the request\_uri field, and calculate the number of visits for each file.

    ```
    * | SELECT regexp_extract(request_uri, '.*\/(file.*)', 1) AS file, count(*) AS count GROUP BY file
    ```

    ![Analyze uri](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2133466161/p224727.png)


## regexp\_like\(key, regexp\) function

The regexp\_like\(key, regexp\) function is used to check whether the value of a specified field matches a specified regular expression. The returned value is of the Boolean type.

-   Syntax

    ```
    regexp_like(key, regexp)
    ```

    -   key: the name of a specified field. The value of the field must be of the varchar type.
    -   regexp: a regular expression.
-   Example

    Check whether the value of the server\_protocol field contains digits.

    ```
    *| select regexp_like(server_protocol, '\d+')
    ```

    ![regexp_like](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8229998161/p232884.png)


## regexp\_replace\(key, regexp, string\) function

The regexp\_replace\(key, regexp, string\) function is used to replace the substring in the value of a specified field. The substring matches a specified regular expression. The new string is returned. The returned result is a string.

-   Syntax

    ```
    regexp_replace(key, regexp, string)
    ```

    -   key: the name of a specified field. The value of the field must be of the varchar type.
    -   regexp: a regular expression.
    -   value: the new string that is used to replace the raw substring.
-   Example

    Replace the value that starts with cn in the region field with **China**, and calculate the number of requests from China.

    ```
    * | select regexp_replace(region, 'cn.*','China') AS region count(*) AS count GROUP BY region
    ```

    ![regexp_replace](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8229998161/p235674.png)


## regexp\_replace\(key, regexp\)

The regexp\_replace\(key, regexp\) function is used to delete the substrings in the value of a specified field. The substrings match a specified regular expression. The substrings that are not deleted are returned. The returned result is a string.

-   Syntax

    ```
    regexp_replace(key, regexp)
    ```

    -   key: the name of a specified field. The value of the field must be of the varchar type.
    -   regexp: a regular expression.
-   Example

    Delete the information of version number in the value of the server\_protocol field, and calculate the number of requests for each communication protocol.

    ```
    *| select regexp_replace(server_protocol, '.\d+') AS server_protocol, count(*) AS count GROUP BY server_protocol
    ```

    ![regexp_replace](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8229998161/p235681.png)


## regexp\_split\(key, regexp\)

The regexp\_split\(key, regexp\) function is used to split the value of a specified field based on a specified regular expression. The returned result is an array.

-   Syntax

    ```
    regexp_split(key, regexp)
    ```

    -   key: the name of a specified field. The value of the field must be of the varchar type.
    -   regexp: a regular expression.
-   Example

    Split the value of the request\_uri field with forward slashes \(/\).

    ```
    * | SELECT regexp_split(request_uri,'/')
    ```

    ![regexp_split](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8229998161/p235709.png)


