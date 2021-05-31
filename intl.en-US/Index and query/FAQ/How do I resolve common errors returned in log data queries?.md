# How do I resolve common errors returned in log data queries?

The returned log data queries may contain the following common errors:

## line 1:44: Column 'my\_key\_field' cannot be resolved;please add the column in the index attribute

-   Cause

    The `my_key_field` key cannot be included in the query statement because the key does not exist.

-   Solution

    In the upper-right corner of the Search & Analysis page, click Index Attributes to create an index for this field and enable the analytics feature for this field.


## Column 'xxxxline' not in GROUP BY clause;please add the column in the index attribute

-   Cause

    You used the GROUP BY clause and included a non-aggregate field in a SELECT statement. However, you did not specify this field in the GROUP BY clause. For example, the key1 field in the `select key1, avg(latency) group by key2` statement is not specified in the GROUP BY clause.

-   Solution

    Use the syntax `select key1,avg(latency) group by key1,key2`.


## sql query must follow search query,please read syntex doc

-   Cause

    You did not specify filter conditions in a statement, for example, `select ip,count(*) group by ip`.

-   Solution

    Use the syntax `*|select ip,count(*) group by ip`.


## please read syntex document,and make sure all related fields are indexed. error after select .error detail:line 1:10: identifiers must not start with a digit; surround the identifier with double quotes

-   Cause

    The column name or variable name referenced in an SQL statement starts with a digit and does not meet the related requirements.

-   Solution

    Change the column name or variable name to a name that starts with a letter.


## please read syntax document,and make sure all related fields are indexed. error after select .error detail:line 1:9: extraneous input '' expecting

-   Cause

    One or more words were misspelled.

-   Solution

    Correct the misspelled words.


## key \(category\) is not config as key value config,if symbol : is in your log,please wrap : with quotation mark "

-   Cause

    No index was created on the category field. As a result, the field cannot be used in query and analysis statements.

-   Solution

    Create an index for this field in the index attributes. For more information, see [Configure indexes](/intl.en-US/Index and query/Configure indexes.md).


## Query exceeded max memory size of 3GB

-   Cause

    The size of memory used by the current query exceeded 3 GB on the server. The probable cause is that a large number of values are still returned in the query result after you use the GROUP BY clause to remove duplicates.

-   Solution

    Reduce the number of keys specified in the GROUP BY clause.


## ErrorType:ColumnNotExists.ErrorPosition,line:0,column:1.ErrorMessage:line 1:123: Column '\_\_raw\_log\_\_' cannot be resolved; it seems \_\_raw\_log\_\_ is wrapper by ";if \_\_raw\_log\_\_ is a string ,not a key field, please use '\_\_raw\_log\_\_'

-   Cause

    The `my_key_field` key cannot be included in the query statement because the key does not exist.

-   Solution

    In the upper-right corner of the Search & Analysis page, click Index Attributes to create an index for this field and enable the analytics feature for this field.


