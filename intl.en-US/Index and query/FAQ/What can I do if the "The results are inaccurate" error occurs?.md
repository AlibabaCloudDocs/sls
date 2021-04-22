# What can I do if the "The results are inaccurate" error occurs?

This topic describes how to resolve the **The results are inaccurate** error that may occur when you query logs.

## Issue

If the **The results are inaccurate** error occurs when you query logs, Log Service fails to scan all log data and the returned results are inaccurate.

## Cause

The error may occur due to the following causes:

-   The time range specified for the query is too large.

    For example, if the specified time range is three months or one year, Log Service cannot scan all data of this time period. Only partial results are returned. Therefore, data accuracy is compromised.

-   The conditions specified in the search statement are too complicated.

    For example, if you specify 30 conditions in a query statement, Log Service cannot read all log data.

-   The amount of data to be read is too large.

    If you specify multiple fields in an analytic statement, and the data volume to be read exceeds the read capacity of a shard, the returned results are inaccurate. This is because each shard can read only 1 GB of data.


## Solution

Narrow down the query time range and perform multiple queries \(up to 10 queries\) to get the complete results.

