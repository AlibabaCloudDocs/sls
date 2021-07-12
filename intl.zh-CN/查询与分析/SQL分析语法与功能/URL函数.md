# URL函数

本文介绍URL函数的语法和示例。

URL函数支持从标准URL路径中提取字段，一个标准的URL如下：

```
[protocol:][//host[:port]][path][?query][#fragment]
```

## 常见URL函数

|函数名|含义|示例|
|输入样例|输出结果|
|:--|:-|:-|
|:---|:---|
|`url_extract_fragment(url)`|提取出URL中的fragment，结果为varchar类型。|`*| select url_extract_fragment('https://sls.console.aliyun.com/#/project/dashboard-demo/categoryList')`|`/project/dashboard-demo/categoryList`|
|`url_extract_host(url)`|提取出URL中的host，结果为varchar类型。|`*|select url_extract_host('http://www.aliyun.com/product/sls')`。|`www.aliyun.com`|
|`url_extract_parameter(url, name)`|提取出URL中的query中name对应的参数值，结果为varchar类型。|`*|select url_extract_parameter('http://www.aliyun.com/product/sls?userid=testuser','userid')`|`testuser`|
|`url_extract_path(url)`|提取出URL中的path，结果为varchar类型。|`*|select url_extract_path('http://www.aliyun.com/product/sls?userid=testuser')`|`/product/sls`|
|`url_extract_port(url)`|提取出URL中的端口，结果为bigint类型。|`*|select url_extract_port('http://www.aliyun.com:80/product/sls?userid=testuser')`|`80`|
|`url_extract_protocol(url)`|提取出URL中的协议，结果为varchar类型。|`*|select url_extract_protocol('http://www.aliyun.com:80/product/sls?userid=testuser')`|`http`|
|`url_extract_query(url)`|提取出URL中的query，结果为varchar类型。|`*|select url_extract_query('http://www.aliyun.com:80/product/sls?userid=testuser')`|`userid=testuser`|
|`url_encode(value)`|对URL进行转义编码。|`*|select url_encode('http://www.aliyun.com:80/product/sls?userid=testuser')`|`http%3a%2f%2fwww.aliyun.com%3a80%2fproduct%2fsls%3fuserid%3dtestuser`|
|`url_decode(value)`|对URL进行解码。|`*|select url_decode('http%3a%2f%2fwww.aliyun.com%3a80%2fproduct%2fsls%3fuserid%3dtestuser')`|`http://www.aliyun.com:80/product/sls?userid=testuser`|

