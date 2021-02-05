# Search syntax

This topic describes the search syntax of Log Service.

## Search types

A search statement specifies one or more search conditions and returns the log entries that meet the specified conditions. Searches are classified by indexing method as full-text searches or field-specific searches, and are also classified by precision as exact searches or fuzzy searches.

**Note:**

-   If you configure full-text indexes and field indexes, the configurations of the field indexes take precedence.
-   You must set the data type of a field to double or long before you specify a value range to query log entries of the field. If the data type of the field is not set to double or long, or the syntax of the value range is invalid, Log Service performs a full-text search. In these cases, the search result may be different from the expected result. For example, if the executed search statement is `owner_id>100` and the data type of the owner\_id field is not double or long, log entries that contain owner\_id, \> \(non-delimiter\), and 100 are returned.
-   If you change the data type of a field from text to double or long, only the equal sign \(=\) can be used to query the log entries that are collected before the change.

-   Full-text searches and field-specific searches

    |Search type|Description|Example|
    |-----------|-----------|-------|
    |Full-text search|After you configure full-text indexes, Log Service splits an entire log entry into multiple words based on specified delimiters. You can query log entries. To query log entries, specify keywords and rules in a search statement. The keywords can be field names or field values.|`PUT and cn-shanghai`: returns the log entries that contain the keywords PUT and cn-shanghai.|
    |Field-specific search|After you configure field indexes, you can query log entries. To query log entries, specify pairs of field name and field value \(Key:Value\). You can perform a basic search or combined search based on the data types of the fields in the field indexes. For more information, see [Data types of indexes](/intl.en-US/Index and query/Data types of indexes.md).|`request_time>60 and request_method:Ge*`: returns the log entries in which the value of the request\_time field is greater than 60 and the value of the request\_method field starts with Ge.|

-   Exact searches and fuzzy searches

    |Search type|Description|Example|
    |-----------|-----------|-------|
    |Exact search|Complete words are used for queries.|    -   `host:www.yl.mock.com`: returns the log entries in which the value of the host field is www.yl.mock.com.
    -   `PUT`: returns the log entries that contain the keyword PUT. |
    |Fuzzy search|You can use an asterisk \(\*\) or a question mark \(?\) as a keyword for a fuzzy search. Each keyword can contain 1 to 64 characters in length and cannot start with an asterisk \(\*\) or a question mark \(?\). If a search condition contains a keyword, Log Service returns a maximum of 100 log entries and each log entry contains a word that matches the keyword pattern. The more accurate a keyword is, the more accurate the search results are.**Note:** The long and double data types do not support asterisks \(\*\) or question marks \(?\) in fuzzy searches. You can use a numeric range for a fuzzy search, for example, status in \[200 299\].

A fuzzy search is a sample-based search that uses the following mechanism:

    -   If you enable the field indexing feature and specify a search field, Log Service randomly takes samples from the index data of the specified field and returns part of the search results.
    -   If you enable the full-text indexing feature and do not specify a search field, Log Service randomly takes samples from the full-text index data and returns part of the search results.
|    -   `addr*`: searches for 100 words that start with addr from log entries, and returns the log entries that contain one or more of these words.
    -   `host:www.yl*`: searches for 100 words that start with www.yl from the value of the host field. Then, Log Service returns the log entries in which the value of the host field contains one or more of these words.
For more information, see [Fuzzy match](/intl.en-US/Index and query/FAQ/Fuzzy match.md).|


## Operators

The following table describes the operators that are supported by search statements.

**Note:**

-   The in operator is case-sensitive. Other operators are not case-sensitive.
-   Log Service uses the following operators: sort, asc, desc, group by, avg, sum, min, max, and limit. If you use these operators as keywords, enclose the operators in double quotation marks \(""\).
-   The following list shows the precedence of the operators in descending order:
    1.  Colon \(:\)
    2.  Double quotations marks \(""\)
    3.  Parentheses \(\)
    4.  and
    5.  not
    6.  or

|Operator|Description|
|:-------|:----------|
|and|The and operator, for example, `request_method:GET and status:200`.By default, if no syntax keyword exists among multiple keywords, the relation is and. For example, `GET 200 cn-shanghai` is equivalent to `GET and 200 and cn-shanghai`. |
|or|The or operator, for example, `request_method:GET or status:200`.|
|not|The not operator, for example, `request_method:GET not status:200` or `not status:200`.|
|\( \)|This operator is used to increase the priority of the search conditions that are enclosed in parentheses \(\). Example: `(request_method:GET or request_method:POST) and status:200`.|
|:|This operator is used for field-specific searches \(Key:Value\). Example: `request_method:GET`.If a field name or field value contains reserved characters such as spaces and colons \(:\), enclose the field name or field value in double quotation marks \(""\). Example: `"file info":apsara`. |
|""|To convert the keyword to an ordinary character, you can use double quotation marks \(""\) to enclose a syntax keyword. For example, `"and"` returns log entries that contain and. In this case, and is not an operator.In a field-specific search, the words in the double quotation marks \(""\) are considered as a whole. |
|\\|The escape character. This character is used to escape double quotation marks \(""\). The escaped double quotation marks \(""\) indicate the double quotation marks \(""\). For example, if the log content is `instance_id:nginx"01"`, you can execute the `instance_id:nginx\"01\"` statement to query log entries.|
|\*|The wildcard character. This character is used to match zero, one, or multiple characters. Example: `host:www.yl.mo*k.com`.**Note:** Log Service searches for 100 words that meet the specified conditions from log entries. Log entries that contain the 100 words and meet the search conditions are returned. |
|?|The wildcard character. This character is used to match a single character. Example: `host:www.yl.mo? k.com`.|
|\>|Queries the log entries in which the value of a specified field is greater than a specified number. Example: `request_time>100`.|
|\>=|Queries the log entries in which the value of a specified field is greater than or equal to a specified number. Example: `request_time>=100`.|
|<|Queries the log entries in which the value of a specified field is smaller than a specified number. Example: `request_time<100`.|
|<=|Queries the log entries in which the value of a specified field is smaller than or equal to a specified number. Example: `request_time<=100`.|
|=|Queries the log entries in which the value of a specified field is equal to a specified number. Equal signs \(=\) and colons \(:\) have the same effect on fields of the double or long data type. For example, `request_time=100` is equivalent to `request_time:100`.|
|in|Queries the log entries in which the value of a specified field is within a numerical range. The brackets \[\] indicate a closed interval and the parentheses \(\) indicate an open interval. A space is used to separate two numbers. Example: `request_time in [100 200]` or `request_time in (100 200]`.**Note:** The characters in must be lowercase. |
|\_\_source\_\_|Queries the log entries of a specified log source. Wildcard characters are supported. Example: `__source__:"192.0.2. *"`.**Note:** The \_\_source\_\_ field is a reserved field in Log Service. This field can be abbreviated to source. If you customize a field in the source format, the custom field conflicts with the reserved source field of Log Service. In this case, to search for the custom field, you must use Source or SOURCE in a search statement. |
|\_\_tag\_\_|Queries log entries based on metadata. Example: `__tag__:__receive_time__:1609837139`.|
|\_\_topic\_\_|Queries the log entries of a specified log topic. Example: `__topic__:nginx_access_log`.|

## Search statement examples

|Expected search result|Search statement|
|:---------------------|:---------------|
|Log entries that contain successful GET requests \(status code: 200 to 299\)|```
request_method:GET and status in [200 299]
``` |
|Log entries that contain GET requests but do not contain the China \(Shanghai\) region|```
request_method:GET not region:cn-shanghai
``` |
|Log entries that contain GET requests or POST requests|```
request_method:GET or request_method:POST
``` |
|Log entries that do not contain GET requests|```
not request_method:GET
``` |
|Log entries that contain successful GET requests or successful POST requests|```
(request_method:GET or request_method:POST) and status in [200 299]
``` |
|Log entries that contain failed GET requests or failed POST requests|```
(request_method:GET or request_method:POST) not status in [200 299]
``` |
|Log entries that contain successful GET requests \(status code: 200 to 299\) and in which the request duration is less than 60 seconds|```
request_method:GET and status in [200 299] not request_time>=60
``` |
|Log entries in which the request duration is equal to 60 seconds|-   ```
request_time:60
```

-   ```
request_time=60
``` |
|Log entries in which the request duration is greater than or equal to 60 seconds and less than 200 seconds|-   ```
request_time>=60 and request_time<200
```

-   ```
request_time in [60 200)
``` |
|Log entries in which the value of the http\_user\_agent field contains Firefox|```
http_user_agent:Firefox
``` |
|Log entries in which the value of the http\_user\_agent field contains Linux and Chrome|-   ```
http_user_agent:"Linux Chrome"
```

-   ```
http_user_agent:Linux and http_user_agent:Chrome
``` |
|Log entries that contain and|```
"and"
```

In this search statement, and is an ordinary string and not an operator. |
|Log entries in which the value of the http\_user\_agent field contains Firefox or Chrome|```
http_user_agent:Firefox or http_user_agent:Chrome
``` |
|Log entries in which the value of the file info field contains apsara|```
"file info":apsara
``` |
|Log entries that start with cn|```
cn*
``` |
|Log entries in which the value of the region field starts with cn|```
region:cn*
``` |
|Log entries in which the value of the region field contains cn\*|```
region:"cn*"
``` |
|Log entries in which the value of the region field ends with hai|Not supported.|
|Log entries that start with mo, end with la, and contain one character between mo and la|```
mo? 1a
``` |
|Log entries that start with mo, end with la, and contain zero, one, or more characters between mo and la|```
mo*1a
``` |
|Log entries that contain words that start with Moz and words that start with Sa|```
Moz* and Sa*
``` |
|Log entries whose log topics are https or http|```
__topic__:https or __topic__:http
``` |
|Log entries that are collected from the 192.0.2.1 host|```
__tag__:__client_ip__:192.0.2.1
```

`__tag__:__client_ip__` indicates the IP address of the host where log entries reside. |
|Log entries in which the value of the remote\_user field is not empty|```
not remote_user:""
``` |
|Log entries in which the value of the remote\_user field is empty|```
remote_user:""
``` |
|Log entries that do not contain the remote\_user field|```
not remote_user:*
``` |
|Log entries that contain the remote\_user field|```
remote_user:*
``` |
|Log entries in which the value of the request\_uri field is /request/path-2|```
request_uri:/request/path-2
``` |

