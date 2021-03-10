# Function overview

Log Service provides the domain-specific language \(DSL\) to construct expression functions that return specific values to help you transform log data.

The following table lists expression functions.

|Type|Function|Description|
|----|--------|-----------|
|[Event check functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Event check functions.md)|Functions such as v, e\_has, e\_not\_has, e\_search, e\_match, e\_match\_any, and e\_match\_all|Returns the values of fields, or checks whether a field or the value of a field meets specified conditions.|
|[Operator functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Operator functions.md)|op\_\* functions|Compares values, performs condition-based or container-based judgment, or performs general multi-value calculation.|
|[Conversion functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Conversion functions.md)|ct\_\* functions|Converts data types among numeric values, strings, and Boolean values, or converts numbers between different numeral systems.|
|[Arithmetic functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Arithmetic functions.md)|op\_\*, math\_\*, and mat\_\* functions|Performs mathematical calculation or multi-value calculation.|
|[String functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/String functions.md)|st\_\* functions|Processes strings.|
|[Date and time functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Date and time functions.md)|dt\_\* functions|Converts time data among UNIX timestamps, datetime objects, and date and time strings, changes time zones, and returns the difference between two time values.|
|[Regular expression functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Regular expression functions.md)|regex\_\* functions|Extracts, returns, replaces, or splits values based on regular expressions.|
|[Grok function](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Grok function.md)|Grok function|Extracts specific values based on regular expressions.|
|[Structured data functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Structured data functions.md)|json\_\*, xml\_\*, and pb\_\* functions|Extracts or parses fields.|
|[Encoding and decoding functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Encoding and decoding functions.md)|url\_\*, html\_\*, md5\_\*, sha1\_\*, and base64\_\* functions|Encodes or decodes data.|
|[Parsing functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Parsing functions.md)|ua\_\* functions|Parses the User-Agent HTTP header.|
|[List functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/List functions.md)|op\_\* and lst\_\* functions|Constructs, returns, or modifies a list.|
|[Dictionary functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Dictionary functions.md)|op\_\* and dct\_\* functions|Constructs, returns, or modifies a dictionary.|
|[Table functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Table functions.md)|tab\_\* functions|Constructs a table from a text file or constructs a dictionary from a table.|
|[Resource functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Resource functions.md)|res\_\* functions|Obtains the values of the advanced parameters of a data transformation task. Pulls data from an Object Storage Service \(OSS\) bucket, Logstores, or a table in an ApsaraDB RDS for MySQL database.|

