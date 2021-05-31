# How do I optimize regular expressions?

You can optimize regular expressions to improve the Logtail collection performance.

The following describes some suggestions about how to optimize regular expressions:

-   Use precise characters.

    Do not arbitrarily use `.*` to match fields because this regular expression can match with a wide range of search results. Such actions can to lead to mismatches occurring or a decrease in matching performance. For example, to return results of fields that consist only of letters, use `[A-Za-z]`.

-   Use correct measure words.

    Do not arbitrarily use plus signs \(+\), commas \(,\), or asterisks. For example, to match target IP addresses, use `\d` instead of `\d+` or `\d{1,3}` because of its higher efficiency.

-   Debug multiple times.

    Debugging is similar to troubleshooting. You can debug the time consumed by your regular expressions at the **Regex101** website, and promptly optimize them if there is a large amount of backtracking.


