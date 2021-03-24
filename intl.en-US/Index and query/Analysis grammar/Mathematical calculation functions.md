# Mathematical calculation functions

This topic describes the syntax of mathematical calculation functions and provides usage examples.

## Basic syntax

**Note:**

-   Mathematical calculation functions support the following operators: `+-*/%`.
-   In the following functions, x and y can be numbers, log fields, or expressions whose calculation result is a number.

|Function|Description|
|:-------|:----------|
|abs\(x\)|Calculates the absolute value of a number.|
|cbrt\(x\)|Calculates the cube root of a number.|
|sqrt\(x\)|Calculates the square root of a number.|
|cosine\_similarity\(x,y\)|Calculates the cosine similarity between x and y.|
|degrees\(x\)|Converts radians to degrees.|
|radians\(x\)|Converts degrees to radians.|
|e\(\)|Calculates the natural logarithm of a number.|
|exp\(x\)|Returns the exponent of a natural logarithm.|
|ln\(x\)|Calculates the natural logarithm of a number.|
|log2\(x\)|Calculates the base-2 logarithm of a number.|
|log10\(x\)|Calculates the base-10 logarithm of a number.|
|log\(x,b\)|Calculates the base-b logarithm of a number.|
|pi\(\)|Returns the value of π to 14 decimal places.|
|pow\(x,b\)|Calculates the value of a number raised to the power of b.|
|rand\(\)|Returns a random number.|
|random\(0,n\)|Returns a random number that is greater than or equal to 0 and less than n.|
|round\(x\)|Returns a number rounded to the nearest integer.|
|round\(x, N\)|Returns a number rounded to N decimal places.|
|floor\(x\)|Returns a number rounded down to the nearest integer.For example, when you execute the `* | SELECT floor(2.5)` statement, 2.0 is returned. |
|ceiling\(x\)|Returns a number rounded up to the nearest integer.For example, when you run the `* | SELECT ceiling(2.5)` statement, 3.0 is returned. |
|from\_base\(varchar, bigint\)|Converts a string to a base-encoded number.|
|to\_base\(x, radix\)|Converts a number to a base-encoded string.|
|truncate\(x\)|Truncates the fractional part of a number.|
|acos\(x\)|Calculates the arc cosine of a number.|
|asin\(x\)|Calculates the arc sine of a number.|
|atan\(x\)|Calculates the arc tangent of a number.|
|atan2\(y,x\)|Calculates the arc tangent of the quotient of a number divided by another number.|
|cos\(x\)|Calculates the cosine of a number.|
|sin\(x\)|Calculates the sine of a number.|
|cosh\(x\)|Calculates the hyperbolic cosine of a number.|
|tan\(x\)|Calculates the tangent of a number.|
|tanh\(x\)|Calculates the hyperbolic tangent of a number.|
|infinity\(\)|Returns a positive infinity value.|
|is\_nan\(x\)|Determines whether a value is a non-numeric value.|

## Example

Compare the number of page views \(PVs\) of today with the number of PVs of yesterday, and show the comparison result as a percentage.

-   Query statement

    ```
    * | SELECT diff [1] AS today, round((diff [3] -1.0) * 100, 2) AS growth FROM (SELECT compare(pv, 86400) as diff FROM (SELECT COUNT(*) as pv FROM website_log))
    ```

-   Query and analysis results

    ![round](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0935556161/p242658.png)


