# General aggregate functions

An aggregate function is invoked to calculate a set of values and return a single value. This topic describes the syntax of general aggregate functions. This topic also provides related examples.

|Function|Description|Example|
|:-------|:----------|:------|
|arbitrary\(KEY\)|Returns a random, non-null value from a specified column.|The following query statement returns an arbitrary value from the request\_method column: ```
* | SELECT arbitrary(request_method) AS request_method
``` |
|avg\(KEY\)|Calculates the arithmetic mean of the values in a specified column.|The following query statement returns the projects whose average latency is greater than 1,000 microseconds. This allows you to analyze the write latency of the projects. ```
method: PostLogstoreLogs | SELECT avg(latency) AS avg_latency, Project GROUP BY Project HAVING avg_latency > 1000
``` |
|checksum\(KEY\)|Calculates the checksum of a specified column and the returned result is encoded in Base64.|The following query statement calculates the checksum of the request\_method column: ```
* | SELECT checksum(request_method)
```

The returned result is `D2UmTL3octI=`. |
|count\(\*\)|Calculates the number of log entries.|The following query statement calculates the number of page views \(PVs\): ```
* | SELECT count(*) AS PV
``` |
|count\(KEY\)|Calculates the number of the log entries that contain a specified field. If the field value of a log entry is null, the log entry is not counted.|The following query statement calculates the number of the log entries that contain the request\_method field: ```
* | SELECT count(request_method)
``` |
|count\(1\)|Calculates the number of log entries. This function is equivalent to `count(*)`.|The following query statement calculates the number of PVs: ```
* | SELECT count(1) AS PV
``` |
|count\_if\(KEY\)|Calculates the number of the log entries that meet a specified condition.|The following query statement calculates the number of requests for the value of the url field. The value ends with abc. ```
* | SELECT count_if(url like ‘%abc’) 
``` |
|geometric\_mean\(KEY\)|Calculates the geometric mean of the values in a specified column.|The following query statement calculates the geometric mean of request durations: ```
* | SELECT geometric_mean(request_time)
``` |
|max\_by\(KEY\_01,KEY\_02\)|Returns the value of KEY\_01 associated with KEY\_02 whose value is the maximum value.|The following query statement returns the time when the highest consumption occurs: ```
* | SELECT max_by(UsageEndTime, PretaxAmount) as time
``` |
|max\_by\(KEY\_01,KEY\_02,n\)|Returns the n values of KEY\_01 associated with KEY\_02 whose values are the first n maximum values. The returned result is a JSON array.

|The following query statement returns the three request methods whose request durations are the three longest request durations: ```
* | SELECT max_by(request_method,request_time,3)
```

The returned result is `["GET","PUT","DELETE"]`. |
|min\_by\(KEY\_01,KEY\_02\)|Returns the value of KEY\_01 associated with KEY\_02 whose value is the minimum value.|The following query statement returns the request method whose request duration is the shortest duration: ```
* | SELECT min_by(request_method,request_time)
``` |
|min\_by\(KEY\_01,KEY\_02,n\)|Returns the n values of KEY\_01 associated with KEY\_02 whose values are the first n minimum values. The returned result is a JSON array.

|The following query statement returns the three request methods whose request durations are the three shortest request durations: ```
* | SELECT min_by(request_method,request_time,3)
```

The returned result is `["GET","PUT","DELETE"]`. |
|max\(KEY\)|Queries the maximum value of a specified column.|The following query statement queries the longest request duration: ```
* | SELECT max(request_time)
``` |
|min\(KEY\)|Queries the minimum value of a specified column.|The following query statement queries the shortest request duration: ```
* | SELECT min(request_time)
``` |
|sum\(KEY\)|Calculates the total value of a specified column.|The following statement calculates the total size of daily NGINX traffic: ```
* | select date_trunc('day',__time__) AS time, sum(body_bytes_sent) AS body_bytes_sent GROUP BY time ORDER BY time
``` |
|bitwise\_and\_agg\(KEY\)|Returns the result of the bitwise AND operation for the values of a specified column. The returned result is in the two's complement format.

|The following query statement performs a bitwise AND operation on all values of the request\_time column: ```
* | SELECT bitwise_and_agg(request_time)
``` |
|bitwise\_or\_agg\(KEY\)|Returns the result of the bitwise OR operation for values of a specified column. The returned result is in the two's complement format.

|The following query statement performs a bitwise OR operation on all values of the request\_time column: ```
* | SELECT bitwise_or_agg(request_time)
``` |

