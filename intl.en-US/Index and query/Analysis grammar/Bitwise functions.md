# Bitwise functions

This topic describes bitwise functions.

|Function|Description|Example|
|:-------|:----------|:------|
|bit\_count\(x, bits\) → bigint|Counts the number of bits set in x in two's complement. The x variable is treated as a signed integer that includes the specified number of bits.|-   SELECT bit\_count\(9, 64\); — 2
-   SELECT bit\_count\(9, 8\); — 2
-   SELECT bit\_count\(-7, 64\); — 62
-   SELECT bit\_count\(-7, 8\); — 6 |
|bitwise\_and\(x, y\) → bigint|Returns the bitwise AND of x and y in two's complement.|N/A|
|bitwise\_not\(x\) → bigint|Returns the bitwise NOT of x in two's complement.|N/A|
|bitwise\_or\(x, y\) → bigint|Returns the bitwise OR of x and y in two's complement.|N/A|
|bitwise\_xor\(x, y\) → bigint|Returns the bitwise XOR of x and y in two's complement.|None|

