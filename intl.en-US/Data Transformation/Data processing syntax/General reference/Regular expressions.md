# Regular expressions

This topic describes the matching modes of regular expressions, how to escape special characters in regular expressions, and the group concept of regular expressions.

## Full match

If a regular expression matches an entire string, the string fully matches the regular expression. For example, the `1234` string fully matches the `\d+` regular expression. However, the `abc123` string partially matches the `\d+` regular expression.

Some functions use regular expressions in partial match mode. You can enclose a regular expression in a caret \(`^`\) and a dollar sign \(`$`\) in the format of `^Regular expression$` to forcibly enable full match. For more information about the regular expression syntax, see [Regular expression operations](https://docs.python.org/3.7/library/re.html#module-re).

The following table lists the matching modes for different functions.

|Type|Function|Matching mode|
|----|--------|-------------|
|Global processing functions|[e\_regex](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Value extraction functions.md)|Partial match|
|[e\_keep\_fields](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Field processing functions.md)|Full match|
|[e\_drop\_fields](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Field processing functions.md)|Full match|
|[e\_rename](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Field processing functions.md)|Full match|
|[e\_kv](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Value extraction functions.md)|Partial match|
|[e\_search\_dict\_map](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Data mapping and enrichment functions.md)|Partial match|
|[e\_search\_table\_map](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Data mapping and enrichment functions.md)|Partial match|
|Expression functions|[e\_match](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Event check functions.md)|The matching mode is controlled by a parameter and is full match by default.|
|[e\_search](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Event check functions.md)|Partial match|
|[regex\_select](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Regular expression functions.md)|Partial match|
|[regex\_findall](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Regular expression functions.md)|Partial match|
|[regex\_match](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Regular expression functions.md)|The matching mode is controlled by a parameter and is partial match by default.|
|[regex\_replace](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Regular expression functions.md)|Partial match|
|[regex\_split](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Regular expression functions.md)|Partial match|

Matching mode examples:

-   `reg_match("abc123", r"\d+")`: The string matches the regular expression. The matching mode is partial match, which is the default mode.
-   `reg_match("abc123", r"\d+", full=True)`: The string does not match the regular expression. The matching mode is full match.
-   `reg_match("abc123", r"^\d+$")`: The string does not match the regular expression. The matching mode is full match based on the syntax of the regular expression.
-   `e_search(r'status~="\d+"')`: Whether the value of the `status` field matches the regular expression depends on the actual value. The matching mode is partial match based on the syntax of the regular expression.
-   `e_search(r'status~="^\d+$"')`: Whether the value of the `status` field matches the regular expression depends on the actual value. The matching mode is full match based on the syntax of the regular expression.

## Escape special characters

Regular expressions may contain special characters. You must escape these characters if you want to use their literal meanings.

-   Escape special characters by using backslashes \(`\`\).
-   Escape special characters by using the `str_regex_escape` function.

    For example, the `e_drop_fields(str_regex_escape("abc.test")` function drops the `abc.test` field. The `e_drop_fields("abc.test")` function drops fields whose names match `abc? test`, where the question mark \(`?`\) represents any character.


## Group

You can use parentheses `()` to enclose a subexpression in a regular expression to create a group. The group can be repeatedly referenced. The following example shows a regular expression without a group and a regular expression with a group:

```
"""
Raw log entry:
SourceIP: 1.1.1.1
Result:
SourceIP: 1.1.1.1
ip: 1.1.1.1
"""
# The regular expression without a group:
e_regex("SourceIP",r"\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}","ip")
# The regular expression with a group:
e_regex("SourceIP", "\d{1,3}(.\d{1,3}){3}", "ip")
```

## Capturing group

The text content that matches a capturing group is cached in the memory. The cached text content can be reused by using a backreference. A group is a capturing group if the subexpression in parentheses \(\) does not start with a question mark followed by a colon \(`?:`\).

By default, all capturing groups are numbered from left to right based on an opening parenthesis. The first group is numbered 1, the second group is numbered 2, and so on. For example:

```
(\d{4})-(\d{2}-(\d{2}))

1     1 2      3     32
```

If a regular expression contains both common capturing groups and named capturing groups, the named capturing groups are numbered after the common capturing groups.

You can directly reference the custom name of a capturing group in regular expressions or programs.

## Non-capturing group

The text content that matches a non-capturing group is not cached in the memory. A group is a non-capturing group if the subexpression in parentheses \(\) starts with a question mark followed by a colon \(`?:`\).

For example, the regular expression `pro(gram|ject)` matches `program` and `project`. If you do not want to cache the matched content in the memory, you can write the regular expression as `pro(?:gram|ject)`. Non-capturing groups simplify the matching process and occupy less memory.

**Note:** `(?:x)` indicates that content is matched with `x` but the matched content is not cached. You can define a subexpression in the \(?:x\) format and use it with operators in a regular expression.

