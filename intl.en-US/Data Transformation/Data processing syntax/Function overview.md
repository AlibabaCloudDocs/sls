# Function overview

This topic describes all functions that you can use to transform data in Log Service.

## Global processing functions

|Type|Function|Description|
|----|--------|-----------|
|[Flow control functions](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Flow control functions.md)|e\_if|Performs an operation if a condition is met. Multiple condition-operation pairs can be specified. -   If a condition is met, the operation that corresponds to the condition is performed. If the condition is not met, the operation is not performed and the next condition is evaluated.
-   If one operation is performed to delete an event, no more operations can be performed on the event. |
|e\_if\_else|Performs an operation based on the evaluated result of a condition.|
|e\_switch|Performs an operation if a condition is met. Multiple condition-operation pairs can be specified. -   If a condition is met, the operation that corresponds to the condition is performed and a result is returned. If the condition is not met, the operation is not performed and the next condition is evaluated.
-   If no specified conditions are met and the default parameter is specified, the default operations that are specified in the default parameter are performed and a result is returned.
-   If one operation is performed to delete an event, no more operations can be performed on the event. |
|e\_compose|Combines multiple operations. -   This function is used to combine multiple operations in the `e_if`, `e_switch`, or `e_if_else` function.
-   The specified operations are performed on an event in sequence and a result is returned.
-   If one operation is performed to delete an event, no more operations can be performed on the event. |
|[Event processing functions](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Event processing functions.md)|e\_drop|Deletes an event if a condition is met.|
|e\_keep|Retains an event if a condition is met.|
|e\_split|Splits an event into multiple events based on the value of a specified field. You can use JMESPath to extract the value of the field, and then split the event.|
|e\_output|Writes an event to a specified destination Logstore. You can specify parameters such as topic, source, and tags. The event is deleted after it is written to the destination Logstore.|
|e\_coutput|Writes an event to a specified destination Logstore. You can specify parameters such as topic, source, and tags. The event is retained after it is written to the destination Logstore. The retained event continues to be transformed.|
|e\_to\_metric|Converts log data to time series data that can be stored in a Metricstore.|
|[Field processing functions](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Field processing functions.md)|e\_drop\_fields|Deletes the log fields that meet a specified condition.|
|e\_keep\_fields|Retains the log fields that meet a specified condition.|
|e\_rename|Renames the log fields that meet a specified condition.|
|[Value assignment function](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Value assignment function.md)|e\_set|Sets the value of a field. Multiple field-value pairs can be specified.|
|[Value extraction functions](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Value extraction functions.md)|e\_regex|Extracts the value of a field in an event by using a regular expression and assigns the value to other fields.|
|e\_json|Performs operations on JSON objects in a specified field. You can set the parameters to expand JSON data, extract JSON data with JMESPath expressions, and expand extracted JSON data.|
|e\_kv|Extracts key-value pairs of multiple source fields by using the quote parameter.|
|e\_kv\_delimit|Extracts key-value pairs of multiple source fields by using delimiters.|
|e\_csv|Extracts the value of a specified source field, parses the value into multiple values based on the user-defined delimiter, and then assigns the parsed values to the predefined fields. The default delimiter is a comma \(,\).|
|e\_tsv|Extracts the value of a specified source field, parses the value into multiple values based on the user-defined delimiter, and then assigns the parsed values to the predefined fields. The default delimiter is \\t.|
|e\_psv|Extracts the value of a specified source field, parses the value into multiple values based on the user-defined delimiter, and then assigns the parsed values to the predefined fields. The default delimiter is a vertical line \(\|\).|
|e\_syslogrfc|Calculates the values of the facility and severity fields and then returns the corresponding level information. The calculation is based on the value of the priority field in an event by using the RFC protocol that is used by syslog.|
|e\_anchor|Extracts the value between the specified start and end positions.|
|[Data mapping and enrichment functions](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Data mapping and enrichment functions.md)|e\_dict\_map|Maps the value of a specified field to a field in a dictionary and returns the value of the matched field.|
|e\_table\_map|Maps the value of a specified field to a row in a table and returns the value of the field in this row.|
|e\_search\_dict\_map|Maps a search string to a key in a dictionary and returns the value of the matched key.|
|e\_search\_table\_map|Maps a search string to a column in a table and returns the value of the field in this column.|
|[Value-added content function](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Value-added content function.md)|e\_threat\_intelligence|Obtains threat intelligence from log fields and outputs the threat intelligence to specified fields.|

## Expression functions

|Type|Function|Description|
|----|--------|-----------|
|[Event check functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Event check functions.md)|v|Extracts the value of a field from an event. If multiple field names are passed to the function, the value of the first field that exists is returned.|
|e\_has|Checks whether a field exists.|
|e\_not\_has|Checks whether a field does not exist.|
|e\_search|Searches for specified events in a simplified method.|
|e\_match, e\_match\_all, and e\_match\_any|Checks whether the value of a field in an event meets the specified condition in an expression.|
|[Operator functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Operator functions.md)|op\_if|Returns an expression based on a specified condition.|
|op\_ifnull|Returns the value of the first expression whose value is not None.|
|op\_coalesce|Returns the value of the first expression whose value is not None.|
|op\_nullif|If Value 1 is equal to Value 2, the value None is returned. Otherwise, Value 1 is returned.|
|op\_and|Invokes the AND operation.|
|op\_not|Invokes the NOT operation.|
|op\_or|Invokes the OR operation.|
|op\_eq|Returns the result that is calculated based on the `a==b` condition. The values of the a and b parameters must be of the same data type, such as string, number, or list.|
|op\_ge|Returns the result that is calculated based on the `a>=b` condition. The values of the a and b parameters must be of the same data type, such as string, number, or list.|
|op\_gt|Returns the result that is calculated based on the `a>b` condition. The values of the a and b parameters must be of the same data type, such as string, number, or list.|
|op\_le|Returns the result that is calculated based on the `a<=b` condition. The values of the a and b parameters must be of the same data type, such as string, number, or list.|
|op\_lt|Returns the result that is calculated based on the `a<b` condition. The values of the a and b parameters must be of the same data type, such as string, number, or list.|
|op\_ne|Returns the result that is calculated based on the `a! =b` condition. The values of the a and b parameters must be of the same data type, such as string, number, or list.|
|op\_len|Calculates the number of characters in a text string. You can use this function in expressions that return strings, tuples, lists, or dictionaries.|
|op\_in|Checks whether a string, tuple, list, or dictionary contains a specified element.|
|op\_not\_in|Checks whether a string, tuple, list, or dictionary does not contain a specified element.|
|op\_slice|Truncates a specified string, array, or tuple.|
|op\_index|Returns the element that corresponds to the index of a specified string, array, or tuple.|
|op\_add|Calculates the sum of multiple values. The values can be strings or numbers.|
|op\_max|Returns the maximum value among the values of multiple fields or expressions.|
|op\_min|Returns the minimum value among the values of multiple fields or expressions.|
|[Conversion functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Conversion functions.md)|ct\_int|Converts the value of a field or an expression to an integer.|
|ct\_float|Converts the value of a field or an expression to a floating-point number.|
|ct\_str|Converts the value of a field or an expression to a string.|
|ct\_bool|Converts the value of a field or an expression to a Boolean value.|
|ct\_chr|Converts the ANSI or Unicode value of a field or an expression to a character.|
|ct\_ord|Converts the value of a field or an expression to an ANSI or a Unicode value.|
|ct\_hex|Converts the value of a field or an expression to a hexadecimal number.|
|ct\_oct|Converts the value of a field or an expression to an octal number.|
|ct\_bin|Converts the value of a field or an expression to a binary number.|
|bin2oct|Converts a binary byte string to an octal string.|
|bin2hex|Converts a binary string to a hexadecimal string.|
|[Arithmetic functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Arithmetic functions.md)|op\_abs|Returns the absolute value of a passed value.|
|op\_div\_floor|Returns the integer part of the quotient after a division of two passed values.|
|op\_div\_true|Returns the quotient of two passed values.|
|op\_pow|Returns a passed value raised to the power of the other passed value.|
|op\_mul|Returns the product of two passed values.|
|op\_neg|Returns the opposite number of a passed value.|
|op\_mod|Returns the remainder of a passed value divided by the other passed value.|
|op\_sub|Returns the difference of two passed values.|
|op\_round|Returns a passed value rounded.|
|op\_sum|Returns the sum of passed values.|
|mat\_ceil|Returns a passed value rounded up to the nearest integer.|
|mat\_exp|Returns Euler's number raised to the power of a passed value.|
|mat\_fabs|Calculates the absolute value of a passed value.|
|mat\_floor|Returns a passed value rounded down to the nearest integer.|
|mat\_log|Returns the logarithm of a passed value with the other passed value as the base.|
|mat\_log10|Returns the base 10 logarithm of a passed value.|
|mat\_sqrt|Returns the square root of a passed value.|
|mat\_degrees|Converts radians to degrees.|
|mat\_radians|Converts degrees to radians.|
|mat\_sin|Returns the sine \(in radians\) of a passed value.|
|mat\_cos|Returns the cosine \(in radians\) of a passed value.|
|mat\_tan|Returns the tangent \(in radians\) of a passed value.|
|mat\_acos|Returns the arc cosine \(in radians\) of a passed value.|
|mat\_asin|Returns the arc sine \(in radians\) of a passed value.|
|mat\_atan|Returns the arc tangent \(in radians\) of a passed value.|
|mat\_atan2|Calculates the arc tangent of X- and Y-coordinates.|
|mat\_atanh|Returns the inverse hyperbolic tangent of a passed value.|
|mat\_hypot|Returns the Euclidean norm of two passed values.|
|[String functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/String functions.md)|str\_format|Formats a string.|
|str\_join|Concatenates the elements in a sequence with a specified string to generate a new string.|
|str\_zip|Splits the values of two expressions concurrently, and then merges them into a single string.|
|str\_encode|Encodes a string by using a specified encoding format.|
|str\_decode|Decodes a string by using a specified decoding format.|
|str\_hex\_escape\_encode|Escapes special characters to Chinese characters. Hexadecimal characters are supported.|
|str\_sort|Sorts the characters in a string.|
|str\_reverse|Reverses the characters in a string.|
|str\_replace|Replaces an old string with a new string based on an applicable rule.|
|str\_logtash\_config\_normalize|Converts the data format of the Logstash configuration language to the JSON format.|
|str\_translate|Replaces the specified characters of a string with the mapping characters.|
|str\_strip|Deletes specified characters from a string.|
|str\_lstrip|Deletes specified characters from the start of a string.|
|str\_rstrip|Deletes specified characters from the end of a string.|
|str\_lower|Converts all uppercase characters in a string to lowercase characters.|
|str\_upper|Converts all lowercase characters in a string to uppercase characters.|
|str\_title|Capitalizes only the first letter of all words.|
|str\_capitalize|Capitalizes only the first letter of a string.|
|str\_swapcase|Interchanges the uppercase letters and lowercase letters in a string.|
|str\_count|Counts the number of occurrences of a character in a string.|
|str\_find|Checks whether an original string contains a specified substring.|
|str\_rfind|Locates the position of the last occurrence of a character in a string.|
|str\_endswith|Checks whether a string ends with a specified suffix.|
|str\_startswith|Checks whether a string starts with a specified string.|
|str\_split|Splits a string with a specified delimiter.|
|str\_splitlines|Splits a string with a line break.|
|str\_partition|Splits a string with a specified delimiter into three parts from left to right.|
|str\_rpartition|Splits a string with a specified delimiter into three parts from right to left.|
|str\_center|Pads a string to a specified length with a specified character.|
|str\_ljust|Pads a string to a fixed length from the end with a specified character.|
|str\_rjust|Pads a string to a specified length from the start with a specified character.|
|str\_zfill|Pads a string to a specified length from the start with 0.|
|str\_expandtabs|Converts `\t` in a string to space characters.|
|str\_isalnum|Checks whether a string consists of only letters and digits.|
|str\_isalpha|Checks whether a string consists of only letters.|
|str\_isascii|Checks whether a string is in the ASCII table.|
|str\_isdecimal|Checks whether a string contains only decimal characters.|
|str\_isdigit|Checks whether a string consists of only digits.|
|str\_isidentifier|Checks whether a string is a valid Python identifier. This function can also check whether a variable name is valid.|
|str\_islower|Checks whether a string consists of lowercase letters.|
|str\_isnumeric|Checks whether a string consists of digits.|
|str\_isprintable|Checks whether all characters in a string are printable characters.|
|str\_isspace|Checks whether a string consists of only whitespace characters.|
|str\_istitle|Checks whether the first letter of all words in a string are in uppercase and whether other letters in the string are in lowercase.|
|str\_isupper|Checks whether all letters in a string are uppercase letters.|
|str\_uuid|Generates a random universally unique identifier \(UUID\).|
|[Date and time functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Date and time functions.md)|dt\_parse|Converts the value of a time expression to a datetime object.|
|dt\_str|Converts the value of a time expression to a string.|
|dt\_parsetimestamp|Converts the value of a time expression to a UNIX timestamp.|
|dt\_prop|Obtains the specified attribute of the value of a time expression.|
|dt\_now|Obtains the current datetime.|
|dt\_today|Obtains the current date and excludes the time.|
|dt\_utcnow|Obtains the current datetime in the current time zone.|
|dt\_fromtimestamp|Converts a UNIX timestamp to a datetime object.|
|dt\_utcfromtimestamp|Converts a UNIX timestamp to a datetime object in the current time zone.|
|dt\_strptime|Converts a time string to a datetime object.|
|dt\_currentstamp|Obtains a UNIX timestamp.|
|dt\_totimestamp|Converts a datetime object to a UNIX timestamp.|
|dt\_strftime|Converts a datetime object to a string in a specified format.|
|dt\_strftimestamp|Converts a UNIX timestamp to a string in a specified format.|
|dt\_truncate|Truncates the value of a time expression based on a specified time granularity.|
|dt\_add|Modifies the value of a time expression based on a specified time granularity.|
|dt\_MO|Offsets the specified time to the date of the previous or next Nth Monday. The offset value N is passed to the `weekday` parameter of the `dt_add` function, where N can be a positive or negative integer. To pass a negative integer, use `op_neg(positive integer)`.|
|dt\_TU|Offsets the specified time to the date of the previous or next Nth Tuesday. The offset value N is passed to the `weekday` parameter of the `dt_add` function, where N can be a positive or negative integer. To pass a negative integer, use `op_neg(positive integer)`.|
|dt\_WE|Offsets the specified time to the date of the previous or next Nth Wednesday. The offset value N is passed to the `weekday` parameter of the `dt_add` function, where N can be a positive or negative integer. To pass a negative integer, use `op_neg(positive integer)`.|
|dt\_TH|Offsets the specified time to the date of the previous or next Nth Thursday. The offset value N is passed to the `weekday` parameter of the `dt_add` function, where N can be a positive or negative integer. To pass a negative integer, use `op_neg(positive integer)`.|
|dt\_FR|Offsets the specified time to the date of the previous or next Nth Friday. The offset value N is passed to the `weekday` parameter of the `dt_add` function, where N can be a positive or negative integer. To pass a negative integer, use `op_neg(positive integer)`.|
|dt\_SA|Offsets the specified time to the date of the previous or next Nth Saturday. The offset value N is passed to the `weekday` parameter of the `dt_add` function, where N can be a positive or negative integer. To pass a negative integer, use `op_neg(positive integer)`.|
|dt\_SU|Offsets the specified time to the date of the previous or next Nth Sunday. The offset value N is passed to the `weekday` parameter of the `dt_add` function, where N can be a positive or negative integer. To pass a negative integer, use `op_neg(positive integer)`.|
|dt\_astimezone|Converts the value of a time expression to a datetime object in a specified time zone.|
|dt\_diff|Obtains the difference between the values of two time expressions based on a specified time granularity.|
|[Regular expression functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Regular expression functions.md)|regex\_select|Extracts a specified value based on a regular expression.|
|regex\_findall|Obtains all values that match a regular expression.|
|regex\_match|Checks whether a value matches a regular expression.|
|regex\_replace|Replaces specified characters in a string based on a regular expression.|
|regex\_split|Splits a value into multiple values.|
|[Grok function](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Grok function.md)|grok|Extracts a specified value based on a regular expression.|
|[Structured data functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Structured data functions.md)|json\_select|Extracts or calculates specified values from a JSON expression based on the JMESPath syntax.|
|json\_parse|Converts a JSON text to a JSON object.|
|xml\_to\_json|Converts XML-formatted data to JSON-formatted data.|
|geo\_parse|Parses an IP address and returns the information about the city, province, and country of the IP address.|
|gzip\_compress|Compresses specified data, encodes the compressed data by using the Base64 algorithm, and then returns the transformed data.|
|gzip\_decompress|Decodes data by using the Base64 algorithm, decompresses the decoded data, and then returns the transformed data.|
|ip\_cidrmatch|Checks whether an IP address belongs to a Classless Inter-Domain Routing \(CIDR\) block.|
|ip\_version|Checks whether the version of an IP address is IPv4 or IPv6.|
|ip\_type|Returns the type of an IP address.|
|ip\_makenet|Converts an IP address to a CIDR block.|
|ip\_to\_format|Converts the format of a CIDR block to a format that specifies the netmask or prefix length of the CIDR block.|
|ip\_overlaps|Checks whether two CIDR blocks overlap.|
|[Encoding and decoding functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Encoding and decoding functions.md)|url\_encoding|Encodes URL data.|
|url\_decoding|Decodes URL data.|
|base64\_encoding|Encodes data by using the Base64 algorithm.|
|base64\_decoding|Decodes data by using the Base64 algorithm.|
|html\_encoding|Encodes data in the HTML format.|
|html\_decoding|Decodes HTML-encoded data.|
|md5\_encoding|Encodes data by using the MD5 algorithm.|
|sha1\_encoding|Encodes data by using the SHA1 algorithm.|
|ip2long|Converts an IP address to a value of the long type.|
|long2ip|Converts a value of the long type to an IP address.|
|[Parsing functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Parsing functions.md)|ua\_parse\_device|Parses the device information in a User-Agent HTTP header.|
|ua\_parse\_os|Parses the operating system information in a User-Agent HTTP header.|
|ua\_parse\_agent|Parses the browser information in the User-Agent HTTP header.|
|ua\_parse\_all|Parses all information in a User-Agent HTTP header.|
|[List functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/List functions.md)|lst\_make|Constructs a list.|
|lst\_insert|Inserts values to a list at a specified position.|
|lst\_append|Adds values to the end of a list.|
|lst\_delete\_at|Deletes a value from a list at a specified position.|
|lst\_reverse|Reverses the order of values in a list.|
|lst\_get|Obtains a value from a list or tuple.|
|[Dictionary functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Dictionary functions.md)|dct\_make|Constructs a dictionary.|
|dct\_update|Updates a dictionary.|
|dct\_delete|Deletes key-value pairs from a dictionary.|
|dct\_keys|Obtains the keys of a dictionary.|
|dct\_values|Obtains the values of a dictionary.|
|dct\_get|Obtains the value that corresponds to a specified key in a dictionary.|
|[Table functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Table functions.md)|tab\_parse\_csv|Constructs a table from a comma-separated values \(CSV\) file.|
|tab\_to\_dict|Constructs a dictionary from a table.|
|[Resource functions](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Resource functions.md)|res\_local|Obtains the values of advanced parameters of a data transformation task.|
|res\_rds\_mysql|Obtains data from a specified database table in ApsaraDB RDS for MySQL. The data can be refreshed at regular intervals.|
|res\_log\_logstore\_pull|Pulls data from another Logstore when a Logstore is being transformed. You can pull data and maintain it in a table in a continuous manner.|
|res\_oss\_file|Obtains the content of an object in a specified bucket from Object Storage Service \(OSS\). The object can be refreshed at regular intervals.|

