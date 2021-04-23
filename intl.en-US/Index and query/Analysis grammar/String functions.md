# String functions

This topic describes the string functions that you can use in Log Service to analyze log data.

## Syntax

**Note:**

-   When you use string functions, you must enclose strings in single quotation marks \(''\). Strings that are not enclosed or enclosed in double quotation marks \(""\) indicate fields names or column names. For example, 'status' indicates the status string, whereas status or "status" indicates the status log field.
-   The key parameter in the following table indicates a log field.

|Function|Description|
|:-------|:----------|
|chr\(number\)|Returns the characters that match the ASCII value of a specified parameter.|
|codepoint\(key\)|Converts a field of the ASCII type to a field value of the bigint type.|
|length\(key\)|Calculates the length of a string. Returns a value of the integer type. |
|lower\(key\)|Converts a string to lowercase letters. Returns a value of the varchar type in lowercase letters. |
|upper\(key\)|Converts a string to uppercase characters. Returns a value of the varchar type in uppercase letters. |
|lpad\(key, length, lpad\_string\)|Pads a string to a specified length on the left of the string. The length parameter is an integer that indicates the length of the padded string.

-   If the length of the string is less than the integer value of the length parameter, the left of the string is padded by a substring.
-   If the length of the string is longer than the integer value of the length parameter, the number of the returned characters is based on the length parameter.

Returns a value of the varchar type. |
|rpad\(key, length,rpad\_string\)|Pads a string to a specified length on the right of the string. The length parameter is an integer that indicates the length of the padded string.

-   If the length of the string is less than the integer value of the length parameter, the right of the string is padded by a substring.
-   If the length of the string is longer than the integer value of the length parameter, the number of the returned characters is based on the length parameter.

Returns a value of the varchar type. |
|trim\(key\)|Deletes the spaces at the beginning and end of a string. Returns a value of the varchar type. |
|ltrim\(key\)|Deletes the spaces at the beginning of a string. Returns a value of the varchar type. |
|rtrim\(key\)|Deletes the spaces at the end of a string. Returns a value of the varchar type. |
|replace\(key,substring,replace\)|Replaces matched characters in a string with specified characters. Returns a value of the varchar type. |
|replace\(key,substring\)|Deletes matched characters from a string. Returns a value of the varchar type. |
|reverse\(key\)|Returns a string in reverse order.|
|split\(key,delimeter,N\)|Splits a string with a specified delimiter and returns a set of N substrings. Returns a value of the array type. |
|split\_part\(key,delimeter,part\)|Splits a string with a specified delimiter and returns the string at the specified position. part is an integer greater than 0.

Returns a value of the varchar type. |
|split\_to\_map\(key, delimiter01, delimiter02\)|Uses the first specified delimiter to split the string for the first time, and then uses the second specified delimiter to split the string for a second time. Returns a value of the map type. |
|position\(substring IN key\)|Returns the position of the specified substring in a string. Returns a value of the integer type. The values start from 1. |
|strpos\(key, substring\)|Returns the position of a specified substring in a string. This function is equivalent to the position\(substring IN key\) function. Returns a value of the integer type. The values start from 1. |
|substr\(key, start\)|Returns a substring at the specified position in a string. The start parameter indicates the position of the substring to be extracted. The value of the start parameter starts at 1.

Returns a value of the varchar type. |
|substr\(key, start, length\)|Returns a substring at the specified position in a string and specifies the length of the substring. The start parameter indicates the position of the substring to be extracted. The value of the start parameter starts at 1. length indicates the length of the substring.

Returns a value of the varchar type. |
|concat\(key01,key02,key03\)|Concatenates multiple substrings into one string. Returns a value of the integer type. The value starts at 1. |
|levenshtein\_distance\(key01, key02\)|Returns the Levenshtein distance of two strings.|
|hamming\_distance \(string1,string2\)|Returns the Hamming distance of two strings.|

## Example

Sample logs:

```
http_user_agent:Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10_5_4; en-us) AppleWebKit/528.4+ (KHTML, like Gecko) Version/4.0dp1 Safari/526.11.2
request_uri:/request/path-1/file-9?0457349059345
scheme:https
server_protocol:HTTP/2.0
region:cn-shanghai
time: upstream_response_time:"80", request_time:"40"
```

-   Use question marks \(?\) as the delimiter to split the value of the request\_uri field and return the first substring. The returned substring indicates the file path. Then, calculate the number of requests that correspond to different paths.

    ```
    * | SELECT count(*) AS PV, split_part(request_uri, '?', 1) AS Path GROUP BY Path ORDER BY pv DESC LIMIT 3
    ```

    ![Top three most accessed file paths](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6737619161/p232326.png)

-   Extract the first four characters of the HTTP part from the server\_protocol field and calculate the number of requests that use the HTTP protocol.

    ```
    * | SELECT substr(server_protocol,1,4) AS protocol, count(*) AS count GROUP BY server_protocol
    ```

    ![substr](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6737619161/p236773.png)

-   Use commas \(,\) and colons \(:\) to split the value of the time field and return a value of the map type.

    ```
    * | SELECT split_to_map(time,',',':')
    ```

    ![split_to_map](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6737619161/p240051.png)

-   Determine whether the value of the http\_user\_agent field starts with the letter M.

    ```
    * | SELECT substr(http_user_agent, 1, 1)=chr(77)
    ```

    ![substr](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6737619161/p240199.png)

-   Return the position of the letter H in the server\_protocol field.

    ```
    * | SELECT strpos(server_protocol,'H')
    ```

    ![strpos](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6737619161/p240191.png)

-   Use a forward slash \(/\) to split the value of the server\_protocol field into two substrings and return a set of the substrings.

    ```
    * | SELECT split(server_protocol,'/',2)
    ```

    ![split](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6737619161/p240196.png)

-   Replace cn in the region field with China.

    ```
    * | select replace(region,'cn','China')
    ```

    ![replace](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6737619161/p240200.png)


